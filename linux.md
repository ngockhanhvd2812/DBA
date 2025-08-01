- [**I. Lộ Trình Học Linux Cho Người Mới Bắt Đầu**](#i-lộ-trình-học-linux-cho-người-mới-bắt-đầu)
    - [**1. Giới Thiệu Và Nền Tảng Linux**](#1-giới-thiệu-và-nền-tảng-linux)
    - [**2. Cài Đặt Và Thiết Lập Môi Trường**](#2-cài-đặt-và-thiết-lập-môi-trường)
    - [**3. Làm Quen Với Terminal \& Lệnh Cơ Bản**](#3-làm-quen-với-terminal--lệnh-cơ-bản)
    - [**4. Quản Lý File Và Thư Mục**](#4-quản-lý-file-và-thư-mục)
    - [**5. Quyền Truy Cập Và Bảo Mật Cơ Bản**](#5-quyền-truy-cập-và-bảo-mật-cơ-bản)
    - [**6. Quản Lý Không Gian Đĩa Và File System**](#6-quản-lý-không-gian-đĩa-và-file-system)
    - [**7. Cài Đặt Phần Mềm**](#7-cài-đặt-phần-mềm)
    - [**8. Quản Lý Tiến Trình**](#8-quản-lý-tiến-trình)
    - [**9. Mạng Và Kết Nối**](#9-mạng-và-kết-nối)
    - [**10. Shell Scripting Cơ Bản**](#10-shell-scripting-cơ-bản)
    - [**11. Quản Lý Ổ Đĩa Với LVM (Logical Volume Manager)**](#11-quản-lý-ổ-đĩa-với-lvm-logical-volume-manager)
    - [**12. Troubleshooting Và Backup**](#12-troubleshooting-và-backup)
    - [**13. Tổng Kết Và Bước Tiếp Theo**](#13-tổng-kết-và-bước-tiếp-theo)
- [**II. GIỚI THIỆU VÀ LỊCH SỬ LINUX**](#ii-giới-thiệu-và-lịch-sử-linux)
    - [**1. Phân rã hệ điều hành Linux**](#1-phân-rã-hệ-điều-hành-linux)
- [**II. CÀI ĐẶT VÀ BẮT ĐẦU**](#ii-cài-đặt-và-bắt-đầu)
- [**III. GIAO DIỆN NGƯỜI DÙNG (GUI VÀ CLI)**](#iii-giao-diện-người-dùng-gui-và-cli)
- [**IV. HỆ THỐNG FILE VÀ ĐIỀU HƯỚNG**](#iv-hệ-thống-file-và-điều-hướng)
- [**V. THAO TÁC VỚI FILE \& THƯ MỤC**](#v-thao-tác-với-file--thư-mục)
- [**VI. NGƯỜI DÙNG VÀ QUYỀN HẠN**](#vi-người-dùng-và-quyền-hạn)
- [**VII. QUẢN LÝ PHẦN MỀM (PACKAGE MANAGER)**](#vii-quản-lý-phần-mềm-package-manager)
- [**VIII. GIÁM SÁT VÀ ĐIỀU KHIỂN TIẾN TRÌNH**](#viii-giám-sát-và-điều-khiển-tiến-trình)
- [**IX. CÔNG CỤ MẠNG CƠ BẢN**](#ix-công-cụ-mạng-cơ-bản)
- [**X. SHELL SCRIPTING VÀ TỰ ĐỘNG HÓA**](#x-shell-scripting-và-tự-động-hóa)
- [**XI. QUẢN LÝ DỊCH VỤ VÀ NÂNG CAO**](#xi-quản-lý-dịch-vụ-và-nâng-cao)
- [**XII. BẢO MẬT VÀ TROUBLESHOOTING CƠ BẢN**](#xii-bảo-mật-và-troubleshooting-cơ-bản)



# **I. Lộ Trình Học Linux Cho Người Mới Bắt Đầu**

### **1. Giới Thiệu Và Nền Tảng Linux**  
🎯 **Mục tiêu**: Hiểu Linux là gì, tại sao dùng. 

**Nội dung học**:  
1. 🐧 **Linux là gì?**  
   - Lịch sử ngắn gọn: từ Unix đến Linus Torvalds  
   - So sánh với Windows/macOS một cách đơn giản
2. 🔧 **Các thành phần cốt lõi**:  
   - Kernel (nhân) - não bộ của hệ thống
   - Distro (Ubuntu, Mint, Fedora...)
   - Shell - cách giao tiếp với máy tính

### **2. Cài Đặt Và Thiết Lập Môi Trường**   
🎯 **Mục tiêu**: Có môi trường Linux để thực hành, làm quen giao diện.  

**Nội dung học**:  
1. 📦 **Chọn Distro cho người mới**:  
   - **Khuyến nghị**: Ubuntu LTS (ổn định, nhiều tài liệu)
   - Tại sao tránh Arch, Gentoo lúc đầu
2. 💿 **Phương pháp cài đặt an toàn**:  
   - **Ưu tiên**: VirtualBox (không ảnh hưởng máy chính)
   - Live USB để thử nghiệm
   - Dual Boot (chỉ khi đã tự tin và **sao lưu dữ liệu**)
3. 🛠 **Hướng dẫn cài đặt từng bước**:  
   - Tải Ubuntu ISO từ trang chính thức
   - Cài VirtualBox, tạo máy ảo
   - Cài Ubuntu với cấu hình cơ bản
4. 🖥 **Làm quen giao diện**:  
   - Desktop Environment (GNOME)
   - Ứng dụng cơ bản: Files, Terminal, Firefox
   - Cài đặt hệ thống cơ bản
5. ⚙️ **Cấu hình cơ bản**:  
   - Thay đổi theme/font cho dễ nhìn
   - Thiết lập PATH cơ bản
   - Cài đặt extension GUI đơn giản 

📝 **Bài tập thực hành**:  
   - Cài Ubuntu trên VirtualBox
   - Mở Terminal và gõ `echo "Xin chào Linux"`
   - Cài đặt ngôn ngữ tiếng Việt và thay đổi theme
   - Tạo folder qua GUI và kiểm tra qua Terminal

📚 **Tài nguyên học tập**:  
   - Video: "How to install Ubuntu on VirtualBox"
   - Ubuntu Desktop Guide (tiếng Việt)

### **3. Làm Quen Với Terminal & Lệnh Cơ Bản**   

🎯 **Mục tiêu**: Thành thạo các lệnh thiết yếu.  

**Nội dung học**:  
1. 🖥 **Terminal là gì và tại sao quan trọng**:  
   - Giao diện dòng lệnh vs giao diện đồ họa
   - Tại sao admin Linux cần biết Terminal
2. 📝 **Cấu trúc lệnh**: `lệnh [tùy-chọn] [đối-số]`  
   - Ví dụ: `ls -l /home`
3. 🆘 **Công cụ trợ giúp**:  
   - `man tên-lệnh` - hướng dẫn chi tiết
   - `lệnh --help` - trợ giúp nhanh
   - Tab completion - tự động hoàn thành
   - Phím mũi tên ↑↓ - lịch sử lệnh
   - Ctrl+R - tìm kiếm lệnh đã dùng
4. 🔍 **Wildcards & pattern**:  
   - `*` (bất kỳ), `?` (1 ký tự), `[]` (phạm vi)
5. 🌎 **Biến môi trường**:  
   - `$PATH` (tìm lệnh), `$HOME` (thư mục nhà)
   - `echo $PATH` để kiểm tra
   - `export VAR=value` để thiết lập tạm thời
6. 💻 **Lệnh cơ bản đầu tiên**:  
   - `pwd` - xem thư mục hiện tại
   - `ls` - liệt kê file/thư mục
   - `cd` - di chuyển thư mục
   - `whoami` - xem tên người dùng
   - `date` - xem ngày giờ
   - `clear` - xóa màn hình

📝 **Bài tập thực hành**:  
   - Thực hành 20 lệnh cơ bản mỗi ngày
   - Tạo cheat sheet cá nhân với các lệnh hay dùng
   - Sử dụng `man` để tìm hiểu 5 lệnh
   - Tạo alias đơn giản: `alias ll='ls -la'`
   - Tìm hiểu và sửa lỗi "command not found" (kiểm tra PATH)

📚 **Tài nguyên học tập**:  
   - "Linux Command Line for Beginners" (free PDF)
   - Interactive terminal: linuxjourney.com

### **4. Quản Lý File Và Thư Mục**  
🎯 **Mục tiêu**: Thành thạo thao tác với file/thư mục - kỹ năng cốt lõi nhất.   

**Nội dung học**:  
1. 📂 **Hiểu cấu trúc thư mục Linux**:  
   - `/` - thư mục gốc
   - `/home` - thư mục người dùng  
   - `/etc` - cấu hình hệ thống
   - `/usr` - ứng dụng người dùng
   - `/var` - dữ liệu thay đổi
   - `/bin` - lệnh hệ thống cơ bản
2. 📋 **Lệnh điều hướng nâng cao**:  
   - `ls -la` - xem chi tiết + file ẩn
   - `cd ~` - về thư mục home
   - `cd ..` - lên thư mục cha
   - `cd -` - về thư mục trước
3. 📑 **Thao tác file/thư mục**:  
   - `touch file.txt` - tạo file trống
   - `mkdir thư-mục` - tạo thư mục
   - `cp file1 file2` - copy file
   - `mv file1 file2` - di chuyển/đổi tên
   - `rm file` - xóa file
   - `rm -r thư-mục` - xóa thư mục
4. 🔄 **Redirection & piping**:  
   - `>` (ghi đè), `>>` (thêm)
   - `|` (kết nối lệnh), `2>` (lỗi)
5. 📖 **Xem và chỉnh sửa file**:  
   - `cat file.txt` - xem nội dung file
   - `less file.txt` - xem file dài
   - `nano file.txt` - chỉnh sửa đơn giản
6. 🔎 **Tìm kiếm cơ bản**:  
   - `find /home -name "*.txt"` - tìm file theo tên
   - `locate "*.log"` - tìm nhanh hơn (cần cập nhật database)
   - `grep "từ-khóa" file.txt` - tìm text trong file
   - `grep -r "error" /var/log` - tìm recursive

📝 **Bài tập thực hành**:  
   - Tạo cấu trúc thư mục dự án cá nhân
   - Copy, move, rename file
   - Tạo và chỉnh sửa file text đơn giản
   - Tìm file theo tên và nội dung
   - Sử dụng redirection và piping để xử lý dữ liệu
   - Thực hành tìm và sửa lỗi "no such file" (kiểm tra pwd, dùng absolute path)

📚 **Tài nguyên học tập**:  
   - Interactive exercises trên cmdchallenge.com
   - "Linux File System" tutorial

### **5. Quyền Truy Cập Và Bảo Mật Cơ Bản**  
🎯 **Mục tiêu**: Hiểu và quản lý quyền file để tránh lỗi "permission denied".  

**Nội dung học**:  
1. 👥 **Khái niệm User và Group**:  
   - Owner (chủ sở hữu), Group (nhóm), Others (người khác)
   - Tại sao cần phân quyền
2. 🔒 **Hiểu quyền truy cập**:  
   - `r` (read) - đọc
   - `w` (write) - ghi
   - `x` (execute) - thực thi
   - Xem quyền với `ls -l`
3. 🛠 **Thay đổi quyền**:  
   - `chmod 755 file` - số học
   - `chmod u+x file` - ký hiệu
   - `chown user:group file` - đổi chủ sở hữu
4. 🧩 **Quyền nâng cao**:  
   - Sticky bit (chỉ chủ sở hữu xóa được)
   - SUID/SGID (ví dụ: lệnh passwd)
5. 👑 **Sudo - quyền quản trị**:  
   - Khi nào cần `sudo`
   - `sudo vs su` - khác biệt
   - Cách sử dụng an toàn
   - Cấu hình sudoers cơ bản
6. 🛡 **Bảo mật cơ bản**:  
   - Tạo mật khẩu mạnh
   - Cập nhật hệ thống thường xuyên
   - Tắt tài khoản root khi không cần
   - SSH hardening cơ bản (sử dụng key-based authentication)
   - Giới thiệu firewall cơ bản (ufw)

📝 **Bài tập thực hành**:  
   - Tạo file và thay đổi quyền truy cập
   - Thực hành lệnh sudo
   - Tạo user mới và phân quyền
   - Thiết lập rule ufw đơn giản (cho phép SSH)
   - Thử nghiệm SUID với lệnh passwd

📚 **Tài nguyên học tập**:  
   - "Linux Permissions Explained" video
   - Ubuntu Security Guide

### **6. Quản Lý Không Gian Đĩa Và File System**  
🎯 **Mục tiêu**: Hiểu cách Linux quản lý ổ đĩa, kiểm tra dung lượng, xử lý ổ đĩa đầy, và thao tác gắn kết ổ đĩa cơ bản.

**Nội dung học**:  
1. 💽 **Filesystem là gì**:  
   - Mount points - điểm gắn kết ổ đĩa  
   - Các loại phổ biến: **ext4** (Linux), **XFS** (Oracle Linux), **NTFS/FAT** (Windows)  
   - Filesystem vs Partition vs LVM  

2. 📊 **Kiểm tra dung lượng**:  
   - `df -h` - dung lượng đã dùng/tổng dung lượng *(human-readable)*  
   - `df -i` - kiểm tra inode *(khi hết inode dù dung lượng còn trống)*  
   - `lsblk` - xem cây thiết bị block  
   - `/proc/partitions` - xem partition từ kernel  

3. 🔍 **Tìm file chiếm dụng**:  
   - `du -sh /path` - tổng dung lượng thư mục  
   - `du -h --max-depth=1 /path` - xem theo cấp độ  
   - `ncdu` - công cụ GUI-like trong terminal *(cần cài)*  
   - `find / -size +100M` - tìm file >100MB  

4. 🧹 **Dọn dẹp không gian**:  
   - Xóa file log cũ: `/var/log/`  
   - Dọn cache package: `sudo apt clean`  
   - Xóa bản cập nhật cũ: `sudo apt autoremove --purge`  
   - Tìm và xóa file tạm: `/tmp/`, `~/.cache/`  

5. 🔌 **Gắn kết (mount) ổ đĩa cơ bản**:  
   - `mount /dev/sdb1 /mnt/data` - gắn phân vùng  
   - `umount /mnt/data` - ngắt gắn kết  
   - Tự động mount qua `/etc/fstab`  
   - Kiểm tra mounted FS với `findmnt` hoặc `mount -l`  
   - Xem thông tin USB/ổ cứng ngoài với `lsblk -f`  

6. ⚠️ **Xử lý tình huống đầy ổ**:  
   - **Triệu chứng**: không ghi được file, ứng dụng crash  
   - **Quy trình khắc phục**:  
     1. Kiểm tra `df -h` và `df -i`  
     2. Tìm thư mục lớn bằng `du`/`ncdu`  
     3. Xóa hoặc di chuyển file lớn  
     4. Mở rộng filesystem *(sẽ học trong LVM)*  

7. 🛡 **Best Practices**:  
   - Luôn để trống 10-20% dung lượng  
   - Tách /home, /var, /tmp ra phân vùng riêng  
   - Giám sát tự động *(sẽ học trong Shell Scripting)*  

📝 **Bài tập thực hành**:  
   - Tạo file 1GB: `dd if=/dev/zero of=testfile bs=1M count=1000`  
   - Theo dõi `df -h` trước/sau khi tạo file  
   - Dùng `ncdu` scan /var và tìm 3 file lớn nhất  
   - Thử nghiệm xóa file log và dọn cache package  
   - **Thực hành mount**:  
     - Tạo thư mục `/mnt/test`  
     - Tạo file hệ thống: `sudo mkfs.ext4 /dev/sdb1` (giả sử có phân vùng sẵn)  
     - Mount thủ công: `sudo mount /dev/sdb1 /mnt/test`  
     - Ghi file vào `/mnt/test` và kiểm tra  
     - Thêm dòng vào `/etc/fstab` để mount tự động  
   - Tạo kịch bản ổ đĩa đầy (>90%) và thực hành xử lý  

📚 **Tài nguyên học tập**:  
   - [Linux Disk Management Cheatsheet](https://linuxhandbook.com/disk-space-commands/)  
   - Video: [How to Clean Up Disk Space on Ubuntu](https://youtu.be/4K4sMvLy7d0)  
   - Guide: [Mounting Drives in Linux](https://linuxize.com/post/how-to-mount-and-unmount-file-systems-in-linux/)  

### **7. Cài Đặt Phần Mềm**  
🎯 **Mục tiêu**: Biết cách cài đặt và quản lý ứng dụng an toàn.  

**Nội dung học**:  
1. 📦 **Package Manager là gì**:  
   - Kho phần mềm tập trung
   - Tự động xử lý dependencies
2. 🔄 **Sử dụng APT (Ubuntu/Debian)**:  
   - `sudo apt update` - cập nhật danh sách
   - `sudo apt install tên-gói` - cài đặt
   - `sudo apt remove tên-gói` - gỡ bỏ
   - `sudo apt upgrade` - cập nhật hệ thống
3. 📋 **Quản lý phần mềm**:  
   - `apt list --installed` - xem đã cài
   - `apt search từ-khóa` - tìm kiếm
   - `sudo apt autoremove` - dọn dẹp
4. 🏪 **Ubuntu Software Center & Snap**:  
   - Cài đặt qua giao diện đồ họa
   - Ưu/nhược điểm của Snap packages
5. ⚙️ **Các package manager khác**:  
   - `dnf`/`yum` (Fedora)
   - `pacman` (Arch)
   - Compile từ source (make/install - khi cần thiết)

📝 **Bài tập thực hành**:  
   - Cài đặt: git, curl, htop, tree
   - Cập nhật toàn bộ hệ thống
   - Gỡ bỏ một ứng dụng không cần
   - Thử cài đặt qua Snap và so sánh với apt
   - Tìm hiểu và sửa lỗi repository (kiểm tra /etc/apt/sources.list)

📚 **Tài nguyên học tập**:  
   - Ubuntu Package Management Guide
   - APT cheat sheet

### **8. Quản Lý Tiến Trình**  
🎯 **Mục tiêu**: Giám sát và điều khiển các chương trình đang chạy.  

**Nội dung học**:  
1. ⚙️ **Process (tiến trình) là gì**:  
   - Mỗi chương trình chạy = 1 process
   - PID (Process ID) - số định danh
2. 👀 **Xem tiến trình**:  
   - `ps aux` - liệt kê tất cả process
   - `top` - xem real-time
   - `htop` - giao diện đẹp hơn (cần cài)
   - `pstree` - xem dạng cây
3. ❌ **Dừng tiến trình**:  
   - `kill PID` - dừng nhẹ nhàng
   - `kill -9 PID` - buộc dừng
   - `pkill tên-chương-trình` - kill theo tên
4. 🏁 **Chạy nền và foreground**:  
   - `lệnh &` - chạy nền
   - `Ctrl+Z` - tạm dừng
   - `jobs` - xem công việc nền
   - `fg` - đưa lên foreground
   - `bg` - tiếp tục chạy nền
   - `nohup` - chạy ngay cả khi logout
5. 🔧 **Systemd services cơ bản**:  
   - `sudo systemctl status tên-service`
   - `sudo systemctl start/stop/restart tên-service`
   - `sudo systemctl enable tên-service` - khởi động cùng hệ thống
   - Các loại unit: `.service`, `.timer`, `.target`
6. 📜 **Logs hệ thống**:  
   - `journalctl -u tên-service -f` - xem log real-time

📝 **Bài tập thực hành**:  
   - Sử dụng htop để giám sát hệ thống
   - Kill process tiêu tốn CPU cao
   - Chạy lệnh ở background và quản lý với jobs
   - Cài đặt và quản lý dịch vụ Apache (systemctl)
   - Xem log của một dịch vụ đang chạy

📚 **Tài nguyên học tập**:  
   - "Linux Process Management" tutorial
   - htop explained

### **9. Mạng Và Kết Nối**  
🎯 **Mục tiêu**: Kết nối Linux với internet và máy tính khác.  

**Nội dung học**:  
1. 🌐 **Kiểm tra kết nối mạng**:  
   - `ping google.com` - test internet
   - `ip addr`/`ifconfig` - xem IP address
   - `traceroute google.com` - theo dõi đường đi
   - `ss`/`netstat` - xem kết nối mạng
2. 🔑 **SSH - Kết nối từ xa**:  
   - Cài đặt SSH server
   - Kết nối: `ssh user@ip-address`
   - Copy file: `scp file user@ip:/path`
   - Tạo SSH key: `ssh-keygen`
   - Cấu hình SSH cơ bản
3. 🛡 **Firewall cơ bản**:  
   - `sudo ufw enable` - bật firewall
   - `sudo ufw allow ssh` - cho phép SSH
   - `sudo ufw status` - xem trạng thái
   - Hiểu cơ bản về iptables
4. 🌐 **Web tools**:  
   - `curl` - gọi API, tải file
   - `wget` - tải file từ web
   - Giới thiệu nmap (scan ports)

📝 **Bài tập thực hành**:  
   - Test kết nối internet
   - Cài đặt SSH và kết nối giữa 2 máy ảo
   - Tạo SSH key và sử dụng xác thực bằng key
   - Sử dụng curl để gọi API đơn giản
   - Thiết lập firewall cơ bản với ufw
   - Khắc phục lỗi kết nối bằng cách kiểm tra firewall

📚 **Tài nguyên học tập**:  
   - "SSH Essentials" guide
   - Basic networking for Linux

### **10. Shell Scripting Cơ Bản**  
🎯 **Mục tiêu**: Tự động hóa công việc lặp đi lặp lại.  

**Nội dung học**:  
1. 📝 **Script là gì và tại sao cần**:  
   - Tự động hóa task
   - Tránh lặp lại công việc
2. 🚀 **Tạo script đầu tiên**:  
   - Shebang: `#!/bin/bash`
   - Quyền thực thi: `chmod +x script.sh`
   - Chạy: `./script.sh`
3. 🔤 **Biến và input**:  
   - `name="John"` - gán biến
   - `echo $name` - sử dụng biến
   - `read -p "Nhập tên: " name` - input từ user
   - `echo "Arguments: $1, $2"` - tham số dòng lệnh
4. 🔄 **Điều kiện và vòng lặp đơn giản**:  
   - `if [ condition ]; then ... fi`
   - `for file in *.txt; do ... done`
   - `while [ condition ]; do ... done`
   - `case` statement
5. ⏰ **Cron - Lập lịch tự động**:  
   - `crontab -e` - chỉnh sửa lịch
   - `0 2 * * * /path/to/script.sh` - chạy 2h sáng mỗi ngày
6. 📄 **Xử lý văn bản cơ bản**:  
   - `sed` và `awk` cơ bản
   - Kết hợp với `grep` và `find`

📝 **Bài tập thực hành**:  
   - Viết script backup thư mục home
   - Script kiểm tra disk space
   - Đặt lịch chạy script tự động
   - Viết script xử lý file log đơn giản với grep/sed
   - Thử nghiệm error handling cơ bản

📚 **Tài nguyên học tập**:  
   - "Bash Scripting Tutorial for Beginners"
   - Cron job generator online

### **11. Quản Lý Ổ Đĩa Với LVM (Logical Volume Manager)**  
🎯 **Mục tiêu**: Hiểu và sử dụng LVM để quản lý không gian lưu trữ linh hoạt, đặc biệt là mở rộng dung lượng ổ cứng khi cần.  
**Nội dung học**:  
1. 💾 **Giới thiệu về LVM**:  
   - LVM là gì và tại sao cần sử dụng
   - So sánh với phân vùng truyền thống (partitioning)
   - Các thành phần chính: Physical Volumes (PV), Volume Groups (VG), Logical Volumes (LV)
   - Ưu điểm của LVM: linh hoạt, dễ mở rộng, snapshot

2. 🔧 **Cài đặt và cấu hình LVM cơ bản**:  
   - Kiểm tra LVM đã cài đặt chưa (`lvm2` package)
   - Tạo Physical Volume từ ổ đĩa mới: `pvcreate /dev/sdb`
   - Tạo Volume Group từ các Physical Volumes: `vgcreate vg_data /dev/sdb`
   - Tạo Logical Volume từ Volume Group: `lvcreate -L 10G -n lv_home vg_data`
   - Định dạng và mount Logical Volume: `mkfs.ext4 /dev/vg_data/lv_home`

3. 📏 **Mở rộng dung lượng ổ cứng bằng LVM**:  
   - **Cách 1: Thêm không gian từ Volume Group hiện có**
     - Kiểm tra không gian trống trong Volume Group: `vgs`
     - Mở rộng Logical Volume: `lvextend -L +5G /dev/vg_data/lv_home`
     - Thay đổi kích thước hệ thống tập tin: `resize2fs /dev/vg_data/lv_home` (cho ext4)
   - **Cách 2: Thêm Physical Volume mới vào Volume Group**
     - Thêm ổ cứng mới vào máy ảo/vật lý
     - Tạo Physical Volume: `pvcreate /dev/sdc`
     - Mở rộng Volume Group: `vgextend vg_data /dev/sdc`
     - Tiếp tục mở rộng Logical Volume như cách 1

4. 🔁 **Các thao tác LVM nâng cao**:  
   - Giảm kích thước Logical Volume (cần backup trước!)
   - Tạo snapshot để backup: `lvcreate -L 1G -s -n lv_home_snap /dev/vg_data/lv_home`
   - Di chuyển dữ liệu giữa các Physical Volumes: `pvmove /dev/sdb`
   - Tạo striped và mirrored volumes cho hiệu năng và redundancy

5. 📊 **Giám sát và quản lý LVM**:  
   - Các lệnh kiểm tra trạng thái chi tiết: `pvdisplay`, `vgdisplay`, `lvdisplay`
   - Sử dụng `lvs`, `vgs`, `pvs` cho thông tin ngắn gọn
   - Kiểm tra không gian trống với `df -h` và `vgs`
   - Xem thông tin hệ thống tập tin: `lsblk`, `blkid`

6. ⚠️ **Lưu ý và best practices khi sử dụng LVM**:  
   - Luôn backup trước khi thay đổi cấu hình
   - Hiểu rõ thứ tự các bước khi mở rộng/giảm kích thước
   - Tương thích với các hệ điều hành khác (nếu dùng dual-boot)
   - Khi nào nên và không nên sử dụng LVM
   - Tích hợp LVM với các công cụ giám sát hệ thống

📝 **Bài tập thực hành**:  
   - Tạo một hệ thống LVM đơn giản trên máy ảo
   - Mở rộng Logical Volume sau khi thêm ổ đĩa mới (cách 1 và cách 2)
   - Tạo snapshot và khôi phục từ snapshot
   - Thực hành giảm kích thước Logical Volume (sau khi backup đầy đủ)
   - Giám sát trạng thái LVM với các lệnh display
   - Tạo kịch bản tự động kiểm tra không gian LVM và cảnh báo
   - Thực hành khắc phục lỗi "out of space" bằng cách mở rộng LV

📚 **Tài nguyên học tập**:  
   - LVM HOWTO từ Linux Documentation Project
   - Video hướng dẫn thực hành LVM trên YouTube
   - "Mastering LVM" tutorial
   - LVM Cheat Sheet: Các lệnh thường dùng
   - Ubuntu LVM Guide (tài liệu chính thức)


### **12. Troubleshooting Và Backup**  
🎯 **Mục tiêu**: Xử lý sự cố và bảo vệ dữ liệu.  

**Nội dung học**:  
1. 📂 **Xem log hệ thống**:  
   - `/var/log/syslog` - log chung
   - `journalctl -f` - xem log real-time
   - `dmesg` - log kernel
2. 🔍 **Debug cơ bản**:  
   - Đọc error message
   - Google error + "ubuntu"
   - Kiểm tra disk space: `df -h`
   - Kiểm tra RAM: `free -h`
   - Sử dụng `strace` để trace system calls
   - Sử dụng `lsof` để xem file đang mở
3. 💾 **Backup dữ liệu**:  
   - `tar -czf backup.tar.gz /home/user` - nén backup
   - `rsync -av source/ destination/` - sync folder
   - Backup lên cloud (Google Drive, Dropbox)
4. 🚑 **Recovery cơ bản**:  
   - Boot từ Live USB
   - Chroot để sửa hệ thống
   - Single-user mode (sửa chữa qua GRUB)

📝 **Bài tập thực hành**:  
   - Tạo backup script tự động
   - Thực hành đọc log khi có lỗi
   - Recovery file đã xóa nhầm
   - Debug một script lỗi bằng strace
   - Thực hành sao lưu và khôi phục thư mục

📚 **Tài nguyên học tập**:  
   - "Linux Troubleshooting Guide"
   - Backup strategies for home users

### **13. Tổng Kết Và Bước Tiếp Theo**  
🎯 **Mục tiêu**: Củng cố kiến thức và định hướng phát triển.  
**Nội dung học**:  
1. 📖 **Review kiến thức đã học**:  
   - Checklist các kỹ năng cơ bản
   - Làm bài test tự đánh giá
   - Best practices: Tránh dùng root, cập nhật định kỳ, backup hàng tuần
2. 🚀 **Dự án thực tế**:  
   - Setup home server đơn giản
   - Tạo website tĩnh với Apache/Nginx
   - Automation script cho công việc hàng ngày
   - **Mới**: Cài đặt LAMP stack (Apache, MySQL, PHP)
3. 📚 **Tài nguyên tiếp tục học**:  
   - "The Linux Command Line" book
   - "UNIX and Linux System Administration Handbook"
   - Linux Academy, Cloud Guru courses
   - Hands-on labs: KodeKloud, A Cloud Guru
   - Free courses trên edX.org ("Introduction to Linux" bởi Linux Foundation)
 
📝 **Bài tập cuối khóa**:  
   - Xây dựng và present 1 dự án nhỏ (ví dụ: home server, script automation)
   - Viết blog chia sẻ journey học Linux
   - Thiết lập hệ thống backup tự động hàng tuần
   - Kiểm tra và khắc phục một lỗi giả định trên hệ thống

# **II. GIỚI THIỆU VÀ LỊCH SỬ LINUX**

Linux là hệ điều hành mã nguồn mở, miễn phí, ổn định và an toàn, được sử dụng rộng rãi từ server đến máy tính cá nhân. Khác với Windows (đóng nguồn, tập trung vào GUI thân thiện) hay macOS (dựa trên Unix, tích hợp tốt với phần cứng Apple), Linux linh hoạt, tùy chỉnh cao, nhưng đòi hỏi học lệnh dòng (CLI) nhiều hơn.

- **Lịch sử ngắn gọn**: Linux được Linus Torvalds tạo ra năm 1991 dựa trên Unix. Kernel (nhân) Linux kết hợp với công cụ GNU (từ Richard Stallman) tạo thành hệ điều hành hoàn chỉnh. Các distro phổ biến: Ubuntu (dễ dùng), Fedora (công nghệ mới), Debian (ổn định).

- **Ưu điểm cho người mới**: Miễn phí, cộng đồng lớn (hỏi đáp trên Stack Overflow, Reddit), ít virus hơn Windows. Nhược điểm: Cài driver phần cứng đôi khi phức tạp.


### **1. Phân rã hệ điều hành Linux**

```mermaid
graph TD
    %% Main GNU/Linux OS decomposition
    subgraph "🌟 Phân rã Hệ điều hành GNU/Linux 🌟"

        HW["💻 Phần cứng (CPU, RAM, Ổ đĩa, Card mạng)"]
        style HW fill:#fff59d,stroke:#f57f17,stroke-width:2px,color:#000,font-weight:bold

        KERNEL["<b>🛠 Linux Kernel (Nhân)</b><br/><i>Tác giả: Linus Torvalds</i>"]
        style KERNEL fill:#ffcdd2,stroke:#c62828,stroke-width:3px,color:#000,font-weight:bold
        
        HW -- "Được quản lý & trừu tượng hóa bởi" --> KERNEL
        
        KERNEL -- "Chức năng cốt lõi" --> KM["⚡ Quản lý Tiến trình (Process Scheduling)"]
        style KM fill:#ffecb3,stroke:#f57c00

        KERNEL -- "Chức năng cốt lõi" --> KMem["💾 Quản lý Bộ nhớ (Memory Management)"]
        style KMem fill:#d1c4e9,stroke:#4527a0

        KERNEL -- "Chức năng cốt lõi" --> KFS["📂 Quản lý Hệ thống File (Filesystem)"]
        style KFS fill:#c8e6c9,stroke:#2e7d32

        KERNEL -- "Chức năng cốt lõi" --> KNet["🌐 Quản lý Mạng (Networking Stack)"]
        style KNet fill:#b3e5fc,stroke:#0277bd

        KERNEL -- "Chức năng cốt lõi" --> KDD["🔌 Quản lý Trình điều khiển Thiết bị (Device Drivers)"]
        style KDD fill:#ffe0b2,stroke:#ef6c00

        USERLAND["<b>👥 Userland / Không gian người dùng</b><br/><i>Mọi thứ ngoài Kernel</i>"]
        style USERLAND fill:#e1bee7,stroke:#6a1b9a,stroke-width:2px
        KERNEL -- "Cung cấp giao diện qua System Calls" --> USERLAND
        
        subgraph "🧩 Các thành phần trong Userland"
            GNU["<b>🔧 Công cụ GNU (GNU Core Utilities)</b><br/><i>Richard Stallman & FSF</i>"]
            style GNU fill:#c8e6c9,stroke:#2e7d32
            GNU --> BinUtils["📜 ls, cp, mv, rm, grep, awk, sed..."]
            
            SHELL["<b>⌨️ Shell (CLI)</b><br/><i>Trình thông dịch lệnh</i>"]
            style SHELL fill:#fff9c4,stroke:#fbc02d
            SHELL --> BASH["🐚 Bash"]
            SHELL --> ZSH["🔮 Zsh"]
            SHELL --> FISH["🐠 Fish"]

            GUI["<b>🖥 Hệ thống Đồ họa</b>"]
            style GUI fill:#bbdefb,stroke:#0d47a1
            GUI --> X11["🖼 X Window System (X11)"]
            GUI --> WAYLAND["🚀 Wayland"]

            DE["<b>🎨 Môi trường Đồ họa (Desktop Environment)</b>"]
            style DE fill:#f8bbd0,stroke:#ad1457
            DE --> GNOME["🦋 GNOME"]
            DE --> KDE["💎 KDE"]
            DE --> XFCE["⚡ XFCE"]

            APPS["<b>📦 Ứng dụng Người dùng</b><br/>Firefox, LibreOffice, GIMP, VLC..."]
            style APPS fill:#cfd8dc,stroke:#37474f
        end

        DISTRO["<b>📀 Linux Distribution (Distro)</b><br/><i>Hệ điều hành hoàn chỉnh</i>"]
        style DISTRO fill:#b3e5fc,stroke:#01579b,stroke-width:4px
        
        KERNEL & GNU & SHELL & GUI & DE & APPS -- "Được đóng gói lại bởi" ---> DISTRO

        USER["👤 Người dùng"]
        style USER fill:#d7ccc8,stroke:#4e342e
        USER -- "Tương tác qua" --> SHELL
        USER -- "Tương tác qua" --> DE
        
    end

    %% Philosophy section (outside main subgraph)
    OS["<b>🌍 Open Source</b><br/>Mã nguồn công khai, cho phép xem, sửa đổi và phân phối lại."]
    style OS fill:#c8e6c9,stroke:#1b5e20

    FSF["<b>🏴 Free Software Foundation (FSF)</b><br/>Thúc đẩy phần mềm tự do."]
    style FSF fill:#ffecb3,stroke:#f57c00

    GNU_Linux["<b>🤔 Tại sao gọi 'GNU/Linux'?</b><br/>Kernel chỉ là một phần, phần lớn công cụ từ GNU."]
    style GNU_Linux fill:#ffcdd2,stroke:#b71c1c

    OS --> FSF --> GNU_Linux

```

---

# **II. CÀI ĐẶT VÀ BẮT ĐẦU**

Chọn distro dễ dùng như Ubuntu hoặc Linux Mint. Tải ISO từ trang chính thức, kiểm tra checksum để tránh file hỏng.

- **Phương pháp cài đặt**: 
  - Live USB: Chạy thử mà không ảnh hưởng ổ cứng (dùng Rufus để tạo USB bootable).
  - Dual Boot: Cài song song Windows (sao lưu dữ liệu trước, phân vùng ổ cứng).
  - Máy ảo: Sử dụng VirtualBox/VMware để chạy Linux trong Windows.

- **Quy trình cài đặt cơ bản**: Chọn ngôn ngữ, phân vùng ổ, tạo user/password. Sau cài, cập nhật hệ thống: `sudo apt update && sudo apt upgrade` (cho Ubuntu).

- **Làm quen**: Đăng nhập, khám phá desktop (GNOME cho Ubuntu). Mật khẩu không hiển thị khi gõ là tính năng bảo mật.

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

---

# **III. GIAO DIỆN NGƯỜI DÙNG (GUI VÀ CLI)**

- **GUI (Graphical User Interface)**: Giao diện đồ họa như Windows. Các DE phổ biến: GNOME (Ubuntu), KDE (Plasma), XFCE (nhẹ). Sử dụng chuột để mở file, cài app từ store.

- **CLI (Command Line Interface)**: Terminal là "siêu năng lực" của Linux. Shell (như Bash) xử lý lệnh. Cấu trúc lệnh: `command [options] [arguments]` (ví dụ: `ls -la /home`).

- **Mở terminal**: Ctrl+Alt+T trên Ubuntu. Lệnh cơ bản: `man command` để xem hướng dẫn, `command --help` để hỗ trợ nhanh.

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

- **Mẹo cho người mới**: Thực hành lệnh trong thư mục test để tránh xóa nhầm. Sử dụng Tab để tự hoàn thành lệnh.

---

# **IV. HỆ THỐNG FILE VÀ ĐIỀU HƯỚNG**

Hệ thống file Linux là cây thư mục bắt đầu từ / (root). Không có ổ C:/ như Windows, mọi thứ là file (kể cả thiết bị).

- **Đường dẫn**: Tuyệt đối (/home/user) hoặc tương đối (Documents từ thư mục hiện tại).

- **Thư mục quan trọng**: /home (dữ liệu user), /etc (cấu hình), /var/log (logs), /usr (phần mềm).

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

---

# **V. THAO TÁC VỚI FILE & THƯ MỤC**

Vòng đời: Tạo → Xem/Sửa → Sao chép/Di chuyển → Xóa. Cẩn thận với rm (không có thùng rác như Windows).

- **Ví dụ thực hành**: Tạo file: `touch test.txt`, sửa: `echo "Hello" > test.txt`, xem: `cat test.txt`.

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

---

# **VI. NGƯỜI DÙNG VÀ QUYỀN HẠN**

Mọi thứ thuộc về user/group. Root (superuser) có quyền tối cao, dùng `sudo` để chạy lệnh với quyền root (cần password).

- **Kiểm tra quyền**: `ls -l` hiển thị rwx (read/write/execute) cho owner/group/others.

- **Thay đổi**: `chmod 755 file` (owner rwx, group/others rx). `chown user file` thay chủ sở hữu.

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
            'activationBkg': '#ffecb3',
            'activationBorderColor': '#ff8f00',
            'noteBkgColor': '#e8f5e8',
            'noteBorderColor': '#4caf50',
            'noteTextColor': '#2e7d32'
        }
    }}%%
```

- **Tạo user mới**: `sudo adduser newuser`, thêm vào group: `sudo usermod -aG sudo newuser`.

---

# **VII. QUẢN LÝ PHẦN MỀM (PACKAGE MANAGER)**

Package manager giúp cài/gỡ phần mềm an toàn. Ubuntu dùng APT, Fedora dùng DNF.

- **Quy trình APT**: Update danh sách → Search → Install → Remove.

- **Ví dụ**: Cài Firefox: `sudo apt install firefox`.

```mermaid
flowchart TD
    subgraph main ["Quy trình làm việc với Trình quản lý gói (APT)"]
        A["Internet"] 
        B["<b>Repositories (Kho chứa phần mềm)</b><br/><i>Các máy chủ chứa hàng ngàn gói phần mềm đã được kiểm duyệt</i>"]
        C["Máy tính Linux của bạn"]
        
        A -- chứa --> B
        B -- "Danh sách phần mềm" --> C
        
        subgraph terminal ["Tương tác của Người dùng qua Terminal"]
            D{Bạn muốn làm gì?}
            
            D -- "Cài một phần mềm mới" --> E1["(1) Cập nhật danh sách gói<br/>sudo apt update"]
            E1 --> E2["(2) Tìm kiếm tên gói<br/>apt search firefox"]
            E2 --> E3["(3) Cài đặt gói<br/>sudo apt install firefox"]
            E3 --> F["✅ Hoàn thành"]
            
            D -- "Gỡ bỏ một phần mềm" --> G1["sudo apt remove firefox"]
            G1 -- "Gỡ cả cấu hình?" --> G2["sudo apt purge firefox"]
            G1 --> F
            G2 --> F

            D -- "Nâng cấp toàn bộ hệ thống" --> H1["sudo apt update"]
            H1 --> H2["sudo apt upgrade"]
            H2 --> F
        end

        C -- "Người dùng ra lệnh" --> D
        
        Note["💡 <b>Lưu ý:</b><br/>- Fedora/CentOS: dnf<br/>- Arch: pacman"]
    end

    style main fill:#f0f8ff,stroke:#69c,stroke-width:2px
    style terminal fill:#fff8dc,stroke:#c96,stroke-width:2px
    style Note fill:#ffffe0,stroke:#cc0,stroke-width:1px
    
    classDef repo fill:#cce5ff,stroke:#4a90e2,stroke-width:1px
    classDef cmd fill:#e8f5e9,stroke:#43a047,stroke-width:1px
    classDef done fill:#dff0d8,stroke:#3c763d,stroke-width:2px
    
    class B repo
    class E1,E2,E3,G1,G2,H1,H2 cmd
    class F done
```

- **Thêm repo**: Chỉnh /etc/apt/sources.list, rồi update.

---

# **VIII. GIÁM SÁT VÀ ĐIỀU KHIỂN TIẾN TRÌNH**

Tiến trình là chương trình đang chạy. PID là ID duy nhất.

- **Giám sát**: `ps aux` (danh sách), `top/htop` (thời gian thực, cài htop nếu cần).

- **Dừng**: `kill PID` (lịch sự), `kill -9 PID` (buộc).

```mermaid
graph TD
    subgraph "Giám sát và Điều khiển Tiến trình"
        
        A["Hệ thống Linux đang chạy nhiều Tiến trình (Processes)"]
        
        A --> B{"Nhiệm vụ của bạn là gì?"}
        
        B -- "Giám sát hệ thống" --> C["Chọn công cụ giám sát"]
        C --> C1["<b>ps aux</b><br/><i>Xem 'ảnh chụp' tất cả tiến trình tại một thời điểm.</i>"]
        C --> C2["<b>top</b> hoặc <b>htop</b><br/><i>Xem các tiến trình và tài nguyên (CPU, RAM) theo thời gian thực.</i>"]
        
        C1 & C2 -- "Kết quả cho thấy" --> D["Danh sách các tiến trình, mỗi tiến trình có một <b>PID (Process ID)</b> duy nhất"]
        D -- "Kết quả cho thấy" --> D["Danh sách các tiến trình, mỗi tiến trình có một <b>PID (Process ID)</b> duy nhất"]
        D -- "Ví dụ: Firefox (PID: 1234) đang bị treo" --> E{"Cần phải dừng tiến trình này"}

        B -- "Điều khiển tiến trình" --> E
        
        E --> F["Sử dụng lệnh <b>kill</b> với PID đã xác định"]
        
        F --> G{Chọn phương pháp dừng}
        G -- "Gửi yêu cầu dừng lịch sự (Mặc định)<br/><i>Cho phép tiến trình dọn dẹp trước khi thoát</i>" --> H1["kill 1234 (Gửi tín hiệu SIGTERM)"]
        G -- "❗️ Buộc dừng ngay lập tức<br/><i>Khi tiến trình không phản hồi yêu cầu dừng</i>" --> H2["kill -9 1234 (Gửi tín hiệu SIGKILL)"]
        
        H1 & H2 --> I["✅ Tiến trình đã được dừng"]
        
        %% Định nghĩa màu sắc
        classDef startNode fill:#e1f5fe,stroke:#0277bd,stroke-width:2px,color:#000
        classDef monitorNode fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
        classDef processNode fill:#e8f5e8,stroke:#388e3c,stroke-width:2px,color:#000
        classDef killNode fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
        classDef danger fill:#ffdede,stroke:#c44,stroke-width:3px,color:#000
        classDef success fill:#e8f5e8,stroke:#4caf50,stroke-width:2px,color:#000
        classDef decision fill:#fff9c4,stroke:#f9a825,stroke-width:2px,color:#000
        
        %% Áp dụng màu sắc
        class A startNode
        class B,E,G decision
        class C,C1,C2 monitorNode
        class D,F processNode
        class H1 killNode
        class H2 danger
        class I success

    end
```

- **Chạy nền**: Thêm & ở cuối lệnh, hoặc dùng `nohup`.

---

# **IX. CÔNG CỤ MẠNG CƠ BẢN**

Kiểm tra kết nối, tải file, truy cập từ xa.

- **Lệnh cơ bản**: `ip addr` (xem IP), `ping google.com` (kiểm tra kết nối), `wget url` (tải file), `curl url` (gửi request), `ssh user@ip` (đăng nhập xa).

```mermaid 
graph LR
    subgraph "Hộp công cụ Mạng trên Linux"
        
        subgraph "Chẩn đoán & Kiểm tra"
            direction TB
            T1("<b>Kiểm tra Cấu hình Mạng Nội bộ</b>") -- Dùng lệnh --> C1("ip address / ifconfig")
            C1 -- "Cho biết" --> R1("Địa chỉ IP, Địa chỉ MAC của bạn")
            
            T2("<b>Kiểm tra Kết nối tới Máy chủ xa</b>") -- Dùng lệnh --> C2("ping google.com")
            C2 -- "Cho biết" --> R2("Máy chủ có phản hồi không?<br/>Độ trễ mạng là bao nhiêu?")
        end
        
        subgraph "Tương tác & Truyền tải Dữ liệu"
            direction TB
            T3("<b>Tải file từ Internet</b>") -- Dùng lệnh --> C3("wget [URL]")
            C3 -- "Kết quả" --> R3("Tải về một file hoặc toàn bộ website")
            
            T4("<b>Truyền tải dữ liệu (Linh hoạt)</b>") -- Dùng lệnh --> C4("curl [URL]")
            C4 -- "Kết quả" --> R4("Hiển thị nội dung, kiểm tra API, tải file...")
        end
        
        subgraph "Truy cập & Điều khiển từ xa"
            direction TB
            T5("<b>Đăng nhập an toàn vào máy khác</b>") -- Dùng lệnh --> C5("ssh user@hostname")
            YourPC["🖥️ Máy của bạn"] -- "ssh aivietnam@server.com" --> RemoteServer["💻 Máy chủ Linux ở xa"]
            C5 -- "Kết quả" --> R5("Bạn có một Terminal đang chạy<br/>trên máy chủ từ xa")
        end
        
    end

    %% Định nghĩa các class màu sắc
    classDef taskNode fill:#e3f2fd,stroke:#1976d2,stroke-width:2px,color:#000,font-weight:bold
    classDef commandNode fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000,font-family:monospace
    classDef resultNode fill:#e8f5e8,stroke:#388e3c,stroke-width:2px,color:#000
    classDef networkCheck fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    classDef dataTransfer fill:#fce4ec,stroke:#c2185b,stroke-width:2px,color:#000
    classDef remoteAccess fill:#e0f2f1,stroke:#00695c,stroke-width:2px,color:#000
    classDef computerNode fill:#fff9c4,stroke:#f9a825,stroke-width:3px,color:#000,font-weight:bold
    
    %% Áp dụng màu sắc cho từng nhóm
    
    %% Nhóm Chẩn đoán & Kiểm tra - Màu tím
    class T1,T2 networkCheck
    class C1,C2 commandNode
    class R1,R2 resultNode
    
    %% Nhóm Tương tác & Truyền tải - Màu hồng
    class T3,T4 dataTransfer
    class C3,C4 commandNode
    class R3,R4 resultNode
    
    %% Nhóm Truy cập từ xa - Màu xanh lá
    class T5 remoteAccess
    class C5 commandNode
    class R5 resultNode
    class YourPC,RemoteServer computerNode
```

- **Cấu hình mạng**: Chỉnh /etc/network/interfaces hoặc dùng nmcli.

---

# **X. SHELL SCRIPTING VÀ TỰ ĐỘNG HÓA**

Shell script là file lệnh để tự động hóa. Bắt đầu bằng #!/bin/bash.

- **Vòng đời**: Ý tưởng → Viết → Cấp quyền (chmod +x) → Chạy.

- **Luồng dữ liệu**: > (redirect), | (pipe), grep/find để tìm kiếm.

```mermaid
%%{init: {'theme':'base'}}%%
timeline
    title Vòng đời của một Shell Script
    
    section Giai đoạn Chuẩn bị
        Ý tưởng 💡 : Tạo script chào hỏi
                   : Hiển thị ngày giờ hệ thống
        
    section Giai đoạn Phát triển  
        Soạn thảo 📝 : nano welcome.sh
                     : Viết nội dung script
        Code Script 💻 : #!/bin/bash
                       : USER_NAME=$(whoami)
                       : echo "Chào mừng, $USER_NAME!"
                       : date
        
    section Giai đoạn Triển khai
        Cấp quyền 🔐 : chmod +x welcome.sh
                     : Cho phép thực thi file
        
    section Giai đoạn Vận hành
        Thực thi ⚡ : ./welcome.sh
                   : Chạy script
        Kết quả 📄 : Chào mừng, aivietnam!
                   : Th 5, 23 thg 5 2024 10-30-00 +07
        Hoàn thành ✅ : Script chạy thành công

```

```mermaid
%%{init: {'theme':'base', 'themeVariables': {'primaryColor': '#FFF9C4', 'primaryTextColor': '#333', 'primaryBorderColor': '#F57F17', 'lineColor': '#666', 'secondaryColor': '#FFF9C4', 'tertiaryColor': '#FFFDE7'}}}%%
graph TD
    subgraph "Luồng dữ liệu trong Shell"

        %% Standard Flow
        subgraph "Luồng Mặc định"
            A["Lệnh ls -l"] -- "Đầu ra chuẩn (stdout)" --> B["Màn hình Terminal"]
        end

        %% Redirection
        subgraph "Chuyển hướng Đầu ra (Redirection)"
            C["Lệnh ls -l"]
            
            C -- "Ghi đè vào file<br/>>" --> D1["ls -l > file_list.txt"]
            D1 --> F1["📄<br/><b>file_list.txt</b><br/><i>Nội dung cũ bị xóa</i>"]

            C -- "Nối vào cuối file<br/>>" --> D2["date >> file_list.txt"]
            D2 --> F2["📄<br/><b>file_list.txt</b><br/><i>Ngày giờ được thêm vào cuối file</i>"]
        end

        %% Piping
        subgraph "Đường ống (Piping) - Kết hợp các lệnh"
            P1["Lệnh 1<br/>ps aux"] -- "stdout (danh sách tiến trình)" --> P_Pipe["<b>|</b>"]
            P_Pipe -- "trở thành stdin của lệnh 2" --> P2["Lệnh 2<br/>grep nginx"]
            P2 -- "stdout (chỉ các dòng chứa nginx)" --> P3["Màn hình Terminal"]
            
            P_Flow("<b>Quy trình:</b><br/>ps aux | grep nginx<br/><i>Liệt kê tất cả tiến trình, sau đó lọc ra những dòng có chứa nginx</i>")
        end
        
    end

    %% Warm Pastel Styling
    classDef commandStyle fill:#FFE0B2,stroke:#FF8A65,stroke-width:2px,color:#D84315
    classDef terminalStyle fill:#E8F5E8,stroke:#66BB6A,stroke-width:2px,color:#2E7D32
    classDef fileStyle fill:#F3E5F5,stroke:#BA68C8,stroke-width:2px,color:#7B1FA2
    classDef pipeStyle fill:#E1F5FE,stroke:#29B6F6,stroke-width:3px,color:#0277BD
    classDef flowStyle fill:#FFF3E0,stroke:#FFB74D,stroke-width:2px,color:#F57C00
    classDef redirectStyle fill:#FCE4EC,stroke:#F06292,stroke-width:2px,color:#C2185B

    class A,C,P1,P2 commandStyle
    class B,P3 terminalStyle
    class F1,F2 fileStyle
    class P_Pipe pipeStyle
    class P_Flow flowStyle
    class D1,D2 redirectStyle
```

```mermaid
graph TD
    subgraph "Chương 15: Cẩm nang Tìm kiếm"
        Start("Tôi cần tìm...") --> Decision{"...một FILE/THƯ MỤC<br/>hay<br/>...NỘI DUNG bên trong file?"}

        Decision -- "Tìm file/thư mục<br/>(dựa trên tên, loại, kích thước, thời gian...)" --> FindCMD["Sử dụng lệnh find"]
        subgraph "Ví dụ với find"
            direction LR
            FindCMD --> F1["Tìm theo tên:<br/>find /home -name '*.log'"]
            FindCMD --> F2["Tìm theo loại (thư mục):<br/>find . -type d -name 'config'"]
            FindCMD --> F3["Tìm file được sửa trong 7 ngày qua:<br/>find . -mtime -7"]
        end

        Decision -- "Tìm một đoạn VĂN BẢN<br/>bên trong file" --> GrepCMD["Sử dụng lệnh grep"]
        subgraph "Ví dụ với grep"
            direction LR
            GrepCMD --> G1["Tìm chữ 'error' trong một file:<br/>grep 'error' /var/log/syslog"]
            GrepCMD --> G2["Tìm không phân biệt hoa/thường (-i):<br/>grep -i 'database' config.ini"]
            GrepCMD --> G3["❗️ Tìm đệ quy trong mọi file (-r):<br/>grep -r 'API_KEY' /var/www/"]
        end
        
        subgraph "Kỹ năng tối thượng: Kết hợp find và grep"
            Combo("Tìm tất cả file .conf có chứa chữ 'port'")
            ComboFlow["find /etc -name '*.conf' -exec grep -H 'port' {} \\;"]
            Combo -- "Thực hiện bằng cách" --> ComboFlow
        end

        FindCMD & GrepCMD --> Combo
        
    end

    %% Colorful Styling with proper yellow background
    classDef startNode fill:#e1f5fe,stroke:#0277bd,stroke-width:3px,color:#000
    classDef decisionNode fill:#fff9c4,stroke:#f9a825,stroke-width:3px,color:#000
    classDef findNode fill:#fff3e0,stroke:#ff9800,stroke-width:3px,color:#000
    classDef grepNode fill:#f3e5f5,stroke:#9c27b0,stroke-width:3px,color:#000
    classDef exampleNode fill:#ffcdd2,stroke:#f44336,stroke-width:2px,color:#000
    classDef comboNode fill:#e8f5e8,stroke:#4caf50,stroke-width:3px,color:#000
    classDef comboFlowNode fill:#e1f5fe,stroke:#03a9f4,stroke-width:2px,color:#000
    classDef importantNode fill:#ffdede,stroke:#c44,stroke-width:3px,color:#000
    
    class Start startNode
    class Decision decisionNode
    class FindCMD findNode
    class GrepCMD grepNode
    class F1,F2,F3 exampleNode
    class G1,G2 exampleNode
    class G3 importantNode
    class Combo comboNode
    class ComboFlow comboFlowNode
```

- **Ví dụ script đơn giản**: Tạo file backup.sh để sao chép file hàng ngày.

---

# **XI. QUẢN LÝ DỊCH VỤ VÀ NÂNG CAO**

Systemd quản lý dịch vụ (như web server).

- **Lệnh**: `systemctl status service`, start/stop/enable/disable.

- **Nén file**: tar cho lưu trữ, gzip/bzip2 cho nén.

```mermaid
stateDiagram-v2
    direction LR
    
    [*] --> Inactive : Gỡ cài đặt / Disable
    
    state "Vòng đời Dịch vụ (Service Lifecycle)" as Lifecycle {
        Inactive: Dịch vụ không chạy và sẽ không tự khởi động
        Active: Dịch vụ đang chạy
        
        Inactive --> Active : systemctl start [service]
        Active --> Inactive : systemctl stop [service]
        
        Active --> active : systemctl restart [service]
        
        state "Trạng thái Bật/Tắt khi khởi động" as BootStatus {
            direction TB
            Enabled: Tự động chạy khi boot máy
            Disabled: Không tự động chạy
            
            Disabled --> Enabled : systemctl enable [service]
            Enabled --> Disabled : systemctl disable [service]
        }
    }

    state "Lệnh kiểm tra quan trọng" as Status {
         [*] --> CheckStatus : systemctl status [service]
         CheckStatus: Xem trạng thái hiện tại (active/inactive),<br/>log gần nhất, và trạng thái enabled/disabled.
    }

    %% Styling to match the yellow background theme
    classDef activeState fill:#e1f5fe,stroke:#0277bd,stroke-width:3px,color:#000
    classDef inactiveState fill:#ffdede,stroke:#c44,stroke-width:2px,color:#000
    classDef enabledState fill:#e8f5e8,stroke:#4caf50,stroke-width:2px,color:#000
    classDef disabledState fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    classDef statusState fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    
    class Active activeState
    class Inactive inactiveState
    class Enabled enabledState
    class Disabled disabledState
    class CheckStatus statusState
```

```mermaid
graph TD
    subgraph "So sánh các định dạng Tar"
        Title["Chọn định dạng phù hợp"]
        
        Title --> Choice{"Yêu cầu của bạn?"}
        
        Choice -- "Tốc độ nhanh<br/>Không cần tiết kiệm dung lượng" --> TarOnly
        Choice -- "Tiết kiệm dung lượng<br/>Truyền qua mạng" --> TarGz
        Choice -- "Nén tối đa<br/>Lưu trữ lâu dài" --> TarBz2
        
        subgraph TarOnly ["📄 .tar - Chỉ đóng gói"]
            TO1["✅ Tạo nhanh nhất"]
            TO2["✅ Giải nén nhanh nhất"]
            TO3["❌ Kích thước lớn"]
            TO4["Lệnh: tar -cvf file.tar folder/"]
        end
        
        subgraph TarGz ["📦 .tar.gz - Nén với gzip"]
            TG1["✅ Cân bằng tốc độ/kích thước"]
            TG2["✅ Phổ biến nhất"]
            TG3["✅ Kích thước vừa phải"]
            TG4["Lệnh: tar -czvf file.tar.gz folder/"]
        end
        
        subgraph TarBz2 ["🗜️ .tar.bz2 - Nén với bzip2"]
            TB1["✅ Kích thước nhỏ nhất"]
            TB2["❌ Tạo chậm nhất"]
            TB3["❌ Giải nén chậm"]
            TB4["Lệnh: tar -cjvf file.tar.bz2 folder/"]
        end
    end

    %% Styling
    classDef titleNode fill:#f3e5f5,stroke:#7b1fa2,stroke-width:3px,color:#000
    classDef choiceNode fill:#fff9c4,stroke:#f9a825,stroke-width:3px,color:#000
    classDef tarNode fill:#e1f5fe,stroke:#0277bd,stroke-width:2px,color:#000
    classDef gzNode fill:#e8f5e8,stroke:#4caf50,stroke-width:2px,color:#000
    classDef bz2Node fill:#fff3e0,stroke:#ff9800,stroke-width:2px,color:#000
    classDef proNode fill:#c8e6c9,stroke:#66bb6a,stroke-width:1px,color:#000
    classDef conNode fill:#ffcdd2,stroke:#f44336,stroke-width:1px,color:#000

    class Title titleNode
    class Choice choiceNode
    class TO4 tarNode
    class TG4 gzNode
    class TB4 bz2Node
    class TO1,TO2,TG1,TG2,TG3,TB1 proNode
    class TO3,TB2,TB3 conNode
```

- **Giải nén**: `tar -xvf file.tar`.

---

# **XII. BẢO MẬT VÀ TROUBLESHOOTING CƠ BẢN**

- **Bảo mật**: Cập nhật thường xuyên, dùng firewall (ufw: `sudo ufw enable`), thay password mạnh, tránh chạy root thường xuyên.

- **Troubleshooting**: Xem log: `journalctl` hoặc /var/log. Lỗi phổ biến: "Permission denied" → dùng sudo. "Command not found" → cài package hoặc kiểm tra PATH.

- **Mẹo**: Sử dụng `df -h` xem dung lượng ổ, `free -h` xem RAM. Nếu hệ thống chậm, kiểm tra tiến trình bằng top.
