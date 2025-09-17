
# 1) Vào SQL/RMAN (trên bcqg1)

```bash
# trên host bcqg-db01
export ORACLE_SID=bcqg1
export ORACLE_HOME=/u01/app/oracle/product/19c/db_1
$ORACLE_HOME/bin/sqlplus / as sysdba
```

Bạn cũng nên mở RMAN để liệt kê nhanh:

```bash
$ORACLE_HOME/bin/rman target /
```

> Lưu ý: chạy các truy vấn dưới đây **ở CDB\$ROOT** và dùng view **GV\$** để nhìn đủ các instance (RAC). Tài khoản cần có quyền `SYSDBA` hoặc `SELECT_CATALOG_ROLE`.

---

# 2) Truy vấn SQL xác nhận trạng thái backup

## 2.1. Trạng thái job RMAN 2 ngày gần nhất

```sql
-- Job RMAN (database/archivelog/controlfile) – tổng quan
SELECT inst_id,
       start_time, end_time,
       input_type, status, output_device_type,
       mbytes_processed, elapsed_seconds
FROM   gv$rman_backup_job_details
WHERE  start_time >= SYSDATE - 2
ORDER  BY start_time DESC;
```

* `STATUS` mong muốn: `COMPLETED`.
* `FAILED` hoặc `COMPLETED WITH WARNINGS` = cần xử lý.

## 2.2. Nhật ký chi tiết thao tác RMAN

```sql
-- Log mức thao tác (BACKUP ARCHIVELOG, CONTROL FILE…)
SELECT inst_id,
       TO_CHAR(start_time,'YYYY-MM-DD HH24:MI:SS') AS start_time,
       TO_CHAR(end_time,'YYYY-MM-DD HH24:MI:SS')   AS end_time,
       operation, status, object_type, output_device_type
FROM   gv$rman_status
WHERE  start_time >= SYSDATE - 2
ORDER  BY recid DESC;
```

Tìm xem có dòng `BACKUP ARCHIVELOG` bị `FAILED/RUNNING WITH ERRORS` hay không.

## 2.3. Các backup piece/set vừa tạo (tình trạng vật lý)

```sql
-- Piece phải ở trạng thái 'A' (Available)
SELECT p.inst_id, p.handle,
       p.start_time, p.completion_time,
       p.bytes/1024/1024 AS mb,
       p.status
FROM   gv$backup_piece p
WHERE  p.completion_time >= SYSDATE - 2
ORDER  BY p.completion_time DESC;
```

`STATUS`: `A`=Available, `D`=Deleted, `X`=Expired.

```sql
-- Backup set có kèm controlfile, incremental level...
SELECT bs.inst_id,
       bs.set_stamp, bs.set_count,
       bs.incremental_level, bs.controlfile_included,
       bs.completion_time
FROM   gv$backup_set bs
WHERE  bs.completion_time >= SYSDATE - 2
ORDER  BY bs.completion_time DESC;
```

## 2.4. Bao phủ ARCHIVELOG (điểm thường gây fail)

```sql
-- Những archived log CHƯA hề được backup (backup_count = 0)
SELECT inst_id, thread#, sequence#, first_time, next_time, dest_id
FROM   gv$archived_log
WHERE  first_time >= SYSDATE - 2
AND    backup_count = 0
ORDER  BY thread#, sequence#;

-- Thống kê lần backup ARCHIVELOG gần nhất
SELECT MAX(completion_time) AS last_archivelog_backup
FROM   v$backup_redolog;
```

## 2.5. Kiểm tra nơi lưu ARCHIVELOG (shared hay local)

```sql
-- Xem cấu hình đích archive
SELECT dest_id, status, target, destination, binding, valid_now
FROM   v$archive_dest
WHERE  status = 'VALID';
```

Nếu là RAC, tốt nhất archive nên ở **ASM/FRA dùng chung** (ví dụ `+FRA/...`) hoặc một shared disk, để node chạy backup nhìn thấy đủ file.

> Nếu bạn dùng **Recovery Catalog**, có thể chạy tương tự trên các view `RC_`:

```sql
-- Khi có catalog
SELECT start_time, end_time, input_type, status
FROM   rc_rman_backup_job_details
WHERE  start_time >= SYSDATE - 2
ORDER  BY start_time DESC;
```

---

# 3) Kiểm tra nhanh bằng RMAN (không cần nhớ SQL)

Trong phiên `rman target /`:

```rman
LIST BACKUP SUMMARY COMPLETED BETWEEN "2025-09-16 19:30:00" AND "2025-09-16 21:00:00";
LIST BACKUP OF ARCHIVELOG FROM TIME "SYSDATE-1";
LIST BACKUP OF CONTROLFILE;
-- Nếu nghi ngờ dữ liệu catalog/controlfile lệch:
CROSSCHECK BACKUP;
REPORT OBSOLETE;
```

---

# 4) Vì sao NetBackup báo status 6 & cách xử lý nhanh

**Triệu chứng trong log:**

* `dbclient ... status: 6` và `the backup failed to back up the requested files`.
* Kịch bản backup có đoạn **`BACKUP ARCHIVELOG ALL DELETE INPUT`** sau database backup.

