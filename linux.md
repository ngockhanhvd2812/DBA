# **I. TỔNG QUAN VỀ LINUX**

## **1. Giới thiệu và Bắt đầu**

### **1.1. Giới thiệu và Bắt đầu**
```mermaid
graph TD
    subgraph "Giới thiệu và Bắt đầu"
        direction LR
        %% Chapters
        C1("Chương 1: Linux là gì?")
        C2("Chương 2: Lựa chọn & Cài đặt Distro")
        C3("Chương 3: Khởi động và làm quen")

        %% Chapter 1 Details
        C1 --> K1("Khái niệm Kernel (Nhân)")
        C1 --> K2("Khái niệm Distribution (Bản phân phối - Distro)")
        C1 --> K3("Triết lý: Mã nguồn mở & Miễn phí")
        K1 & K2 -- Kết hợp --> D1("<b>Kernel Linux</b> + <b>Bộ công cụ GNU</b> + <b>Phần mềm khác</b> = <b>Distro</b>")

        %% Chapter 2 Details
        C2 --> D_Choose("Lựa chọn Distro phổ biến")
            D_Choose --> D_Ubuntu("Ubuntu (Phổ biến, dễ dùng)")
            D_Choose --> D_Mint("Linux Mint (Giao diện thân thiện)")
            D_Choose --> D_Fedora("Fedora (Công nghệ mới)")

        C2 --> I_Method("Lựa chọn phương pháp cài đặt")
            I_Method --> I1("<b>Live USB</b><br/><i>Chạy thử không cần cài đặt</i>")
            I1 -.-> Note1("💡 Khuyến khích cho người mới")
            I_Method --> I2("<b>Dual Boot</b><br/><i>Cài song song với Windows</i>")
            I_Method --> I3("<b>Máy ảo (Virtual Machine)</b><br/><i>Cài Linux bên trong Windows/macOS</i>")

        %% Chapter 3 Details
        C3 --> P1("Quy trình cài đặt cơ bản") --> P2("Tạo User & Mật khẩu")
        P2 --> P3("Đăng nhập lần đầu") --> P4("Khám phá Giao diện Desktop")
        P3 -- "Mật khẩu không hiển thị khi gõ<br/>(Đây là tính năng bảo mật)" --> P2
    end

    %% Relationships
    C1 -- Nền tảng cho --> C2
    C2 -- Điều kiện cần để --> C3

    %% Styles
    style C1 fill:#ffebcc,stroke:#cc6600,stroke-width:2px
    style C2 fill:#e6f2ff,stroke:#3366cc,stroke-width:2px
    style C3 fill:#e6ffe6,stroke:#33cc33,stroke-width:2px
    style K1 fill:#fff2cc,stroke:#cc9900
    style K2 fill:#fff2cc,stroke:#cc9900
    style K3 fill:#fff2cc,stroke:#cc9900
    style D1 fill:#ffffcc,stroke:#cccc00,stroke-dasharray: 5 5
    style D_Ubuntu fill:#f2dede,stroke:#a94442
    style D_Mint fill:#dff0d8,stroke:#3c763d
    style D_Fedora fill:#d9edf7,stroke:#31708f
    style I1 fill:#fcf8e3,stroke:#8a6d3b
    style I2 fill:#fcf8e3,stroke:#8a6d3b
    style I3 fill:#fcf8e3,stroke:#8a6d3b
    style P1 fill:#f5f5f5,stroke:#999
    style P2 fill:#f5f5f5,stroke:#999
    style P3 fill:#f5f5f5,stroke:#999
    style P4 fill:#f5f5f5,stroke:#999
``` 

### **1.2. Phân rã hệ điều hành Linux**

