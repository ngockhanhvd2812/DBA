### **Hướng Dẫn Toàn Diện Cài Đặt Kaspersky Endpoint Security for Linux (KESL) và Network Agent trên Máy Chủ Linux Mới**

#### **Giới thiệu**

Hướng dẫn này sẽ chỉ cho bạn cách cài đặt và cấu hình hai thành phần quan trọng của Kaspersky trên một máy chủ Linux hoàn toàn mới:

1.  **Kaspersky Endpoint Security for Linux (KESL):** Đây là chương trình chống virus chính, cung cấp khả-năng bảo vệ máy chủ của bạn khỏi phần mềm độc hại, mã độc tống tiền và các mối đe dọa khác.
2.  **Kaspersky Network Agent (KLNAgent):** Đây là "cầu nối" giao tiếp giữa máy chủ Linux của bạn và máy chủ quản trị trung tâm **Kaspersky Security Center (KSC)**. Nhờ có Network Agent, quản trị viên có thể quản lý, đặt chính sách, cập nhật và theo dõi máy chủ của bạn từ xa.

#### **Yêu cầu chuẩn bị**

*   Quyền truy cập `root` vào máy chủ Linux.
*   Các file cài đặt: `kesl-11.2.0-4528.x86_64.rpm` và `klnagent64-11.0.0-38.x86_64.rpm`.
*   Kết nối mạng từ máy chủ Linux đến máy chủ quản trị KSC.

---

### **Bước 1: Chuẩn bị Mạng - Mở Cổng Tường Lửa**

**Mục tiêu:** Đảm bảo máy chủ Linux (Client) có thể "nói chuyện" được với máy chủ quản trị Kaspersky Security Center (KSC).

**Lý thuyết:** Giao tiếp giữa KLNAgent và KSC diễn ra qua các cổng mạng cụ thể. Nếu tường lửa (firewall) chặn các cổng này, KLNAgent sẽ không thể kết nối, đồng bộ, nhận chính sách hay gửi trạng thái về KSC.

**Hành động:**
Bạn cần cấu hình tường lửa (trên máy chủ Linux hoặc trên thiết bị mạng) để cho phép kết nối **đi ra (OUTGOING)** từ máy chủ Linux của bạn đến địa chỉ IP máy chủ KSC mới là `IP của Kaspersky Security Center (KSC) Administration Server ` qua các cổng sau:

*   **Cổng `13000`:** Dùng cho kết nối được mã hóa SSL (an toàn). Đây là cổng ưu tiên.
*   **Cổng `14000`:** Dùng cho kết nối không mã hóa.

**Sơ đồ Mermaid:**

*   **Luồng giao tiếp giữa Client và KSC:**

```mermaid
graph LR
    subgraph sg1["🖥️ Máy Chủ Linux Client"]
        A["KLNAgent"]
    end
    
    subgraph sg2["🌐 Mạng Doanh Nghiệp"]
        B{"Firewall"}
    end
    
    subgraph sg3["⚙️ Máy Chủ Quản Trị KSC"]
        C["Kaspersky Security Center<br/>Administration Server"]
    end
    
    A -->|"Gửi yêu cầu kết nối"| B
    B -->|"Kiểm tra luật<br/>Cho phép cổng 13000, 14000"| C
    
    classDef clientStyle fill:#e1f5fe,stroke:#01579b,stroke-width:2px,color:#000
    classDef networkStyle fill:#fff3e0,stroke:#e65100,stroke-width:2px,color:#000
    classDef serverStyle fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px,color:#000
    classDef firewallStyle fill:#ffebee,stroke:#c62828,stroke-width:3px,color:#000
    
    class A clientStyle
    class B firewallStyle
    class C serverStyle


```

```mermaid
graph TD
    ClientLinux -- Gửi dữ liệu qua cổng 13000 (SSL) --> KSCServer
    ClientLinux -- Gửi dữ liệu qua cổng 14000 (Non-SSL) --> KSCServer
```

---

### **Bước 2: Cài Đặt và Cấu Hình Kaspersky Network Agent (KLNAgent)**

**Mục tiêu:** Cài đặt "cầu nối" KLNAgent lên máy chủ Linux.

**Lý thuyết:** Network Agent là thành phần bắt buộc phải có để KSC có thể quản lý máy chủ này. Việc cài đặt bao gồm hai phần: cài đặt gói `.rpm` và chạy script cấu hình `postinstall.pl` để khai báo địa chỉ KSC ban đầu.

**Hành động:**

1.  **Cài đặt gói RPM:**
    Mở Terminal và chạy lệnh sau (đảm bảo bạn đang ở trong thư mục chứa file `.rpm`):
    ```bash
    rpm -Uvh klnagent64-11.0.0-38.x86_64.rpm
    ```
    *   **Ghi chú:** Nếu bạn nhận được thông báo "package ... is already installed", điều đó là bình thường và bạn có thể bỏ qua để tiếp tục với bước tiếp theo.