**Lý do thường gặp (đặc biệt khi RAC/ASM):**

1. **ARCHIVELOG không ở shared disk** hoặc node backup không thấy được file → RMAN báo “expected archived log not found…”, NetBackup nhận lỗi status 6.
2. File archive vừa bị di chuyển/xóa bởi tác vụ khác → thiếu file lúc RMAN gom `ALL`.
3. Quyền hệ thống không đủ để **xóa** file khi `DELETE INPUT` (nếu backup thành công nhưng xóa lỗi thường ra “COMPLETED WITH WARNINGS”, tuy nhiên một số trường hợp kịch bản vẫn fail toàn job).
4. Lựa chọn `FILESPERSET 10` trên storage dedupe làm luồng gom file “thô” lớn; Veritas khuyến nghị **`FILESPERSET 1`** để tối ưu dedupe & đôi khi tránh timeout gom file.

**Cách khoanh vùng/sửa nhanh:**

* Tạm **bỏ `DELETE INPUT`** để xác minh bước backup có pass:

  ```rman
  BACKUP AS COMPRESSED BACKUPSET ARCHIVELOG ALL;  -- bỏ DELETE INPUT
  ```
* Hoặc chỉ backup **NOT BACKED UP** và **SKIP INACCESSIBLE**:

  ```rman
  BACKUP AS COMPRESSED BACKUPSET ARCHIVELOG
    NOT BACKED UP 1 TIMES
    SKIP INACCESSIBLE
    DELETE INPUT;  -- thêm lại sau khi xác minh
  ```
* Với dedupe STU: đặt **`FILESPERSET 1`**:

  ```rman
  CONFIGURE DEVICE TYPE SBT PARALLELISM 4;
  BACKUP INCREMENTAL LEVEL 0
    FILESPERSET 1
    AS COMPRESSED BACKUPSET DATABASE;
  ```
* Đảm bảo ARCHIVELOG nằm ở **ASM/FRA shared** (ví dụ `+FRA`) hoặc chạy job luôn trên node tạo ra archive đó.
* Kiểm tra quyền user OS của RMAN/NetBackup agent với thư mục archive.

---

# 5) Lý thuyết ngắn gọn cho người mới

* **RMAN job “thành công”** khi:

  1. `GV$RMAN_BACKUP_JOB_DETAILS.STATUS = 'COMPLETED'`.
  2. Các **backup piece** tương ứng ở `GV$BACKUP_PIECE.STATUS = 'A'`.
  3. Với ARCHIVELOG: `GV$ARCHIVED_LOG.BACKUP_COUNT > 0` cho các sequence mới sinh.
* **NetBackup status 6** chỉ là “vỏ” báo RMAN trả lỗi — muốn biết “lỗi gì” phải xem **RMAN status/alert log** (thư mục `bphdb/dbclient` của NetBackup và alert log Oracle).
* `DELETE INPUT` sẽ **xóa file archive local** sau khi backup **thành công**. Trong RAC, **archivelog phải ở chỗ mọi node truy cập được**.

---

# 6) Sơ đồ mermaid – luồng backup Oracle → NetBackup (dedupe)

```mermaid
sequenceDiagram
    autonumber
    participant DBA as DBA/RMAN
    participant ORA as Oracle DB (bcqg1)
    participant SBT as Channel SBT_TAPE (libobk.so64)
    participant NBU as NetBackup Client (bphdb/bpbrm)
    participant MED as Media Server (bptm)
    participant STG as Storage Server (PureDisk/WORM02-POOL)

    DBA->>ORA: RUN { BACKUP DATABASE; BACKUP ARCHIVELOG ... }
    ORA->>SBT: Gọi channel TYPE 'SBT_TAPE'
    SBT->>NBU: Gọi libobk.so64 (NetBackup for Oracle)
    NBU->>MED: Khởi tạo job, cấp tài nguyên STU (WORM02-STU)
    MED->>STG: Ghi backup piece vào Dedup Pool (PureDiskVolume)
    STG-->>MED: Trả kết quả ghi (dedupe/compress)
    MED-->>NBU: Hoàn tất ghi / mã hóa in-transit
    NBU-->>SBT: Trả mã trạng thái
    SBT-->>ORA: RMAN nhận trạng thái (COMPLETED/FAILED)
    ORA-->>DBA: Cập nhật view V$RMAN_* và V$BACKUP_*
```

---

## Mẹo kiểm tra “nhanh – gọn – chuẩn”

* **1 lệnh xác nhận gần nhất**:

```sql
SELECT status, input_type, start_time, end_time
FROM   v$rman_backup_job_details
WHERE  start_time = (SELECT MAX(start_time) FROM v$rman_backup_job_details);
```

* **Danh sách ARCHIVELOG chưa được backup** (dễ gây fail):

```sql
SELECT thread#, sequence#, first_time
FROM   gv$archived_log
WHERE  backup_count = 0
ORDER  BY thread#, sequence#;
```

---

Nếu bạn muốn, mình có thể soạn nhanh **một script SQL/RMAN hoàn chỉnh** cho môi trường bcqg\* (đã tối ưu FILESPERSET cho dedupe, thêm `SKIP INACCESSIBLE`) để bạn dán chạy và đối chiếu.
