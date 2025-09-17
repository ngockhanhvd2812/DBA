# Phụ Lục: Bổ Sung Cho Người Mới (Theo Từng Bước)

Mục này tổng hợp lý thuyết, sơ đồ Mermaid và gợi ý thao tác dành cho người mới bắt đầu, bám theo từng bước trong `DB/test_backup.md`.

Lưu ý khi xem sơ đồ Mermaid:
- flowchart/graph: theo luồng quyết định; sequence: theo thứ tự trao đổi giữa thành phần.
- Màu xanh lá = OK, vàng = cảnh báo, đỏ = lỗi/cần xử lý.

## Bước 1) Kết nối SQLPlus/RMAN

Mục tiêu: nắm vững biến môi trường Oracle (ORACLE_HOME/ORACLE_SID), quyền SYSDBA, và khác biệt V$ vs GV$ khi RAC.

Khái niệm nhanh:
- V$ = view cho một instance; GV$ = Global View gom thông tin tất cả instance (RAC).
- CDB/PDB: khi chạy câu lệnh kiểm tra, hãy đảm bảo ở CDB$ROOT để có cấu hình toàn cục.

Sơ đồ kết nối cơ bản:
```mermaid
flowchart LR
    A[Shell/OS] --> B[Set ORACLE_HOME/ORACLE_SID]
    B --> C[sqlplus / as sysdba]
    B --> D[rman target /]
    C --> E[(Instance 1)]
    D --> E
    E --> F{CDB$ROOT?}
    F -->|Yes| G[Câu lệnh toàn cục]
    F -->|No| H[Đổi sang CDB$ROOT]

    style G fill:#c8e6c9
    style H fill:#fff9c4
```

V$ vs GV$ trong RAC:
```mermaid
graph TD
    subgraph "RAC Cluster"
      I1[Instance 1]:::node
      I2[Instance 2]:::node
    end
    V[V$ Views] --> I1
    GV[GV$ Views] --> I1
    GV --> I2

    classDef node fill:#e3f2fd
    style V fill:#fce4ec
    style GV fill:#e8f5e8
```

Gợi ý: luôn xác định ORACLE_SID trước khi vào sqlplus/rman; trong RAC, dùng GV$ để nhìn theo inst_id.

## Bước 2) Xác nhận trạng thái backup (SQL)

Mục tiêu: đánh giá nhanh job backup (loại, thời điểm, kết quả) và tìm mối liên hệ giữa job, set, piece.

Cột quan trọng trong GV$RMAN_BACKUP_JOB_DETAILS:
- input_type: DATABASE | ARCHIVELOG | CONTROLFILE | SPFILE
- status: COMPLETED | COMPLETED WITH WARNINGS | FAILED | RUNNING
- mbytes_processed/elapsed_seconds: gợi ý dung lượng / hiệu năng

Sơ đồ quyết định đánh giá job:
```mermaid
flowchart TD
    A[Lấy job 2 ngày gần] --> B{Status?}
    B -->|COMPLETED| C[OK]
    B -->|COMPLETED WITH WARNINGS| D[Xem warning chi tiết]
    B -->|FAILED| E[Xem chi tiết lỗi]
    B -->|RUNNING| F[Theo dõi]
    D --> G[Khoanh vùng warning]
    E --> H[Khoanh vùng lỗi]

    style C fill:#c8e6c9
    style D fill:#fff9c4
    style E fill:#ffcdd2
    style F fill:#bbdefb
```

Đường đi của một archived log:
```mermaid
sequenceDiagram
    participant LGWR as LGWR
    participant ARCH as ARCH
    participant RMAN as RMAN
    participant DISK as ASM/FRA
    participant CATA as Control/Catalog

    LGWR->>ARCH: Switch redo
    ARCH->>DISK: Ghi archived log
    RMAN->>DISK: Backup archived log
    RMAN->>CATA: Đánh dấu BACKUP_COUNT
    RMAN->>DISK: (Tuỳ chọn) DELETE INPUT
```

## Bước 3) Kiểm tra nhanh bằng RMAN

Mục tiêu: dùng LIST/CROSSCHECK/REPORT để kiểm tra thời gian gần, tình hình nhận dạng backup.

Tóm tắt chức năng:
- LIST: liệt kê nhanh cái đang có
- CROSSCHECK: đối soát metadata và thực tế
- REPORT: đánh giá obsolete/need backup

Sơ đồ thao tác:
```mermaid
flowchart TD
    A[LIST BACKUP] --> B{Bất định?}
    B -->|Thiếu| C[CROSSCHECK]
    C --> D[REPORT OBSOLETE]
    D --> E{Xoá obsolete?}
    E -->|Yes| F[DELETE OBSOLETE]
    E -->|No| G[Giữ lại]

    style F fill:#e8f5e9
    style G fill:#fff3e0
```

DISK vs SBT_TAPE (NetBackup):
```mermaid
graph TD
   A[RMAN Channels] --> B[DISK]
   A --> C[SBT_TAPE]
   B --> D[ASM/FRA/FS]
   C --> E[libobk/NetBackup]

   style B fill:#e3f2fd
   style C fill:#fce4ec
```

## Bước 4) NetBackup Status 6 & xử lý