```mermaid
graph TD
    subgraph "Phân rã hệ điều hành Linux"
        
        A["Người dùng"] -- "Tương tác qua" --> B{Shell / Giao diện Đồ họa}
        style A fill:#ffe6e6,stroke:#333,stroke-width:2px
        style B fill:#99ccff,stroke:#333,stroke-width:2px

        subgraph "Không gian Người dùng (User Space)"
            B -- "Gửi yêu cầu tới" --> C["Các công cụ và ứng dụng"]
            style C fill:#fff2b3,stroke:#333,stroke-width:2px

            C -- "Ví dụ" --> D["Trình duyệt Web, Trình gõ văn bản, Lệnh (ls, cp)..."]
            style D fill:#ffffcc,stroke:#333,stroke-width:1.5px

            D -- "Sử dụng các thư viện hệ thống (GNU Core Utilities)" --> E["System Libraries (glibc)"]
            style E fill:#ccffcc,stroke:#333,stroke-width:2px
        end

        E -- "Giao tiếp qua System Calls" --> F{Kernel Linux}

        subgraph "Không gian Nhân (Kernel Space) - TRÁI TIM HỆ THỐNG"
            F -- "Quản lý toàn bộ phần cứng" --> G["Phần cứng Máy tính"]
            style F fill:#ff99ff,stroke:#333,stroke-width:4px

            F -- "Chịu trách nhiệm" --> F1["Quản lý Tiến trình (CPU)"]
            F -- "Chịu trách nhiệm" --> F2["Quản lý Bộ nhớ (RAM)"]
            F -- "Chịu trách nhiệm" --> F3["Quản lý Hệ thống File (Ổ đĩa)"]
            F -- "Chịu trách nhiệm" --> F4["Quản lý Thiết bị (Chuột, Bàn phím...)"]

            style F1 fill:#ffcccc,stroke:#333
            style F2 fill:#ffcccc,stroke:#333
            style F3 fill:#ffcccc,stroke:#333
            style F4 fill:#ffcccc,stroke:#333
        end

        G --> H["CPU, RAM, Ổ cứng, Card mạng, ..."]
        style G fill:#d9d9d9,stroke:#333
        style H fill:#f2f2f2,stroke:#333

        %% Note
        Note1("<b>Tổng kết:</b><br/>Một <b>Bản phân phối (Distro)</b> như Ubuntu<br/>sẽ đóng gói tất cả các thành phần này<br/>(Kernel, Shell, Công cụ, Ứng dụng)<br/>thành một hệ điều hành hoàn chỉnh.")
        style Note1 fill:#ccf2ff,stroke:#333,stroke-width:1.5px
    end
``` 

---