2.  **Chạy script cấu hình:**
    ```bash
    /opt/kaspersky/klnagent64/lib/bin/setup/postinstall.pl
    ```
3.  **Trả lời các câu hỏi cấu hình:**
    *   `Please enter Administration Server DNS-name or static IP-address`: Nhập IP của Kaspersky Security Center (KSC) Administration Server mà Network Agent sẽ cố gắng kết nối **ban đầu**
    *   `Please enter Administration Server port number [14000]:`: Nhấn `Enter` (chấp nhận mặc định).
    *   `Please enter Administration Server ssl port number [13000]:`: Nhấn `Enter` (chấp nhận mặc định).
    *   `Please enter 'Y' to confirm that you want to use SSL encryption... [Y]:`: Nhấn `Enter` (chấp nhận mặc định).
    *   `Please choose connection gateway mode: [1]:`: Nhấn `Enter` hoặc gõ `1` rồi nhấn `Enter`.

**Sơ đồ Mermaid:**

*   **Quy trình cài đặt KLNAgent:**

```mermaid
sequenceDiagram
    participant User
    participant Terminal
    User->>Terminal: rpm -Uvh klnagent64...rpm
    Terminal-->>User: Cài đặt hoàn tất
    User->>Terminal: /opt/kaspersky/klnagent64/lib/bin/setup/postinstall.pl
    Terminal-->>User: Vui lòng nhập địa chỉ KSC:
    User->>Terminal: Nhập IP của Kaspersky Security Center (KSC) Administration Server 
    Terminal-->>User: Vui lòng nhập cổng [14000]:
    User->>Terminal: (Nhấn Enter)
    Terminal-->>User: Vui lòng nhập cổng SSL [13000]:
    User->>Terminal: (Nhấn Enter)
    Terminal-->>User: Sử dụng SSL? [Y]:
    User->>Terminal: (Nhấn Enter)
    Terminal-->>User: Cấu hình hoàn tất!
```

---

### **Bước 3: Cập Nhật Địa Chỉ IP Máy Chủ KSC và Kiểm Tra**

**Mục tiêu:** Chuyển hướng KLNAgent đến địa chỉ IP mới của máy chủ KSC và xác nhận kết nối.

**Lý thuyết:** Sau khi cài đặt KLNAgent với địa chỉ IP cũ, chúng ta sử dụng công cụ `klmover` để "di chuyển" (move) kết nối của agent sang địa chỉ IP mới. Lệnh `klnagchk` là công cụ chẩn đoán để kiểm tra trạng thái kết nối của agent.

**Hành động:**

1.  **Chuyển hướng đến IP mới:**
    ```bash
    /opt/kaspersky/klnagent64/bin/klmover -address IP của Kaspersky Security Center (KSC) Administration Server 
    ```
2.  **Kiểm tra trạng thái kết nối:**
    ```bash
    /opt/kaspersky/klnagent64/bin/klnagchk
    ```
    *   Trong kết quả hiển thị, hãy tìm dòng `Server address` và đảm bảo nó hiển thị đúng địa chỉ IP mới: `IP của Kaspersky Security Center (KSC) Administration Server `.


---

### **Bước 4: Cài Đặt và Cấu Hình Kaspersky Endpoint Security (KESL)**

**Mục tiêu:** Cài đặt chương trình chống virus chính và thực hiện cấu hình ban đầu.

**Lý thuyết:** Tương tự như KLNAgent, việc cài đặt KESL bao gồm cài đặt gói `.rpm` và chạy script cấu hình `kesl-setup.pl`. Trong quá trình này, bạn sẽ được hỏi về các thỏa thuận pháp lý và các cài đặt cơ bản.

**Hành động:**

1.  **Cài đặt gói RPM:**
    ```bash
    rpm -Uvh kesl-11.2.0-4528.x86_64.rpm
    ```
2.  **Chạy script cấu hình:**
    ```bash
    /opt/kaspersky/kesl/bin/kesl-setup.pl
    ```
