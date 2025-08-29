
### **Hướng Dẫn Toàn Diện Và An Toàn Về Quy Trình Cập Nhật Oracle Linux 8**

#### **Giai đoạn 1: Chuẩn Bị và Kiểm Tra Sức Khỏe**

**Bước 1: Cài Đặt Các Công Cụ Hỗ Trợ**
*   **Mục đích:** Đảm bảo bạn có đủ các plugin và công cụ cần thiết cho các bước sau.
*   **Lệnh:**
    ```bash
    sudo dnf install -y dnf-plugins-core python3-dnf-plugin-versionlock lvm2 rsync tar
    ```

**Bước 2: Dọn Dẹp Cache và Làm Mới Thông Tin Repo**
*   **Mục đích:** Xóa bỏ dữ liệu cache cũ có thể gây lỗi và đảm bảo DNF lấy thông tin mới nhất từ các kho lưu trữ.
*   **Lệnh:**
    ```bash
    sudo dnf clean all
    sudo dnf makecache
    ```

**Bước 3: Kiểm Tra "Sức Khỏe" Của Hệ Thống Gói**
*   **Mục đích:** Phát hiện sớm các vấn đề như thiếu phụ thuộc hoặc gói bị trùng lặp có thể làm quá trình cập nhật thất bại.
*   **Lệnh:**
    ```bash
    # Kiểm tra các phụ thuộc chưa được đáp ứng (output phải trống)
    sudo dnf repoquery --unsatisfied

    # Kiểm tra các gói bị cài đặt trùng lặp phiên bản (output phải trống)
    sudo dnf repoquery --duplicates
    
    # Nếu lệnh trên có kết quả, hãy chạy lệnh sau để gỡ bỏ các bản trùng lặp
    sudo dnf remove --duplicates
    ```

**Bước 4: Kiểm Tra Tính Toàn Vẹn Của Các Gói Đã Cài**
*   **Mục đích:** Xác minh xem có file hệ thống nào bị thay đổi hoặc hỏng so với bản gốc hay không.
*   **Lệnh:**
    ```bash
    # Lệnh này sẽ quét toàn bộ hệ thống. Output lý tưởng là không có gì.
    sudo rpm -Va
    ```

**Bước 5: Kiểm Tra Tài Nguyên Hệ Thống**
*   **Mục đích:** Đảm bảo bạn có đủ dung lượng đĩa (đặc biệt là ở `/` và `/boot`) và tài nguyên hệ thống để quá trình cập nhật diễn ra suôn sẻ.
*   **Lệnh:**
    ```bash
    # Kiểm tra dung lượng đĩa
    df -h

    # Kiểm tra bộ nhớ RAM và swap
    free -m
    
    # Giới hạn số lượng kernel được giữ lại để tránh làm đầy /boot
    echo 'installonly_limit=3' | sudo tee -a /etc/dnf/dnf.conf
    ```

---

#### **Giai đoạn 2: Sao Lưu và Lên Kế Hoạch**

**Bước 6: Sao Lưu Toàn Diện**
*   **Mục đích:** Tạo một điểm phục hồi an toàn tuyệt đối trước khi thực hiện bất kỳ thay đổi nào. **Không bao giờ bỏ qua bước này.**

*   **Tùy chọn A: Dùng LVM Snapshot (Nhanh và hiệu quả nhất)**
    ```bash
    # Tạo snapshot với tên có ngày tháng hiện tại
    sudo lvcreate -s -L 10G -n root_snap_$(date +%Y%m%d) /dev/ol/root
    ```
    *Lưu ý: Thay đổi `-L 10G` và `/dev/ol/root` cho phù hợp với hệ thống của bạn.*

*   **Tùy chọn B: Sao Lưu Thủ Công**
    ```bash
    # Tạo thư mục chứa file sao lưu
    BACKUP_DIR=~/backup_$(date +%Y%m%d)
    mkdir -p $BACKUP_DIR

    # Sao lưu toàn bộ thư mục cấu hình /etc
    sudo tar -czvpf $BACKUP_DIR/etc_backup.tar.gz /etc

    # Sao lưu danh sách tất cả các gói đã cài đặt
    rpm -qa | sort > $BACKUP_DIR/installed_packages.txt

    # (Tùy chọn) Sao lưu dữ liệu quan trọng khác, ví dụ /home hoặc /var/www
    sudo tar -czvpf $BACKUP_DIR/home_backup.tar.gz /home
    ```

**Bước 7: Mô Phỏng Quá Trình Cập Nhật (Dry Run)**
*   **Mục đích:** Xem trước chính xác những gì DNF sẽ làm (gói nào sẽ được cài, nâng cấp, gỡ bỏ) mà không thực sự thay đổi hệ thống.
*   **Lệnh:**
    ```bash
    sudo dnf update --assumeno
    ```