## **2. Giao diện Dòng lệnh (CLI)**
- **Đây là phần quan trọng nhất, tập trung vào các lệnh cơ bản để tương tác với hệ thống.**
```mermaid
graph TD
    subgraph "Giao diện Dòng lệnh (CLI)"
        %% Chapters
        C4("Chương 4: Hello, Shell!")
        C5("Chương 5: Điều hướng Hệ thống file")
        C6("Chương 6: Thao tác với File & Thư mục")
        C7("Chương 7: Xem và Chỉnh sửa File")

        C4 --> S1("Terminal là gì?")
        C4 --> S2("Shell là gì? (vd: Bash)")
        C4 --> S3("Cấu trúc câu lệnh:<br/><b>command [options] [arguments]</b>")

        C5 -- "Các lệnh cốt lõi" --> C5_Nav
        subgraph C5_Nav [Điều hướng]
            L1("<b>pwd</b><br/><i>Tôi đang ở đâu?</i>")
            L2("<b>ls</b><br/><i>Có gì trong thư mục này?</i>")
                L2 -- "Option: xem chi tiết" --> L2_opt1("ls -l")
                L2 -- "Option: xem file ẩn" --> L2_opt2("ls -a")
            L3("<b>cd [đường_dẫn]</b><br/><i>Di chuyển tới...</i>")
                L3 -- "Ví dụ: về thư mục nhà" --> L3_ex1("cd ~")
                L3 -- "Ví dụ: quay lại thư mục trước" --> L3_ex2("cd -")
                L3 -- "Ví dụ: lên 1 cấp" --> L3_ex3("cd ..")
        end

        C6 -- "Các lệnh phổ biến" --> C6_Manage
        subgraph C6_Manage [Quản lý File & Thư mục]
            direction LR
            subgraph "Thư mục"
                M1("<b>mkdir [tên]</b><br/>Tạo thư mục")
                M2("<b>rmdir [tên]</b><br/>Xóa thư mục rỗng")
            end
            subgraph "File & Thư mục"
                M3("<b>touch [tên_file]</b><br/>Tạo file rỗng")
                M4("<b>cp [nguồn] [đích]</b><br/>Sao chép")
                M5("<b>mv [nguồn] [đích]</b><br/>Di chuyển / Đổi tên")
                M6("<b>rm [tên]</b><br/>Xóa file")
                M6 -- "❗️ Cẩn thận: Xóa vĩnh viễn" --> M6_warn("rm -r [thư_mục]<br/>Xóa thư mục và nội dung")
            end
        end

        C7 -- "Hai nhu cầu chính" --> C7_ViewEdit
        subgraph C7_ViewEdit [Xem & Sửa]
            V1("<b>Xem nội dung</b>")
                V1 --> V1a("<b>cat [file]</b><br/>Xem toàn bộ file")
                V1 --> V1b("<b>less [file]</b><br/>Xem từng trang (linh hoạt hơn)")
            V2("<b>Chỉnh sửa nội dung</b>")
                V2 --> V2a("<b>nano [file]</b><br/>Trình soạn thảo văn bản<br/>đơn giản, dễ dùng")
                V2a -- "Các phím tắt hiển thị ở dưới" --> V2a_note
                V2 --> V2b("<b>vim/vi [file]</b><br/>Trình soạn thảo mạnh mẽ<br/>nhưng cần thời gian học")
        end
    end

    %% Styles
    style C4 fill:#fde9d9,stroke:#d38c1f
    style C5 fill:#e1f5fe,stroke:#039be5
    style C6 fill:#f1f8e9,stroke:#689f38
    style C7 fill:#fff3e0,stroke:#fb8c00
    style L1 fill:#fffde7,stroke:#f9a825
    style L2 fill:#fffde7,stroke:#f9a825
    style L2_opt1 fill:#fff9c4,stroke:#fbc02d
    style L2_opt2 fill:#fff9c4,stroke:#fbc02d
    style L3 fill:#fffde7,stroke:#f9a825
    style L3_ex1 fill:#e8f5e9,stroke:#43a047
    style L3_ex2 fill:#e8f5e9,stroke:#43a047
    style L3_ex3 fill:#e8f5e9,stroke:#43a047
    style M1 fill:#e0f7fa,stroke:#006064
    style M2 fill:#e0f7fa,stroke:#006064
    style M3 fill:#fce4ec,stroke:#880e4f
    style M4 fill:#fce4ec,stroke:#880e4f
    style M5 fill:#fce4ec,stroke:#880e4f
    style M6 fill:#fce4ec,stroke:#880e4f
    style M6_warn fill:#ffebee,stroke:#c62828
    style V1 fill:#ede7f6,stroke:#512da8
    style V1a fill:#ede7f6,stroke:#512da8
    style V1b fill:#ede7f6,stroke:#512da8
    style V2 fill:#f3e5f5,stroke:#7b1fa2
    style V2a fill:#f3e5f5,stroke:#7b1fa2
    style V2b fill:#f3e5f5,stroke:#7b1fa2
```  
---

