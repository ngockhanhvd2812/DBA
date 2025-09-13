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
    G --> H[Xác minh:<br/>systemctl status firewalld,<br/>sestatus]:::gray
    H --> I[Máy chủ sẵn sàng cài đặt DB]:::success
    
    subgraph WARNINGS["⚠️ CẢNH BÁO QUAN TRỌNG"]
        W1[CẢNH BÁO: Loại trừ MongoDB<br/>khỏi yum update<br/>--exclude=mongodb*,mongo*]:::warning
    end
    
    B -.->|Luôn loại trừ<br/>MongoDB| W1
    W1 --> B

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
```

---

## 1) Cập nhật hệ thống (`yum update -y`) 🔄💡

**Lý do:** Trên OL8, `yum` là giao diện dựa trên DNF. Chạy cập nhật đầy đủ đảm bảo bạn có kernel, glibc, OpenSSL và trình điều khiển thiết bị hiện tại trước khi cài đặt cơ sở dữ liệu—giảm thiểu bất ngờ sau khi cài đặt và tránh các vấn đề không tương thích ABI. Nếu kernel mới được kéo về, hãy khởi động lại một lần trước khi tiếp tục. ([Tài liệu Oracle][1], [phoenixNAP | Global IT Services][2])

Trong một số trường hợp, bạn có thể muốn loại trừ một số gói khỏi việc cập nhật để duy trì tính ổn định của hệ thống hoặc tránh các vấn đề tương thích với phần mềm cơ sở dữ liệu của bạn. Đối với các cài đặt MongoDB, bạn nên loại trừ các gói MongoDB khỏi việc cập nhật hệ thống để duy trì tính nhất quán về phiên bản. Bạn có thể sử dụng tùy chọn `--exclude` với yum để ngăn chặn các gói cụ thể khỏi việc cập nhật.

⚠️ **CẢNH BÁO QUAN TRỌNG**: Việc loại trừ các gói MongoDB khỏi lệnh `yum update` là **BẮT BUỘC** và **CỰC KỲ QUAN TRỌNG**. Nếu không thực hiện đúng bước này, có thể gây ra **hậu quả nghiêm trọng** đến hệ thống MongoDB đang chạy, bao gồm nhưng không giới hạn ở: mất dữ liệu, hỏng cấu hình, không tương thích phiên bản, và thậm chí làm cho toàn bộ cụm MongoDB không hoạt động. Hãy chắc chắn rằng bạn luôn sử dụng tùy chọn `--exclude=mongodb*,mongo*` khi chạy lệnh `yum update` trên hệ thống đã cài đặt MongoDB.

**Sơ đồ minh họa cảnh báo quan trọng:**

```mermaid
flowchart TD
    A[Bắt đầu cập nhật hệ thống] --> B[yum update -y]
    B --> C{Có cài MongoDB?}
    C -->|Có| D[CẢNH BÁO: Phải loại trừ MongoDB]
    D --> E[Sử dụng: yum update -y --exclude=mongodb*,mongo*]
    C -->|Không| E
    E --> F[Tiếp tục quy trình]
    
    style D fill:#ef4444,color:#fff,stroke:#7f1d1d,stroke-width:2px
    style E fill:#3b82f6,color:#fff,stroke:#1e40af,stroke-width:2px
```

**Lệnh:**

```bash
sudo yum update -y   # 📦 Cập nhật tất cả các gói
# Nếu kernel hoặc thư viện quan trọng được cập nhật, khởi động lại một lần:
sudo reboot          # 🔁 Khởi động lại nếu cần

# Để loại trừ các gói MongoDB khỏi cập nhật:
sudo yum update -y --exclude=mongodb*,mongo*   # 🚫 Loại trừ các gói MongoDB
```

**Mini-flow:**

```mermaid
flowchart LR
    U[Chạy yum update -y]:::blue --> W[CẢNH BÁO:<br/>Loại trừ MongoDB<br/>--exclude=mongodb*,mongo*]:::warning
    W --> K{Kernel được cập nhật?}:::amber
    K -->|Có| R[Khởi động lại ngay]:::purple
    K -->|Không| N[Tiếp tục]:::green

    classDef blue fill:#60a5fa,stroke:#1e3a8a,color:#fff;
    classDef amber fill:#fbbf24,stroke:#92400e,color:#111;
    classDef purple fill:#a78bfa,stroke:#4c1d95,color:#fff;
    classDef green fill:#4ade80,stroke:#065f46,color:#063;
    classDef warning fill:#ef4444,color:#fff,stroke:#7f1d1d,stroke-width:2px;