Mục tiêu: hiểu vì sao lỗi, định vị nhanh và xử lý an toàn.

Điểm kiểm tra log theo lớp:
```mermaid
flowchart LR
    A[Oracle Alert Log] --> B[RMAN output]
    B --> C[NetBackup dbclient/bphdb]
    C --> D[Media server bptm]
    D --> E[Storage server logs]

    style A fill:#bbdefb
    style B fill:#bbdefb
    style C fill:#bbdefb
    style D fill:#bbdefb
    style E fill:#bbdefb
```

Khoanh vùng nguyên nhân nhanh:
```mermaid
graph TD
  S[Status 6] --> A[Archive log]
  S --> P[Permission]
  S --> F[Fileset]
  S --> N[Network]
  A --> A1[Không shared]
  A --> A2[Bị xoá/mất]
  P --> P1[OS/Agent]
  F --> F1[FILESPERSET cao]
  N --> N1[Timeout]

  style S fill:#ffcdd2
  style A1 fill:#fff9c4
  style A2 fill:#fff9c4
  style P1 fill:#fff9c4
  style F1 fill:#fff9c4
  style N1 fill:#fff9c4
```

## Bước 5) Lý thuyết cốt lõi

Thuật ngữ nhanh cho người mới:
- Backup Set: đơn vị logic, gồm nhiều Piece
- Backup Piece: file vật lý được lưu trữ
- Image Copy: bản sao 1:1 datafile
- Channel: luồng I/O; TYPE DISK hoặc SBT_TAPE
- FRA: vùng Oracle quản lý backup/archivelog
- Retention Policy: RECOVERY WINDOW hoặc REDUNDANCY

Minh hoạ Retention Policy:
```mermaid
graph TD
  RP[Retention Policy] --> RW[RECOVERY WINDOW x DAYS]
  RP --> RD[REDUNDANCY n]
  RW --> T1[Giữ đủ để hồi về điểm]
  RD --> T2[Giữ n bản backup gần nhất]

  style RW fill:#e8f5e9
  style RD fill:#e8f5e9
```

Backup Set vs Image Copy:
```mermaid
graph LR
  A[Backup Strategy] --> BS[Backup Set]
  A --> IC[Image Copy]
  BS --> B1[Compress/Dedupe tốt]
  BS --> B2[Khôi phục có bước]
  IC --> I1[Phục hồi nhanh]
  IC --> I2[Kém tiết kiệm dung lượng]

  style BS fill:#e3f2fd
  style IC fill:#fce4ec
```

## Bước 6) Luồng backup Oracle với NetBackup

Xử lý nhanh trong chuỗi backup:
```mermaid
sequenceDiagram
    participant RMAN
    participant SBT as SBT/NetBackup
    participant MED as MediaSrv
    participant STG as Storage
    RMAN->>SBT: Gọi BACKUP ...
    SBT->>MED: Khởi tạo job
    MED->>STG: Ghi piece
    alt Archived log không shared
      STG-->>MED: Lỗi truy cập
      MED-->>SBT: Fail
      SBT-->>RMAN: Status 6
    else OK
      STG-->>MED: OK
      MED-->>SBT: OK
      SBT-->>RMAN: COMPLETED
    end
```

## Bước 7) Hướng dẫn thao tác chi tiết

Checklist thao tác nhanh:
- Xác định ORACLE_SID/ORACLE_HOME và quyền SYSDBA
- Kết nối sqlplus/rman
- Chạy các truy vấn job/piece/archivelog
- Dùng LIST/CROSSCHECK/REPORT
- Ghi nhận và báo cáo

Luồng thao tác gợi ý:
```mermaid
flowchart TD
    A[Chuẩn bị] --> B[Kết nối]
    B --> C[Kiểm tra SQL]
    C --> D[Kiểm tra RMAN]
    D --> E{Sự cố?}
    E -->|Có| F[Xử lý]
    E -->|Không| G[Hoàn tất]

    style G fill:#c8e6c9
    style F fill:#fff9c4
```

## Bước 8) Tóm tắt quy trình

Quyết định một-trang cho người mới:
```mermaid
flowchart LR
   S[Bắt đầu] --> J[Job status]
   J -->|OK| P[Pieces]
   J -->|NOK| X[Xử lý lỗi]
   P -->|A| L[Archive logs]
   P -->|X/D| X
   L -->|OK| R[RMAN crosscheck/report]
   L -->|NOK| X
   R --> C[Kết luận OK]

   style C fill:#c8e6c9
   style X fill:#ffcdd2
```

---

FAQ người mới:
- Nên dùng DELETE INPUT khi nào? => Khi đã xác minh backup archivelog OK, tránh rủi ro lỗi truy cập.
- RECOVERY WINDOW hay REDUNDANCY? => Tuỳ RPO; WINDOW tối ưu cho phục hồi theo mốc thời gian.
- DISK hay SBT? => DISK đơn giản/nhanh, SBT dùng khi tích hợp bên thứ 3 (NetBackup,...).

Mẫu lệnh RMAN tham khảo:
```rman
LIST BACKUP SUMMARY;
LIST BACKUP OF DATABASE;
LIST BACKUP OF ARCHIVELOG FROM TIME "SYSDATE-1";
CROSSCHECK BACKUP;
REPORT OBSOLETE;
```

