# **I. TỔNG QUAN VỀ LINUX**

## **1. Giới thiệu và Bắt đầu**

- Giải thích các khái niệm nền tảng và các bước đầu tiên để tiếp cận Linux
```mermaid
graph TD
    subgraph "Phần 1: Giới thiệu và Bắt đầu"
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

- Sơ đồ phân rã cấu trúc của một hệ điều hành Linux và mối quan hệ giữa các thành phần cốt lõi của nó.

```mermaid
graph TD
    subgraph "Chương 1 (Chi tiết): Phân rã một hệ điều hành Linux"
        
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
- Đây là phần quan trọng nhất, tập trung vào các lệnh cơ bản để tương tác với hệ thống.
```mermaid
graph TD
    subgraph "Phần 2: Giao diện Dòng lệnh (CLI)"
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
- Sơ đồ này mô tả các tác vụ quản trị thiết yếu như quản lý người dùng, phần mềm và tiến trình.
```mermaid
flowchart TD
    subgraph "Phần 3: Quản trị Hệ thống Cơ bản"
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
- Giới thiệu về mạng, tự động hóa với script và giao diện đồ họa
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