```

---

## 2) Dừng và vô hiệu hóa tường lửa của hệ điều hành (`firewalld`) 🔥🚫

**Lý thuyết:** `firewalld` là dịch vụ tường lửa cấp cao chạy trên nftables. Nó tổ chức các quy tắc thành **zones** và **services** và được bật theo mặc định trên RHEL/OL8. Nhiều nhóm DB tạm thời vô hiệu hóa nó trong quá trình khởi tạo ban đầu, sau đó bật lại với các ngoại lệ dịch vụ/cổng phù hợp. Nếu bạn phải giữ nó tắt trong quá trình cài đặt, *ít nhất* hãy ghi lại các biện pháp kiểm soát bù đắp (VPC/nhóm bảo mật bị hạn chế, cách ly máy chủ). ([Tài liệu Red Hat][3])

**Các lệnh (ít xâm lấn nhất):**

```bash
sudo systemctl stop firewalld     # ⏹️ Dừng dịch vụ
sudo systemctl disable firewalld  # 📴 Vô hiệu hóa khi khởi động
# Tùy chọn chặn cứng để không gì có thể tự khởi động nó:
sudo systemctl mask firewalld     # 🔒 Ngăn chặn khởi động vô tình
```

*(Masking ngăn các đơn vị khác khởi động nó vô tình; bỏ mask sau này để khôi phục.)* ([Sách điện tử LFCS][4], [oracle-hub][5])

**Mini-flow:**

```mermaid
flowchart TD
    A2[Kiểm tra firewalld]:::gray --> B2{Hoạt động?}:::amber
    B2 -->|Có| C2[systemctl stop firewalld]:::red
    C2 --> D2[systemctl disable firewalld]:::orange
    D2 --> E2[Tùy chọn: systemctl mask firewalld]:::purple
    B2 -->|Không| F2[Tiếp tục]:::green
    E2 --> F2

    classDef gray fill:#d1d5db,stroke:#374151,color:#111;
    classDef amber fill:#f59e0b,stroke:#92400e,color:#111;
    classDef red fill:#ef4444,stroke:#7f1d1d,color:#fff;
    classDef orange fill:#fb923c,stroke:#c2410c,color:#111;
    classDef purple fill:#8b5cf6,stroke:#4c1d95,color:#fff;
    classDef green fill:#22c55e,stroke:#065f46,color:#0a0;
```

> ⚠️ **Ghi chú bảo mật:** Ưu tiên mở chỉ các cổng của DB trong zone đúng thay vì tắt hoàn toàn tường lửa, đặc biệt trên các máy chủ đa người dùng hoặc có thể truy cập từ Internet. ([Tài liệu Red Hat][3])

---

## 3) Vô hiệu hóa SELinux (chỉ trong thời gian cài đặt) 🛡️⚙️

**Lý thuyết:** SELinux là hệ thống kiểm soát truy cập bắt buộc (MAC) giới hạn các tiến trình thông qua nhãn và chính sách. Các chế độ là **enforcing**, **permissive**, và **disabled**. Đối với các bản build lệch khỏi "đường dẫn/cổng tiêu chuẩn," SELinux có thể chặn các thao tác cho đến khi điều chỉnh chính sách. Các nhà cung cấp đôi khi yêu cầu bạn vô hiệu hóa nó trong quá trình cài đặt; giải pháp an toàn hơn là **permissive** (ghi log từ chối, không chặn). Red Hat thường khuyến nghị permissive thay vì vô hiệu hóa hoàn toàn; nếu bạn *có* vô hiệu hóa, cần khởi động lại. ([Tài liệu Red Hat][6])

**Các lệnh (như bạn đã yêu cầu):**

```bash
# Chỉnh sửa file cấu hình ✏️
sudo vi /etc/selinux/config
# Thay đổi dòng này:
#   SELINUX=enforcing
# thành:
#   SELINUX=disabled   # ❌ Vô hiệu hóa

# Áp dụng bằng cách khởi động lại:
sudo reboot           # 🔁 Áp dụng các thay đổi
```

**Các lựa chọn thay thế (an toàn hơn trong quá trình khắc phục sự cố):**

```bash
# Tạm thời (cho đến khi khởi động lại): chế độ permissive 🟡
sudo setenforce 0

