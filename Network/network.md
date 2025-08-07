# Mạng máy tính

# [https://ngockhanhvd2812.github.io/network/](https://ngockhanhvd2812.github.io/network/)

# **1. Sơ đồ luồng hoạt động của IP và Subnet Mask**

## **1.1. Địa chỉ IP (Internet Protocol Address):**

Là "số nhà" của mỗi thiết bị trong mạng, giúp các thiết bị định vị và liên lạc với nhau. Ví dụ, trong sơ đồ, **192.168.1.10** là địa chỉ IP của **Máy tính 1**.

## **1.2. Subnet Mask (Mặt nạ mạng con):**

Không phải địa chỉ, mà là quy tắc cho biết phần nào trong IP là **mạng (Network ID)** và phần nào là **thiết bị (Host ID)**. Nó giúp thiết bị hiểu cách phân chia và định tuyến trong mạng.

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-071412.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-071412.png)

# 2. Default Gateway (Cổng mặc định)

Default Gateway là thiết bị trung gian (thường là Router) có nhiệm vụ chuyển tiếp dữ liệu từ mạng nội bộ ra bên ngoài (Internet hoặc các mạng khác). Có thể hình dung nó như “cổng làng” hoặc “người gác cổng” của hệ thống mạng.

**Giải thích đơn giản:**

- **Giao tiếp trong mạng nội bộ:**
    
    Nếu thiết bị của bạn (ví dụ: 192.168.1.10) muốn liên lạc với một thiết bị khác trong cùng mạng (như 192.168.1.25), dữ liệu sẽ được gửi trực tiếp mà không cần đi qua Gateway.
    
- **Giao tiếp với bên ngoài:**
    
    Khi thiết bị cần truy cập một địa chỉ IP bên ngoài mạng nội bộ (ví dụ: 8.8.8.8), nó sẽ chuyển dữ liệu đến **Default Gateway** (ví dụ: 192.168.1.1). Gateway sau đó sẽ chịu trách nhiệm định tuyến gói tin ra ngoài Internet.
    

**Lưu ý quan trọng:**

Địa chỉ IP của Default Gateway **phải nằm trong cùng mạng** với thiết bị gửi. Nếu không, thiết bị sẽ không thể liên lạc được với Gateway – tức là không thể ra Internet.

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-083418.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-083418.png)

**Khi thiết bị kết nối mạng, có hai câu hỏi quan trọng cần được giải quyết:**

- **Làm sao thiết bị có được địa chỉ IP, Subnet Mask, và Gateway?**
    
    → Câu trả lời là **DHCP**.
    
- **Làm sao thiết bị biết địa chỉ IP tương ứng với tên miền như `google.com`?**
    
    → Câu trả lời là **DNS**.
    

### **2.1. DHCP (Dynamic Host Configuration Protocol) – “Người Phân Phối”**

Là giao thức chịu trách nhiệm **tự động cấp phát thông tin cấu hình mạng** cho thiết bị: IP, Subnet Mask, Default Gateway, DNS... Khi bạn kết nối Wifi hoặc cắm dây mạng, DHCP sẽ gán ngay cho thiết bị một bộ địa chỉ hợp lệ mà không cần bạn thiết lập thủ công.

### **2.2. DNS (Domain Name System) – “Người Thông Dịch”**

Là hệ thống giúp **chuyển đổi tên miền (như `google.com`) thành địa chỉ IP** mà máy tính có thể xử lý. DNS hoạt động như một cuốn danh bạ toàn cầu, giúp thiết bị tìm đúng đích đến mỗi khi bạn truy cập một trang web bằng tên.

# 3. Sơ Đồ Tổng Quan – Quá Trình Thiết Bị Kết Nối Mạng Từ A đến Z

Khi một thiết bị (như laptop, điện thoại) kết nối vào mạng, có **bốn thành phần cốt lõi** cùng phối hợp để giúp thiết bị hoạt động bình thường:

## 3.**1. DHCP (Dynamic Host Configuration Protocol)** – *“Người phát số”*

Ngay khi thiết bị kết nối mạng, **DHCP Server** (thường nằm trong router) sẽ tự động cấp:

- Địa chỉ IP riêng biệt
- Subnet Mask (để xác định "làng" của mình)
- Default Gateway
- Địa chỉ DNS

## 3.**2. IP Address & Subnet Mask** – *“Địa chỉ nhà và bản đồ làng”*

- **IP Address** là định danh của thiết bị trong mạng.
- **Subnet Mask** cho biết thiết bị nào đang ở trong cùng mạng nội bộ để có thể giao tiếp trực tiếp.

## 3.**3. Default Gateway** – *“Người gác cổng làng”*

- Nếu thiết bị cần liên lạc với địa chỉ **bên ngoài mạng nội bộ**, nó sẽ gửi dữ liệu đến Gateway.
- Gateway sẽ chuyển tiếp các gói tin ra Internet và xử lý chiều ngược lại.

## 3.**4. DNS (Domain Name System)** – *“Người phiên dịch”*

- Khi người dùng nhập một tên miền như `google.com`, DNS sẽ **dịch tên đó thành địa chỉ IP** (ví dụ: `8.8.8.8`) để thiết bị có thể kết nối được.

![image.png](image.png)

# **4. DHCP (Dynamic Host Configuration Protocol)**

**Quy trình DORA (4 bước cấp phát địa chỉ IP):**

1. **Discover:** Thiết bị gửi yêu cầu tìm DHCP Server.
2. **Offer:** DHCP Server phản hồi, đề xuất một địa chỉ IP khả dụng.
3. **Request:** Thiết bị gửi yêu cầu chính thức xin sử dụng địa chỉ đó.
4. **Acknowledge:** DHCP Server xác nhận và cung cấp đầy đủ thông tin cấu hình mạng.

**Lợi ích của DHCP:**

- **Tránh xung đột IP:** Đảm bảo mỗi thiết bị được cấp một địa chỉ IP duy nhất.
- **Tự động cấu hình:** Người dùng không cần nhập tay các thông số mạng.
- **Quản lý tập trung:** Quản trị viên có thể cấu hình toàn bộ hệ thống mạng từ một điểm duy nhất.

![image.png](image%201.png)

# **5. DNS (Domain Name System)**

**Quy trình phân giải tên miền:**

1. Khi người dùng nhập một tên miền, máy tính sẽ gửi truy vấn đến **DNS Resolver** gần nhất (được cấp qua DHCP).
2. Nếu Resolver không biết, nó sẽ truy vấn theo chuỗi cấp bậc:
    - **Root Server:** Trả lời ai quản lý tên miền cấp cao `.com`.
    - **TLD Server:** Chỉ ra máy chủ quản lý `google.com`.
    - **Authoritative Server:** Trả về địa chỉ IP chính xác của `google.com`.
3. DNS Resolver lưu kết quả vào bộ nhớ tạm (cache) để tăng tốc cho các lần truy cập sau.

**Tóm lại:**

- **DHCP** cấp quyền truy cập mạng (IP, Gateway, DNS).
- **DNS** giúp bạn "đi lại" trong mạng bằng cách dịch tên miền sang địa chỉ IP.

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-075234.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-075234.png)

# 6. Cơ Chế Giao Tiếp Trong Mạng Nội Bộ: IP, MAC và ARP

Khi một thiết bị tham gia vào mạng, nó được nhận dạng qua hai loại địa chỉ:

- **IP Address (Internet Protocol Address)** – *Địa chỉ lớp 3:*
    
    Là định danh logic, có thể thay đổi theo từng mạng hoặc khi thiết bị di chuyển. Dùng để xác định vị trí của thiết bị trong mạng.
    
- **MAC Address (Media Access Control Address)** – *Địa chỉ lớp 2:*
    
    Là địa chỉ vật lý gắn với card mạng (NIC), được nhà sản xuất gán cố định và là duy nhất trên toàn cầu.
    

> 🔎 Trong mạng nội bộ (cùng subnet), thiết bị không chỉ cần biết IP của thiết bị khác mà còn phải biết MAC để có thể truyền dữ liệu chính xác đến đúng đối tượng.
> 

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-084332.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-084332.png)