## **3. Quản trị Hệ thống Cơ bản**
- **Sơ đồ này mô tả các tác vụ quản trị thiết yếu như quản lý người dùng, phần mềm và tiến trình.**
```mermaid
flowchart TD
    subgraph "Quản trị Hệ thống Cơ bản"
        %% Chapters
        C8("Chương 8: Người dùng & Quyền hạn")
        C9("Chương 9: Quản lý Phần mềm")
        C10("Chương 10: Quản lý Tiến trình")

        %% Chapter 8 Details
        C8 --> U1("User thường vs. <b>root</b> (Superuser)")
        U1 --> U2("Lệnh <b>sudo</b><br/><i>Thực thi lệnh với quyền root</i>")
        C8 --> P1("Hiểu về Quyền hạn File")
        P1 --> P2("<b>r (read)</b> - w (write) - <b>x (execute)</b><br/>Đọc - Ghi - Thực thi")
        P1 --> P3("Phân quyền cho: <b>User - Group - Others</b>")
        P1 --> P4("Các lệnh thay đổi quyền")
            P4 --> P4a("<b>chmod [quyền] [file]</b><br/><i>(vd: chmod 755 script.sh)</i>")
            P4 --> P4b("<b>chown [user] [file]</b><br/>Thay đổi chủ sở hữu")

        %% Chapter 9 Details
        C9 --> PM1("Trình quản lý gói (Package Manager)")
        PM1 --> PM2{Hệ thống Distro}
            PM2 -- Debian/Ubuntu --> APT("<b>apt</b>")
            PM2 -- Fedora/CentOS --> DNF("<b>dnf / yum</b>")
            PM2 -- Arch --> PACMAN("<b>pacman</b>")

        APT --> APT_Flow
        subgraph APT_Flow [Quy trình với APT]
            direction LR
            A1("sudo apt update") --> A2("apt search")
            A2 --> A3("sudo apt install")
            A3 --> A4("sudo apt remove")
        end

        %% Chapter 10 Details
        C10 --> PR1("Tiến trình (Process) là gì?")
        PR1 -- "Là một chương trình đang chạy" --> PR2
        C10 --> PR3("Các lệnh giám sát")
            PR3 --> PR3a("ps aux")
            PR3 --> PR3b("top / htop")
        C10 --> PR4("Dừng một tiến trình")
            PR4 --> PR4a("kill [PID]")
            PR4a -- "Nếu không hiệu quả" --> PR4b("kill -9 [PID]")
    end

    %% Styles
    style C8 fill:#fff2e6,stroke:#d46b00
    style C9 fill:#e6f7ff,stroke:#3399ff
    style C10 fill:#e6ffe6,stroke:#33cc33
    style U1 fill:#fff5e6,stroke:#e67300
    style U2 fill:#fff5e6,stroke:#e67300
    style P1 fill:#fafafa,stroke:#999
    style P2 fill:#fafafa,stroke:#999
    style P3 fill:#fafafa,stroke:#999
    style P4 fill:#fafafa,stroke:#999
    style P4a fill:#f0f0f0,stroke:#666
    style P4b fill:#f0f0f0,stroke:#666
    style PM1 fill:#e6f7ff,stroke:#3399ff
    style APT fill:#d9edf7,stroke:#31708f
    style DNF fill:#f2dede,stroke:#a94442
    style PACMAN fill:#dff0d8,stroke:#3c763d
    style A1 fill:#fcf8e3,stroke:#8a6d3b
    style A2 fill:#fcf8e3,stroke:#8a6d3b
    style A3 fill:#fcf8e3,stroke:#8a6d3b
    style A4 fill:#fcf8e3,stroke:#8a6d3b
    style PR1 fill:#f9f9f9,stroke:#999
    style PR3a fill:#f9f9f9,stroke:#999
    style PR3b fill:#f9f9f9,stroke:#999
    style PR4a fill:#fff0f0,stroke:#c00
    style PR4b fill:#fff0f0,stroke:#c00

``` 
---
## **4. Mạng và các Công cụ Nâng cao** 
- **Giới thiệu về mạng, tự động hóa với script và giao diện đồ họa**
```mermaid 
graph TD
    subgraph "Phần 4: Mạng & Nâng cao"
        %% Chapters
        C11("Chương 11: Kiến thức Mạng cơ bản")
        C12("Chương 12: Giới thiệu Shell Scripting")
        C13("Chương 13: Giao diện Đồ họa (GUI)")

        %% Chapter 11 Details
        C11 --> N1("Các lệnh kiểm tra mạng")
        N1 --> N1a("ip address / ifconfig")
        N1 --> N1b("ping [địa_chỉ_ip / tên_miền]")

        C11 --> N2("SSH (Secure Shell)")
        N2 -- "Cú pháp" --> N2a("ssh [user]@[địa_chỉ_ip]")
        MyPC["Máy của bạn"] -- ssh --> Server["Máy chủ Linux"]

        %% Chapter 12 Details
        C12 --> S1("Shell Script là gì?")
        S1 --> S2("Cấu trúc script")
        S2 --> S2_File
        subgraph S2_File [my_script.sh]
            S2a("#!/bin/bash")
            S2b("echo 'Hello, World!'")
            S2c("date")
        end
        S2_File -- "Cấp quyền" --> S3("chmod +x my_script.sh")
        S3 -- "Chạy" --> S4("./my_script.sh")

        %% Chapter 13 Details
        C13 --> G1("GUI hoạt động thế nào?")
        G1 --> G2("Desktop Environment (DE)")
        G2 --> G3("Các DE phổ biến")
            G3 --> G3a("GNOME")
            G3 --> G3b("KDE Plasma")
            G3 --> G3c("XFCE")
        C13 --> G4("Các ứng dụng đồ họa thường dùng")
    end

    %% Styles
    style C11 fill:#e8f4f8,stroke:#0277bd
    style C12 fill:#f3e8ff,stroke:#6a1b9a
    style C13 fill:#fffde7,stroke:#f9a825
    style N1 fill:#e1f5fe,stroke:#0288d1
    style N1a fill:#e1f5fe,stroke:#0288d1
    style N1b fill:#e1f5fe,stroke:#0288d1
    style N2 fill:#e1f5fe,stroke:#0288d1
    style N2a fill:#e1f5fe,stroke:#0288d1
    style MyPC fill:#f9fbe7,stroke:#afb42b
    style Server fill:#f9fbe7,stroke:#afb42b
    style S1 fill:#fce4ec,stroke:#ad1457
    style S2 fill:#fce4ec,stroke:#ad1457
    style S2a fill:#fce4ec,stroke:#ad1457
    style S2b fill:#fce4ec,stroke:#ad1457
    style S2c fill:#fce4ec,stroke:#ad1457
    style S3 fill:#fbe9e7,stroke:#bf360c
    style S4 fill:#fbe9e7,stroke:#bf360c
    style G1 fill:#fff3e0,stroke:#e65100
    style G2 fill:#fff3e0,stroke:#e65100
    style G3a fill:#fff3e0,stroke:#e65100
    style G3b fill:#fff3e0,stroke:#e65100
    style G3c fill:#fff3e0,stroke:#e65100
    style G4 fill:#fff3e0,stroke:#e65100
``` 