# Xác minh chế độ hiện tại 🔍
getenforce
sestatus
```

*(Sử dụng `sestatus` / `getenforce` để xác nhận chế độ và cài đặt thời gian khởi động.)* ([Tài liệu Red Hat][6], [LinuxConfig][7])

**Mini-flow:**

```mermaid
flowchart TD
    S1[Mở /etc/selinux/config]:::blue --> S2{Giá trị hiện tại?}:::amber
    S2 -->|enforcing/permissive| S3[Đặt SELINUX=disabled]:::red
    S3 --> S4[Lưu file]:::gray
    S4 --> S5[Khởi động lại]:::purple
    S5 --> S6[Xác minh với 'sestatus']:::teal
    S2 -->|đã vô hiệu hóa| S6

    classDef blue fill:#3b82f6,stroke:#1e40af,color:#fff;
    classDef amber fill:#fbbf24,stroke:#92400e,color:#111;
    classDef red fill:#ef4444,stroke:#7f1d1d,color:#fff;
    classDef gray fill:#9ca3af,stroke:#111,color:#111;
    classDef purple fill:#a78bfa,stroke:#4c1d95,color:#fff;
    classDef teal fill:#14b8a6,stroke:#115e59,color:#052;
```

> ⚠️ **Ghi chú bảo mật:** Vô hiệu hóa SELinux loại bỏ một lớp cách ly quan trọng (ví dụ, khối lượng công việc container phụ thuộc vào nó để tách biệt tiến trình). Nếu bạn đang chạy container cùng với DB, **không** nên để SELinux bị vô hiệu hóa. Lên kế hoạch bật lại và tạo các ngoại lệ chính sách sau khi DB ổn định. ([Cổng thông tin khách hàng Red Hat][8])

---

## 4) Xác minh sau khi khởi động lại ✅🔍

**Tại sao:** Xác minh ngăn chặn các trạng thái "nửa áp dụng" (ví dụ, cấu hình SELinux đã thay đổi nhưng chưa khởi động lại).

**Các lệnh:**

```bash
# Tường lửa nên không hoạt động 🔥
systemctl status firewalld

# SELinux nên báo cáo là đã vô hiệu hóa 🛡️
sestatus
```

**Mini-flow:**

```mermaid
flowchart LR
    V1[systemctl status firewalld]:::gray --> V2{Không hoạt động?}:::amber
    V2 -->|Có| V3[sestatus]:::blue
    V2 -->|Không| V4[Lặp lại bước firewalld]:::red
    V3 --> V5{Đã vô hiệu hóa?}:::amber
    V5 -->|Có| V6[Sẵn sàng cài đặt DB]:::green
    V5 -->|Không| V7[Chỉnh sửa cấu hình + khởi động lại]:::orange

    classDef gray fill:#d1d5db,stroke:#111827,color:#111;
    classDef amber fill:#fde047,stroke:#92400e,color:#111;
    classDef blue fill:#60a5fa,stroke:#1e3a8a,color:#fff;
    classDef red fill:#f87171,stroke:#7f1d1d,color:#fff;
    classDef green fill:#86efac,stroke:#065f46,color:#063;
    classDef orange fill:#fdba74,stroke:#c2410c,color:#111;
```

---

## 5) Khôi phục nhanh & các lựa chọn thay thế an toàn (sau khi cài đặt) ⏪🔐

* **Bật lại tường lửa** 🔥 và chỉ mở các cổng DB cần thiết (ví dụ, 1521 cho Oracle, 5432 cho PostgreSQL) sử dụng các dịch vụ/quy tắc `firewall-cmd`. ([Tài liệu Red Hat][3])
* **Bật lại SELinux** 🛡️ sang `enforcing` (hoặc ít nhất là `permissive`) và tạo các quyền cần thiết thông qua `audit2allow` thay vì để nó tắt. ([Tài liệu Red Hat][6], [Information Security Stack Exchange][9])

```bash
# Tường lửa
sudo systemctl unmask firewalld        # 🔓 Bỏ mask
sudo systemctl enable --now firewalld
# ví dụ: mở dịch vụ PostgreSQL vĩnh viễn
sudo firewall-cmd --add-service=postgresql --permanent  # 📡 Mở cổng
sudo firewall-cmd --reload

# SELinux (cấu hình + khởi động lại về enforcing)
sudo sed -i 's/^SELINUX=.*/SELINUX=enforcing/' /etc/selinux/config  # 🛡️ Khôi phục
sudo reboot
```

---

## Khối copy-paste 📋✨

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