## **ARP (Address Resolution Protocol): Cầu nối giữa IP và MAC**

Khi một thiết bị muốn gửi dữ liệu tới một IP trong cùng mạng lần đầu tiên, nó cần xác định MAC tương ứng. Lúc này, giao thức **ARP** sẽ thực hiện chức năng sau:

- **Gửi truy vấn ARP (ARP Request):**
    
    Thiết bị phát một gói tin dạng quảng bá (broadcast) đến toàn mạng, hỏi:
    
    *“Thiết bị nào đang sử dụng IP 192.168.1.25?”*
    
- **Nhận phản hồi ARP (ARP Reply):**
    
    Thiết bị sở hữu IP đó sẽ trả lời:
    
    *“Tôi đang sử dụng địa chỉ đó, đây là MAC của tôi: AA:BB:CC:DD:EE:FF”*
    
- **Lưu vào ARP Cache:**
    
    Máy gửi sẽ lưu thông tin ánh xạ IP–MAC vào bảng ARP để sử dụng cho các lần truyền dữ liệu tiếp theo, giúp giảm tải cho mạng và tăng tốc độ giao tiếp.
    
    **Tóm lại:**
    
    - **IP** giúp thiết bị biết phải gửi đến đâu.
    - **MAC** giúp thiết bị gửi đúng tới ai.
    - **ARP** giúp thiết bị biết "địa chỉ vật lý" (MAC) khi chỉ có "địa chỉ logic" (IP).
    
    ![image.png](image%202.png)
    

# 7. Port – Tổ Chức Giao Tiếp Bên Trong Thiết Bị

**IP là địa chỉ nhà – Port là số phòng**

Nếu máy tính như một **tòa chung cư**:

- **Địa chỉ IP**: Là địa chỉ của cả tòa nhà (ví dụ: `192.168.1.10`).
- **Port (Cổng)**: Là số phòng của từng "căn hộ" bên trong – tức từng ứng dụng hoặc tiến trình đang sử dụng mạng.

> Mỗi ứng dụng đang hoạt động sẽ “ngồi” ở một port riêng, và dữ liệu từ Internet sẽ được gửi đến đúng port tương ứng.
> 

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-090143.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-090143.png)

# **8. Giao Thức Truyền Tải: TCP và UDP**

Khi dữ liệu đã xác định được đích đến (qua IP và Port), câu hỏi tiếp theo là: **Dữ liệu sẽ được gửi như thế nào?**

Lúc này, hai giao thức vận chuyển chính được sử dụng là **TCP** và **UDP** – mỗi loại có đặc điểm riêng và được ứng dụng tùy theo mục đích truyền tải.

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-085906.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-085906.png)

# **9. NAT (Network Address Translation) và NAT Table**

**NAT** là quá trình giúp thiết bị trong mạng nội bộ sử dụng địa chỉ **Private IP** (ví dụ: `192.168.1.10`) có thể **giao tiếp với Internet thông qua một địa chỉ Public IP duy nhất** (ví dụ: `123.45.67.89`), bằng cách chuyển đổi địa chỉ IP nguồn trong gói tin.

## **9.1. Cơ chế hoạt động**

Khi một thiết bị trong mạng nội bộ gửi dữ liệu ra ngoài Internet, quá trình diễn ra như sau:

1. **Thiết bị gửi yêu cầu**, gói tin có IP nguồn là `192.168.1.10`, IP đích là địa chỉ máy chủ ngoài Internet (ví dụ: `8.8.8.8`).
2. **Router thực hiện NAT**, thay địa chỉ IP nguồn `192.168.1.10` bằng địa chỉ công cộng `123.45.67.89`.
3. **Router lưu thông tin ánh xạ** vào một bảng gọi là **NAT Table** (bảng dịch địa chỉ), ví dụ:
    
    `192.168.1.10:54321 → 123.45.67.89:61001`
    
4. **Server ngoài Internet trả lời** về địa chỉ `123.45.67.89:61001`.
5. **Router tra cứu NAT Table**, tìm thấy ánh xạ ban đầu và chuyển gói tin trở lại cho thiết bị `192.168.1.10:54321`.