## **5. Hệ thống file**

### **5.1. Khám phá Hệ thống File Linux**

```mermaid
flowchart TD
    subgraph "🌳 Khám phá Hệ thống File Linux"
        
        %% Filesystem Structure
        subgraph "📂 Cấu trúc Cây thư mục (Một vài thư mục quan trọng)"
            Root("📀 /")
            Root --> home("🏠 /home<br/><i>Thư mục nhà của người dùng</i>")
            Root --> bin_sbin("⚙️ /bin & /sbin<br/><i>Chứa các lệnh thiết yếu</i>")
            Root --> etc("📑 /etc<br/><i>Chứa file cấu hình hệ thống</i>")
            Root --> var("📝 /var<br/><i>Dữ liệu thay đổi thường xuyên (logs, web...)</i>")
            Root --> usr("📦 /usr<br/><i>Phần mềm và dữ liệu của người dùng</i>")
            home --> user_dir("👤 /home/ten_ban<br/><i>Đây là nơi bạn bắt đầu! (~)</i>")
        end

        %% Navigation Commands Workflow
        subgraph "🖥️ Quy trình Điều hướng trong Terminal"
            direction LR
            Start("📍 Bạn đang ở <b>/home/ten_ban</b>")
            Start -- "Kiểm tra vị trí hiện tại?" --> Q1["<b>pwd</b><br/><i>(print working directory)</i>"]
            Q1 -- "Output: /home/ten_ban" --> Start
            
            Start -- "Xem có gì bên trong?" --> Q2["<b>ls</b>"]
            Q2 -- "Xem chi tiết hơn (quyền, kích thước...)" --> Q2_opt1["<b>ls -lh</b>"]
            Q2_opt1 --> Start
            
            Start -- "Muốn đi vào thư mục 'Documents'" --> Q3["<b>cd Documents</b><br/><i>(Đường dẫn tương đối)</i>"]
            Q3 --> New_Location("📂 Bạn đang ở <b>/home/ten_ban/Documents</b>")
            
            New_Location -- "Muốn quay về thư mục cha" --> Q4["<b>cd ..</b>"]
            Q4 --> Start
            
            New_Location -- "Muốn quay về thư mục nhà từ bất cứ đâu" --> Q5["<b>cd ~</b> hoặc <b>cd</b>"]
            Q5 --> Start
            
            Start -- "Muốn đi thẳng tới thư mục cấu hình" --> Q6["<b>cd /etc</b><br/><i>(Đường dẫn tuyệt đối)</i>"]
            Q6 --> Etc_Location("📑 Bạn đang ở <b>/etc</b>")
        end
    end

    %% 🔥 Class definitions (màu nền kiểu vàng nhạt)
    classDef rootNode fill:#FFD700,stroke:#333,stroke-width:1px;
    classDef homeNode fill:#FFB347,stroke:#333;
    classDef binNode fill:#FF6961,stroke:#333;
    classDef etcNode fill:#77DD77,stroke:#333;
    classDef varNode fill:#84DFFF,stroke:#333;
    classDef usrNode fill:#CBA6F7,stroke:#333;
    classDef userNode fill:#FF85C1,stroke:#333;

    classDef start fill:#FFF3B0,stroke:#333;
    classDef cmd1 fill:#FFAFCC,stroke:#333;
    classDef cmd2 fill:#FFD6A5,stroke:#333;
    classDef cmd2_opt fill:#FFDAC1,stroke:#333;
    classDef cmd3 fill:#CAFFBF,stroke:#333;
    classDef newLoc fill:#B5EAD7,stroke:#333;
    classDef cmd4 fill:#FF9AA2,stroke:#333;
    classDef cmd5 fill:#F1C6E7,stroke:#333;
    classDef cmd6 fill:#9BF6FF,stroke:#333;
    classDef etcLoc fill:#BDE0FE,stroke:#333;

    %% Assign classes
    class Root rootNode;
    class home homeNode;
    class bin_sbin binNode;
    class etc etcNode;
    class var varNode;
    class usr usrNode;
    class user_dir userNode;

    class Start start;
    class Q1 cmd1;
    class Q2 cmd2;
    class Q2_opt1 cmd2_opt;
    class Q3 cmd3;
    class New_Location newLoc;
    class Q4 cmd4;
    class Q5 cmd5;
    class Q6 cmd6;
    class Etc_Location etcLoc;

    %% 🎨 Styling mũi tên
    linkStyle default stroke:#FF5733,stroke-width:2px,fill:none;

``` 

