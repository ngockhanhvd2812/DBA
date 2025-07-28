# Phần 1: Giới thiệu và Bắt đầu
**Giải thích các khái niệm nền tảng và các bước đầu tiên để tiếp cận Linux**
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
``` 

---

# Phần 2: Giao diện Dòng lệnh (CLI)
**Đây là phần quan trọng nhất, tập trung vào các lệnh cơ bản để tương tác với hệ thống.** 
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
```  
---

# Phần 3: Quản trị Hệ thống Cơ bản
**Sơ đồ này mô tả các tác vụ quản trị thiết yếu như quản lý người dùng, phần mềm và tiến trình.** 
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
            P4 --> P4a("<b>chmod [quyền] [file]</b><br/>Thay đổi quyền truy cập<br/><i>(vd: chmod 755 script.sh)</i>")
            P4 --> P4b("<b>chown [user] [file]</b><br/>Thay đổi chủ sở hữu")

        %% Chapter 9 Details
        C9 --> PM1("Trình quản lý gói (Package Manager)")
        PM1 --> PM2{Hệ thống Distro}
            PM2 -- Debian/Ubuntu --> APT("<b>apt</b>")
            PM2 -- Fedora/CentOS --> DNF("<b>dnf / yum</b>")
            PM2 -- Arch --> PACMAN("<b>pacman</b>")
        
        APT --> APT_Flow
        subgraph APT_Flow [Quy trình quản lý phần mềm với APT]
            direction LR
            A1("sudo apt update<br/><i>Cập nhật danh sách gói</i>") --> A2("apt search [tên]<br/><i>Tìm kiếm gói</i>")
            A2 --> A3("sudo apt install [tên_gói]<br/><i>Cài đặt phần mềm</i>")
            A3 --> A4("sudo apt remove [tên_gói]<br/><i>Gỡ bỏ phần mềm</i>")
        end
        
        %% Chapter 10 Details
        C10 --> PR1("Tiến trình (Process) là gì?")
        PR1 -- "Là một chương trình đang chạy" --> PR2
        C10 --> PR3("Các lệnh giám sát")
            PR3 --> PR3a("<b>ps aux</b><br/>Xem tất cả tiến trình đang chạy")
            PR3 --> PR3b("<b>top / htop</b><br/>Xem tài nguyên hệ thống<br/>(CPU, RAM) theo thời gian thực")
        C10 --> PR4("Dừng một tiến trình")
            PR4 --> PR4a("<b>kill [PID]</b><br/>Gửi tín hiệu dừng (PID lấy từ ps/top)")
            PR4a -- "Nếu không hiệu quả" --> PR4b("<b>kill -9 [PID]</b><br/>Buộc dừng ngay lập tức")
    end
``` 
---
# Phần 4: Mạng và các Công cụ Nâng cao
**Giới thiệu về mạng, tự động hóa với script và giao diện đồ họa.** 
```mermaid 
graph TD
    subgraph "Phần 4: Mạng & Nâng cao"
        %% Chapters
        C11("Chương 11: Kiến thức Mạng cơ bản")
        C12("Chương 12: Giới thiệu Shell Scripting")
        C13("Chương 13: Giao diện Đồ họa (GUI)")

        %% Chapter 11 Details
        C11 --> N1("Các lệnh kiểm tra mạng")
        N1 --> N1a("<b>ip address / ifconfig</b><br/>Xem địa chỉ IP")
        N1 --> N1b("<b>ping [địa_chỉ_ip / tên_miền]</b><br/>Kiểm tra kết nối tới máy chủ khác")
        
        C11 --> N2("<b>SSH (Secure Shell)</b><br/>Đăng nhập và điều khiển<br/>máy tính khác từ xa qua CLI")
        N2 -- "Cú pháp" --> N2a("ssh [user]@[địa_chỉ_ip]")
        MyPC["Máy tính của bạn"] -- ssh user@server.com --> Server["Máy chủ Linux ở xa"]

        %% Chapter 12 Details
        C12 --> S1("Shell Script là gì?<br/><i>Một file văn bản chứa<br/>chuỗi các câu lệnh Linux</i>")
        S1 --> S2("Cấu trúc một script đơn giản")
        S2 --> S2_File
        subgraph S2_File [my_script.sh]
            S2a("<b>#!/bin/bash</b> (Shebang)")
            S2b("echo 'Hello, World!'")
            S2c("date")
        end
        S2_File -- "Bước 1: Cấp quyền thực thi" --> S3("chmod +x my_script.sh")
        S3 -- "Bước 2: Chạy script" --> S4("./my_script.sh")
        
        %% Chapter 13 Details
        C13 --> G1("Giao diện đồ họa (GUI) hoạt động thế nào?")
        G1 --> G2("<b>Desktop Environment (DE)</b><br/>Bộ sưu tập các thành phần<br/>tạo nên giao diện người dùng")
        G2 --> G3("Các DE phổ biến")
            G3 --> G3a("<b>GNOME</b><br/>Hiện đại, đơn giản (mặc định trên Ubuntu, Fedora)")
            G3 --> G3b("<b>KDE Plasma</b><br/>Linh hoạt, nhiều tùy chỉnh, đẹp mắt")
            G3 --> G3c("<b>XFCE</b><br/>Nhẹ, nhanh, phù hợp máy cấu hình yếu")
        C13 --> G4("Các ứng dụng đồ họa thường dùng<br/><i>(Trình duyệt, Office, Media Player...)</i>")
    end
``` 