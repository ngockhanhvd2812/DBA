# Danh sách kiểm tra trước khi cài đặt DB (hệ họ RHEL/OL8) 🐧💾

> Các bản phân phối mục tiêu: Oracle Linux 8 / RHEL 8 clones. Các lệnh sử dụng `yum`, được hỗ trợ bởi DNF trên OL8—vì vậy `yum update -y` vẫn hợp lệ và quen thuộc. ([Tài liệu Oracle][1], [phoenixNAP | Global IT Services][2])

---

## 0) Quy trình tổng thể 🗺️

```mermaid
flowchart TD
    A[Máy chủ OL8 mới]:::start --> B[Cập nhật gói<br/>yum update -y]:::blue
    B --> C{Cần khởi động lại?}:::amber
    C -->|Kernel/cập nhật quan trọng| D[Khởi động lại]:::purple
    C -->|Không| E[Dừng & Vô hiệu hóa firewalld]:::green
    D --> E
    E --> F[Đặt SELINUX=disabled<br/>/etc/selinux/config]:::orange
    F --> G[Khởi động lại]:::teal
    G --> H[Xác minh:<br/>systemctl status firewalld,<br/>sestatus,<br/>rpm -q kernel-uek,<br/>uname -r]:::gray
    H --> I[Máy chủ sẵn sàng cài đặt DB]:::success
    
    subgraph WARNINGS["⚠️ CẢNH BÁO QUAN TRỌNG"]
        W1[CẢNH BÁO: Loại trừ MongoDB<br/>khỏi yum update<br/>--exclude=mongodb*,mongo*]:::warning
    end
    
    subgraph MONGO_EXCLUDE["🛑 3 BƯỚC LOẠI TRỪ MONGODB"]
        M1[Sao lưu dnf.conf<br/>cp /etc/dnf/dnf.conf /etc/dnf/dnf.conf.bak]:::mongo_step
        M2[Chỉnh sửa dnf.conf<br/>excludepkgs=mongodb-org*]:::mongo_step
        M3[Kiểm tra<br/>dnf check-update | grep -i mongo]:::mongo_step
    end
    
    B -.-> M1
    M3 --> B
    
    classDef start fill:#1f2937,color:#fff,stroke:#0ea5e9,stroke-width:2px;
    classDef blue fill:#3b82f6,color:#fff,stroke:#1e40af;
    classDef amber fill:#f59e0b,color:#111,stroke:#92400e;
    classDef purple fill:#8b5cf6,color:#fff,stroke:#4c1d95;
    classDef green fill:#22c55e,color:#0a0,stroke:#065f46;
    classDef orange fill:#fb923c,color:#111,stroke:#c2410c;
    classDef teal fill:#14b8a6,color:#072,stroke:#115e59;
    classDef gray fill:#9ca3af,color:#111,stroke:#374151;
    classDef success fill:#84cc16,color:#102a12,stroke:#365314;
    classDef warning fill:#ef4444,color:#fff,stroke:#7f1d1d,stroke-width:2px,stroke-dasharray: 5 5;
    classDef mongo_step fill:#f97316,color:#fff,stroke:#c2410c,stroke-width:2px;
```

---

## 1) Cập nhật hệ thống (`yum update -y`) 🔄💡

**Lý do:** Trên OL8, `yum` là giao diện dựa trên DNF. Chạy cập nhật đầy đủ đảm bảo bạn có kernel, glibc, OpenSSL và trình điều khiển thiết bị hiện tại trước khi cài đặt cơ sở dữ liệu—giảm thiểu bất ngờ sau khi cài đặt và tránh các vấn đề không tương thích ABI. Nếu kernel mới được kéo về, hãy khởi động lại một lần trước khi tiếp tục. ([Tài liệu Oracle][1], [phoenixNAP | Global IT Services][2])

Trong một số trường hợp, bạn có thể muốn loại trừ một số gói khỏi việc cập nhật để duy trì tính ổn định của hệ thống hoặc tránh các vấn đề tương thích với phần mềm cơ sở dữ liệu của bạn. Đối với các cài đặt MongoDB, bạn nên loại trừ các gói MongoDB khỏi việc cập nhật hệ thống để duy trì tính nhất quán về phiên bản. Bạn có thể sử dụng tùy chọn `--exclude` với yum để ngăn chặn các gói cụ thể khỏi việc cập nhật.