### **5.2. Vòng đời File & Thư mục**
```mermaid
flowchart TD
    subgraph "Vòng đời File & Thư mục"
    
        A_Start(Bắt đầu)
        
        A_Start --> B_Create{"Bạn muốn tạo gì?"}
        B_Create -- "File rỗng" --> C1["touch report.txt"]
        B_Create -- "Thư mục" --> C2["mkdir projects"]
        
        C1 --> D_State1("File report.txt tồn tại")
        C2 --> D_State2("Thư mục projects tồn tại")
        
        D_State1 -- "Xem nhanh nội dung" --> E1["cat report.txt"]
        D_State1 -- "Xem nội dung theo trang" --> E2["less report.txt"]
        D_State1 -- "Chỉnh sửa nội dung" --> E3["nano report.txt"]
        E1 & E2 & E3 --> D_State1
        
        D_State1 -- "Sao chép file" --> F1["cp report.txt report_backup.txt"]
        D_State1 -- "Đổi tên file" --> F2["mv report.txt final_report.txt"]
        D_State1 -- "Di chuyển file vào thư mục projects" --> F3["mv report.txt projects/"]
        
        F1 & F2 & F3 --> G_Managed("File đã được quản lý")
        
        G_Managed -- "Xóa file<br/>(Hành động không thể hoàn tác!)" --> H_Delete_File["rm final_report.txt"]
        D_State2 -- "Xóa thư mục<br/>(Phải rỗng hoặc dùng -r)" --> H_Delete_Dir["rm -r projects"]
        
        H_Delete_File & H_Delete_Dir --> I_End(Kết thúc vòng đời)

        classDef start fill:#d4f1f9,stroke:#05a,stroke-width:2px;
        classDef command fill:#e6f7ff,stroke:#07c,stroke-width:1px;
        classDef state fill:#e6ffe6,stroke:#292,stroke-width:1px;
        classDef decision fill:#fff9e6,stroke:#d90,stroke-width:1px;
        classDef danger fill:#ffdede,stroke:#c44,stroke-width:2px;
        classDef endNode fill:#f9f9f9,stroke:#999,stroke-width:1px;

        class A_Start start;
        class I_End endNode;
        class C1,C2,E1,E2,E3,F1,F2,F3 command;
        class D_State1,D_State2,G_Managed state;
        class B_Create decision;
        class H_Delete_File,H_Delete_Dir danger;
    end
```  