3.  **Trả lời các câu hỏi cấu hình:**
    *   `[en_US.UTF-8]:`: Nhấn `Enter` (chấp nhận ngôn ngữ mặc định).
    *   `Press ENTER to display the EULA...`: Nhấn `Enter`, đọc qua (nhấn `Space` để cuộn), sau đó nhấn `Q` để thoát.
    *   `I confirm that I have fully read... EULA [y/n]:`: Gõ `y` và nhấn `Enter`.
    *   `I am aware and agree that my data will be handled... Privacy Policy [y/n]:`: Gõ `y` và nhấn `Enter`.
    *   `I confirm that I have fully read... Kaspersky Security Network Statement [y/n]:`: Gõ `n` và nhấn `Enter`.
        *   **Giải thích về KSN:** Kaspersky Security Network (KSN) là một dịch vụ đám mây giúp phát hiện các mối đe dọa mới nhanh hơn. Tuy nhiên, nhiều tổ chức chọn **'n' (không tham gia)** vì lý do chính sách bảo mật, không cho phép dữ liệu (dù đã được ẩn danh) gửi ra ngoài mạng nội bộ.
    *   `Specify user to grant the 'admin' role to (leave empty to skip):`: Nhấn `Enter`.
    *   `Specify the update source... [KLServers]:`: Nhập `Nhập IP của Kaspersky Security Center (KSC) Administration Server ` và nhấn `Enter`.
    *   `...enter 'no' [n]:`: Gõ `n` và nhấn `Enter`.
    *   `Do you want to download the latest databases now? [y]:`: Gõ `y` và nhấn `Enter`.
    *   `Do you want to set other update settings? [y]`: Gõ `n` và nhấn `Enter`.
    *   `Do you want to enable scheduled updates? [y]:`: Gõ `n` và nhấn `Enter`.
    *   `Enter an empty string to add the built-in trial key:`: Nhấn `Enter`.

**Sơ đồ Mermaid:**

*   **Luồng cấu hình KESL:**

```mermaid
flowchart TD
    A[Bắt đầu kesl-setup.pl] --> B{Chấp nhận EULA?};
    B -- Yes --> C{Chấp nhận Privacy Policy?};
    C -- Yes --> D{Tham gia KSN?};
    D -- No --> E[Thiết lập nguồn cập nhật];
    E --> F[Kích hoạt bản dùng thử];
    F --> G[Hoàn tất];
```

---

### **Bước 5: Tạm Dừng và Vô Hiệu Hóa Dịch Vụ**

**Mục tiêu:** Tạm thời dừng các dịch vụ Kaspersky để đảm bảo việc cài đặt các gói phụ thuộc không bị xung đột.

**Lý thuyết:** Trước khi cài đặt các thư viện hệ thống (như các module Perl), việc dừng các ứng dụng đang chạy sử dụng chúng là một thói quen tốt để tránh các lỗi không mong muốn. Chúng ta cũng vô hiệu hóa chúng để chúng không tự khởi động lại nếu máy chủ cần reboot giữa chừng.

**Hành động:** (Chọn bộ lệnh phù hợp với hệ điều hành của bạn)

*   **Đối với hệ điều hành hiện đại (CentOS/RHEL 7+, Ubuntu 15+):**
    ```bash
    systemctl stop kesl-supervisor
    systemctl stop klnagent64
    systemctl disable kesl-supervisor
    systemctl disable klnagent64
    ```
*   **Đối với hệ điều hành cũ hơn (CentOS/RHEL 6):**
    ```bash
    service kesl-supervisor stop
    service klnagent64 stop
    chkconfig kesl-supervisor off
    chkconfig klnagent64 off
    ```

**Sơ đồ Mermaid:**

*   **Trạng thái dịch vụ:**

```mermaid
graph TD
    A[Trạng thái: Đang chạy & Tự khởi động]
    B(Chạy lệnh stop & disable)
    C[Trạng thái: Đã dừng & Không tự khởi động]
    A --> B --> C
```

---

### **Bước 6: Cài Đặt Các Gói Phụ Thuộc của Perl**

**Mục tiêu:** Cài đặt tất cả các thư viện Perl cần thiết để KESL và KLNAgent hoạt động đầy đủ chức năng.

**Lý thuyết:** Các script của Kaspersky được viết bằng Perl và yêu cầu nhiều module cụ thể. 

**Hành động:**

Chạy một lệnh duy nhất để cài đặt tất cả các gói Perl cần thiết (đã bỏ đi phần phiên bản cụ thể):

```bash
yum install -y perl-podlators perl-Encode perl-Storable perl-Socket 'perl-Scalar-List-Utils' perl-threads-shared perl perl-HTTP-Tiny perl-Pod-Perldoc perl-Text-ParseWords perl-Pod-Usage perl-macros perl-Exporter perl-Time-Local perl-Carp perl-PathTools perl-Pod-Simple perl-File-Path perl-threads perl-Getopt-Long perl-parent perl-Pod-Escapes perl-libs perl-constant perl-Time-HiRes perl-File-Temp perl-Filter
```
*   **Ghi chú:** `perl-Scalar-List-Utils` được đặt trong dấu nháy đơn vì tên của nó chứa các ký tự đặc biệt có thể bị shell hiểu nhầm.

**Sơ đồ Mermaid:**

*   **Logic xử lý lỗi cài đặt gói:**