⚠️ **CẢNH BÁO QUAN TRỌNG**: Việc loại trừ các gói MongoDB khỏi lệnh `yum update` là **BẮT BUỘC** và **CỰC KỲ QUAN TRỌNG**. Nếu không thực hiện đúng bước này, có thể gây ra **hậu quả nghiêm trọng** đến hệ thống MongoDB đang chạy, bao gồm nhưng không giới hạn ở: mất dữ liệu, hỏng cấu hình, không tương thích phiên bản, và thậm chí làm cho toàn bộ cụm MongoDB không hoạt động. Hãy chắc chắn rằng bạn luôn sử dụng tùy chọn `--exclude=mongodb*,mongo*` khi chạy lệnh `yum update` trên hệ thống đã cài đặt MongoDB.

### 3 bước cực kỳ quan trọng để loại bỏ MongoDB trước khi yum update 🛑

Để đảm bảo MongoDB không bị ảnh hưởng bởi các bản cập nhật hệ thống, hãy thực hiện 3 bước sau trước khi chạy lệnh `yum update`:

1. **Sao lưu file cấu hình DNF:**
   ```bash
   cp /etc/dnf/dnf.conf /etc/dnf/dnf.conf.bak
   ```

2. **Chỉnh sửa file cấu hình DNF để loại bỏ MongoDB:**
   ```bash
   vi /etc/dnf/dnf.conf
   # Thêm dòng sau vào file:
   excludepkgs=mongodb-org*
   ```

3. **Kiểm tra xem MongoDB có còn trong danh sách cập nhật không:**
   ```bash
   dnf check-update | grep -i mongo
   ```


```
# 1) Cập nhật OS
sudo yum update -y --exclude=mongodb*,mongo*  # Loại trừ các gói MongoDB

# 2) Tắt tường lửa
sudo systemctl stop firewalld
sudo systemctl disable firewalld
# tùy chọn nhưng được khuyến nghị để tránh khởi động vô tình:
sudo systemctl mask firewalld

# 3) Vô hiệu hóa SELinux (vĩnh viễn)
sudo vi /etc/selinux/config   # đặt: SELINUX=disabled
sudo reboot

# 4) Xác minh sau khi khởi động lại
systemctl status firewalld
sestatus
# Kiểm tra kernel version
rpm -q kernel-uek
uname -r
```

**Sơ đồ minh họa cảnh báo quan trọng:**

```
# Bắt đầu cập nhật hệ thống
sudo yum update -y
# Nếu cài MongoDB, loại trừ MongoDB
sudo yum update -y --exclude=mongodb*,mongo*

# Tắt tường lửa
sudo systemctl stop firewalld
sudo systemctl disable firewalld
# tùy chọn nhưng được khuyến nghị để tránh khởi động vô tình:
sudo systemctl mask firewalld

# Vô hiệu hóa SELinux (vĩnh viễn)
sudo vi /etc/selinux/config   # đặt: SELINUX=disabled
sudo reboot

# Xác minh sau khi khởi động lại
systemctl status firewalld
sestatus
```

**Lệnh:**

```
# 1) Cập nhật OS
sudo yum update -y --exclude=mongodb*,mongo*  # Loại trừ các gói MongoDB

# 2) Tắt tường lửa
sudo systemctl stop firewalld
sudo systemctl disable firewalld
# tùy chọn nhưng được khuyến nghị để tránh khởi động vô tình:
sudo systemctl mask firewalld

# 3) Vô hiệu hóa SELinux (vĩnh viễn)
sudo vi /etc/selinux/config   # đặt: SELINUX=disabled
sudo reboot

# 4) Xác minh sau khi khởi động lại
systemctl status firewalld
sestatus
```

**Mini-flow:**