## **6. Người dùng và quyền hạn**

### **6.1. Mối quan hệ tổng quan**

```mermaid
graph TD
    U[User<br/>username, UID, homeDirectory] 
    G[Group<br/>groupname, GID]
    F[File<br/>filename, permissions]
    P[Permission<br/>READ, WRITE, EXECUTE]
    
    OP[Owner Permission]
    GP[Group Permission] 
    OT[Other Permission]
    
    CMD1[sudo: Thực thi với quyền root]
    CMD2[chmod: Thay đổi quyền]
    CMD3[chown: Thay đổi chủ sở hữu]
    
    U ---|là thành viên của| G
    U ---|sở hữu| F
    G ---|sở hữu| F
    
    F --- OP
    F --- GP
    F --- OT
    
    OP ---|kế thừa| P
    GP ---|kế thừa| P
    OT ---|kế thừa| P
    
    CMD1 -.-> U
    CMD2 -.-> F
    CMD3 -.-> F
    
    classDef userClass fill:#e1f5fe,stroke:#01579b,stroke-width:3px,color:#000
    classDef groupClass fill:#f3e5f5,stroke:#4a148c,stroke-width:3px,color:#000
    classDef fileClass fill:#fff3e0,stroke:#e65100,stroke-width:3px,color:#000
    classDef permissionClass fill:#e8f5e8,stroke:#1b5e20,stroke-width:3px,color:#000
    classDef ownerPermClass fill:#ffebee,stroke:#c62828,stroke-width:2px,color:#000
    classDef groupPermClass fill:#fff8e1,stroke:#f57f17,stroke-width:2px,color:#000
    classDef otherPermClass fill:#fce4ec,stroke:#880e4f,stroke-width:2px,color:#000
    classDef commandClass fill:#f1f8e9,stroke:#33691e,stroke-width:2px,color:#000
    
    class U userClass
    class G groupClass
    class F fileClass
    class P permissionClass
    class OP ownerPermClass
    class GP groupPermClass
    class OT otherPermClass
    class CMD1,CMD2,CMD3 commandClass
``` 