---

#### **Giai đoạn 3: Thực Thi**

**Bước 8: (Tùy chọn) Khóa Các Gói Quan Trọng**
*   **Mục đích:** Ngăn không cho một số gói cụ thể (ví dụ như kernel hoặc một phiên bản database) bị cập nhật nếu bạn muốn giữ chúng ở phiên bản hiện tại.
*   **Lệnh:**
    ```bash
    # Ví dụ: Khóa tất cả các gói kernel
    sudo dnf versionlock add kernel*

    # Để xem danh sách các gói đã khóa
    sudo dnf versionlock list
    ```

**Bước 9: Thực Hiện Cập Nhật**
*   **Mục đích:** Áp dụng các bản cập nhật.
*   **Lưu ý quan trọng:** Hãy thực hiện bước này từ **console vật lý (TTY, `Ctrl+Alt+F3`)** thay vì qua SSH để tránh rủi ro mất kết nối mạng giữa chừng.

*   **Tùy chọn A: Cập nhật chỉ các bản vá bảo mật (An toàn nhất cho server)**
    ```bash
    sudo dnf update --security -y
    ```

*   **Tùy chọn B: Cập nhật toàn bộ các gói có phiên bản mới**
    ```bash
    sudo dnf update -y
    ```

---

#### **Giai đoạn 4: Xác Minh và Dọn Dẹp**

**Bước 10: Kiểm Tra Yêu Cầu Khởi Động Lại**
*   **Mục đích:** Xác định xem các thay đổi (đặc biệt là kernel) có yêu cầu khởi động lại hệ thống để có hiệu lực hay không.
*   **Lệnh:**
    ```bash
    sudo dnf needs-restarting -r
    ```    *Nếu lệnh này báo "Reboot is required", bạn cần thực hiện bước tiếp theo.*

**Bước 11: Khởi Động Lại An Toàn (Nếu cần)**
*   **Mục đích:** Áp dụng các cập nhật ở cấp độ kernel.
*   **Lệnh:**
    ```bash
    # Thông báo cho tất cả người dùng đang đăng nhập
    sudo wall "Hệ thống sẽ khởi động lại để áp dụng cập nhật trong 5 phút."
    sleep 300

    # Khởi động lại
    sudo reboot
    ```

**Bước 12: Xác Minh Sau Khi Cập Nhật**
*   **Mục đích:** Đảm bảo hệ thống đã khởi động thành công với kernel mới và các dịch vụ quan trọng đang hoạt động.
*   **Lệnh:**
    ```bash
    # Kiểm tra thời gian hoạt động và phiên bản kernel
    uptime
    uname -r

    # Kiểm tra trạng thái của một dịch vụ quan trọng (ví dụ: sshd, httpd)
    sudo systemctl status sshd
    ```
    *Lưu ý: Hãy kiểm tra các ứng dụng và dịch vụ chính của bạn để đảm bảo chúng vẫn hoạt động bình thường.*

**Bước 13: Dọn Dẹp Hệ Thống**
*   **Mục đích:** Gỡ bỏ các gói phụ thuộc không còn được sử dụng và xóa các file sao lưu/snapshot cũ sau khi đã chắc chắn mọi thứ ổn định.
*   **Lệnh:**
    ```bash
    # Gỡ bỏ các gói mồ côi (orphaned packages)
    sudo dnf autoremove -y

    # (Sau vài ngày) Nếu hệ thống ổn định, hãy xóa snapshot LVM cũ
    sudo lvremove /dev/ol/root_snap_<tendate>
    
    # (Tùy chọn) Gỡ bỏ khóa phiên bản nếu bạn muốn cập nhật chúng trong tương lai
    sudo dnf versionlock delete kernel*
    ```

---

#### **Kế Hoạch B: Quy Trình Phục Hồi (Rollback)**

*   **Chỉ thực hiện khi quá trình cập nhật gây ra lỗi nghiêm trọng.**

*   **Cách 1: Dùng LVM Snapshot (Ưu tiên)**
    *Khởi động từ một Live CD/USB và chạy lệnh sau:*
    ```bash
    sudo lvconvert --merge /dev/ol/root_snap_<tendate>
    ```
    *Sau đó reboot, hệ thống sẽ trở về trạng thái y hệt như lúc bạn tạo snapshot.*

*   **Cách 2: Dùng Lịch Sử DNF**
    ```bash
    # Xem ID của giao dịch cập nhật gần nhất
    sudo dnf history

    # Hoàn tác giao dịch đó (thay <ID> bằng số tương ứng)
    sudo dnf history undo <ID> -y
    ```