# Oracle Linux 8 (systemd) + MongoDB Enterprise 8.0.x

## Bước 0 — Điều kiện tiên quyết (OL8, phiên bản MongoDB, kernel)

* **Oracle Linux chỉ được hỗ trợ với RHCK (Red Hat Compatible Kernel), không hỗ trợ UEK.** Xác minh kernel đang chạy:

  ```bash
  uname -r
  # nếu thấy 'uek' trong chuỗi version -> đang dùng UEK (không được hỗ trợ)
  ```

  Tài liệu: [Install MongoDB — Supported Platforms (v8.0)](https://www.mongodb.com/docs/v8.0/installation/) → ghi chú \[1]\[2] “MongoDB only supports Oracle Linux running the Red Hat Compatible Kernel (RHCK)…”. ([MongoDB][1])

* **Phiên bản MongoDB 8.0**: Một số khuyến nghị (THP, TCMalloc) **chỉ áp dụng từ 8.0 trở lên**. Xem [Release Notes 8.0](https://www.mongodb.com/docs/manual/release-notes/8.0/). ([MongoDB][2])

---

## Bước 1 — Chỉnh `/etc/sysctl.conf` (kernel & TCP keepalive)

Mở file:

```bash
sudo vi /etc/sysctl.conf
```

Thêm/chỉnh các dòng sau (giá trị khởi điểm MongoDB khuyến nghị cho hệ thống lớn + keepalive 120s):

```ini
vm.swappiness = 1
vm.max_map_count = 131060
fs.file-max = 98000
kernel.pid_max = 64000
kernel.threads-max = 64000
net.ipv4.tcp_keepalive_time = 120
```

**Nguồn & lý do:**

* “For large systems, the following values provide a good starting point… `fs.file-max 98000`, `kernel.pid_max 64000`, `kernel.threads-max 64000`, `vm.max_map_count 131060`.” (Operations Checklist). [Mở link](https://www.mongodb.com/docs/manual/administration/production-checklist-operations/#linux) ([MongoDB][3])
* `vm.swappiness`: khuyến nghị đặt **1 (hoặc 0)** trong môi trường chạy chung dịch vụ để hạn chế swap. [Production Notes](https://www.mongodb.com/docs/manual/administration/production-notes/#manage-swap-space-and-swappiness) ([MongoDB][4])
* Keepalive: khuyến nghị **120 giây** để giảm nguy cơ rớt kết nối qua LB/cloud. [Production Notes → Adjust tcp\_keepalive\_time](https://www.mongodb.com/docs/manual/administration/production-notes/#adjust-tcp-keepalive-time) ([MongoDB][4])

**Đoạn văn nguyên văn (verbatim):**

* “`fs.file-max` value of **98000**,”
* “`kernel.pid_max` value of **64000**,”
* “`kernel.threads-max` value of **64000**,”
* “`vm.max_map_count` value of **131060**”  — *trích* [Operations Checklist](https://www.mongodb.com/docs/manual/administration/production-checklist-operations/#linux). ([MongoDB][3])
* “**Set `vm.swappiness` to `1` or `0`.**” — *trích* [Production Notes](https://www.mongodb.com/docs/manual/administration/production-notes/#manage-swap-space-and-swappiness). ([MongoDB][4])

> Gợi ý: Để kiểm tra trước khi sửa, chạy:
>
> ```bash
> sysctl fs.file-max kernel.pid_max kernel.threads-max vm.max_map_count vm.swappiness net.ipv4.tcp_keepalive_time
> ```

---

## Bước 2 — Áp dụng ngay các thay đổi sysctl (không cần reboot)

Áp dụng tạm thời (ví dụ keepalive):

```bash
sudo sysctl -w net.ipv4.tcp_keepalive_time=120
# hoặc nạp lại toàn bộ cấu hình từ file
sudo sysctl -p /etc/sysctl.conf
```

**Đoạn văn nguyên văn (verbatim) & cảnh báo persist:**

* “To change the `tcp_keepalive_time` value, you can use… `sudo sysctl -w net.ipv4.tcp_keepalive_time=<value>`”
* “**These operations do not persist across system reboots.**” — *trích* [FAQ: Diagnostics → Does TCP keepalive time affect MongoDB Deployments?](https://www.mongodb.com/docs/manual/faq/diagnostics/#does-tcp-keepalive-time-affect-mongodb-deployments). ([MongoDB][5])

> Lưu ý: Giá trị keepalive **> 300s** sẽ bị **mongod/mongos** override về **300s** cho socket của chúng (chi tiết trong FAQ). [Xem mục keepalive](https://www.mongodb.com/docs/manual/faq/diagnostics/#adjusting-the-tcp-keepalive-value) ([MongoDB][5])

---

## Bước 3 — Đặt ulimit cho dịch vụ `mongod` qua systemd (khuyến nghị)

Tạo/hiệu chỉnh **drop-in** cho unit `mongod`:

```bash
sudo systemctl edit mongod
```

Trong khối `[Service]` thêm/kiểm tra:

```ini
[Service]
LimitNOFILE=64000
LimitNPROC=64000
LimitMEMLOCK=infinity
LimitAS=infinity
LimitFSIZE=infinity
LimitCPU=infinity
```

Lưu và nạp lại:

```bash
sudo systemctl daemon-reload
```

**Đoạn văn nguyên văn (verbatim):**

* “`LimitNOFILE=64000`”
* “`LimitNPROC=64000`” — *trích ví dụ systemd* trong [UNIX ulimit Settings](https://www.mongodb.com/docs/manual/reference/ulimit/#linux-distributions-using-systemd). ([MongoDB][6])

**Ghi chú RHEL/OL8:** “With RHEL / CentOS 8, separate `nproc` values are **no longer necessary**. The `ulimit` command is sufficient…” — [UNIX ulimit Settings](https://www.mongodb.com/docs/manual/reference/ulimit/#red-hat-linux-enterprise-server-and-centos). ([MongoDB][6])

**Bắt buộc sau khi đổi ulimit:**

* “**Always remember to restart your `mongod` and `mongos` instances after changing the `ulimit` settings to ensure that the changes take effect.**” — [UNIX ulimit Settings](https://www.mongodb.com/docs/manual/reference/ulimit/#recommended-ulimit-settings). ([MongoDB][6])

---

## Bước 4 — **Bật Transparent Huge Pages (THP)** cho MongoDB 8.0+

Từ 8.0, MongoDB dùng TCMalloc nâng cấp, **yêu cầu THP bật trước khi `mongod` khởi động**.
**Service systemd mẫu (theo tài liệu MongoDB):**

```bash
sudo tee /etc/systemd/system/enable-transparent-huge-pages.service >/dev/null <<'EOF'
[Unit]
Description=Enable Transparent Hugepages (THP)
DefaultDependencies=no
After=sysinit.target local-fs.target
Before=mongod.service

[Service]
Type=oneshot
ExecStart=/bin/sh -c 'echo always | tee /sys/kernel/mm/transparent_hugepage/enabled > /dev/null && echo defer+madvise | tee /sys/kernel/mm/transparent_hugepage/defrag > /dev/null && echo 0 | tee /sys/kernel/mm/transparent_hugepage/khugepaged/max_ptes_none > /dev/null && echo 1 | tee /proc/sys/vm/overcommit_memory > /dev/null'

[Install]
WantedBy=basic.target
EOF

sudo systemctl daemon-reload
sudo systemctl start enable-transparent-huge-pages
sudo systemctl enable enable-transparent-huge-pages
```

Kiểm tra nhanh:

```bash
cat /sys/kernel/mm/transparent_hugepage/enabled
cat /sys/kernel/mm/transparent_hugepage/defrag
```

**Đoạn văn nguyên văn (verbatim):**

* “**In MongoDB 8.0 and later, ensure that THP is enabled before `mongod` starts…**” — [TCMalloc Performance](https://www.mongodb.com/docs/manual/administration/tcmalloc-performance/#enable-transparent-hugepages-thp). ([MongoDB][7])

**Lưu ý RHEL/OL:** có bản dùng path **`/sys/kernel/mm/redhat_transparent_hugepage/enabled`** → chỉnh lại `ExecStart` nếu cần. [Chi tiết tại đây](https://www.mongodb.com/docs/manual/administration/tcmalloc-performance/#note). ([MongoDB][7])

> Nếu bạn chạy **MongoDB ≤ 7.0**, hướng dẫn là **tắt THP** (trang riêng cho phiên bản cũ cũng dẫn rõ: “Starting in MongoDB 8.0… enable THP”). [Disable THP (legacy guidance)](https://www.mongodb.com/docs/manual/tutorial/disable-transparent-huge-pages/) ([MongoDB][8])

---

## Bước 5 — Khởi động lại MongoDB để nhận cấu hình mới

```bash
sudo systemctl restart mongod
```

* Nhắc lại yêu cầu **restart** sau khi đổi `ulimit`. [UNIX ulimit Settings](https://www.mongodb.com/docs/manual/reference/ulimit/#recommended-ulimit-settings) ([MongoDB][6])

---

## Kiểm tra sau cấu hình

```bash
# 1) Giới hạn hiện áp lên tiến trình mongod
cat /proc/$(pidof mongod)/limits

# 2) Các sysctl quan trọng
sysctl vm.swappiness vm.max_map_count fs.file-max kernel.pid_max kernel.threads-max net.ipv4.tcp_keepalive_time

# 3) Log dịch vụ
sudo journalctl -u mongod -b --no-pager | tail -n 100
```

* Nếu **open files < 64000**, MongoDB sẽ sinh **startup warning**. Tham chiếu: [Production Notes → Recommended Configuration](https://www.mongodb.com/docs/manual/administration/production-notes/#recommended-configuration). ([MongoDB][4])

---

## (Tùy chọn) Mẹo tinh chỉnh thêm cho OL8

* **Cgroups & swappiness**: trong môi trường cgroup, để đảm bảo `vm.swappiness` được áp dụng, cân nhắc cấu hình liên quan (xem ghi chú trong Production Notes). [Link](https://www.mongodb.com/docs/manual/administration/production-notes/#notes-on-swappiness-and-cgroups) ([MongoDB][4])
* **Connection pools**: điều chỉnh kích thước pool phù hợp tải; keepalive 120 giúp giảm rớt kết nối qua LB. [Production Notes](https://www.mongodb.com/docs/manual/administration/production-notes/#adjust-tcp-keepalive-time) | [Tuning Connection Pool](https://www.mongodb.com/docs/manual/tutorial/connection-pool-performance-tuning/) ([MongoDB][4])

---

## Tài liệu gốc đã trích dẫn (bấm để mở)

1. **Operations Checklist for Self-Managed Deployments (MongoDB 8.0)** — giá trị kernel khởi điểm, keepalive 120s
   👉 [https://www.mongodb.com/docs/manual/administration/production-checklist-operations/](https://www.mongodb.com/docs/manual/administration/production-checklist-operations/) ([MongoDB][3])

2. **FAQ: Does TCP keepalive time affect MongoDB Deployments?** — lệnh `sysctl -w`, cảnh báo **không persist** qua reboot & ngưỡng 300s
   👉 [https://www.mongodb.com/docs/manual/faq/diagnostics/#does-tcp-keepalive-time-affect-mongodb-deployments](https://www.mongodb.com/docs/manual/faq/diagnostics/#does-tcp-keepalive-time-affect-mongodb-deployments) ([MongoDB][5])

3. **UNIX ulimit Settings (MongoDB 8.0)** — mẫu `LimitNOFILE/LimitNPROC` (systemd) & yêu cầu **restart**
   👉 [https://www.mongodb.com/docs/manual/reference/ulimit/](https://www.mongodb.com/docs/manual/reference/ulimit/) ([MongoDB][6])

4. **TCMalloc Performance (MongoDB 8.0)** — **bật THP** trước khi `mongod` start; service mẫu; lưu ý path `redhat_transparent_hugepage`
   👉 [https://www.mongodb.com/docs/manual/administration/tcmalloc-performance/](https://www.mongodb.com/docs/manual/administration/tcmalloc-performance/) ([MongoDB][7])

5. **Production Notes (MongoDB 8.0)** — `vm.swappiness` (1 hoặc 0), keepalive 120s, cảnh báo “open files < 64000”
   👉 [https://www.mongodb.com/docs/manual/administration/production-notes/](https://www.mongodb.com/docs/manual/administration/production-notes/) ([MongoDB][4])

6. **Install MongoDB — Supported Platforms (v8.0)** — OL8 **chỉ hỗ trợ RHCK**, **không UEK**
   👉 [https://www.mongodb.com/docs/v8.0/installation/](https://www.mongodb.com/docs/v8.0/installation/) ([MongoDB][1])

7. *(tham khảo lịch sử/đối chiếu)* **Disable Transparent Huge Pages (legacy)** — dành cho bản ≤ 7.0
   👉 [https://www.mongodb.com/docs/manual/tutorial/disable-transparent-huge-pages/](https://www.mongodb.com/docs/manual/tutorial/disable-transparent-huge-pages/) ([MongoDB][8])

---

### Tóm tắt nhanh các trích dẫn **verbatim** đã dùng

* “`fs.file-max` value of **98000**,”
* “`kernel.pid_max` value of **64000**,”
* “`kernel.threads-max` value of **64000**,”
* “`vm.max_map_count` value of **131060**” — từ *Operations Checklist*. ([MongoDB][3])
* “**Set `vm.swappiness` to `1` or `0`.**” — từ *Production Notes*. ([MongoDB][4])
* “To change the `tcp_keepalive_time`… `sudo sysctl -w net.ipv4.tcp_keepalive_time=<value>`” + “**These operations do not persist across system reboots.**” — từ *FAQ: Diagnostics*. ([MongoDB][5])
* “`LimitNOFILE=64000`”, “`LimitNPROC=64000`” + “**Always remember to restart…**” — từ *UNIX ulimit Settings*. ([MongoDB][6])
* “**In MongoDB 8.0 and later, ensure that THP is enabled before `mongod` starts…**” — từ *TCMalloc Performance*. ([MongoDB][7])


[1]: https://www.mongodb.com/docs/v8.0/installation/ "Install MongoDB - Database Manual - MongoDB Docs"
[2]: https://www.mongodb.com/docs/manual/release-notes/8.0/?utm_source=chatgpt.com "Release Notes for MongoDB 8.0"
[3]: https://www.mongodb.com/docs/manual/administration/production-checklist-operations/ "Operations Checklist for Self-Managed Deployments - Database Manual - MongoDB Docs"
[4]: https://www.mongodb.com/docs/manual/administration/production-notes/ "Production Notes for Self-Managed Deployments - Database Manual - MongoDB Docs"
[5]: https://www.mongodb.com/docs/manual/faq/diagnostics/ "FAQ: Self-Managed MongoDB Diagnostics - Database Manual - MongoDB Docs"
[6]: https://www.mongodb.com/docs/manual/reference/ulimit/ "UNIX ulimit Settings for Self-Managed Deployments - Database Manual - MongoDB Docs"
[7]: https://www.mongodb.com/docs/manual/administration/tcmalloc-performance/ "TCMalloc Performance Optimization for a Self-Managed Deployment - Database Manual - MongoDB Docs"
[8]: https://www.mongodb.com/docs/manual/tutorial/disable-transparent-huge-pages/?utm_source=chatgpt.com "Disable Transparent Hugepages (THP) for Self-Managed ... - MongoDB"