### **6.2. Quy trình kiểm tra quyền truy cập**
```mermaid
flowchart TD
    Start([Yêu cầu truy cập file]) --> CheckUser{Kiểm tra User}
    
    CheckUser --> IsOwner{User là<br/>chủ sở hữu?}
    IsOwner -->|Có| CheckOwnerPerm[Kiểm tra Owner Permission]
    IsOwner -->|Không| CheckGroup{User thuộc<br/>Group sở hữu?}
    
    CheckGroup -->|Có| CheckGroupPerm[Kiểm tra Group Permission]
    CheckGroup -->|Không| CheckOtherPerm[Kiểm tra Other Permission]
    
    CheckOwnerPerm --> OwnerResult{Có quyền?}
    CheckGroupPerm --> GroupResult{Có quyền?}
    CheckOtherPerm --> OtherResult{Có quyền?}
    
    OwnerResult -->|Có| Allow([Cho phép truy cập])
    OwnerResult -->|Không| Deny([Từ chối truy cập])
    
    GroupResult -->|Có| Allow
    GroupResult -->|Không| Deny
    
    OtherResult -->|Có| Allow
    OtherResult -->|Không| Deny
    
    Deny --> UseSudo{Sử dụng sudo?}
    UseSudo -->|Có| Allow
    UseSudo -->|Không| End([Kết thúc])
    
    Allow --> End
    
    classDef startEndClass fill:#e3f2fd,stroke:#0d47a1,stroke-width:3px,color:#000
    classDef decisionClass fill:#fff3e0,stroke:#ef6c00,stroke-width:3px,color:#000
    classDef processClass fill:#f1f8e9,stroke:#2e7d32,stroke-width:2px,color:#000
    classDef ownerClass fill:#ffebee,stroke:#c62828,stroke-width:2px,color:#000
    classDef groupClass fill:#fff8e1,stroke:#f57f17,stroke-width:2px,color:#000
    classDef otherClass fill:#fce4ec,stroke:#880e4f,stroke-width:2px,color:#000
    classDef allowClass fill:#e8f5e8,stroke:#1b5e20,stroke-width:3px,color:#000
    classDef denyClass fill:#ffebee,stroke:#d32f2f,stroke-width:3px,color:#000
    classDef sudoClass fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    
    class Start,End startEndClass
    class CheckUser,IsOwner,CheckGroup,OwnerResult,GroupResult,OtherResult,UseSudo decisionClass
    class CheckOwnerPerm ownerClass
    class CheckGroupPerm groupClass
    class CheckOtherPerm otherClass
    class Allow allowClass
    class Deny denyClass
``` 

### **6.3. Quy trình thay đổi quyền**
```mermaid
sequenceDiagram
    participant U as 👤 User
    participant S as 🖥️ System
    participant F as 📁 File
    participant P as 🔐 Permission
    
    Note over U,P: Quy trình thay đổi quyền file với chmod
    
    U->>+S: chmod 755 filename
    S->>+F: Kiểm tra quyền hiện tại
    F->>-S: Trả về thông tin quyền cũ
    
    S->>S: Phân tích mode (755)
    Note over S: Owner: rwx (7)<br/>Group: r-x (5)<br/>Other: r-x (5)
    
    alt Cập nhật quyền Owner
        S->>+P: Thiết lập Owner Permission = rwx
        P->>-S: Cập nhật thành công
    end
    
    alt Cập nhật quyền Group  
        S->>+P: Thiết lập Group Permission = r-x
        P->>-S: Cập nhật thành công
    end
    
    alt Cập nhật quyền Other
        S->>+P: Thiết lập Other Permission = r-x
        P->>-S: Cập nhật thành công
    end
    
    S->>+F: Áp dụng tất cả quyền mới
    F->>F: Lưu thay đổi vào metadata
    F->>-S: Hoàn tất cập nhật
    
    S->>-U: Lệnh chmod thực hiện thành công
    
    Note over U,P: File hiện có quyền 755 (rwxr-xr-x)
    
    %%{init: {
        'theme': 'base',
        'themeVariables': {
            'primaryColor': '#e8f4fd',
            'primaryTextColor': '#1565c0',
            'primaryBorderColor': '#1976d2',
            'lineColor': '#42a5f5',
            'secondaryColor': '#fff8e1',
            'tertiaryColor': '#f3e5f5',
            'background': '#fafafa',
            'actorBkg': '#e1f5fe',
            'actorBorder': '#0277bd',
            'actorTextColor': '#01579b',
            'activationBkgColor': '#ffecb3',
            'activationBorderColor': '#ff8f00',
            'noteBkgColor': '#e8f5e8',
            'noteBorderColor': '#4caf50',
            'noteTextColor': '#2e7d32'
        }
    }}%%
``` 
