- [Mô Hình Master-Slave Trong Cơ Sở Dữ Liệu](#mô-hình-master-slave-trong-cơ-sở-dữ-liệu)
  - [**Giai Đoạn 1 – Khởi Động: Nắm Khái Niệm Cơ Bản**](#giai-đoạn-1--khởi-động-nắm-khái-niệm-cơ-bản)
  - [**Giai Đoạn 2 – Hiểu Sâu Cơ Chế Hoạt Động**](#giai-đoạn-2--hiểu-sâu-cơ-chế-hoạt-động)
  - [**Giai Đoạn 3 – Phân Tích Thành Phần Cốt Lõi**](#giai-đoạn-3--phân-tích-thành-phần-cốt-lõi)
  - [**Giai Đoạn 4 – Đánh Giá Lợi Ích Và Giới Hạn**](#giai-đoạn-4--đánh-giá-lợi-ích-và-giới-hạn)
  - [**Giai Đoạn 5 – Ứng Dụng Thực Tế Và Triển Khai**](#giai-đoạn-5--ứng-dụng-thực-tế-và-triển-khai)


### Mô Hình Master-Slave Trong Cơ Sở Dữ Liệu

#### **Giai Đoạn 1 – Khởi Động: Nắm Khái Niệm Cơ Bản**

**Nội dung phân tích:**  
Mô hình Master-Slave là một kiến trúc replication dữ liệu phổ biến trong cơ sở dữ liệu, nơi một server chính (Master) chịu trách nhiệm xử lý tất cả các hoạt động ghi (write operations như INSERT, UPDATE, DELETE), trong khi các server phụ (Slave) chỉ đọc (read operations như SELECT) và sao chép dữ liệu từ Master để đảm bảo tính nhất quán.  

- **Định nghĩa:** Master-Slave là mô hình phân cấp, nơi Master là "chủ" kiểm soát dữ liệu chính, và Slave là "nô lệ" replicate dữ liệu một chiều từ Master. Điều này giúp phân tải (load balancing) bằng cách đẩy các truy vấn đọc sang Slave, giảm tải cho Master.  
- **Kiến trúc và vai trò:** Master ghi dữ liệu vào binary log (nhật ký thay đổi), Slave kết nối để sao chép và áp dụng thay đổi. Vai trò: Master đảm bảo tính toàn vẹn dữ liệu (ACID), Slave hỗ trợ scalability ngang (horizontal scaling) cho đọc. Đây là mô hình asynchronous replication mặc định, nghĩa là Slave có thể lag một chút so với Master.

**Mermaid sơ đồ:**  
Sử dụng **flowchart** để minh họa quan hệ Master → Slave (hướng một chiều).

```mermaid
flowchart LR
    A[Client Write] --> B[Master Server]
    B --> C[Binary Log]
    C --> D[Slave Server 1]
    C --> E[Slave Server 2]
    F[Client Read] --> D
    F --> E
    
    subgraph "Replication Flow"
        C --> D
        C --> E
    end
    
    %% Styling với nhiều màu
    style A fill:#ff6b6b,stroke:#d63031,stroke-width:2px,color:#fff
    style B fill:#4ecdc4,stroke:#00b894,stroke-width:3px,color:#fff
    style C fill:#45b7d1,stroke:#0984e3,stroke-width:2px,color:#fff
    style D fill:#96ceb4,stroke:#00b894,stroke-width:2px,color:#fff
    style E fill:#ffeaa7,stroke:#fdcb6e,stroke-width:2px,color:#000
    style F fill:#fd79a8,stroke:#e84393,stroke-width:2px,color:#fff
```

---

#### **Giai Đoạn 2 – Hiểu Sâu Cơ Chế Hoạt Động**

**Nội dung phân tích:**  
Cơ chế hoạt động dựa trên replication asynchronous (hoặc semi-synchronous tùy cấu hình). Quy trình chính là: Master ghi thay đổi vào binary log, Slave sử dụng I/O Thread để sao chép log vào relay log cục bộ, rồi SQL Thread áp dụng thay đổi vào database của Slave.  

- **Quy trình replication:**  
  1. Master ghi thay đổi (ví dụ: UPDATE) vào binary log dưới dạng sự kiện (events).  
  2. Slave kết nối Master qua I/O Thread, sao chép binary log vào relay log.  
  3. SQL Thread trên Slave đọc relay log và thực thi SQL để đồng bộ dữ liệu.  
- **Truyền dữ liệu một chiều và cơ chế đồng bộ:** Dữ liệu chỉ chảy từ Master sang Slave (không ngược lại), đảm bảo Master là nguồn chân lý duy nhất. Đồng bộ dựa trên vị trí log (log position), Slave theo dõi để tránh duplicate hoặc miss data. Nếu Slave disconnect, nó sẽ resume từ vị trí cuối cùng.

**Mermaid sơ đồ:**  
- **sequenceDiagram** mô tả tiến trình replication từng bước.

```mermaid
sequenceDiagram
    participant Client
    participant Master
    participant Slave_IO as Slave I/O Thread
    participant Slave_SQL as Slave SQL Thread
    
    activate Client
    Client->>Master: Write Operation (UPDATE/INSERT)
    deactivate Client
    
    activate Master
    Master->>Master: Record in Binary Log
    Master->>Slave_IO: Send Log Events
    deactivate Master
    
    activate Slave_IO
    Slave_IO->>Master: Request Binary Log Events
    Slave_IO->>Slave_IO: Write to Relay Log
    Slave_IO->>Slave_SQL: Relay Log Ready
    deactivate Slave_IO
    
    activate Slave_SQL
    Slave_SQL->>Slave_SQL: Execute SQL on Slave DB
    deactivate Slave_SQL
    
    note over Client,Master: Write Phase
    note over Slave_IO,Slave_SQL: Replication Phase

```

- **stateDiagram-v2** mô tả trạng thái dữ liệu (viết → log → đọc).

```mermaid
stateDiagram-v2
    [*] --> Write_Request: Client sends write
    Write_Request --> Master_DB: Master processes
    Master_DB --> Binary_Log: Log event
    Binary_Log --> Relay_Log: Slave I/O copies
    Relay_Log --> Slave_DB: SQL Thread applies
    Slave_DB --> Read_Response: Client reads from Slave
    Read_Response --> [*]
    
    %% Styling với màu sắc đa dạng
    classDef writePhase fill:#ff6b6b,stroke:#d63031,stroke-width:3px,color:#fff
    classDef masterPhase fill:#4ecdc4,stroke:#00b894,stroke-width:3px,color:#fff
    classDef logPhase fill:#45b7d1,stroke:#0984e3,stroke-width:3px,color:#fff
    classDef relayPhase fill:#96ceb4,stroke:#00b894,stroke-width:3px,color:#fff
    classDef slavePhase fill:#ffeaa7,stroke:#fdcb6e,stroke-width:3px,color:#000
    classDef readPhase fill:#fd79a8,stroke:#e84393,stroke-width:3px,color:#fff
    
    class Write_Request writePhase
    class Master_DB masterPhase
    class Binary_Log logPhase
    class Relay_Log relayPhase
    class Slave_DB slavePhase
    class Read_Response readPhase
```

---

#### **Giai Đoạn 3 – Phân Tích Thành Phần Cốt Lõi**

**Nội dung phân tích:**  
Các thành phần chính trong replication Master-Slave bao gồm:  
- **Binary Log:** Nhật ký nhị phân trên Master, lưu trữ tất cả thay đổi dữ liệu dưới dạng events (không phải SQL raw để tránh vấn đề tương thích).  
- **I/O Thread:** Trên Slave, chịu trách nhiệm kết nối Master, đọc binary log và ghi vào relay log.  
- **Relay Log:** File tạm trên Slave, lưu trữ bản sao của binary log trước khi áp dụng.  
- **SQL Thread:** Trên Slave, đọc relay log và thực thi SQL để cập nhật database.  

Cách phối hợp: Binary Log là nguồn, I/O Thread đảm bảo truyền dữ liệu, Relay Log làm buffer để tránh mất mát, SQL Thread đảm bảo áp dụng nhất quán. Để giữ dữ liệu nhất quán, sử dụng GTID (Global Transaction ID) hoặc log position; nếu có xung đột, cần can thiệp thủ công.

**Mermaid sơ đồ:**  
- **block diagram** (sử dụng classDiagram để mô tả cấu trúc và kết nối, vì Mermaid không có block chính thức nhưng classDiagram phù hợp cho components).

```mermaid
classDiagram
    class Master {
        +Binary Log
        +Dump Thread
        +Process Writes()
    }
    
    class Slave {
        +IO Thread
        +Relay Log  
        +SQL Thread
        +Handle Reads()
    }
    
    class BinaryLog {
        +Event Records
        +Position Tracking
        +Transaction IDs
    }
    
    class RelayLog {
        +Buffered Events
        +Local Storage
        +Sync Status
    }
    
    Master --> Slave : Replication Stream
    Master --> BinaryLog : Writes to
    Slave --> RelayLog : Uses
    
    style Master fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    style Slave fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    style BinaryLog fill:#fff3e0,stroke:#e65100,stroke-width:2px
    style RelayLog fill:#fce4ec,stroke:#c2185b,stroke-width:2px
```

---

#### **Giai Đoạn 4 – Đánh Giá Lợi Ích Và Giới Hạn**

**Nội dung phân tích:**  
- **Ưu điểm:**  
  - Scalability: Dễ mở rộng đọc bằng cách thêm Slave, lý tưởng cho workload read-heavy (ví dụ: 80% read, 20% write).  
  - Backup: Slave có thể dùng làm backup nóng (hot backup) mà không ảnh hưởng Master.  
  - Disaster Recovery: Hỗ trợ failover (thăng cấp Slave thành Master nếu Master fail), giảm downtime.  
- **Nhược điểm:**  
  - Latency: Slave có thể lag (replication delay), dẫn đến dữ liệu không nhất quán tạm thời.  
  - Chỉ chống lỗi cho workload đọc: Không hỗ trợ write scalability; Master là single point of failure. Không phù hợp cho write-heavy hoặc cần high availability write.  

So sánh với Master-Master (multi-master): Master-Slave đơn giản hơn nhưng kém linh hoạt (không hỗ trợ write hai chiều).

**Mermaid sơ đồ:**  
- **pie chart** trực quan lợi ích vs giới hạn (giả sử tỷ lệ dựa trên phân tích chung: 60% lợi ích, 40% giới hạn).

```mermaid
pie title Lợi Ích vs Giới Hạn
    "Scalability" : 30
    "Backup & Recovery" : 30
    "Latency" : 20
    "Single Point Failure" : 20
```

- **quadrantChart** so sánh với Master-Master.

```mermaid
quadrantChart
    title So Sánh Master-Slave vs Master-Master
    x-axis Low Complexity --> High Complexity
    y-axis Low Scalability --> High Scalability
    quadrant-1 High Scalability, High Complexity
    quadrant-2 High Scalability, Low Complexity
    quadrant-3 Low Scalability, Low Complexity
    quadrant-4 Low Scalability, High Complexity
    Master-Slave: [0.3, 0.4]
    Master-Master: [0.7, 0.8]
```

---

#### **Giai Đoạn 5 – Ứng Dụng Thực Tế Và Triển Khai**

**Nội dung phân tích:**  
- **Trường hợp sử dụng thực tế:**  
  - Web app: Như e-commerce (Amazon-style), nơi read (xem sản phẩm) nhiều hơn write (mua hàng).  
  - Hệ thống đọc lớn: Analytics, reporting từ Slave để tránh tải Master.  
  - Backup cluster: Sử dụng Slave cho offsite backup hoặc geo-replication.  
- **Cách triển khai trong MySQL / PostgreSQL:**  
  - **MySQL:** Bật binary log trên Master (`log_bin=ON`), tạo user replication, trên Slave dùng `CHANGE MASTER TO MASTER_HOST='master_ip', MASTER_USER='repl', MASTER_PASSWORD='pass'; START SLAVE;`. Giám sát bằng `SHOW SLAVE STATUS;`.  
  - **PostgreSQL:** Sử dụng streaming replication: Cấu hình `wal_level = replica` trên Master, tạo replication slot, trên Slave dùng `pg_basebackup` rồi chỉnh `primary_conninfo` trong `postgresql.conf` và restart.  

Failover: Nếu Master fail, thăng cấp Slave (dùng `STOP SLAVE; RESET MASTER;`) và redirect traffic.

**Mermaid sơ đồ:**  
- **Flowchart** mô tả triển khai thực tế với các use case:

```mermaid
flowchart LR
    subgraph "Production Environment"
        Master[Master DB<br/>Writes Only]
        
        subgraph "Read Replicas"
            Slave1[Slave 1<br/>Web Traffic]
            Slave2[Slave 2<br/>Analytics]
            Slave3[Slave 3<br/>Backup]
        end
        
        subgraph "Applications"
            WebApp[E-commerce App<br/>Read Heavy]
            Analytics[Analytics System<br/>Complex Queries]
            Backup[Backup Service<br/>Offsite Storage]
        end
    end
    
    Master -->|Binary Log Stream| Slave1
    Master -->|Binary Log Stream| Slave2
    Master -->|Binary Log Stream| Slave3
    
    WebApp -->|Writes| Master
    WebApp -->|Reads| Slave1
    Analytics -->|Heavy Queries| Slave2
    Backup -->|Backup Data| Slave3
    
    style Master fill:#e17055,stroke:#d63031,stroke-width:3px,color:#fff
    style Slave1 fill:#74b9ff,stroke:#0984e3,stroke-width:2px,color:#fff
    style Slave2 fill:#00b894,stroke:#00a085,stroke-width:2px,color:#fff
    style Slave3 fill:#fdcb6e,stroke:#e17055,stroke-width:2px,color:#000
    style WebApp fill:#a29bfe,stroke:#6c5ce7,stroke-width:2px,color:#fff
    style Analytics fill:#fd79a8,stroke:#e84393,stroke-width:2px,color:#fff
    style Backup fill:#81ecec,stroke:#00cec9,stroke-width:2px,color:#000
```

- **Flowchart** cho kịch bản failover:

```mermaid
flowchart TD
    A[Master Failure Detected] --> B[Health Check Failed]
    B --> C[Alert Admin/Auto-Failover]
    C --> D{Choose Best Slave}
    D --> E[Stop Replication on Selected Slave]
    E --> F[Promote Slave to Master]
    F --> G[Redirect Application Traffic]
    G --> H[Reconfigure Other Slaves]
    H --> I[Resume Normal Operations]
    I --> J[Update Documentation]
    
    subgraph "Critical Path"
        D --> E --> F --> G
    end
    
    subgraph "Recovery Phase"
        H --> I --> J
    end
    
    %% Styling với màu sắc phong phú
    style A fill:#ff7675,stroke:#d63031,stroke-width:3px,color:#fff
    style B fill:#fd79a8,stroke:#e84393,stroke-width:2px,color:#fff
    style C fill:#fdcb6e,stroke:#e17055,stroke-width:2px,color:#000
    style D fill:#6c5ce7,stroke:#5f3dc4,stroke-width:2px,color:#fff
    style E fill:#74b9ff,stroke:#0984e3,stroke-width:2px,color:#fff
    style F fill:#00b894,stroke:#00a085,stroke-width:3px,color:#fff
    style G fill:#55a3ff,stroke:#2d3436,stroke-width:2px,color:#fff
    style H fill:#a29bfe,stroke:#6c5ce7,stroke-width:2px,color:#fff
    style I fill:#00cec9,stroke:#00b894,stroke-width:2px,color:#fff
    style J fill:#81ecec,stroke:#00cec9,stroke-width:2px,color:#000
```

- **Flowchart** Kiến trúc triển khai thực tế:

```mermaid
flowchart TB
    subgraph "Application Layer"
        APP1[Web App 1]
        APP2[Web App 2]
        LB[Load Balancer]
    end
    
    subgraph "Database Layer"
        MASTER[Master DB - Write Operations]
        SLAVE1[Slave DB 1 - Read Replica]
        SLAVE2[Slave DB 2 - Read Replica]
        SLAVE3[Slave DB 3 - Analytics]
    end
    
    subgraph "Replication Flow"
        BINLOG[Binary Log]
        RELAY1[Relay Log 1]
        RELAY2[Relay Log 2]
        RELAY3[Relay Log 3]
    end
    
    APP1 --> LB
    APP2 --> LB
    LB -->|Write| MASTER
    LB -->|Read| SLAVE1
    LB -->|Read| SLAVE2
    
    MASTER --> BINLOG
    BINLOG --> RELAY1
    BINLOG --> RELAY2
    BINLOG --> RELAY3
    RELAY1 --> SLAVE1
    RELAY2 --> SLAVE2
    RELAY3 --> SLAVE3
    
    %% Styling với màu sắc
    style MASTER fill:#e17055,stroke:#d63031,stroke-width:4px,color:#fff
    style SLAVE1 fill:#74b9ff,stroke:#0984e3,stroke-width:2px,color:#fff
    style SLAVE2 fill:#81ecec,stroke:#00cec9,stroke-width:2px,color:#fff
    style SLAVE3 fill:#a29bfe,stroke:#6c5ce7,stroke-width:2px,color:#fff
    style LB fill:#fdcb6e,stroke:#e17055,stroke-width:2px,color:#000
    style BINLOG fill:#ff7675,stroke:#d63031,stroke-width:2px,color:#fff
    style APP1 fill:#00b894,stroke:#00a085,stroke-width:2px,color:#fff
    style APP2 fill:#00b894,stroke:#00a085,stroke-width:2px,color:#fff
    style RELAY1 fill:#dda0dd,stroke:#9370db,stroke-width:2px,color:#000
    style RELAY2 fill:#dda0dd,stroke:#9370db,stroke-width:2px,color:#000
    style RELAY3 fill:#dda0dd,stroke:#9370db,stroke-width:2px,color:#000
```