![image.png](image%203.png)

## **9.2. NAT Table**

là bảng ghi tạm thời được Router sử dụng để theo dõi và ánh xạ các kết nối giữa thiết bị trong mạng nội bộ (Private IP) và thế giới bên ngoài (Public IP). Nhờ vào bảng này, Router có thể phân biệt được gói tin nào thuộc về thiết bị nào, ngay cả khi tất cả thiết bị cùng chia sẻ một địa chỉ IP công cộng.

### **Cách hoạt động của NAT Table**

Khi nhiều thiết bị trong mạng nội bộ cùng truy cập Internet, Router thực hiện:

1. **Chuyển đổi địa chỉ IP nguồn** của từng thiết bị thành địa chỉ Public chung của Router.
2. **Gán mỗi kết nối một cổng (Port) khác nhau**, giúp phân biệt các luồng dữ liệu riêng biệt.
3. **Lưu thông tin ánh xạ** vào NAT Table dưới dạng:

| IP nội bộ | Port nội bộ | IP công cộng | Port công cộng |
| --- | --- | --- | --- |
| 192.168.1.10 | 54321 | 123.45.67.89 | 61001 |
| 192.168.1.20 | 54322 | 123.45.67.89 | 61002 |

> Như vậy, mặc dù cả hai thiết bị cùng sử dụng một địa chỉ công cộng (123.45.67.89), chúng vẫn được phân biệt rõ ràng thông qua cặp địa chỉ IP–Port.
> 

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-092911.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-092911.png)

# **10. Firewall – Tường Lửa**

- Sau khi thiết bị đã được bảo vệ một phần nhờ NAT (ẩn địa chỉ nội bộ sau Router), vẫn còn nguy cơ từ những tấn công trực tiếp vào chính Router hoặc địa chỉ Public. Đó là lúc một thành phần quan trọng khác cần được triển khai: **Firewall (Tường lửa)**.

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-094310.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-094310.png)

- **Firewall** là một hệ thống an ninh mạng, thường được tích hợp trong Router, có nhiệm vụ **kiểm soát lưu lượng ra vào** mạng dựa trên **các bộ quy tắc (rules)** được thiết lập sẵn.
- Firewall xử lý lưu lượng mạng theo các quy tắc cụ thể, thường được cấu hình dựa trên:

| Tiêu chí | Mục đích |
| --- | --- |
| Địa chỉ IP nguồn / đích | Kiểm soát ai được phép truy cập |
| Port | Xác định ứng dụng hoặc dịch vụ liên quan |
| Giao thức (TCP/UDP) | Phân biệt loại kết nối |
| Trạng thái kết nối | Cho phép hoặc từ chối dựa trên lịch sử gói tin |

> 🔑 Nguyên tắc mặc định:
> 
> - **Chiều đi ra (outbound):** Mở khá thoải mái – thiết bị nội bộ được tin tưởng.
> - **Chiều đi vào (inbound):** Rất khắt khe – mặc định **chặn tất cả**, chỉ cho phép các kết nối phản hồi hoặc có “giấy phép” rõ ràng (Port Forwarding).

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-094556.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-094556.png)

- **Port Forwarding** là một kỹ thuật cho phép thiết bị bên ngoài truy cập trực tiếp vào một dịch vụ bên trong mạng nội bộ.

> Khi cấu hình Port Forwarding, Router sẽ nhận gói tin từ Internet trên một cổng (port) cụ thể, và chuyển tiếp nó đến đúng thiết bị nội bộ (với IP và port tương ứng).
> 

![image.png](image%204.png)

### **Tóm lược vai trò của NAT và Firewall**

| Thành phần | Chức năng chính | Loại bảo vệ |
| --- | --- | --- |
| **NAT** | Dịch địa chỉ IP nội bộ → công cộng, và ngược lại | **Ẩn thiết bị nội bộ**, phòng thủ bị động |
| **Firewall** | Kiểm soát kết nối ra/vào theo quy tắc | **Ngăn chặn truy cập trái phép**, phòng thủ chủ động |

Một thiết bị Router hiện đại tại gia thường **tích hợp cả NAT và Firewall**, giúp:

- Giảm khả năng bị tấn công trực tiếp từ Internet
- Quản lý lưu lượng mạng hiệu quả hơn
- Cho phép người dùng mở truy cập ra ngoài khi thật sự cần thiết

# 11. VPN (Virtual Private Network)

## **11.1. Trước khi có VPN**

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-101533.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-101533.png)

## **11.2. Sau khi kết nối VPN**

| Bước | Hành động | Mục đích |
| --- | --- | --- |
| 1. Mã hóa | Bọc dữ liệu lại bằng thuật toán bảo mật | Ngăn bị đọc trộm |
| 2. Gửi qua đường hầm | Truyền dữ liệu qua kênh riêng đến VPN Server | Bảo vệ trong suốt quá trình di chuyển |
| 3. Giải mã tại điểm cuối | Máy chủ VPN giải mã và chuyển tiếp dữ liệu | Truy cập dịch vụ Internet an toàn |
- **Sơ đồ 1:**
    
    ![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-102323.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-102323.png)
    

- **Sơ đồ 2:**

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-102448.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-102448.png)

# **12. Vòng đời của một gói tin**

## **12.1. Giai Đoạn 1: Chuẩn Bị & Tìm "Địa Chỉ" (DHCP & DNS)**

**Mục tiêu:** Trước khi có thể gửi bất cứ thứ gì, điện thoại của Alice phải có một danh tính trên mạng (IP) và phải biết địa chỉ của người nhận (IP máy chủ WhatsApp).

**Kiến thức áp dụng:**

- **DHCP (Mục 4):** Tự động cấp phát địa chỉ IP, Gateway và DNS.
- **DNS (Mục 5):** Chuyển đổi tên miền (whatsapp.com) thành địa chỉ IP.

![image.png](image%205.png)

## **12.2. Giai Đoạn 2: Chuẩn Bị & Tìm "Địa Chỉ" (DHCP & DNS)**

**Mục tiêu:** Tin nhắn đã sẵn sàng, nhưng để gửi nó ra ngoài Internet, trước tiên điện thoại của Alice phải gửi nó đến "cổng làng" (Default Gateway - Router). Để làm điều đó, nó cần biết địa chỉ vật lý (MAC) của Router.

**Kiến thức áp dụng:**

- **Default Gateway (Mục 2):** Cửa ngõ để đi ra khỏi mạng nội bộ.
- **IP và MAC (Mục 6):** Địa chỉ logic (IP) để biết *đi đâu*, địa chỉ vật lý (MAC) để biết giao cho *ai*.
- **ARP (Mục 6):** Giao thức cầu nối, tìm địa chỉ MAC khi đã biết địa chỉ IP.

![image.png](image%206.png)

## **12.3. Giai Đoạn 3: Qua Cổng An Ninh Ra Thế Giới (NAT & Firewall)**

**Mục tiêu:** Gói tin đã đến Router. Bây giờ, Router phải "che giấu" địa chỉ IP nội bộ của Alice và gửi gói tin ra Internet một cách an toàn.

**Kiến thức áp dụng:**

- **NAT (Mục 9):** Dịch địa chỉ IP riêng tư thành IP công cộng. Hoạt động như một "người đại diện" hoặc "người đeo mặt nạ".
- **Firewall (Mục 10):** "Người gác cổng" kiểm tra các gói tin ra/vào, đảm bảo an ninh.

![image.png](image%207.png)

## **12.4. Sơ Đồ Hoàn Chỉnh: Bức Tranh Toàn Cảnh**

![Mermaid Chart - Create complex, visual diagrams with text. A smarter way of creating diagrams.-2025-07-16-110647.png](Mermaid_Chart_-_Create_complex_visual_diagrams_with_text._A_smarter_way_of_creating_diagrams.-2025-07-16-110647.png)

## **12.5. Ứng dụng thực tế: Sơ Đồ Chẩn Đoán Lỗi Mạng**

![Untitled diagram _ Mermaid Chart-2025-07-16-114509.png](Untitled_diagram___Mermaid_Chart-2025-07-16-114509.png)