```mermaid
graph TD
    A["🚀 Bắt đầu cài đặt phụ thuộc"] --> B{"📋 Thử cài đặt với phiên bản<br/>cụ thể từ file .txt"}
    
    B -->|"✅ Thành công"| F["🎉 Hoàn tất"]
    B -->|"❌ Lỗi 'No match for argument'"| C["🔧 Xóa thông tin phiên bản<br/>khỏi tên gói"]
    
    C --> D["🔄 Chạy lại lệnh 'yum install'<br/>với tên gói chung"]
    
    D -->|"✅ Thành công"| F
    D -->|"❌ Vẫn lỗi"| E["🔍 Kiểm tra lại kết nối mạng /<br/>cấu hình kho lưu trữ 'yum'"]
    
    E --> G{"🛠️ Đã khắc phục được<br/>vấn đề kết nối?"}
    G -->|"✅ Có"| D
    G -->|"❌ Không"| H["📞 Liên hệ quản trị viên<br/>hệ thống"]
    
    classDef startStyle fill:#e3f2fd,stroke:#1976d2,stroke-width:3px,color:#000
    classDef processStyle fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    classDef decisionStyle fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    classDef successStyle fill:#e8f5e8,stroke:#388e3c,stroke-width:3px,color:#000
    classDef errorStyle fill:#ffebee,stroke:#d32f2f,stroke-width:2px,color:#000
    classDef endStyle fill:#fce4ec,stroke:#c2185b,stroke-width:3px,color:#000
    
    class A startStyle
    class C,D processStyle
    class B,G decisionStyle
    class F successStyle
    class E errorStyle
    class H endStyle

```

---

### **Bước 7: Khởi Động và Kích Hoạt Lại Dịch Vụ**

**Mục tiêu:** Khởi động lại các dịch vụ Kaspersky và thiết lập chúng tự động chạy khi hệ thống khởi động.

**Lý thuyết:** Sau khi đã cài đặt xong tất cả các thành phần và phụ thuộc, chúng ta cần khởi động lại các dịch vụ để chúng hoạt động bình thường. Lệnh `enable` đảm bảo rằng ngay cả khi bạn khởi động lại máy chủ, Kaspersky sẽ luôn được bật để bảo vệ.

**Hành động:** (Chọn bộ lệnh phù hợp với hệ điều hành của bạn)

*   **Đối với hệ điều hành hiện đại (CentOS/RHEL 7+, Ubuntu 15+):**
    ```bash
    systemctl start kesl-supervisor
    systemctl start klnagent64
    systemctl enable kesl-supervisor
    systemctl enable klnagent64
    ```
*   **Đối với hệ điều hành cũ hơn (CentOS/RHEL 6):**
    ```bash
    service kesl-supervisor start
    service klnagent64 start
    chkconfig kesl-supervisor on
    chkconfig klnagent64 on
    ```

**Sơ đồ Mermaid:**

*   **Kích hoạt lại dịch vụ:**

```mermaid
graph TD
    A[Trạng thái: Đã dừng & Không tự khởi động]
    B(Chạy lệnh start & enable)
    C[Trạng thái: Đang chạy & Tự khởi động]
    A --> B --> C
```

---

### **Bước 8: Kiểm Tra Cuối Cùng và Các Bước Tiếp Theo**

**Mục tiêu:** Xác nhận lần cuối rằng mọi thứ hoạt động chính xác.

**Lý thuyết:** Sau khi hoàn tất, bạn nên kiểm tra lại kết nối của KLNAgent và xác nhận sự hiện diện của máy chủ trên giao diện KSC.

**Hành động:**

1.  **Chạy lại `klnagchk` trên máy chủ Linux:**
    ```bash
    /opt/kaspersky/klnagent64/bin/klnagchk
    ```
    *   Đảm bảo rằng nó báo cáo kết nối thành công đến `IP của Kaspersky Security Center (KSC) Administration Server `.

2.  **Đăng nhập vào Kaspersky Security Center (KSC):**
    *   Tìm máy chủ Linux của bạn trong danh sách các thiết bị được quản lý.
    *   Kiểm tra trạng thái của nó. Ban đầu có thể là màu vàng hoặc xám, sau vài phút đồng bộ, nó nên chuyển sang màu xanh lá cây, cho thấy trạng thái "OK".

**Sơ đồ Mermaid:**

*   **Quy trình xác thực cuối cùng:**

```mermaid
sequenceDiagram
    participant Admin
    participant ClientLinux
    participant KSCServer
    Admin->>ClientLinux: Chạy lệnh klnagchk
    ClientLinux->>KSCServer: Gửi gói tin đồng bộ (Sync)
    KSCServer-->>ClientLinux: Phản hồi thành công
    ClientLinux-->>Admin: Hiển thị kết quả: Kết nối OK
    Admin->>KSCServer: Mở giao diện Web/Console KSC
    KSCServer-->>Admin: Hiển thị trạng thái ClientLinux là "OK"
```