```
# 1) Cập nhật OS
sudo yum update -y --exclude=mongodb*,mongo*  # Loại trừ các gói MongoDB

# 2) Tắt tường lửa
sudo systemctl stop firewalld
sudo systemctl disable firewalld
# tùy chọn nhưng được khuyến nghị để tránh khởi động vô tình:
sudo systemctl mask firewalld

# 3) Vô hiệu hóa SELinux (vĩnh viễn)
sudo vi /etc/selinux/config   # đặt: SELINUX=disabled
sudo reboot

# 4) Xác minh sau khi khởi động lại
systemctl status firewalld
sestatus
```

### 3 bước cực kỳ quan trọng để loại bỏ MongoDB trước khi yum update (thêm vào quy trình) 🛑

```
# 1) Sao lưu cấu hình DNF
cp /etc/dnf/dnf.conf /etc/dnf/dnf.conf.bak

# 2) Thêm loại trừ MongoDB vào cấu hình DNF
echo "excludepkgs=mongodb-org*" >> /etc/dnf/dnf.conf

# 3) Kiểm tra xem MongoDB còn trong danh sách cập nhật không
dnf check-update | grep -i mongo
```

---

### Nguồn 📚

* OL8 sử dụng YUM dựa trên DNF; `yum update -y` hợp lệ trên OL8. ([Tài liệu Oracle][1], [phoenixNAP | Global IT Services][2])
* `firewalld` là gì (zones/services) và tại sao nên ưu tiên các ngoại lệ quy tắc thay vì tắt hoàn toàn. ([Tài liệu Red Hat][3])
* Các lệnh vô hiệu hóa/mask và ví dụ quản lý dịch vụ. ([Sách điện tử LFCS][4], [oracle-hub][5])
* Các chế độ SELinux và thay đổi vĩnh viễn thông qua `/etc/selinux/config`; ưu tiên permissive thay vì vô hiệu hóa. ([Tài liệu Red Hat][6])
* Kiểm tra trạng thái SELinux (`sestatus`, `getenforce`). ([LinuxConfig][7])
* Tại sao để SELinux bị vô hiệu hóa là rủi ro (đặc biệt với container). ([Cổng thông tin khách hàng Red Hat][8])
* Các hàm ý bảo mật của việc vô hiệu hóa SELinux. ([Information Security Stack Exchange][9])

---

[1]: https://docs.oracle.com/en/operating-systems/oracle-linux/8/relnotes8.0/ol8.0-ComparingYumVersion3WithDNF.html?utm_source=chatgpt.com "So sánh Yum Phiên bản 3 Với DNF - Trung tâm Trợ giúp Oracle"
[2]: https://phoenixnap.com/kb/dnf-vs-yum?utm_source=chatgpt.com "DNF vs. YUM: Tìm hiểu sự khác biệt {So sánh Song song}"
[3]: https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/using-and-configuring-firewalld_configuring-and-managing-networking?utm_source=chatgpt.com "Sử dụng và cấu hình firewalld - Red Hat"
[4]: https://www.tecmint.com/manage-firewalld-and-ufw-on-linux/?utm_source=chatgpt.com "Cách Quản lý Firewalld và UFW cho Bảo mật Linux - Tecmint"
[5]: https://community.oracle.com/customerconnect/discussion/719456/how-to-disable-firewalld-service-permanently?utm_source=chatgpt.com "Cách Vô hiệu hóa Dịch vụ Firewalld Vĩnh viễn - Cộng đồng Oracle"
[6]: https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/using_selinux/changing-selinux-states-and-modes_using-selinux?utm_source=chatgpt.com "Thay đổi trạng thái và chế độ SELinux - Red Hat"
[7]: https://linuxconfig.org/how-to-check-selinux-operational-mode?utm_source=chatgpt.com "Kiểm tra Chế độ Hoạt động SELinux trên Hệ thống Linux - LinuxConfig.org"
[8]: https://access.redhat.com/articles/6144032?utm_source=chatgpt.com "Tại sao việc vô hiệu hóa SELinux khi chạy container là ý tưởng tồi"
[9]: https://security.stackexchange.com/questions/104090/what-are-the-security-implications-of-disabling-selinux?utm_source=chatgpt.com "Các hàm ý bảo mật của việc vô hiệu hóa SELinux?"