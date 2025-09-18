- [**Hướng Dẫn Cài Đặt MongoDB Sharded Cluster**](#hướng-dẫn-cài-đặt-mongodb-sharded-cluster)
  - [**Mục Lục Lý Thuyết và Thực Hành**](#mục-lục-lý-thuyết-và-thực-hành)
    - [**Phần I: Lý Thuyết Nền Tảng MongoDB Sharding**](#phần-i-lý-thuyết-nền-tảng-mongodb-sharding)
    - [**Phần II: Thực Hành Triển Khai**](#phần-ii-thực-hành-triển-khai)
  - [**Phần I: Lý Thuyết Nền Tảng MongoDB Sharding**](#phần-i-lý-thuyết-nền-tảng-mongodb-sharding-1)
    - [**Chương 0: Khởi động Nhanh Dành Cho Người Mới**](#chương-0-khởi-động-nhanh-dành-cho-người-mới)
      - [**Bức tranh tổng quan**](#bức-tranh-tổng-quan)
      - [**Thuật ngữ cốt lõi nên thuộc lòng**](#thuật-ngữ-cốt-lõi-nên-thuộc-lòng)
      - [**Một truy vấn đi qua cluster ra sao?**](#một-truy-vấn-đi-qua-cluster-ra-sao)
      - [**Checklist tự học**](#checklist-tự-học)
    - [**Chương 1: Kiến Trúc MongoDB Sharded Cluster**](#chương-1-kiến-trúc-mongodb-sharded-cluster)
      - [**Các Thành Phần Cốt Lõi:**](#các-thành-phần-cốt-lõi)
    - [**Chương 2: Nguyên Lý Phân Tán Dữ liệu (Data Distribution)**](#chương-2-nguyên-lý-phân-tán-dữ-liệu-data-distribution)
      - [**Shard Key - Chìa Khóa Của Sharding**](#shard-key---chìa-khóa-của-sharding)
      - [**Chiến lược phân mảnh phổ biến**](#chiến-lược-phân-mảnh-phổ-biến)
      - [**Vòng đời của một chunk**](#vòng-đời-của-một-chunk)
    - [**Chương 3: Cơ Chế Replica Set và High Availability**](#chương-3-cơ-chế-replica-set-và-high-availability)
      - [**Replica Set Fundamentals**](#replica-set-fundamentals)
    - [**Chương 4: Security Authentication và Authorization Flow**](#chương-4-security-authentication-và-authorization-flow)
      - [**Multi-Layer Security Architecture**](#multi-layer-security-architecture)
    - [**Chương 5: Backup Strategy và Disaster Recovery**](#chương-5-backup-strategy-và-disaster-recovery)
      - [**Comprehensive Backup Strategy**](#comprehensive-backup-strategy)
  - [**Kiến Trúc Triển Khai Cụ Thể**](#kiến-trúc-triển-khai-cụ-thể)
    - [**Sơ đồ lộ trình triển khai**](#sơ-đồ-lộ-trình-triển-khai)
    - [**Giai đoạn 1: Chuẩn bị Môi trường (Làm trên CẢ 3 MÁY)**](#giai-đoạn-1-chuẩn-bị-môi-trường-làm-trên-cả-3-máy)
      - [**1. Cấu hình File `/etc/hosts`**](#1-cấu-hình-file-etchosts)
      - [**2. Tắt Transparent Huge Pages (THP)**](#2-tắt-transparent-huge-pages-thp)
      - [**3. Tinh chỉnh Kernel (`sysctl`) và Giới hạn (`ulimit`)**](#3-tinh-chỉnh-kernel-sysctl-và-giới-hạn-ulimit)
    - [**Giai đoạn 2: Cài đặt và Chuẩn bị Tài nguyên**](#giai-đoạn-2-cài-đặt-và-chuẩn-bị-tài-nguyên)
      - [**1. Cài đặt MongoDB**](#1-cài-đặt-mongodb)
      - [**2. Tạo KeyFile (Xác thực nội bộ)**](#2-tạo-keyfile-xác-thực-nội-bộ)
      - [**3. Tạo Thư mục Dữ liệu và Log**](#3-tạo-thư-mục-dữ-liệu-và-log)
      - [**4. Mở Firewall**](#4-mở-firewall)
    - [**Giai đoạn 3: Dựng Cụm Config Server**](#giai-đoạn-3-dựng-cụm-config-server)
      - [**1. Tạo File Cấu hình (Trên CẢ 3 MÁY)**](#1-tạo-file-cấu-hình-trên-cả-3-máy)
      - [**2. Khởi động Config Server**](#2-khởi-động-config-server)
      - [**3. Khởi tạo Replica Set và Tạo User Admin**](#3-khởi-tạo-replica-set-và-tạo-user-admin)
      - [**4. Bật Xác thực và Khởi động lại**](#4-bật-xác-thực-và-khởi-động-lại)
      - [**4. Bật Xác thực và Khởi động lại**](#4-bật-xác-thực-và-khởi-động-lại-1)
    - [**Giai đoạn 4: Dựng các Cụm Shard**](#giai-đoạn-4-dựng-các-cụm-shard)
      - [**Bước 1: Chuẩn bị File Cấu hình (Bật Security Từ Đầu)**](#bước-1-chuẩn-bị-file-cấu-hình-bật-security-từ-đầu)
      - [**Bước 2: Khởi động các Node**](#bước-2-khởi-động-các-node)
      - [**Bước 3: Khởi tạo Replica Set (Sử dụng Localhost Exception)**](#bước-3-khởi-tạo-replica-set-sử-dụng-localhost-exception)
      - [**Bước 4: Tạo Tài khoản Quản trị trên PRIMARY (Tiếp tục dùng Exception)**](#bước-4-tạo-tài-khoản-quản-trị-trên-primary-tiếp-tục-dùng-exception)
      - [**Bước 5: Hoàn tất và Kiểm tra**](#bước-5-hoàn-tất-và-kiểm-tra)
    - [**Giai đoạn 5: Dựng Mongos và Hoàn thiện Cluster**](#giai-đoạn-5-dựng-mongos-và-hoàn-thiện-cluster)
      - [**1. Tạo File Cấu hình Mongos (Trên 1 máy)**](#1-tạo-file-cấu-hình-mongos-trên-1-máy)
      - [**2. Khởi động Mongos**](#2-khởi-động-mongos)
      - [**3. Thêm các Shard vào Cluster**](#3-thêm-các-shard-vào-cluster)
      - [**4. Kích hoạt Sharding và Test**](#4-kích-hoạt-sharding-và-test)
    - [**Giai đoạn 6: Debug, Kiểm tra và Lưu ý Production**](#giai-đoạn-6-debug-kiểm-tra-và-lưu-ý-production)
    - [**Giai đoạn 7: Thao tác và Truy vấn Dữ liệu (Aggregation Framework)**](#giai-đoạn-7-thao-tác-và-truy-vấn-dữ-liệu-aggregation-framework)
      - [**1. Bài toán 1: Lọc, Sắp xếp và Định hình Dữ liệu**](#1-bài-toán-1-lọc-sắp-xếp-và-định-hình-dữ-liệu)
      - [**2. Bài toán 2: Thống kê và Nhóm Dữ liệu**](#2-bài-toán-2-thống-kê-và-nhóm-dữ-liệu)
      - [**3. Bài toán 3: Join Dữ liệu giữa các Collection**](#3-bài-toán-3-join-dữ-liệu-giữa-các-collection)
    - [**Giai đoạn 8: Quản trị Bảo mật và Người dùng**](#giai-đoạn-8-quản-trị-bảo-mật-và-người-dùng)
      - [**1. Tạo User và Gán Role có sẵn**](#1-tạo-user-và-gán-role-có-sẵn)
      - [**2. Tạo Role Tùy chỉnh (Custom Role)**](#2-tạo-role-tùy-chỉnh-custom-role)
      - [**3. Cấu hình Audit Log (Ghi lại hoạt động)**](#3-cấu-hình-audit-log-ghi-lại-hoạt-động)
    - [**Giai đoạn 9: Quản trị Nâng cao và Vận hành Replica Set**](#giai-đoạn-9-quản-trị-nâng-cao-và-vận-hành-replica-set)
      - [**1. Điều chỉnh Primary (Election)**](#1-điều-chỉnh-primary-election)
      - [**2. Cấu hình Hidden Node**](#2-cấu-hình-hidden-node)
      - [**3. Điều chỉnh Kích thước Oplog**](#3-điều-chỉnh-kích-thước-oplog)
    - [**Giai đoạn 10: Sao lưu, Phục hồi và Giám sát**](#giai-đoạn-10-sao-lưu-phục-hồi-và-giám-sát)
      - [**1. Sao lưu và Phục hồi (`mongodump` / `mongorestore`)**](#1-sao-lưu-và-phục-hồi-mongodump--mongorestore)
      - [**2. Phục hồi tại một thời điểm (Point-in-Time Recovery)**](#2-phục-hồi-tại-một-thời-điểm-point-in-time-recovery)
      - [**3. Giám sát Hiệu năng**](#3-giám-sát-hiệu-năng)
    - [**Giai đoạn 11: Tối ưu Hiệu năng Truy vấn với Index**](#giai-đoạn-11-tối-ưu-hiệu-năng-truy-vấn-với-index)
      - [**1. Các loại Index cơ bản và cách tạo**](#1-các-loại-index-cơ-bản-và-cách-tạo)
      - [**2. Phân tích Kế hoạch Thực thi (Execution Plan)**](#2-phân-tích-kế-hoạch-thực-thi-execution-plan)
    - [**Giai đoạn 12: Chiến lược Sharding và Phân phối Dữ liệu**](#giai-đoạn-12-chiến-lược-sharding-và-phân-phối-dữ-liệu)
      - [**1. Lựa chọn Chiến lược Shard Key**](#1-lựa-chọn-chiến-lược-shard-key)
      - [**2. Bẫy người mới khi chọn Shard Key**](#2-bẫy-người-mới-khi-chọn-shard-key)
      - [**3. Kiểm tra Phân phối Dữ liệu**](#3-kiểm-tra-phân-phối-dữ-liệu)
    - [**Giai đoạn 13: Giám sát Nâng cao và Tự động hóa Vận hành**](#giai-đoạn-13-giám-sát-nâng-cao-và-tự-động-hóa-vận-hành)
      - [**1. Phân tích các Tác vụ đang chạy (`db.currentOp`)**](#1-phân-tích-các-tác-vụ-đang-chạy-dbcurrentop)
      - [**2. "Hạ sát" các Truy vấn Gây hại (`db.killOp`)**](#2-hạ-sát-các-truy-vấn-gây-hại-dbkillop)
      - [**3. Tự động hóa Giám sát Tình trạng Replica Set**](#3-tự-động-hóa-giám-sát-tình-trạng-replica-set)
    - [**Giai đoạn 14: Kỹ thuật Phục hồi Nâng cao - Point-in-Time Recovery (PITR)**](#giai-đoạn-14-kỹ-thuật-phục-hồi-nâng-cao---point-in-time-recovery-pitr)
      - [**Quy trình Cứu dữ liệu khi Xóa nhầm**](#quy-trình-cứu-dữ-liệu-khi-xóa-nhầm)
    - [**Giai đoạn 15: Tinh chỉnh Tầng Lưu trữ (Storage Engine Tuning)**](#giai-đoạn-15-tinh-chỉnh-tầng-lưu-trữ-storage-engine-tuning)
      - [**1. Cấu hình Kích thước WiredTiger Cache**](#1-cấu-hình-kích-thước-wiredtiger-cache)
    - [**Tổng kết và Con đường Tiếp theo**](#tổng-kết-và-con-đường-tiếp-theo)
  - [**Phần III: Vận Hành Production và Best Practices**](#phần-iii-vận-hành-production-và-best-practices)
    - [**Giai đoạn 16: Production Readiness Checklist**](#giai-đoạn-16-production-readiness-checklist)
      - [**1. Security Hardening cho Production**](#1-security-hardening-cho-production)
      - [**2. Systemd Service Files cho Production**](#2-systemd-service-files-cho-production)
      - [**3. Advanced Monitoring với Prometheus và Grafana**](#3-advanced-monitoring-với-prometheus-và-grafana)
    - [**Giai đoạn 17: Advanced Operational Procedures**](#giai-đoạn-17-advanced-operational-procedures)
      - [**1. Zero-Downtime Maintenance Procedures**](#1-zero-downtime-maintenance-procedures)
      - [**2. Disaster Recovery Procedures**](#2-disaster-recovery-procedures)
    - [**Giai đoạn 18: Performance Optimization và Capacity Planning**](#giai-đoạn-18-performance-optimization-và-capacity-planning)
      - [**1. Advanced Performance Monitoring**](#1-advanced-performance-monitoring)
      - [**2. Capacity Planning Guidelines**](#2-capacity-planning-guidelines)
    - [**Giai đoạn 19: Troubleshooting Common Issues**](#giai-đoạn-19-troubleshooting-common-issues)
      - [**1. Balancer Issues**](#1-balancer-issues)
      - [**2. Replica Set Election Issues**](#2-replica-set-election-issues)
    - [**Kết luận: MongoDB Production Excellence**](#kết-luận-mongodb-production-excellence)


# **Hướng Dẫn Cài Đặt MongoDB Sharded Cluster**

## **Mục Lục Lý Thuyết và Thực Hành**

### **Phần I: Lý Thuyết Nền Tảng MongoDB Sharding**
- [**Chương 1: Kiến Trúc MongoDB Sharded Cluster**](#chương-1-kiến-trúc-mongodb-sharded-cluster)
- [**Chương 2: Nguyên Lý Phân Tán Dữ Liệu (Data Distribution)**](#chương-2-nguyên-lý-phân-tán-dữ-liệu-data-distribution)
- [**Chương 3: Cơ Chế Replica Set và High Availability**](#chương-3-cơ-chế-replica-set-và-high-availability)
- [**Chương 4: Security Authentication và Authorization Flow**](#chương-4-security-authentication-và-authorization-flow)
- [**Chương 5: Backup Strategy và Disaster Recovery**](#chương-5-backup-strategy-và-disaster-recovery)

### **Phần II: Thực Hành Triển Khai**

---

## **Phần I: Lý Thuyết Nền Tảng MongoDB Sharding**

### **Chương 0: Khởi động Nhanh Dành Cho Người Mới**

#### **Bức tranh tổng quan**

Trước khi chạm tới lệnh `mongosh`, hãy nhìn toàn cảnh hệ thống: MongoDB thường bắt đầu bằng một replica set để đảm bảo an toàn dữ liệu. Khi kích thước và lưu lượng tăng liên tục, sharding sẽ chia tải sang nhiều nhóm máy thay vì dồn mọi thứ vào một node duy nhất.

- `Replica Set` giải quyết bài toán chịu lỗi và đọc mở rộng nhưng không giúp tháo gỡ điểm nghẽn ghi.
- `Sharding` tách dữ liệu thành từng phần nhỏ (chunk) theo `shard key`, cho phép mở rộng tuyến tính khi thêm shard.
- `Mục tiêu cuối` là cân bằng giữa tính sẵn sàng, hiệu năng và chi phí vận hành.

```mermaid
flowchart TD
    START[Bắt đầu đánh giá hệ thống] --> DATA{Dữ liệu tăng nhanh?}
    DATA -->|Có| WRITE{Áp lực đọc/ghi vượt 70% tài nguyên?}
    WRITE -->|Có| SHARD[Chuẩn bị chiến lược Sharding]
    WRITE -->|Không| SCALE_UP[Tối ưu & Scale Up]
    DATA -->|Không| MONITOR[Tiếp tục giám sát]
    SHARD --> PLAN[Chọn shard key & mô hình triển khai]
    PLAN --> POC[Thử nghiệm trên môi trường staging]
    POC --> PROD[Triển khai production]
    SCALE_UP --> MONITOR
    MONITOR --> START
    style SHARD fill:#ffe0b2,stroke:#fb8c00,stroke-width:2px
    style PLAN fill:#c8e6c9,stroke:#43a047
    style POC fill:#bbdefb,stroke:#1e88e5
    style PROD fill:#f8bbd0,stroke:#c2185b
```

#### **Thuật ngữ cốt lõi nên thuộc lòng**

Năm thuật ngữ dưới đây lặp đi lặp lại xuyên suốt phần thực hành. Hãy chắc rằng bạn nắm được ý nghĩa trước khi bước tiếp.

| Thuật ngữ | Giải thích ngắn | Ghi nhớ nhanh |
| --- | --- | --- |
| `Shard key` | Trường/nhóm trường được dùng để phân chia dữ liệu, ảnh hưởng trực tiếp đến cân bằng tải | Chọn sai sẽ rất khó sửa, hãy thử nghiệm kỹ |
| `Chunk` | Khối dữ liệu logic ~64 MB được balancer di chuyển giữa các shard | Chia nhỏ để cân bằng và tránh hotspot |
| `Mongos` | Bộ định tuyến truy vấn, không lưu dữ liệu | Luôn triển khai ít nhất 2 instance để chịu lỗi |
| `Config Server` | Nơi lưu metadata về chunk, shard, version | Mất cụm config là toàn cluster ngừng hoạt động |
| `Balancer` | Tác vụ nền tự động cân bằng chunk | Chạy định kỳ, có thể tắt khi cần bảo trì |

#### **Một truy vấn đi qua cluster ra sao?**

Hiểu rõ đường đi của một request giúp bạn debug nhanh hơn khi gặp lỗi timeout hoặc kết quả không đồng nhất.

```mermaid
sequenceDiagram
    participant Client as Ứng dụng
    participant Mongos as Mongos (Router)
    participant Config as Config Server RS
    participant ShardA as Shard A
    participant ShardB as Shard B
    Client->>Mongos: Gửi truy vấn {userId: 123}
    Mongos->>Config: Tra cứu metadata shard key
    Config-->>Mongos: Vị trí chunk & phiên bản
    Mongos->>ShardA: Định tuyến truy vấn đọc/ghi
    ShardA-->>Mongos: Kết quả/ACK
    Mongos-->>Client: Trả phản hồi cuối cùng
    Mongos->>ShardB: (Nếu truy vấn toàn cục) gửi thêm pipeline phụ
    ShardB-->>Mongos: Trả về partial result
```

Khi metadata bị lỗi thời, Mongos sẽ đồng bộ lại từ Config Server trước khi tiếp tục; đây là lý do đôi khi bạn thấy độ trễ tăng nhẹ sau các thay đổi lớn.

#### **Checklist tự học**

Để tiến bộ nhanh, hãy xen kẽ đọc tài liệu với thực hành ngắn và nhật ký quan sát.

- Ôn lại sự khác nhau giữa `replication` và `sharding` bằng sơ đồ tự vẽ.
- Hoàn thành ít nhất một lần lab cài đặt sharding trên môi trường ảo hóa cá nhân.
- Luôn chạy `sh.status()` sau mỗi thay đổi để kiểm chứng chunk và zone.
- Theo dõi log của từng thành phần để hiểu rõ trình tự khởi động.

```mermaid
flowchart LR
    READ[Đọc tài liệu chính thức] --> NOTE[Liệt kê khái niệm mới]
    NOTE --> LAB[Triển khai lab nhỏ]
    LAB --> OBSERVE[Quan sát log & metric]
    OBSERVE --> REVIEW[Đánh giá lại thiết kế shard key]
    REVIEW --> READ
    style LAB fill:#c5e1a5,stroke:#558b2f
    style OBSERVE fill:#ffe0b2,stroke:#ef6c00
```


### **Chương 1: Kiến Trúc MongoDB Sharded Cluster**

```mermaid
flowchart TD
    subgraph "Kiến Trúc Khái Niệm"
        APP[Ứng Dụng Client] --> ROUTER[Router Truy Vấn<br/>Mongos]
        ROUTER --> METADATA[Lưu Trữ Metadata<br/>Server Cấu Hình]
        ROUTER --> SHARDS[Lưu Trữ Dữ Liệu<br/>Shards]
        
        subgraph "Thành Phần Cấp Cao"
            METADATA --> CS[Server Cấu Hình<br/>Replica Set]
            SHARDS --> S1[Shard 1<br/>Replica Set]
            SHARDS --> S2[Shard 2<br/>Replica Set]
            SHARDS --> S3[Shard N<br/>Replica Set]
        end
    end
    
    subgraph "Luồng Dữ Liệu"
        WRITE[Thao Tác Ghi] --> ROUTER
        READ[Thao Tác Đọc] --> ROUTER
        ROUTER --> |"Định Tuyến Dựa Trên<br/>Shard Key"| TARGET[Shard Đích]
        TARGET --> RESULT[Kết Quả Thao Tác]
        RESULT --> APP
    end
    
    style APP fill:#e3f2fd
    style ROUTER fill:#fff3e0
    style METADATA fill:#f3e5f5
    style SHARDS fill:#e8f5e8
```

**MongoDB Sharded Cluster** là một kiến trúc phân tán được thiết kế để giải quyết hai thách thức chính trong quản lý cơ sở dữ liệu quy mô lớn:

1. **Horizontal Scaling (Mở rộng theo chiều ngang)**: Thay vì nâng cấp phần cứng của một máy chủ duy nhất (vertical scaling), sharding cho phép phân tán dữ liệu và tải truy vấn trên nhiều máy chủ.

2. **High Availability (Tính sẵn sàng cao)**: Mỗi thành phần trong cluster đều được triển khai dưới dạng Replica Set, đảm bảo không có Single Point of Failure.

#### **Các Thành Phần Cốt Lõi:**

**1. Mongos (Query Router)**
- Là điểm tiếp xúc duy nhất giữa ứng dụng và cluster
- Không lưu trữ dữ liệu, chỉ định tuyến truy vấn
- Phân tích Shard Key để xác định shard đích
- Có thể triển khai nhiều instance để cân bằng tải

```mermaid
graph TD
    subgraph "Thành Phần Mongos"
        A[Tiến Trình Mongos] --> B[Router Truy Vấn]
        A --> C[Pool Kết Nối]
        A --> D[Cache Metadata]
        B --> E[Định Tuyến Shard]
        C --> F[Kết Nối Client]
        D --> G[Đồng Bộ Server Cấu Hình]
    end
    
    style A fill:#fff3e0
    style B fill:#e3f2fd
    style C fill:#f3e5f5
    style D fill:#e8f5e8
```

**2. Config Servers**
- Lưu trữ metadata của toàn bộ cluster
- Quản lý thông tin về chunks, shards, và shard keys
- Luôn triển khai dưới dạng Replica Set (tối thiểu 3 nodes)
- Cực kỳ quan trọng - nếu mất Config Server, toàn bộ cluster sẽ không hoạt động

```mermaid
graph TD
    subgraph "Replica Set Server Cấu Hình"
        A[Server Cấu Hình 1<br/>Primary] <-- "Sao Chép" --> B[Server Cấu Hình 2<br/>Secondary]
        B <-- "Sao Chép" --> C[Server Cấu Hình 3<br/>Secondary]
        C <-- "Sao Chép" --> A
    end
    
    subgraph "Lưu Trữ Metadata"
        A --> D[Metadata Cluster]
        A --> E[Metadata Shard]
        A --> F[Metadata Chunk]
    end
    
    style A fill:#e8f5e8,stroke:#388e3c
    style B fill:#f1f8e9,stroke:#689f38
    style C fill:#f1f8e9,stroke:#689f38
```

**3. Shards**
- Các máy chủ thực sự lưu trữ dữ liệu
- Mỗi shard là một Replica Set độc lập
- Dữ liệu được phân phối dựa trên Shard Key
- Có thể thêm/bớt shard theo nhu cầu

```mermaid
graph TD
    subgraph "Cấu Trúc Replica Set Shard"
        P[Node Primary<br/>Đọc/Ghi] <-- "Sao Chép Oplog" --> S1[Node Secondary<br/>Chỉ Đọc]
        S1 <-- "Sao Chép" --> S2[Node Secondary<br/>Chỉ Đọc]
        S2 <-- "Sao Chép" --> P
    end
    
    subgraph "Phân Bố Dữ Liệu Shard"
        P --> D1[Chunk Dữ Liệu 1]
        P --> D2[Chunk Dữ Liệu 2]
        P --> D3[Chunk Dữ Liệu N]
    end
    
    style P fill:#e8f5e8,stroke:#388e3c,stroke-width:3px
    style S1 fill:#f1f8e9,stroke:#689f38
    style S2 fill:#f1f8e9,stroke:#689f38
```

### **Chương 2: Nguyên Lý Phân Tán Dữ liệu (Data Distribution)**

```mermaid
flowchart TD
    subgraph "Quy Trình Sharding"
        DOC["Tài Liệu Mới<br/>{_id: 'abc', userId: 12345, data: '...'}"] --> SK["Trích Xuất Shard Key<br/>userId: 12345"]
        SK --> HASH["Tính Toán Hash/Range<br/>hash(12345) = 0x7A2B..."]
        HASH --> CHUNK["Xác Định Chunk Đích<br/>Chunk_A: [0x7000... - 0x8000...]"]
        CHUNK --> SHARD["Định Tuyến Đến Shard<br/>Shard02"]
        SHARD --> STORE["Lưu Trữ Tài Liệu"]
    end
    
    subgraph "Quản Lý Chunk"
        SIZE["Giám Sát Kích Thước Chunk"] --> SPLIT{"Kích Thước > 64MB?"}
        SPLIT -->|Có| DIVIDE["Chia Nhỏ Chunk"]
        SPLIT -->|Không| CONTINUE["Tiếp Tục Giám Sát"]
        DIVIDE --> BALANCE["Kích Hoạt Trình Cân Bằng"]
        BALANCE --> MIGRATE["Di Chuyển Chunks"]
    end
    
    style DOC fill:#e3f2fd
    style SHARD fill:#e8f5e8
    style SPLIT fill:#fff3e0
```

#### **Shard Key - Chìa Khóa Của Sharding**

Shard Key là một hoặc nhiều trường trong document được sử dụng để quyết định document đó sẽ được lưu trữ trên shard nào. Đây là quyết định quan trọng nhất khi thiết kế sharded cluster.

```mermaid
graph TD
    subgraph "Quy Trình Chọn Shard Key"
        A[Bộ Sưu Tập Tài Liệu] --> B[Phân Tích Mẫu Truy Vấn]
        B --> C[Xác Định Truy Vấn Thường Xuyên]
        C --> D[Đánh Giá Các Trường Ứng Cử]
        D --> E[Kiểm Tra Tính Phân Giải]
        E --> F[Phân Bố Ghi]
        F --> G[Hiệu Suất Đọc]
        G --> H[Chọn Shard Key]
    end
    
    subgraph "Tiêu Chí Đánh Giá Shard Key"
        I[Tính Phân Giải Cao] --> J[Phân Bố Tốt]
        K[Cập Nhật Ít Khi] --> L[Chunk Ổn Định]
        M[Nhắm Mục Tiêu Truy Vấn] --> N[Định Tuyến Hiệu Quả]
    end
    
    H --> I
    H --> K
    H --> M
    
    style A fill:#e3f2fd
    style H fill:#e8f5e8
    style I fill:#f3e5f5
    style K fill:#fff3e0
    style M fill:#e0f2f1
```

**Các Chiến Lược Shard Key:**

1. **Hashed Sharding**
   - MongoDB tính hash của shard key value
   - Phân phối đều nhất, tránh hotspot
   - Không hiệu quả cho range queries
   - Thích hợp cho write-heavy workloads

```mermaid
graph TD
    subgraph "Quy Trình Sharding Băm"
        A[Giá Trị Shard Key] --> B[Hàm Băm]
        B --> C[Kết Quả Băm]
        C --> D[Ánh Xạ Phạm Vi Chunk]
        D --> E[Shard Đích]
    end
    
    subgraph "Ví Dụ Phân Bố Băm"
        S1[Shard 1<br/>Phạm Vi Băm 0-FFF]
        S2[Shard 2<br/>Phạm Vi Băm 1000-1FFF]
        S3[Shard 3<br/>Phạm Vi Băm 2000-2FFF]
        S4[Shard N<br/>Phạm Vi Băm ...]
    end
    
    style A fill:#e3f2fd
    style E fill:#e8f5e8
    style S1 fill:#f3e5f5
    style S2 fill:#f3e5f5
    style S3 fill:#f3e5f5
    style S4 fill:#f3e5f5
```

2. **Ranged Sharding**
   - Phân chia dựa trên giá trị thực của shard key
   - Hiệu quả cho range queries
   - Có nguy cơ tạo hotspot nếu chọn key không phù hợp
   - Thích hợp khi có query patterns rõ ràng

```mermaid
graph TD
    subgraph "Quy Trình Sharding Phạm Vi"
        A[Giá Trị Shard Key] --> B[So Sánh Phạm Vi]
        B --> C[Ánh Xạ Phạm Vi Chunk]
        C --> D[Shard Đích]
    end
    
    subgraph "Ví Dụ Phân Bố Phạm Vi"
        S1[Shard 1<br/>Giá Trị 1-1000]
        S2[Shard 2<br/>Giá Trị 1001-2000]
        S3[Shard 3<br/>Giá Trị 2001-3000]
        S4[Shard N<br/>Giá Trị ...]
    end
    
    style A fill:#e3f2fd
    style D fill:#e8f5e8
    style S1 fill:#fff3e0
    style S2 fill:#fff3e0
    style S3 fill:#fff3e0
    style S4 fill:#fff3e0
```

**Chunk và Balancing:**
- Dữ liệu được tổ chức thành các chunk (mặc định 64MB)
- Balancer tự động di chuyển chunk giữa các shard
- Quá trình migration diễn ra trong suốt, không ảnh hưởng availability

```mermaid
flowchart TD
    subgraph "Quy Trình Cân Bằng Chunk"
        A[Chèn Dữ Liệu] --> B[Tạo Chunk]
        B --> C[Giám Sát Kích Thước Chunk]
        C --> D{"Kích Thước > 64MB?"}
        D -->|Có| E[Chia Nhỏ Chunk]
        D -->|Không| F[Tiếp Tục Giám Sát]
        E --> G[Kích Hoạt Trình Cân Bằng]
        G --> H[Di Chuyển Chunk]
        H --> I[Shard Đích]
    end
    
    subgraph "Luồng Làm Việc Trình Cân Bằng"
        J[Trình Cân Bằng Khởi Động] --> K[Kiểm Tra Phân Bố]
        K --> L{"Phát Hiện Mất Cân Bằng?"}
        L -->|Có| M[Chọn Chunks]
        L -->|Không| N[Chờ Khoảng Thời Gian]
        M --> O[Di Chuyển Chunks]
        O --> P[Cập Nhật Metadata]
        P --> Q[Di Chuyển Hoàn Tất]
    end
    
    style A fill:#e3f2fd
    style I fill:#e8f5e8
    style J fill:#fff3e0
    style Q fill:#c8e6c9
```


#### **Chiến lược phân mảnh phổ biến**

Việc lựa chọn chiến lược phân mảnh nên dựa trên hành vi truy vấn thực tế thay vì cảm tính. Ba chiến lược dưới đây là nền tảng của mọi cụm MongoDB hiện đại.

```mermaid
graph LR
    KEY[Thiết kế shard key] --> RANGE[Range based]
    KEY --> HASH[Hash based]
    KEY --> ZONE[Zone/Tag based]
    RANGE --> RANGE_PLUS[Truy vấn theo khoảng hiệu quả]
    RANGE --> RANGE_MINUS[Nguy cơ hotspot nếu key tăng tuần tự]
    HASH --> HASH_PLUS[Cân bằng tự nhiên giữa các shard]
    HASH --> HASH_MINUS[Không tối ưu cho truy vấn range]
    ZONE --> ZONE_PLUS[Đặt dữ liệu gần người dùng]
    ZONE --> ZONE_MINUS[Quản trị zone phức tạp hơn]
    style KEY fill:#fff3e0,stroke:#fb8c00,stroke-width:2px
    style RANGE fill:#bbdefb,stroke:#1e88e5
    style HASH fill:#c8e6c9,stroke:#43a047
    style ZONE fill:#f8bbd0,stroke:#c2185b
```

- **Range based**: Phù hợp với dữ liệu thời gian hoặc chuỗi tăng dần; cần kết hợp tiền tố ngẫu nhiên để tránh hotspot.
- **Hash based**: Giúp phân tán đều dữ liệu nhưng làm mất khả năng quét theo khoảng; thường dùng cho workload ghi mạnh.
- **Zone based**: Cho phép gom dữ liệu theo vùng (region, tenant) để tuân thủ quy định pháp lý.

#### **Vòng đời của một chunk**

Chunk có chu kỳ riêng từ lúc sinh ra cho tới khi được di chuyển sang shard khác. Hiểu chu kỳ này giúp bạn giải thích các cảnh báo của balancer.

```mermaid
stateDiagram-v2
    [*] --> Duoc_Tao
    Duoc_Tao: Được tạo sau khi insert đầu tiên
    Duoc_Tao --> Giam_Sat: Theo dõi kích thước chunk
    Giam_Sat --> Bi_Tach: Vượt ngưỡng cấu hình
    Bi_Tach --> Can_Bang: Balancer kích hoạt migrate
    Can_Bang --> On_Dinh: Chunk ổn định ở shard mới
    Can_Bang --> Loi: Migration thất bại
    Loi --> Can_Bang: Retry sau khi khắc phục
    On_Dinh --> Giam_Sat
```

- Luôn kiểm tra `config.locks` và `config.changelog` khi cần điều tra hoạt động của balancer.
- Khi migration thất bại, MongoDB sẽ retry sau thời gian trễ; bạn có thể can thiệp thủ công bằng `moveChunk`.
- Việc thu nhỏ chunk (`mergeChunks`) chỉ nên làm khi chắc chắn shard key hỗ trợ.

### **Chương 3: Cơ Chế Replica Set và High Availability**

```mermaid
sequenceDiagram
    participant C as Client
    participant P as Primary
    participant S1 as Secondary 1
    participant S2 as Secondary 2
    participant A as Arbiter
    
    Note over P,S2: Hoạt Động Bình Thường
    C->>P: Thao Tác Ghi
    P->>S1: Sao Chép qua Oplog
    P->>S2: Sao Chép qua Oplog
    P->>C: Xác Nhận Ghi
    
    Note over P,S2: Tình Huống Primary Hỏng
    P->>X: Primary Hỏng
    S1->>S2: Yêu Cầu Bầu Cử
    S1->>A: Yêu Cầu Bình Chọn
    S2->>S1: Đồng Ý Bình Chọn
    A->>S1: Đồng Ý Bình Chọn
    S1->>S1: Trở Thành Primary Mới
    
    Note over S1,S2: Dịch Vụ Tiếp Tục
    C->>S1: Thao Tác Ghi (đến primary mới)
    S1->>S2: Tiếp Tục Sao Chép
```

#### **Replica Set Fundamentals**

Mỗi shard trong MongoDB cluster đều được triển khai dưới dạng Replica Set để đảm bảo high availability và data durability.

```mermaid
graph TD
    subgraph "Cấu Trúc Replica Set"
        P[Node Primary<br/>Đọc/Ghi] <-- "Sao Chép Oplog" --> S1[Node Secondary<br/>Chỉ Đọc]
        S1 <-- "Sao Chép" --> S2[Node Secondary<br/>Chỉ Đọc]
        S2 <-- "Sao Chép" --> P
        P --> A[Node Arbiter<br/>Chỉ Bình Chọn]
    end
    
    style P fill:#e8f5e8,stroke:#388e3c,stroke-width:3px
    style S1 fill:#f1f8e9,stroke:#689f38
    style S2 fill:#f1f8e9,stroke:#689f38
    style A fill:#fff3e0,stroke:#f57c00
```

**Các Loại Node trong Replica Set:**

1. **Primary Node**
   - Nhận tất cả write operations
   - Có thể phục vụ read operations
   - Duy trì oplog (operations log)
   - Tự động được bầu chọn qua election process

2. **Secondary Nodes**
   - Sao chép dữ liệu từ Primary via oplog
   - Có thể phục vụ read operations (với read preference)
   - Tham gia vào election process
   - Có thể được cấu hình với different priorities

3. **Arbiter Node** (Optional)
   - Chỉ tham gia voting, không lưu trữ dữ liệu
   - Giúp phá tie trong election với số node chẵn
   - Tiết kiệm tài nguyên trong small deployments

```mermaid
graph TD
    subgraph "Replica Set với Arbiter"
        P[Primary<br/>Lưu Trữ Dữ Liệu]
        S[Secondary<br/>Lưu Trữ Dữ Liệu]
        A[Arbiter<br/>Chỉ Bình Chọn]
    end
    
    P <-- "Sao Chép Dữ Liệu" --> S
    P <-- "Heartbeat" --> A
    S <-- "Heartbeat" --> A
    A <-- "Điều Phối Bình Chọn" --> P
    A <-- "Điều Phối Bình Chọn" --> S
    
    style P fill:#e8f5e8,stroke:#388e3c
    style S fill:#f1f8e9,stroke:#689f38
    style A fill:#fff3e0,stroke:#f57c00
```

**Election Process:**
- Heartbeat mechanism giữa các nodes
- Automatic failover khi Primary không response
- Majority voting để bầu Primary mới
- Write concern và read concern để control consistency

```mermaid
flowchart TD
    subgraph "Các Bước Quy Trình Bầu Cử"
        A[Phát Hiện Node Hỏng] --> B[Hết Thời Gian Heartbeat]
        B --> C[Khởi Động Bầu Cử]
        C --> D[Quy Trình Bình Chọn]
        D --> E{"Đa Số Bình Chọn?"}
        E -->|Có| F[Primary Mới Được Bầu]
        E -->|Không| G[Thử Bầu Cử Lại]
        F --> H[Khôi Phục Dịch Vụ]
    end
    
    subgraph "Tiêu Chí Bình Chọn"
        I[Ưu Tiên Node]
        J[Sức Khỏe Node]
        K[Trạng Thái Đồng Bộ Oplog]
        L[Kết Nối Mạng]
    end
    
    style A fill:#e3f2fd
    style F fill:#e8f5e8
    style H fill:#c8e6c9
```

### **Chương 4: Security Authentication và Authorization Flow**

```mermaid
flowchart TD
    subgraph "Quy Trình Xác Thực Client"
        CLIENT[Ứng Dụng Client] --> MONGOS[Router Mongos]
        MONGOS --> |"1. Xác Minh Thông Tin"| AUTHDB[(Cơ Sở Dữ Liệu Xác Thực)]
        AUTHDB --> |"2. Trả Về Thông Tin Người Dùng + Vai Trò"| MONGOS
        MONGOS --> |"3. Ủy Quyền Yêu Cầu"| OPERATION[Thực Hiện Thao Tác]
    end
    
    subgraph "Xác Thực Cluster Nội Bộ"
        MONGOS2[Mongos] --> |"KeyFile/x.509"| CONFIG[Server Cấu Hình]
        CONFIG --> |"KeyFile/x.509"| SHARD1[Shard 1]
        SHARD1 --> |"Xác Thực Replica Set"| SHARD1_SEC[Shard 1 Secondary]
    end
    
    subgraph "Ủy Quyền (RBAC)"
        USER[Người Dùng] --> ROLES[Vai Trò Được Gán]
        ROLES --> PRIVILEGES[Quyền Hạn Vai Trò]
        PRIVILEGES --> RESOURCES[Cơ Sở Dữ Liệu/Collection]
        RESOURCES --> ACTIONS[Hành Động Được Phép]
    end
    
    style CLIENT fill:#e3f2fd
    style AUTHDB fill:#fff3e0
    style ACTIONS fill:#e8f5e8
```

#### **Multi-Layer Security Architecture**

```mermaid
flowchart TD
    subgraph "Mô Hình Bảo Mật Đa Tầng"
        CLIENT[Ứng Dụng Client] --> NETWORK[Tầng Bảo Mật Mạng]
        NETWORK --> AUTH[Tầng Xác Thực]
        AUTH --> AUTHZ[Tầng Ủy Quyền]
        AUTHZ --> AUDIT[Tầng Kiểm Toán]
        AUDIT --> DATA[Dữ Liệu Được Bảo Vệ]
    end
    
    subgraph "Bảo Mật Mạng"
        FIREWALL[Cấu Hình Tường Lửa<br/>Kiểm Soát Truy Cập Cổng]
        SSL[Mã Hóa SSL/TLS<br/>Kết Nối Client]
        INTERNAL[Mã Hóa Nội Bộ<br/>Giao Tiếp Cluster]
    end
    
    subgraph "Phương Pháp Xác Thực"
        SCRAM[SCRAM-SHA-256<br/>Tên Người Dùng/Mật Khẩu]
        X509[Chứng Chỉ x.509<br/>Xác Thực PKI]
        KEYFILE[Xác Thực KeyFile<br/>Cluster Nội Bộ]
    end
    
    subgraph "Cấp Độ Ủy Quyền"
        BUILTIN[Vai Trò Tích Hợp<br/>read, readWrite, dbAdmin]
        CUSTOM[Vai Trò Tùy Chỉnh<br/>Quyền Hạn Chi Tiết]
        RESOURCE[Quyền Truy Cập Tài Nguyên<br/>Cơ Sở Dữ Liệu/Collection]
    end
    
    subgraph "Kiểm Toán & Giám Sát"
        TRAIL[Nhật Ký Kiểm Toán<br/>Sự Kiện Bảo Mật]
        LOGGING[Ghi Nhật Ký<br/>Mẫu Truy Cập]
        COMPLIANCE[Báo Cáo Tuân Thủ<br/>Yêu Cầu Pháp Lý]
    end
    
    NETWORK --> FIREWALL
    NETWORK --> SSL
    NETWORK --> INTERNAL
    
    AUTH --> SCRAM
    AUTH --> X509
    AUTH --> KEYFILE
    
    AUTHZ --> BUILTIN
    AUTHZ --> CUSTOM
    AUTHZ --> RESOURCE
    
    AUDIT --> TRAIL
    AUDIT --> LOGGING
    AUDIT --> COMPLIANCE
    
    style CLIENT fill:#e3f2fd
    style DATA fill:#e8f5e8
    style FIREWALL fill:#fff3e0
    style SCRAM fill:#f3e5f5
    style BUILTIN fill:#e0f2f1
    style TRAIL fill:#fce4ec
```

MongoDB sharded cluster sử dụng mô hình bảo mật đa tầng:

**1. Network Security**
- Firewall configuration cho từng port
- SSL/TLS encryption cho client connections
- Inter-cluster communication encryption

**2. Authentication (Xác thực)**
- **Client Authentication**: SCRAM-SHA-256, x.509 certificates
- **Internal Authentication**: Shared keyFile hoặc x.509 certificates
- **Database Users**: Role-based access control

**3. Authorization (Ủy quyền)**
- **Built-in Roles**: read, readWrite, dbAdmin, userAdmin, etc.
- **Custom Roles**: Fine-grained privileges
- **Resource-specific Permissions**: Database/collection level control

**4. Auditing** (Enterprise Only)
- Comprehensive audit trail
- Security event logging
- Compliance reporting

### **Chương 5: Backup Strategy và Disaster Recovery**

```mermaid
flowchart TD
    subgraph "Chiến Lược Sao Lưu"
        PROD[Dữ Liệu Production] --> FULL["Sao Lưu Toàn Bộ<br/>Hàng ngày qua mongodump"]
        PROD --> OPLOG["Sao Lưu Tăng Dần Oplog<br/>Mỗi 15 phút"]
        PROD --> SNAPSHOT["Snapshot Lưu Trữ<br/>Snapshot LVM/Cloud"]
    end
    
    subgraph "Kịch Bản Phục Hồi"
        DISASTER["Thảm Họa Hoàn Toàn"] --> FULL_RESTORE["Phục Hồi Toàn Bộ<br/>+ Phát lại Oplog Mới Nhất"]
        CORRUPTION["Dữ Liệu Bị Hỏng"] --> PITR["Phục Hồi Theo Thời Gian<br/>Phát lại oplog đến thời điểm"]
        ACCIDENT["Xóa Nhầm"] --> SELECTIVE["Phục Hồi Chọn Lọc<br/>Collection cụ thể"]
    end
    
    subgraph "Xác Minh Phục Hồi"
        RESTORE[Dữ Liệu Đã Phục Hồi] --> VERIFY["Kiểm Tra Tính Toàn Vẹn"]
        VERIFY --> INDEX["Xây Dựng Lại Index"]
        INDEX --> TEST["Kiểm Thử Ứng Dụng"]
        TEST --> PRODUCTION["Trở Lại Production"]
    end
    
    style PROD fill:#e3f2fd
    style PITR fill:#fff3e0
    style PRODUCTION fill:#e8f5e8
```

#### **Comprehensive Backup Strategy**

**1. Full Backups**
- Complete database snapshots using `mongodump`
- Scheduled daily during low-traffic periods
- Stored in multiple locations (local + remote)
- Retention policy based on business requirements

**2. Incremental Backups**
- Oplog backup every 15-30 minutes
- Enables point-in-time recovery
- Smaller storage footprint
- Faster backup process

**3. Storage-Level Snapshots**
- Filesystem or volume snapshots
- Fastest backup and restore
- Requires storage-level consistency
- Ideal for large datasets

**4. Cross-Region Replication**
- Delayed replica sets for disaster recovery
- Geographic distribution of data
- Protection against regional disasters
- Network-level redundancy

---

## **Kiến Trúc Triển Khai Cụ Thể**

### **Sơ đồ lộ trình triển khai**

Phần thực hành dài phía sau sẽ hiệu quả hơn khi bạn hình dung rõ các chặng chính. Sơ đồ dưới đây gom toàn bộ hành trình thành những điểm neo dễ nhớ.

```mermaid
flowchart LR
    STEP1[Giai đoạn 1<br/>Chuẩn bị hệ điều hành] --> STEP2[Giai đoạn 2<br/>Cài đặt gói & dịch vụ]
    STEP2 --> STEP3[Giai đoạn 3<br/>Config Server Replica Set]
    STEP3 --> STEP4[Giai đoạn 4<br/>Shard Replica Set]
    STEP4 --> STEP5[Giai đoạn 5<br/>Mongos & Kết nối]
    STEP5 --> STEP6[Giai đoạn 6-15<br/>Vận hành nâng cao]
    style STEP3 fill:#bbdefb,stroke:#1e88e5
    style STEP4 fill:#c8e6c9,stroke:#43a047
    style STEP5 fill:#ffe0b2,stroke:#fb8c00
```

- Mỗi giai đoạn nên được ghi lại trong nhật ký triển khai để dễ rollback.
- Ưu tiên hoàn thành phòng lab end-to-end trước khi chạm tới môi trường production.
- Khi làm việc nhóm, hãy chia rõ vai trò: người cấu hình hệ điều hành, người dựng replica set, người kiểm thử.


```mermaid
graph TB
    subgraph "Ứng Dụng Client"
        APP1[Ứng Dụng 1]
        APP2[Ứng Dụng 2]
    end
    
    subgraph "Router Truy Vấn"
        MONGOS[Mongos<br/>Cổng 27020]
    end
    
    subgraph "Replica Set Server Cấu Hình"
        CFG1[Cấu Hình 1<br/>Cổng 27010]
        CFG2[Cấu Hình 2<br/>Cổng 27010]
        CFG3[Cấu Hình 3<br/>Cổng 27010]
    end
    
    subgraph "Replica Set Shard 1"
        S1P[Shard1 Primary<br/>Cổng 27011]
        S1S1[Shard1 Secondary<br/>Cổng 27011]
        S1S2[Shard1 Secondary<br/>Cổng 27011]
    end
    
    subgraph "Replica Set Shard 2"
        S2P[Shard2 Primary<br/>Cổng 27012]
        S2S1[Shard2 Secondary<br/>Cổng 27012]
        S2S2[Shard2 Secondary<br/>Cổng 27012]
    end
    
    subgraph "Replica Set Shard 3"
        S3P[Shard3 Primary<br/>Cổng 27013]
        S3S1[Shard3 Secondary<br/>Cổng 27013]
        S3S2[Shard3 Secondary<br/>Cổng 27013]
    end
    
    APP1 --> MONGOS
    APP2 --> MONGOS
    MONGOS --> CFG1
    MONGOS --> CFG2
    MONGOS --> CFG3
    MONGOS --> S1P
    MONGOS --> S2P
    MONGOS --> S3P
    
    CFG1 --- CFG2
    CFG2 --- CFG3
    CFG3 --- CFG1
    
    S1P --- S1S1
    S1S1 --- S1S2
    S1S2 --- S1P
    
    S2P --- S2S1
    S2S1 --- S2S2
    S2S2 --- S2P
    
    S3P --- S3S1
    S3S1 --- S3S2
    S3S2 --- S3P

    classDef clientApp fill:#E3F2FD,stroke:#1976D2,stroke-width:2px,color:#000
    classDef router fill:#FFF3E0,stroke:#F57C00,stroke-width:3px,color:#000
    classDef configServer fill:#F3E5F5,stroke:#7B1FA2,stroke-width:2px,color:#000
    classDef shardPrimary fill:#E8F5E8,stroke:#388E3C,stroke-width:3px,color:#000
    classDef shardSecondary fill:#F1F8E9,stroke:#689F38,stroke-width:2px,color:#000
    
    class APP1,APP2 clientApp
    class MONGOS router
    class CFG1,CFG2,CFG3 configServer
    class S1P,S2P,S3P shardPrimary
    class S1S1,S1S2,S2S1,S2S2,S3S1,S3S2 shardSecondary
```

Bài hướng dẫn này sẽ giúp bạn dựng một cụm MongoDB sharding hoàn chỉnh, tập trung vào việc giải thích bản chất và chỉ ra những "bẫy" mà người mới thường gặp.

**Kiến trúc cuối cùng:**
*   **1 Cụm Config Server Replica Set (3 members)**: "Bộ não" lưu trữ metadata của cluster.
*   **3 Cụm Shard Replica Set (mỗi cụm 3 members)**: Nơi lưu trữ và phân tán dữ liệu.
*   **1 Tiến trình Mongos (Query Router)**: Cổng giao tiếp duy nhất cho ứng dụng.

---

### **Giai đoạn 1: Chuẩn bị Môi trường (Làm trên CẢ 3 MÁY)**

```mermaid
flowchart TD
    A["Bắt đầu thiết lập môi trường"] --> B["Cấu hình /etc/hosts"]
    B --> B1["Thiết lập IP addresses:<br/>mongo-1: 192.168.0.38<br/>mongo-2: 192.168.0.241<br/>mongo-3: 192.168.0.215"]
    B1 --> C["Đặt hostname duy nhất"]
    C --> D["Tắt Transparent Huge Pages"]
    D --> E["Cấu hình tham số sysctl"]
    E --> F["Thiết lập giá trị ulimit"]
    F --> G["Xử lý SELinux contexts"]
    G --> H["Môi trường sẵn sàng"]
    
    %% Định nghĩa style classes
    classDef startNode fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    classDef configNode fill:#e3f2fd,stroke:#1976d2,stroke-width:2px,color:#000
    classDef detailNode fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    classDef systemNode fill:#fff8e1,stroke:#ffa000,stroke-width:2px,color:#000
    classDef endNode fill:#e8f5e8,stroke:#388e3c,stroke-width:3px,color:#000
    
    %% Áp dụng styles
    class A startNode
    class B,C configNode
    class B1 detailNode
    class D,E,F,G systemNode
    class H endNode
```

Đây là bước nền tảng quyết định sự ổn định của cả hệ thống.

#### **1. Cấu hình File `/etc/hosts`**

*   **Mục đích cốt lõi:** Việc sử dụng các hostname dễ nhớ (ví dụ: `mongo-1`) thay vì địa chỉ IP trực tiếp (ví dụ: `192.168.0.38`) mang lại nhiều lợi ích quan trọng, đặc biệt trong một môi trường phân tán như MongoDB sharded cluster:
    *   **Dễ đọc và quản lý:** Cấu hình trở nên trực quan hơn rất nhiều. Sẽ dễ dàng hơn để nhớ và tham chiếu đến `mongo-1` trong các file cấu hình hoặc khi gõ lệnh, thay vì một dãy số IP.
    *   **Tính nhất quán trong giao tiếp:** Trong các hệ thống phân tán, các thành phần thường xuyên cần giao tiếp với nhau bằng cách gọi tên. Nếu các node MongoDB trong cluster sử dụng hostname, chúng có thể tự nhận diện và tìm thấy nhau một cách đáng tin cậy.
    *   **Giảm thiểu lỗi cấu hình:** Khi địa chỉ IP thay đổi (mặc dù không mong muốn trong production, nhưng có thể xảy ra trong môi trường lab hoặc phát triển), bạn chỉ cần cập nhật một lần trong file `/etc/hosts` thay vì phải tìm kiếm và sửa đổi nhiều file cấu hình của MongoDB.
    *   **Tránh "name resolution lộn xộn":** Nếu mỗi máy có một ánh xạ IP-hostname khác nhau hoặc không đầy đủ, các node sẽ không thể xác định chính xác các node khác trong cluster. Điều này dẫn đến các lỗi khó gỡ rối như "node không tìm thấy", "replica set không thể bầu chọn primary", hoặc "mongos không thể kết nối đến config server", gây mất ổn định toàn bộ cluster.

*   **Hostname của hệ thống:** Bên cạnh việc ánh xạ IP sang hostname trong `/etc/hosts`, mỗi máy chủ cũng cần có một hostname *duy nhất* của riêng nó. Hostname này là cách mà hệ điều hành và các ứng dụng (bao gồm MongoDB) tự nhận diện mình trong mạng. Trong một replica set hoặc sharded cluster, mỗi thành viên phải có một định danh duy nhất để tránh xung đột và cho phép các thuật toán bầu chọn hoạt động chính xác.

⚠️ **BẪY NGƯỜI MỚI - Giai đoạn 1:**
-   **Hosts file không đồng nhất giữa các máy:** Đây là lỗi phổ biến nhất. Nếu `mongo-1` biết `mongo-2` là `192.168.0.241`, nhưng `mongo-2` lại nghĩ `mongo-1` là một IP khác hoặc không tìm thấy, chúng sẽ không thể thiết lập kết nối nội bộ. Kết quả là "name resolution lộn xộn" và cluster không thể khởi tạo hoặc hoạt động đúng.
-   **Hostname trùng/đổi hostname nhưng không reboot:** Khi bạn đặt một hostname mới cho máy bằng `hostnamectl`, một số ứng dụng hoặc dịch vụ (bao gồm MongoDB) có thể không nhận ra sự thay đổi ngay lập tức mà vẫn sử dụng hostname cũ cho đến khi chúng được khởi động lại hoặc hệ thống được reboot. Nếu hai máy trong cluster vô tình có cùng một hostname (hoặc một máy vẫn sử dụng hostname cũ trùng với máy khác), MongoDB sẽ bị nhầm lẫn và không thể quản lý các thành viên của replica set.
-   **Quên kiểm tra `/etc/hosts` trên TẤT CẢ máy:** Việc chỉ kiểm tra trên một hoặc hai máy có thể dẫn đến một máy bị cô lập, không thể resolve được các máy khác, gây ra lỗi kết nối và sự cố trong cluster.

*   **Thực hiện đúng:**
    1.  **Mở file `/etc/hosts`:** Sử dụng `vi /etc/hosts` (hoặc trình soạn thảo yêu thích) để chỉnh sửa file này.
    2.  **Thêm các dòng ánh xạ:** Thêm danh sách các cặp IP-hostname cho *tất cả các node* trong cluster vào cuối file. **Điểm mấu chốt là nội dung của file `/etc/hosts` trên CẢ 3 MÁY PHẢI GIỐNG HỆT NHAU.** Điều này đảm bảo mỗi node đều có một "bản đồ" mạng nhất quán và chính xác về tất cả các node khác.
        ```
        # --- Mongo Cluster ---
        192.168.0.38   mongo-1
        192.168.0.241  mongo-2
        192.168.0.215  mongo-3
        ```
    3.  **Đặt hostname duy nhất cho từng máy:** Trên mỗi máy, bạn sẽ chạy lệnh `hostnamectl set-hostname <tên-hostname>` tương ứng.
        *   `hostnamectl set-hostname mongo-1` (Trên máy có IP 192.168.0.38)
        *   `hostnamectl set-hostname mongo-2` (Trên máy có IP 192.168.0.241)
        *   `hostnamectl set-hostname mongo-3` (Trên máy có IP 192.168.0.215)
        Thao tác này đảm bảo rằng mỗi máy tự nhận diện mình với một cái tên riêng biệt và nhất quán với những gì đã định nghĩa trong `/etc/hosts`. Sau khi đặt hostname, tốt nhất nên khởi động lại hoặc ít nhất đăng xuất/đăng nhập lại để đảm bảo tất cả các dịch vụ nhận hostname mới.

* **Minh hoạ:**

```mermaid
graph TD
    subgraph "Hệ thống"
        M1["mongo-1 (192.168.0.38)"]
        M2["mongo-2 (192.168.0.241)"]
        M3["mongo-3 (192.168.0.215)"]
    end

    subgraph "File /etc/hosts trên M1"
        H1["192.168.0.38 mongo-1<br>192.168.0.241 mongo-2<br>192.168.0.215 mongo-3"]
    end

    subgraph "File /etc/hosts trên M2"
        H2["192.168.0.38 mongo-1<br>192.168.0.241 mongo-2<br>192.168.0.215 mongo-3"]
    end

    subgraph "File /etc/hosts trên M3"
        H3["192.168.0.38 mongo-1<br>192.168.0.241 mongo-2<br>192.168.0.215 mongo-3"]
    end

    M1 -- "Đọc file hosts" --> H1
    M2 -- "Đọc file hosts" --> H2
    M3 -- "Đọc file hosts" --> H3

    M1 -.->|ping mongo-2| M2
    M2 -.->|ping mongo-3| M3
    M3 -.->|ping mongo-1| M1

    style H1 fill:#c8e6c9,stroke:#388e3c
    style H2 fill:#c8e6c9,stroke:#388e3c
    style H3 fill:#c8e6c9,stroke:#388e3c
```

```mermaid
flowchart TD
    A[Bắt đầu: Cấu hình /etc/hosts và Hostname] --> B{Mục đích:<br/>Dùng Hostname dễ nhớ thay vì IP<br/>Giúp cấu hình rõ ràng, quản lý dễ dàng<br/>và giao tiếp nhất quán cho MongoDB Cluster};
    
    subgraph "Trên TẤT CẢ 3 MÁY"
        IP1(192.168.0.38)
        IP2(192.168.0.241)
        IP3(192.168.0.215)
        
        S1["Mở và Sửa file /etc/hosts"]
        S1 --> S2["Thêm ánh xạ IP <-> Hostname:<br/>(Nội dung file phải GIỐNG HỆT NHAU trên CẢ 3 MÁY)"];
        S2 --> HostEntries{<br/># --- Mongo Cluster ---<br/>192.168.0.38 mongo-1<br/>192.168.0.241 mongo-2<br/>192.168.0.215 mongo-3<br/>}
    end
    
    subgraph "Trên TỪNG MÁY RIÊNG BIỆT"
        M1["Máy 1 (192.168.0.38):<br/>hostnamectl set-hostname mongo-1"]
        M2["Máy 2 (192.168.0.241):<br/>hostnamectl set-hostname mongo-2"]
        M3["Máy 3 (192.168.0.215):<br/>hostnamectl set-hostname mongo-3"]
    end
    
    B --> S1
    HostEntries --> M1
    HostEntries --> M2
    HostEntries --> M3
    
    subgraph "⚠️ BẪY NGƯỜI MỚI (Cần tránh)"
        P1["Hosts file KHÔNG ĐỒNG NHẤT"];
        P2["Hostname TRÙNG/Không reboot sau đổi"];
        P3["Quên kiểm tra trên TẤT CẢ máy"];
    end
    
    M1 --> F[Cluster sẵn sàng giao tiếp bằng Hostname];
    M2 --> F;
    M3 --> F;
    
    %% Định nghĩa style classes
    classDef startNode fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    classDef purposeNode fill:#e3f2fd,stroke:#1976d2,stroke-width:2px,color:#000
    classDef sharedConfig fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    classDef uniqueConfig fill:#e0f2f1,stroke:#00695c,stroke-width:2px,color:#000
    classDef resultNode fill:#e8f5e8,stroke:#388e3c,stroke-width:3px,color:#000
    classDef trapNode fill:#ffcdd2,stroke:#c62828,stroke-width:2px,color:#000
    
    %% Áp dụng styles
    class A startNode
    class B purposeNode
    class S1,S2,HostEntries sharedConfig
    class M1,M2,M3 uniqueConfig
    class F resultNode
    class P1,P2,P3 trapNode
```

#### **2. Tắt Transparent Huge Pages (THP)**

*   **Mục đích cốt lõi:** THP gây sụt giảm hiệu năng nghiêm trọng cho MongoDB. Phải tắt vĩnh viễn để đảm bảo hiệu suất ổn định và tối ưu cho cơ sở dữ liệu.

*   **THP là gì và tại sao nó tồn tại?**
    *   **Transparent Huge Pages (THP)** là một tính năng của kernel Linux, được thiết kế để tăng hiệu năng bằng cách cho phép các ứng dụng sử dụng các "trang bộ nhớ lớn" (huge pages, thường là 2MB thay vì 4KB mặc định) một cách tự động và minh bạch.
    *   Mục đích của THP là giảm số lượng các entry trong bảng chuyển đổi địa chỉ (Translation Lookaside Buffer - TLB), từ đó tăng tốc độ truy cập bộ nhớ cho các ứng dụng sử dụng lượng lớn bộ nhớ một cách tuần tự hoặc có cấu trúc rất lớn.

*   **Tại sao THP lại gây hại cho MongoDB?**
    *   **Xung đột với cách quản lý bộ nhớ của MongoDB (WiredTiger):** MongoDB, đặc biệt là với storage engine WiredTiger, có cơ chế quản lý bộ nhớ của riêng nó rất hiệu quả. WiredTiger chủ động ánh xạ (map) và giải ánh xạ (unmap) các vùng bộ nhớ nhỏ, đồng thời duy trì các cấu trúc dữ liệu của riêng mình trong RAM (ví dụ: WiredTiger cache).
    *   **Phân mảnh bộ nhớ và độ trễ cao:** Khi THP được bật, nó sẽ cố gắng gom các trang bộ nhớ 4KB nhỏ của WiredTiger thành các trang 2MB lớn. Quá trình này có thể dẫn đến các vấn đề nghiêm trọng:
        *   **Tăng độ trễ (Latency Spikes):** Việc cấp phát hoặc giải phóng các huge pages 2MB đòi hỏi nhiều tài nguyên hơn và có thể bị đình trệ. Điều này gây ra "stop-the-world" pauses (tạm dừng toàn bộ hoạt động) trong micro giây hoặc mili giây, gây ra các đỉnh độ trễ đột biến mà MongoDB rất nhạy cảm.
        *   **Hiệu quả cache kém:** Các hoạt động I/O ngẫu nhiên và nhỏ của MongoDB không được hưởng lợi từ huge pages mà thậm chí còn bị cản trở. Việc ghi đè lên các trang bộ nhớ lớn có thể làm giảm hiệu quả của cả cache nội bộ của WiredTiger và cache của hệ điều hành.
        *   **Khó khăn trong quản lý bộ nhớ:** Khi kernel cố gắng duy trì các huge pages, nó có thể gặp khó khăn trong việc tìm kiếm các khối bộ nhớ lớn liên tục, dẫn đến tăng việc sử dụng swap không cần thiết hoặc gây áp lực lên bộ nhớ.
    *   **Tóm lại:** Mặc dù THP có lợi cho một số ứng dụng khoa học hoặc tính toán hiệu năng cao, nhưng đối với các cơ sở dữ liệu như MongoDB, vốn có mô hình truy cập bộ nhớ rất đặc thù và yêu cầu độ trễ thấp, THP lại trở thành một tác nhân gây cản trở hiệu năng nghiêm trọng và khó lường.

*   **Tại sao phải tắt vĩnh viễn?**
    *   Nếu chỉ tắt THP thủ công bằng các lệnh `echo never` trong shell, nó sẽ chỉ có hiệu lực cho đến khi hệ thống khởi động lại.
    *   MongoDB cần môi trường ổn định liên tục, do đó, THP phải được tắt một cách vĩnh viễn và tự động sau mỗi lần khởi động lại hệ thống, thường là thông qua một `systemd service` hoặc cấu hình `grub`. Phương pháp `systemd service` được đề xuất trong hướng dẫn là cách hiệu quả và đáng tin cậy.

⚠️ **BẪY NGƯỜI MỚI - Giai đoạn 1:**
-   **Tắt THP thủ công, quên daemonize** → reboot xong THP bật lại, các vấn đề hiệu năng sẽ quay trở lại.
-   **Service file sai cấu hình Before/After** → không đảm bảo thứ tự khởi động. Nếu service tắt THP chạy sau `mongod`, thì `mongod` có thể đã khởi động với THP bật và bị ảnh hưởng.
-   **Không test lại sau reboot** → tưởng đã tắt nhưng thực tế vẫn bật. Luôn cần kiểm tra `cat /sys/kernel/mm/transparent_hugepage/enabled` sau khi hệ thống khởi động lại để xác nhận `[never]`.

💡 **MẸO:** Sau khi tạo service, luôn reboot và kiểm tra `cat /sys/kernel/mm/transparent_hugepage/enabled` phải có `[never]`.
*   **Thực hiện đúng:**

    Sơ đồ dưới đây minh họa cách `systemd` đảm bảo service `disable-transparent-huge-pages` luôn chạy trước `mongod`, tạo ra một môi trường khởi động an toàn và tối ưu.

    ```mermaid
    flowchart TD
        A[System Boot] --> B[systemd starts services]
        B --> C["disable-transparent-huge-pages.service<br/>(runs because of 'Before=mongod.service')"]
        C --> D["mongod.service<br/>(starts after THP is disabled)"]
        D --> E[MongoDB runs in an optimized environment]

        subgraph "Verification"
            F["cat /sys/kernel/mm/transparent_hugepage/enabled"] --> G["Output contains '[never]'"]
        end

        style C fill:#e8f5e8,stroke:#388e3c
        style D fill:#e3f2fd,stroke:#1976d2
        style E fill:#c8e6c9
    ```

    1.  Tạo file service: `vi /etc/systemd/system/disable-transparent-huge-pages.service`

    2.  Dán nội dung chính xác sau:
    
```bash
[Unit]
# Mô tả service
Description=Disable Transparent Huge Pages (THP)

# Không phụ thuộc mặc định khác, để service chạy sớm trong quá trình boot
DefaultDependencies=no

# Chạy sau khi hệ thống khởi tạo cơ bản xong (sysinit + local fs)
After=sysinit.target local-fs.target

# Đảm bảo service này chạy trước khi mongod khởi động
Before=mongod.service

[Service]
# Chạy một lần duy nhất rồi thoát
Type=oneshot

# Lệnh tắt THP (chỉ disable "enabled")
# Nếu cần tắt cả "defrag" thì nên thêm check file defrag và echo never
ExecStart=/bin/sh -c 'echo never | tee /sys/kernel/mm/transparent_hugepage/enabled > /dev/null'

[Install]
# Service sẽ được kéo vào basic.target (chạy rất sớm khi boot)
# Thường production hay dùng multi-user.target để quản lý dễ hơn
WantedBy=basic.target
```

3.  Kích hoạt service:
```bash
# Nạp lại toàn bộ unit files từ disk vào memory của systemd
# Bắt buộc khi bạn vừa tạo mới hoặc sửa file .service
systemctl daemon-reload

# Khởi động service ngay lập tức (chỉ chạy cho lần boot hiện tại)
systemctl start disable-transparent-huge-pages

# Bật service để tự động chạy lại khi reboot
systemctl enable disable-transparent-huge-pages

# 👆 Nếu muốn gọn, có thể gộp start + enable bằng:
# systemctl enable --now disable-transparent-huge-pages

```
4.  Kiểm tra: `cat /sys/kernel/mm/transparent_hugepage/enabled` phải có `[never]`.

5.  (Tuỳ chọn) Kiểm tra trạng thái của  disable-transparent-huge-pages để đọc log khởi chạy
```bash
# Kiểm tra trạng thái service disable-transparent-huge-pages
# - Cho biết service đang "active" (đang chạy thành công) hay "failed"
# - Hiển thị log khởi chạy từ journalctl (giúp debug khi lỗi)
# - Dùng sau khi start/enable để chắc chắn service chạy đúng
systemctl status disable-transparent-huge-pages
``` 

📌 Khi chạy bạn sẽ thấy:

* `Active: active (exited)` → nghĩa là service chạy 1 lần thành công rồi thoát (Type=oneshot).
* Nếu có lỗi (ví dụ file không tồn tại) → bạn sẽ thấy `failed` và log lỗi ngay dưới.  
   
* **Minh hoạ**:

```mermaid
flowchart TD
    A["🚀 Bắt đầu: Tắt Transparent Huge Pages THP"] --> B{"🎯 Mục đích:<br/>Tránh sụt giảm hiệu năng<br/>nghiêm trọng cho MongoDB"}
    
    subgraph "📋 Các Bước Thực Hiện"
        B --> C["📄 Bước 1: Tạo file service<br/>/etc/systemd/system/disable-transparent-huge-pages.service"]
        C --> D["✏️ Bước 2: Dán nội dung cấu hình service<br/>ExecStart: echo never tee các đường dẫn THP"]
        D --> E["⚙️ Bước 3: Kích hoạt service"]
        E --> E1["🔄 systemctl daemon-reload"]
        E1 --> E2["▶️ systemctl start disable-transparent-huge-pages"]
        E2 --> E3["🔗 systemctl enable disable-transparent-huge-pages"]
    end
    
    E3 --> F["🔍 Bước 4: Kiểm tra trạng thái THP<br/>cat /sys/kernel/mm/transparent_hugepage/enabled"]
    F --> G{"✅ Kết quả mong muốn:<br/>always madvise [never]"}
    
    G -->|"Thành công"| H["🎉 THP đã tắt vĩnh viễn và chính xác"]
    G -->|"Thất bại"| I["❌ Cần khắc phục lỗi"]
    
    I --> J{"🚨 Các lỗi thường gặp"}
    
    subgraph "⚠️ Bẫy Người Mới - Cần Tránh"
        J --> P1["🔧 Tắt THP thủ công<br/>quên daemonize"]
        J --> P2["📝 Service file sai cấu hình<br/>Before/After dependencies"]
        J --> P3["🔄 Không test lại<br/>sau reboot"]
    end
    
    P1 --> K["🔄 Quay lại bước 2"]
    P2 --> K
    P3 --> K
    K --> D
    
    style A fill:#fff3e0,stroke:#f57c00,stroke-width:3px,color:#000
    style B fill:#e3f2fd,stroke:#1976d2,stroke-width:2px,color:#000
    style C fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    style D fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    style E fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    style E1 fill:#e8eaf6,stroke:#5e35b1,stroke-width:2px,color:#000
    style E2 fill:#e8eaf6,stroke:#5e35b1,stroke-width:2px,color:#000
    style E3 fill:#e8eaf6,stroke:#5e35b1,stroke-width:2px,color:#000
    style F fill:#ffebee,stroke:#d32f2f,stroke-width:2px,color:#000
    style G fill:#e8f5e8,stroke:#388e3c,stroke-width:3px,color:#000
    style H fill:#c8e6c9,stroke:#2e7d32,stroke-width:4px,color:#000
    style I fill:#ffcdd2,stroke:#c62828,stroke-width:2px,color:#000
    style J fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    style P1 fill:#ffcdd2,stroke:#c62828,stroke-width:2px,color:#000
    style P2 fill:#ffcdd2,stroke:#c62828,stroke-width:2px,color:#000
    style P3 fill:#ffcdd2,stroke:#c62828,stroke-width:2px,color:#000
    style K fill:#fff9c4,stroke:#f9a825,stroke-width:2px,color:#000
``` 

#### **3. Tinh chỉnh Kernel (`sysctl`) và Giới hạn (`ulimit`)**

*   **Mục đích cốt lõi:** Đảm bảo hệ điều hành cung cấp đủ "sức mạnh" và "không gian" cho MongoDB hoạt động mượt mà, đặc biệt khi hệ thống phải xử lý lượng dữ liệu lớn và nhiều kết nối cùng lúc. Các điều chỉnh này giúp MongoDB tránh khỏi các giới hạn ngầm của hệ điều hành, ngăn ngừa các lỗi liên quan đến tài nguyên như "quá nhiều file đang mở" hoặc "hết bộ nhớ ảo".

*   **Kernel Parameters (qua `sysctl`):** Đây là các thiết lập cấp thấp của hệ điều hành, ảnh hưởng đến cách kernel quản lý tài nguyên như bộ nhớ, mạng, và tiến trình. Việc tinh chỉnh các tham số này giúp tối ưu hóa cách MongoDB tương tác với nhân Linux, ví dụ:
    *   **Quản lý bộ nhớ:** Giảm xu hướng hệ điều hành sử dụng swap (vùng nhớ trên đĩa), giữ dữ liệu quan trọng của MongoDB trong RAM. Tăng giới hạn số lượng vùng nhớ ảo mà một tiến trình có thể ánh xạ (quan trọng cho WiredTiger Storage Engine).
    *   **Tối ưu mạng:** Cải thiện khả năng quản lý kết nối mạng, đảm bảo MongoDB có thể mở đủ số lượng cổng và duy trì kết nối hiệu quả.
    *   **Giới hạn tiến trình/file:** Tăng giới hạn tổng số file hệ thống có thể mở, cũng như số lượng PID và luồng có thể chạy, đảm bảo MongoDB có đủ không gian cho các tiến trình và luồng cần thiết.
    *   **Tối ưu NUMA:** Trên các hệ thống có kiến trúc bộ nhớ NUMA, việc điều chỉnh giúp giảm độ trễ khi truy cập bộ nhớ.

*   **Giới hạn Người dùng (qua `ulimit`):** Đây là các giới hạn áp đặt cho từng người dùng hoặc nhóm người dùng về lượng tài nguyên mà họ có thể sử dụng (ví dụ: số file tối đa có thể mở, số tiến trình tối đa có thể chạy). Với `mongod` thường chạy dưới user `mongod`, việc tăng các giới hạn này là rất quan trọng để tránh tình trạng "hết tài nguyên" khi tải cao.

⚠️ **BẪY NGƯỜI MỚI - Giai đoạn 1:**
-   **`ulimit` chỉnh trong shell:** Các lệnh `ulimit` chạy trực tiếp trong terminal chỉ có hiệu lực cho session hiện tại và sẽ mất khi bạn đóng terminal hoặc khởi động lại máy. **Phải cấu hình vĩnh viễn qua `/etc/security/limits.d/`**.
-   **Không áp dụng ngay bằng `sysctl -p`:** Sau khi sửa `sysctl.conf`, các thay đổi sẽ chỉ có hiệu lực sau khi reboot hoặc khi được áp dụng thủ công bằng `sysctl -p`.
-   **Quên thêm NUMA parameter:** Trên các máy chủ có kiến trúc NUMA, việc thiếu cấu hình tối ưu có thể dẫn đến hiệu năng kém do kernel cố gắng ưu tiên bộ nhớ cục bộ quá mức, gây ra độ trễ cao.

**Lưu ý quan trọng: Tại sao tạo file trong `/etc/sysctl.d/` mà không sửa `/etc/sysctl.conf`?**
*   **Thực hành tốt nhất (Best Practice)**: Trong các hệ thống Linux hiện đại (như CentOS/RHEL 8), việc tạo các file cấu hình riêng biệt trong thư mục `/etc/sysctl.d/` là phương pháp được khuyến nghị. Điều này giúp:
    *   **Dễ quản lý**: Mỗi ứng dụng hoặc mục đích tinh chỉnh có file riêng, giúp hệ thống gọn gàng, dễ theo dõi và bảo trì hơn.
    *   **Tránh xung đột**: Bạn không làm thay đổi các cấu hình mặc định của hệ thống trong `/etc/sysctl.conf`, tránh được các lỗi không mong muốn hoặc xung đột với các cài đặt của gói phần mềm khác.
    *   **Dễ gỡ bỏ**: Khi không còn cần MongoDB, bạn chỉ cần xóa file cấu hình này mà không cần dò tìm và sửa đổi một file lớn.
*   **Cách hoạt động**: Khi hệ thống khởi động hoặc chạy `sysctl --system`, nó sẽ đọc `/etc/sysctl.conf` trước, sau đó là tất cả các file `.conf` trong `/etc/sysctl.d/` theo thứ tự bảng chữ cái. Nếu có tham số trùng lặp, giá trị trong file được đọc sau cùng sẽ được áp dụng.


*   **Thực hiện đúng:**

    Quy trình này được minh họa như sau, tách biệt rõ ràng việc tinh chỉnh Kernel và giới hạn User:

    ```mermaid
    flowchart TD
        subgraph "Kernel Tuning (sysctl)"
            A["Admin edits<br/>/etc/sysctl.d/99-mongodb.conf"] --> B["Runs 'sysctl --system' OR reboots"]
            B --> C["Kernel applies settings<br/>(vm.swappiness, vm.max_map_count, etc.)"]
        end

        subgraph "User Limits (ulimit)"
            D["Admin creates<br/>/etc/security/limits.d/99-mongodb.conf"] --> E["Restarts mongod service<br/>'systemctl restart mongod'"]
            E --> F["'mongod' process inherits new limits<br/>(nofile=64000, nproc=64000)"]
        end

        C --> G((MongoDB runs with optimal OS resources))
        F --> G

        style A fill:#fff3e0
        style D fill:#f3e5f5
        style C fill:#e8f5e8
        style F fill:#e8f5e8
        style G fill:#c8e6c9
    ```

    1.  Tạo một file cấu hình mới cho MongoDB:
    Chúng ta sẽ tạo một file có tên `99-mongodb.conf` trong thư mục `/etc/sysctl.d/` `(vi /etc/sysctl.d/99-mongodb.conf)`. Số `99` đảm bảo file này được đọc sau cùng (theo thứ tự bảng chữ cái) để các tinh chỉnh của MongoDB sẽ ghi đè lên bất kỳ cài đặt nào khác nếu có xung đột. 

```bash
    # Giảm sử dụng swap tối đa (MongoDB không thích swap vì tăng latency)
    vm.swappiness = 1

    # Tối ưu NUMA - Vô hiệu hóa zone reclaim mode để tránh độ trễ NUMA
    vm.zone_reclaim_mode = 0

    # Mở rộng dải port ephemeral (outbound TCP connection) từ 1024 → 65530
    # Giúp hỗ trợ nhiều kết nối đồng thời khi có nhiều client/app
    net.ipv4.ip_local_port_range = 1024 65530

    # Tăng số lượng memory map tối đa cho 1 process (MongoDB dùng nhiều mmap cho index/collection)
    # Mặc định 65530 là quá thấp, tăng lên để tránh lỗi "out of memory maps"
    vm.max_map_count = 9999999

    # Tăng số lượng file descriptors toàn hệ thống
    # Đảm bảo MongoDB có thể mở nhiều file dữ liệu, log, connection socket
    fs.file-max = 6815744
```
2.  Áp dụng ngay: `sysctl --system`  để load tất cả file trong `/etc/sysctl.d/, /run/sysctl.d/, /usr/lib/sysctl.d/ + /etc/sysctl.conf`. Nó mô phỏng đúng hành vi khi reboot.

3.  Tạo file cấu hình `/etc/security/limits.d/99-mongodb.conf` vĩnh viễn cho user `mongod` :
```bash
# /etc/security/limits.d/99-mongodb.conf
# File này đặt giới hạn (ulimit) vĩnh viễn cho user mongod.
# Áp dụng khi mongod login hoặc khi service mongod khởi động lại.

# Số file descriptors (ulimit -n), MongoDB cần nhiều FD cho connections và file data
mongod soft nofile 64000
mongod hard nofile 64000

# Số process/threads (ulimit -u), đảm bảo MongoDB có thể tạo nhiều worker threads
mongod soft nproc 64000
mongod hard nproc 64000

# Memory lock (ulimit -l), cho phép mongod lock memory (thường dùng để tránh swap key pages, TLS…)
mongod soft memlock unlimited
mongod hard memlock unlimited

# File size (ulimit -f), giới hạn kích thước file mà process tạo ra → unlimited để không chặn write
mongod soft fsize unlimited
mongod hard fsize unlimited

# CPU time (ulimit -t), giới hạn tổng thời gian CPU mà process dùng → unlimited
mongod soft cpu unlimited
mongod hard cpu unlimited

# Virtual memory (ulimit -v), giới hạn address space process → unlimited để MongoDB không bị giới hạn RAM ảo
mongod soft as unlimited
mongod hard as unlimited

# Resident memory (ulimit -m), thường Linux hiện đại bỏ qua, nhưng có thể set bằng rss
# Optional: có thể bỏ nếu kernel > 2.4.30 vì ignore
mongod soft rss unlimited
mongod hard rss unlimited

```

* Sau khi sửa file `/etc/security/limits.d/99-mongodb.conf`, bạn chỉ cần **restart mongod** để nó nhận limit mới:

```bash
systemctl restart mongod
```
* Muốn chắc chắn, kiểm tra:

```bash
pid=$(pgrep -xo mongod)
cat /proc/$pid/limits | egrep 'Max open files|Max processes|Max locked memory'
```

* **Minh hoạ**:

```mermaid 
flowchart TD
    A[Bắt đầu: Tinh chỉnh Kernel và Ulimit] --> B{Mục đích:<br/>Cung cấp ĐỦ TÀI NGUYÊN HỆ THỐNG<br/>cho MongoDB hoạt động ổn định};
    
    subgraph "1. Tinh chỉnh Kernel (sysctl)"
        C1["Sửa /etc/sysctl.conf"] --> C2["Thêm các tham số tối ưu<br/>(Bộ nhớ, Mạng, Giới hạn File/Process, NUMA)"];
        C2 --> C3["Áp dụng ngay:<br/>sysctl -p"];
    end
    
    subgraph "2. Thiết lập Giới hạn Người dùng (ulimit)"
        D1["Tạo file /etc/security/limits.d/99-mongodb-limits.conf"] --> D2["Cấu hình giới hạn vĩnh viễn<br/>cho user mongod và root<br/>(Số file mở, số tiến trình/luồng)"];
    end
    
    B --> C1;
    B --> D1;
    
    subgraph "⚠️ BẪY NGƯỜI MỚI (Cần tránh)"
        P1["Chỉnh ulimit tạm thời trong shell"];
        P2["Không áp dụng sysctl ngay lập tức"];
        P3["Quên tối ưu NUMA trên server phù hợp"];
    end
    
    C3 --> E[Hệ thống sẵn sàng với tài nguyên tối ưu];
    D2 --> E;
    
    E --> F[MongoDB hoạt động MẠNH MẼ và ỔN ĐỊNH];
    
    %% Định nghĩa style classes
    classDef startNode fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    classDef purposeNode fill:#e3f2fd,stroke:#1976d2,stroke-width:2px,color:#000
    classDef configStep fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    classDef applyStep fill:#c8e6c9,stroke:#388e3c,stroke-width:2px,color:#000
    classDef endNode fill:#e8f5e8,stroke:#388e3c,stroke-width:3px,color:#000
    classDef trapNode fill:#ffcdd2,stroke:#c62828,stroke-width:2px,color:#000
    
    %% Áp dụng styles
    class A startNode
    class B purposeNode
    class C1,D1 configStep
    class C2,D3 applyStep
    class C3,D2 applyStep
    class E,F endNode
    class P1,P2,P3 trapNode
``` 

### **Giai đoạn 2: Cài đặt và Chuẩn bị Tài nguyên**

```mermaid
flowchart TD
    subgraph "Cấu Hình Mạng & Cổng Kết Nối"
        subgraph "mongo-1 (192.168.0.38)"
            M1C[Máy Chủ Cấu Hình<br/>Cổng 27010]
            M1S1[Shard1 RS<br/>Cổng 27011]
            M1S2[Shard2 RS<br/>Cổng 27012] 
            M1S3[Shard3 RS<br/>Cổng 27013]
            M1MOS[Router Mongos<br/>Cổng 27020]
        end
        
        subgraph "mongo-2 (192.168.0.241)"
            M2C[Máy Chủ Cấu Hình<br/>Cổng 27010]
            M2S1[Shard1 RS<br/>Cổng 27011]
            M2S2[Shard2 RS<br/>Cổng 27012]
            M2S3[Shard3 RS<br/>Cổng 27013]
        end
        
        subgraph "mongo-3 (192.168.0.215)"
            M3C[Máy Chủ Cấu Hình<br/>Cổng 27010]
            M3S1[Shard1 RS<br/>Cổng 27011]
            M3S2[Shard2 RS<br/>Cổng 27012]
            M3S3[Shard3 RS<br/>Cổng 27013]
        end
    end
    
    subgraph "Thiết Lập Bảo Mật & Tài Nguyên"
        A[Cài Đặt MongoDB] --> B[Tạo KeyFile]
        B --> C["Tạo bằng openssl<br/>Base64 756 ký tự"]
        C --> D["Đặt quyền 400<br/>Chủ Sở Hữu: mongod:mongod"]
        D --> E["Sao Chép Đến Các Node<br/>Keyfile Giống Nhau"]
        E --> F["Tạo Thư Mục Dữ Liệu<br/>/data/config, /data/shard1-3"]
        F --> G["Cấu Hình Tường Lửa<br/>Cổng 27010-27020"]
        G --> H["Tài Nguyên Sẵn Sàng"]
    end
    
    subgraph "Giao Tiếp Giữa Các Node"
        M1C -.->|"Replica Set"| M2C
        M2C -.->|"Replica Set"| M3C
        M3C -.->|"Replica Set"| M1C
        
        M1S1 -.->|"Shard01 RS"| M2S1
        M2S1 -.->|"Shard01 RS"| M3S1
        M3S1 -.->|"Shard01 RS"| M1S1
    end
    
    style M1MOS fill:#fff3e0
    style H fill:#e8f5e8
    style C fill:#ffebee
```

#### **1. Cài đặt MongoDB**

*   **Thực hiện đúng (nếu cài lại từ đầu):**
    ```bash
    yum remove mongodb* -y
    rm -rf /var/log/mongodb /var/lib/mongo /tmp/*.sock
    yum install mongodb-org -y
    rpm -qa | grep mongodb-org # Xác nhận phiên bản 7.0+
    ```

#### **2. Tạo KeyFile (Xác thực nội bộ)**

*   **Mục đích:** Mật khẩu chung để các thành viên trong cluster (mongod, mongos) tin tưởng và giao tiếp với nhau.

⚠️ **BẪY NGƯỜI MỚI - Giai đoạn 2:**
- **Keyfile khác nhau giữa các máy** → nội bộ từ chối bắt tay
- **Quên `chmod 400`** → MongoDB từ chối khởi động vì keyfile không an toàn
- **Tạo thư mục bằng `root` rồi quên `chown mongod:mongod`** → "Permission denied"

Sơ đồ dưới đây minh họa quy trình tạo, sao chép và bảo mật keyfile trên tất cả các node.

```mermaid
sequenceDiagram
    participant M1 as mongo-1 (Admin)
    participant M2 as mongo-2 (Server)
    participant M3 as mongo-3 (Server)

    M1->>M1: openssl rand ... > /data/mongo-keyfile
    M1->>M1: chown mongod:mongod /data/mongo-keyfile
    M1->>M1: chmod 400 /data/mongo-keyfile

    Note over M1: Keyfile created and secured on mongo-1

    M1->>M2: scp /data/mongo-keyfile user@mongo-2:/tmp/
    M2->>M2: sudo mv /tmp/mongo-keyfile /data/
    M2->>M2: sudo chown mongod:mongod /data/mongo-keyfile
    M2->>M2: sudo chmod 400 /data/mongo-keyfile

    Note over M2: Keyfile secured on mongo-2

    M1->>M3: scp /data/mongo-keyfile user@mongo-3:/tmp/
    M3->>M3: sudo mv /tmp/mongo-keyfile /data/
    M3->>M3: sudo chown mongod:mongod /data/mongo-keyfile
    M3->>M3: sudo chmod 400 /data/mongo-keyfile

    Note over M3: Keyfile secured on mongo-3

    Note over M1, M3: All nodes now have an identical, secured keyfile.
```

*   **Thực hiện đúng (Làm trên `mongo-1`, sau đó copy đi):**
    1.  Tạo thư mục và file key:
        ```bash
        mkdir -p /data
        openssl rand -base64 756 | tee /data/mongo-keyfile >/dev/null
        ```
    2.  **Cực kỳ quan trọng:** Đặt đúng chủ sở hữu và quyền:
        ```bash
        chown mongod:mongod /data/mongo-keyfile
        chmod 400 /data/mongo-keyfile
        ```
    3.  Copy keyfile sang 2 máy còn lại và **set lại quyền trên từng máy đó**:

        Trước tiên, hãy thử copy trực tiếp đến tài khoản `root` trên các máy đích:
        ```bash
        # Trên máy mongo-1 (nơi có keyfile gốc)
        scp /data/mongo-keyfile root@mongo-2:/data/
        scp /data/mongo-keyfile root@mongo-3:/data/
        ```

        **💡 Xử lý tình huống: Không thể `scp` trực tiếp đến tài khoản `root`**

        Trong nhiều môi trường production hoặc cài đặt bảo mật cao, việc đăng nhập trực tiếp bằng `root` qua SSH (bao gồm cả `scp` dưới quyền `root`) thường bị vô hiệu hóa. Khi cố gắng chạy các lệnh `scp` ở trên, bạn có thể nhận được các thông báo lỗi như:
        *   `Permission denied (publickey,password).`
        *   `Authentication failed.`
        *   `ssh: connect to host mongo-2 port 22: Permission denied` (nếu SSH qua root bị chặn hoàn toàn)

        Nếu gặp trường hợp này, bạn sẽ cần thực hiện các bước sau:

        *   **Bước 1: `scp` đến một tài khoản người dùng có quyền SSH trên máy đích.**
            Giả sử bạn có một tài khoản người dùng thông thường (ví dụ: `youruser`) trên `mongo-2` và `mongo-3`, và tài khoản này có thể SSH vào. Bạn có thể copy keyfile đến thư mục `/home/youruser/` hoặc `/tmp/` của tài khoản đó.
            
            **Quan trọng:** Đảm bảo tài khoản `youruser` có quyền ghi vào thư mục đích tạm thời này. Thông thường, thư mục `/home/youruser/` và `/tmp/` đều cho phép người dùng sở hữu ghi vào. Bạn có thể kiểm tra bằng lệnh `ls -ld /home/youruser` hoặc `ls -ld /tmp` sau khi SSH vào máy đích. Nếu cột quyền có ký tự `w` (write) cho `owner`, bạn có thể ghi vào đó.

            ```bash
            # Trên máy mongo-1 (nơi có keyfile gốc)
            # Thay 'youruser' bằng tên user thực tế của bạn trên máy đích
            scp /data/mongo-keyfile youruser@mongo-2:/home/youruser/mongo-keyfile
            scp /data/mongo-keyfile youruser@mongo-3:/home/youruser/mongo-keyfile
            ```

        *   **Bước 2: SSH vào từng máy đích và di chuyển file, sau đó đặt lại quyền.**
            Sau khi `scp` thành công, bạn cần đăng nhập vào từng máy đích (ví dụ: `mongo-2`) để di chuyển file `mongo-keyfile` về đúng vị trí `/data/` và đặt lại quyền.

            ```bash
            # Trên máy mongo-2:
            ssh youruser@mongo-2
            
            # Sau khi SSH thành công, chạy các lệnh sau (sử dụng để có quyền root):
            mv /home/youruser/mongo-keyfile /data/
            chown mongod:mongod /data/mongo-keyfile
            chmod 400 /data/mongo-keyfile
            exit # Thoát khỏi phiên SSH
            ```

            Thực hiện tương tự cho `mongo-3`:

            ```bash
            # Trên máy mongo-3:
            ssh youruser@mongo-3
            
            # Sau khi SSH thành công, chạy các lệnh sau:
            mv /home/youruser/mongo-keyfile /data/
            chown mongod:mongod /data/mongo-keyfile
            chmod 400 /data/mongo-keyfile
            exit # Thoát khỏi phiên SSH
            ```

        *   **Bước 3: Xác nhận quyền đã đúng trên TẤT CẢ các máy.**
            Sau khi hoàn thành các bước trên cho cả 3 máy (`mongo-1`, `mongo-2`, `mongo-3`), hãy kiểm tra lại quyền của keyfile trên mỗi máy để đảm bảo mọi thứ chính xác:

            ```bash
            # Chạy lệnh này trên từng máy: mongo-1, mongo-2, mongo-3
            ls -l /data/mongo-keyfile
            
            # Kết quả mong muốn sẽ giống như sau (hãy chú ý cột quyền `-r--------` và chủ sở hữu `mongod mongod`):
            
            -r--------. 1 mongod mongod 1024 Aug 30 08:30 /data/mongo-keyfile
            ```


#### **3. Tạo Thư mục Dữ liệu và Log**

```mermaid
flowchart TD
    subgraph "Directory Structure Creation"
        START[Start: Create MongoDB Directories] --> MKDIR[Create Directory Structure]
        MKDIR --> CHOWN[Set Proper Ownership]
        CHOWN --> VERIFY[Verify Permissions]
        VERIFY --> COMPLETE[Directories Ready]
    end
    
    subgraph "Directory Structure"
        DATA["/data/"]
        DATA --> CONFIG["/data/config/<br/>(Config Server Data)"]
        DATA --> SHARD1["/data/shard1/<br/>(Shard 1 Data)"]
        DATA --> SHARD2["/data/shard2/<br/>(Shard 2 Data)"]
        DATA --> SHARD3["/data/shard3/<br/>(Shard 3 Data)"]
    end
    
    subgraph "Commands Execution"
        CMD1["mkdir -p /data/config /data/shard1 /data/shard2 /data/shard3"]
        CMD2["chown -R mongod:mongod /data"]
        CMD3["ls -la /data/"]
    end
    
    subgraph "⚠️ Common Pitfalls"
        PITFALL1["Create as root user<br/>❌ Permission denied"]
        PITFALL2["Forget chown command<br/>❌ Access denied"]
        PITFALL3["Wrong directory structure<br/>❌ MongoDB startup fails"]
    end
    
    MKDIR --> CMD1
    CHOWN --> CMD2
    VERIFY --> CMD3
    
    style DATA fill:#e3f2fd
    style CONFIG fill:#fff3e0
    style SHARD1 fill:#e8f5e8
    style SHARD2 fill:#e8f5e8
    style SHARD3 fill:#e8f5e8
    style PITFALL1 fill:#ffebee
    style PITFALL2 fill:#ffebee
    style PITFALL3 fill:#ffebee
```

*   **Bẫy người mới:** Tạo thư mục bằng `root` và quên `chown`, dẫn đến lỗi "Permission denied".
*   **Thực hiện đúng (Trên CẢ 3 MÁY):**
    ```bash
    mkdir -p /data/config /data/shard1 /data/shard2 /data/shard3
    chown -R mongod:mongod /data
    ```

#### **4. Mở Firewall**

```mermaid
flowchart TD
    subgraph "Firewall Configuration Process"
        START[Start: Configure Firewall] --> ADD[Add Port Rules]
        ADD --> PERM[Make Rules Permanent]
        PERM --> RELOAD[Reload Firewall]
        RELOAD --> VERIFY[Verify Configuration]
        VERIFY --> COMPLETE[Firewall Ready]
    end
    
    subgraph "Port Requirements"
        PORTS["MongoDB Ports:<br/>27010 - Config Server<br/>27011 - Shard 1<br/>27012 - Shard 2<br/>27013 - Shard 3<br/>27020 - Mongos"]
    end
    
    subgraph "Commands"
        CMD1["firewall-cmd --add-port=27010-27020/tcp --permanent"]
        CMD2["firewall-cmd --reload"]
        CMD3["firewall-cmd --list-all"]
    end
    
    subgraph "⚠️ Common Pitfalls"
        PITFALL1["Add --permanent but forget --reload<br/>❌ Rules not active"]
        PITFALL2["Wrong port range<br/>❌ Connection refused"]
        PITFALL3["Forget to check firewall status<br/>❌ Silent failures"]
    end
    
    ADD --> CMD1
    PERM --> CMD1
    RELOAD --> CMD2
    VERIFY --> CMD3
    
    style PORTS fill:#e3f2fd
    style CMD1 fill:#fff3e0
    style CMD2 fill:#fff3e0
    style CMD3 fill:#fff3e0
    style PITFALL1 fill:#ffebee
    style PITFALL2 fill:#ffebee
    style PITFALL3 fill:#ffebee
```

*   **Bẫy người mới:** Thêm rule `--permanent` nhưng quên `--reload`.
*   **Thực hiện đúng (Trên CẢ 3 MÁY):**
    ```bash
    firewall-cmd --add-port=27010-27020/tcp --permanent
    firewall-cmd --reload
    # Nếu không thấy port, kiểm tra firewall: firewall-cmd --list-all
    ```

---

### **Giai đoạn 3: Dựng Cụm Config Server**

```mermaid
sequenceDiagram
    participant Admin as Administrator
    participant CFG1 as Config Server 1
    participant CFG2 as Config Server 2
    participant CFG3 as Config Server 3
    
    Admin->>CFG1: Create config file
    Admin->>CFG2: Create config file
    Admin->>CFG3: Create config file
    
    Admin->>CFG1: Start mongod --config
    Admin->>CFG2: Start mongod --config
    Admin->>CFG3: Start mongod --config
    
    Admin->>CFG1: Connect and rs.initiate()
    CFG1->>CFG2: Replica Set sync
    CFG1->>CFG3: Replica Set sync
    
    CFG1->>Admin: PRIMARY elected
    Admin->>CFG1: Create admin user
    Admin->>CFG1: Enable authorization
    
    Note over CFG1,CFG3: Config Server Replica Set Ready
```

#### **1. Tạo File Cấu hình (Trên CẢ 3 MÁY)**

*   **File `/etc/mongod-config.conf`:**
```yaml
systemLog:
  destination: file
  logAppend: true
  path: /data/config.log
 
storage:
  dbPath: /data/config
#  journal:
#    enabled: true
 
processManagement:
#  fork: true  # fork and run in background
#  pidFilePath: /data/mongod-config.pid  # location of pidfile
  timeZoneInfo: /usr/share/zoneinfo
 
net:
  port: 27010
  bindIp: 0.0.0.0  # Enter 0.0.0.0,:: to bind to all IPv4 and IPv6 addresses or, alternatively, use the net.bindIpAll setting.

security:
#  authorization: enabled
  keyFile: /data/mongo-keyfile 
replication:
   replSetName: "Rep1"
sharding:
   clusterRole: configsvr
```
*   **Bẫy Người Mới:** Vội vàng bật `authorization: enabled`. Điều này sẽ chặn bạn khởi tạo replica set và tạo user admin đầu tiên (vấn đề "con gà quả trứng"). Quy trình đúng là giữ `keyFile` (xác thực nội bộ) nhưng tạm tắt `authorization` (xác thực client).

    **Giải thích chi tiết về Bẫy Người Mới này:**

    `keyFile` và `authorization: enabled` phục vụ hai mục đích bảo mật khác nhau:
    *   **`keyFile` (Internal Authentication):** Đây là "mật khẩu chung" để các tiến trình MongoDB trong cluster (Config Server, Shard, Mongos) tin tưởng và giao tiếp an toàn với nhau. Khi `keyFile` được cấu hình, các node có thể tự nhận diện và xây dựng các Replica Set hoặc kết nối với nhau trong cluster. Nó hoạt động độc lập với việc xác thực người dùng cuối.
    *   **`authorization: enabled` (Client Authentication):** Khi tham số này được bật, MongoDB sẽ yêu cầu BẤT KỲ kết nối từ bên ngoài (ví dụ: công cụ `mongosh` của bạn, ứng dụng client) phải cung cấp tên người dùng và mật khẩu hợp lệ trước khi được phép thực hiện các thao tác.

    **Vấn đề "Con gà và Quả trứng" xảy ra khi bạn bật `authorization: enabled` quá sớm:**
    1.  Ở giai đoạn này, bạn đang cố gắng khởi tạo một Config Server Replica Set lần đầu tiên bằng lệnh `rs.initiate()` thông qua `mongosh`.
    2.  Nếu `authorization: enabled` đã được bật trong file cấu hình, thì ngay cả khi `mongosh` kết nối từ cùng một máy chủ (localhost), nó vẫn sẽ bị coi là một client "chưa được xác thực".
    3.  MongoDB sẽ **từ chối** thực hiện lệnh `rs.initiate()` vì bạn không có quyền. Bạn sẽ thấy lỗi như `Not authorized to run command 'replSetInitiate'` hoặc tương tự.
    4.  Điều trớ trêu là, để có quyền, bạn cần phải có một user admin. Nhưng user admin lại chỉ có thể được tạo *sau khi* Replica Set đã được khởi tạo thành công và có một node `PRIMARY` (nơi các thao tác ghi dữ liệu, bao gồm tạo user, có thể diễn ra).
    5.  Bạn bị mắc kẹt: không thể khởi tạo Replica Set vì thiếu user, nhưng không thể tạo user vì Replica Set chưa được khởi tạo. Đây chính là vấn đề "con gà và quả trứng".

    **Quy trình đúng đắn để tránh lỗi này:**
    *   Tạm thời **COMMENT** hoặc SET `authorization: false` trong file cấu hình `mongod-config.conf` khi khởi động Config Server lần đầu.
    *   Sau khi các Config Server đã khởi động, kết nối bằng `mongosh` (lúc này không cần xác thực) và chạy `rs.initiate()` để thiết lập Replica Set.
    *   Đợi cho đến khi một node được bầu làm `PRIMARY`.
    *   **Trên node `PRIMARY` đó**, tạo user admin đầu tiên bằng lệnh `db.createUser()`.
    *   Sau khi user admin đã tồn tại, bạn mới nên **BỎ COMMENT** dòng `authorization: enabled` trong file cấu hình trên **TẤT CẢ** các Config Server và **khởi động lại an toàn** các tiến trình `mongod` để kích hoạt bảo mật. Từ lúc này, mọi kết nối từ client sẽ yêu cầu xác thực.

#### **2. Khởi động Config Server**
*   **Thực hiện đúng (Trên CẢ 3 MÁY):**
    ```bash
    sudo -u mongod /usr/bin/mongod --config /etc/mongod-config.conf &
    tail -f /data/config.log # Theo dõi log để tìm "waiting for connections"
    ```
*   **Lưu ý:** ` &` phù hợp cho lab. Môi trường production nên tạo file unit systemd để quản lý dịch vụ chuyên nghiệp hơn.

#### **3. Khởi tạo Replica Set và Tạo User Admin**

*   **Bẫy người mới:** Tạo user trước khi `initiate`; bối rối vì prompt mặc định là `test>`.
*   **Thực hiện đúng (Chỉ làm trên 1 máy):**
    1.  Kết nối: `mongosh localhost:27010`
    2.  Khởi tạo replica set:
        ```javascript
        rs.initiate({
          _id: "Rep1", configsvr: true,
          members: [
            { _id: 0, host: "mongo-1:27010" },
            { _id: 1, host: "mongo-2:27010" },
            { _id: 2, host: "mongo-3:27010" }
          ]
        })
        ```
    3.  **Đợi node lên PRIMARY** (prompt chuyển thành `Rep1 [primary]>`):
        ```javascript
        // Lệnh để chờ tự động
        while (!db.hello().isWritablePrimary) { sleep(1000); print("...waiting for PRIMARY"); }
        ```
    4.  **Khi đã có PRIMARY**, tạo ngay user admin đầu tiên:
        
⚠️ **BẪY BẢO MẬT QUAN TRỌNG:**
- **Dùng mật khẩu text trong script** → rò rỉ qua shell history
- **LUÔN dùng `passwordPrompt()` thay vì hard-code mật khẩu**

        ```javascript
        use admin
        db.createUser({
          user: "mongodba", 
          pwd: passwordPrompt(), // <-- Nhập an toàn thay vì hard-code
          roles: [{role: "root", db: "admin"}]
        })
        ```
    1.  *(Tùy chọn)*: Nếu muốn một node mạnh hơn luôn được ưu tiên làm PRIMARY, bạn có thể chỉnh `priority`. Mặc định không cần thiết.
        ```javascript
        cfg = rs.conf()
        cfg.members[0].priority = 3 // Node mongo-1 ưu tiên cao nhất
        rs.reconfig(cfg)
        ```
    2.  Thoát khỏi mongosh: `exit`

#### **4. Bật Xác thực và Khởi động lại**

*   **Thực hiện (Trên CẢ 3 MÁY):**
    1.  Sửa file `/etc/mongod-config.conf`, **bỏ comment** dòng `authorization: enabled`.
    2.  Khởi động lại tiến trình một cách an toàn:
        ```bash
        # Gửi tín hiệu SIGTERM (15) để shutdown an toàn, tránh kill -9
        pkill -15 -f "mongod-config.conf"
        sudo -u mongod /usr/bin/mongod --config /etc/mongod-config.conf &
        ```
    3.  Kiểm tra đăng nhập bằng tài khoản admin:
        `mongosh --port 27010 sudo -u mongodba --authenticationDatabase admin`
        (Sẽ prompt nhập password an toàn)

---
**💡 XỬ LÝ LỖI PHỔ BIẾN: LỠ BẬT `authorization: enabled` QUÁ SỚM HOẶC QUÊN TẠO USER ADMIN NGAY LẬP TỨC**

Nếu bạn đã lỡ bỏ comment dòng `authorization: enabled` trong file cấu hình `/etc/mongod-config.conf` (hoặc khởi động lại Config Server với `authorization` bật) **trước khi tạo user admin** (`mongodba`), bạn sẽ gặp lỗi `MongoServerError[Unauthorized]` khi cố gắng thực hiện bất kỳ lệnh quản trị nào (ví dụ: `rs.initiate()`, `rs.conf()`, `db.createUser()`).

**Cách khắc phục tình huống này để tiếp tục thiết lập Config Server:**

**Mục tiêu:** Tạm thời tắt `authorization` để có thể kết nối `mongosh` mà không cần xác thực, từ đó tạo được user admin, sau đó mới bật lại bảo mật.

**Thực hiện các bước sau trên CẢ 3 MÁY CONFIG SERVER (`mongo-1`, `mongo-2`, `mongo-3`):**

1.  **Dừng tiến trình `mongod` của Config Server đang chạy:**
    ```bash
    pkill -15 -f "mongod --config /etc/mongod-config.conf"
    sleep 5 # Chờ 5 giây để tiến trình dừng hẳn
    ```
    *   **Giải thích:** Bước này đảm bảo tiến trình MongoDB đang chạy với cấu hình `authorization` bật bị tắt hoàn toàn.

2.  **Sửa file cấu hình `/etc/mongod-config.conf` để tắt `authorization` tạm thời:**
    ```bash
    vi /etc/mongod-config.conf
    ```
    Tìm dòng `authorization: enabled` và **COMMENT** nó lại bằng cách thêm dấu `#` vào đầu dòng:
    ```yaml
    security:
      keyFile: /data/mongo-keyfile
      # authorization: enabled  # <-- Đảm bảo dòng này có dấu # ở đầu
    ```    Lưu và đóng file (`:wq`).
    *   **Giải thích:** Việc này cho phép MongoDB khởi động mà không yêu cầu xác thực người dùng từ client.

3.  **Khởi động lại tiến trình `mongod` của Config Server:**
    ```bash
    sudo -u mongod /usr/bin/mongod --config /etc/mongod-config.conf &
    tail -f /data/config.log # Kiểm tra log để đảm bảo không có lỗi xác thực
    ```
    *   **Giải thích:** Tiến trình `mongod` giờ sẽ khởi động với `authorization` đã tắt.

**Sau khi hoàn thành 3 bước khắc phục trên cả 3 máy, bạn đã có thể tiếp tục với các bước thiết lập Config Server:**

*   **Nếu bạn chưa từng chạy `rs.initiate()` thành công (ví dụ: bị lỗi `Unauthorized` ngay từ đầu):**
    Quay lại **mục "3. Khởi tạo Replica Set và Tạo User Admin"** ở trên và thực hiện lại **từ bước 1 ("Kết nối: `mongosh localhost:27010`")**. Lần này, mọi lệnh sẽ chạy thành công vì `authorization` đã tắt.

*   **Nếu bạn đã chạy `rs.initiate({})` nhưng quên thêm các member hoặc quên tạo user admin:**
    Bạn có thể sử dụng `rs.reconfig()` để thêm các thành viên còn lại và sau đó tạo user admin (như đã hướng dẫn trong phần "Bẫy Người Mới" của mục này).

    *   **Kết nối `mongosh` trên máy PRIMARY (ví dụ `mongo-1`):**
        ```bash
        mongosh --port 27010
        ```
    *   **Lấy cấu hình hiện tại và thêm các thành viên khác:**
        ```javascript
        cfg = rs.conf()
        cfg.members = [
          { _id: 0, host: "mongo-1:27010" },
          { _id: 1, host: "mongo-2:27010" },
          { _id: 2, host: "mongo-3:27010" }
        ];
        cfg.configsvr = true;
        rs.reconfig(cfg, { force: true });
        ```
    *   **Chờ PRIMARY và tạo user admin:**
        ```javascript
        while (!db.hello().isWritablePrimary) { sleep(1000); print("...waiting for PRIMARY"); }
        use admin
        db.createUser({user: "mongodba", pwd: passwordPrompt(), roles:[{role: "root", db: "admin"}]})
        exit
        ```

---

#### **4. Bật Xác thực và Khởi động lại**

*   **Thực hiện (Trên CẢ 3 MÁY):**
    1.  Sửa file `/etc/mongod-config.conf`, **bỏ comment** dòng `authorization: enabled`.
    2.  Khởi động lại tiến trình một cách an toàn:
        ```bash
        # Gửi tín hiệu SIGTERM (15) để shutdown an toàn, tránh kill -9
        pkill -15 -f "mongod --config /etc/mongod-config.conf"
        sudo -u mongod /usr/bin/mongod --config /etc/mongod-config.conf &
        ```
    3.  Kiểm tra đăng nhập bằng tài khoản admin:
        `mongosh --port 27010 sudo -u mongodba --authenticationDatabase admin`
        (Sẽ prompt nhập password an toàn)


---

### **Giai đoạn 4: Dựng các Cụm Shard**

```mermaid
graph TD
    subgraph "Shard Creation Process"
        A[Create Shard Config Files] --> B[Start Shard Processes]
        B --> C[Initialize Replica Sets]
        C --> D[Verify Shard Status]
    end
    
    subgraph "Shard 1 - Port 27011"
        S1A[mongo-1:27011]
        S1B[mongo-2:27011]
        S1C[mongo-3:27011]
        S1A --- S1B
        S1B --- S1C
        S1C --- S1A
    end
    
    subgraph "Shard 2 - Port 27012"
        S2A[mongo-1:27012]
        S2B[mongo-2:27012]
        S2C[mongo-3:27012]
        S2A --- S2B
        S2B --- S2C
        S2C --- S2A
    end
    
    subgraph "Shard 3 - Port 27013"
        S3A[mongo-1:27013]
        S3B[mongo-2:27013]
        S3C[mongo-3:27013]
        S3A --- S3B
        S3B --- S3C
        S3C --- S3A
    end
    
    C --> S1A
    C --> S2A
    C --> S3A
```

#### **Bước 1: Chuẩn bị File Cấu hình (Bật Security Từ Đầu)**

*   **File `/etc/mongod-shard1.conf`**
```yaml
systemLog:
  destination: file
  logAppend: true
  logRotate: reopen
  path: /data/shard1.log
 
storage:
  dbPath: /data/shard1
#  journal:
#   enabled: true
 
processManagement:
# fork: true  # fork and run in background
#  pidFilePath: /data/mongod-shard1.pid  # location of pidfile
  timeZoneInfo: /usr/share/zoneinfo
 
net:
  port: 27011
  bindIp: 0.0.0.0  # Enter 0.0.0.0,:: to bind to all IPv4 and IPv6 addresses or, alternatively, use the net.bindIpAll setting.
 
security:
  authorization: enabled
  keyFile: /data/mongo-keyfile 
replication:
   replSetName: "shard01" 
sharding:
   clusterRole: shardsvr
```
---
*   **File `/etc/mongod-shard2.conf`**
```yaml
systemLog:
  destination: file
  logAppend: true
  logRotate: reopen
  path: /data/shard2.log
 
storage:
  dbPath: /data/shard2
#  journal:
#   enabled: true
 
processManagement:
# fork: true  # fork and run in background
#  pidFilePath: /data/mongod-shard2.pid  # location of pidfile
  timeZoneInfo: /usr/share/zoneinfo
 
net:
  port: 27012
  bindIp: 0.0.0.0  # Enter 0.0.0.0,:: to bind to all IPv4 and IPv6 addresses or, alternatively, use the net.bindIpAll setting.
 
security:
  authorization: enabled
  keyFile: /data/mongo-keyfile 
replication:
   replSetName: "shard02" 
sharding:
   clusterRole: shardsvr
``` 
---
*   **File `/etc/mongod-shard3.conf`**
```yaml
systemLog:
  destination: file
  logAppend: true
  logRotate: reopen
  path: /data/shard3.log
 
storage:
  dbPath: /data/shard3
#  journal:
#   enabled: true
 
processManagement:
# fork: true  # fork and run in background
#  pidFilePath: /data/mongod-shard3.pid  # location of pidfile
  timeZoneInfo: /usr/share/zoneinfo
 
net:
  port: 27013
  bindIp: 0.0.0.0  # Enter 0.0.0.0,:: to bind to all IPv4 and IPv6 addresses or, alternatively, use the net.bindIpAll setting.
 
security:
  authorization: enabled
  keyFile: /data/mongo-keyfile 
replication:
   replSetName: "shard03" 
sharding:
   clusterRole: shardsvr
``` 

#### **Bước 2: Khởi động các Node**

*   **Thực hiện (Trên CẢ 3 MÁY):**
    ```bash
    sudo -u mongod /usr/bin/mongod --config /etc/mongod-shard1.conf &
    sudo -u mongod /usr/bin/mongod --config /etc/mongod-shard2.conf &
    sudo -u mongod /usr/bin/mongod --config /etc/mongod-shard3.conf &
    # Kiểm tra: ps -ef | grep mongo phải thấy các tiến trình mongod đang chạy
    ```

---

#### **Bước 3: Khởi tạo Replica Set (Sử dụng Localhost Exception)**

Sơ đồ dưới đây giải thích cơ chế "ngoại lệ localhost" và cách chúng ta tận dụng nó để thiết lập bảo mật cho các Shard.

```mermaid
graph TD
    subgraph "Localhost Exception Explained"
        A["mongod starts with 'authorization: enabled'"] --> B{"Database có user nào chưa?"}
        B -- "Chưa có user nào" --> C["✅ Localhost Exception: BẬT<br/>Cho phép kết nối từ localhost<br/>KHÔNG cần xác thực"]
        B -- "Đã có ít nhất 1 user" --> D["❌ Localhost Exception: TẮT<br/>Mọi kết nối (kể cả localhost)<br/>ĐỀU PHẢI xác thực"]
    end

    subgraph "Quy trình dựng Shard"
        E["1. Start các node shard<br/>(chưa có user)"] --> F["2. Kết nối từ localhost<br/>(mongosh --port 27011)"]
        F -- "Nhờ Localhost Exception" --> G["3. Chạy 'rs.initiate()' thành công"]
        G --> H["4. Tạo user admin đầu tiên<br/>trên PRIMARY"]
        H --> I["5. Exception tự động TẮT"]
        I --> J["✅ Shard được bảo mật"]
    end

    style C fill:#e8f5e8,stroke:#388e3c
    style D fill:#ffebee,stroke:#d32f2f
    style J fill:#c8e6c9
```

Bạn có thể kết nối mà không cần xác thực vì MongoDB cho phép "localhost exception" khi chưa có người dùng nào được tạo. Lệnh `rs.initiate` sẽ thành công vì giao tiếp nội bộ giữa các node đã được bảo vệ bằng `keyFile`.

*   **Thực hiện trên một máy bất kỳ (ví dụ `mongo-1`):**

    *   **Khởi tạo replica set `shard01` (ưu tiên `mongo-1` làm Primary):**
        ```bash
        mongosh --port 27011 # Kết nối không cần auth nhờ exception
        rs.initiate(
          {
            _id: "shard01",
            members: [
              { _id : 0, host : "mongo-1:27011", priority: 3 },
              { _id : 1, host : "mongo-2:27011", priority: 2 },
              { _id : 2, host : "mongo-3:27011", priority: 1 }
            ]
          }
        )
        rs.status() # Kiểm tra để xác nhận vai trò PRIMARY
        exit
        ```

    *   **Khởi tạo replica set `shard02` (ưu tiên `mongo-2` làm Primary):**
        ```bash
        mongosh --port 27012
        rs.initiate(
          {
            _id: "shard02",
            members: [
              { _id : 0, host : "mongo-1:27012", priority: 1 },
              { _id : 1, host : "mongo-2:27012", priority: 3 },
              { _id : 2, host : "mongo-3:27012", priority: 2 }
            ]
          }
        )
        rs.status()
        exit
        ```

    *   **Khởi tạo replica set `shard03` (ưu tiên `mongo-3` làm Primary):**
        ```bash
        mongosh --port 27013
        rs.initiate(
          {
            _id: "shard03",
            members: [
              { _id : 0, host : "mongo-1:27013", priority: 2 },
              { _id : 1, host : "mongo-2:27013", priority: 1 },
              { _id : 2, host : "mongo-3:27013", priority: 3 }
            ]
          }
        )
        rs.status()
        exit
        ```
    Chờ khoảng 10-20 giây để các replica set ổn định và bầu cử xong PRIMARY.

👉 Với cấu hình này:

* **Shard01** sẽ ưu tiên `mongo-1` làm Primary.
* **Shard02** sẽ ưu tiên `mongo-2` làm Primary.
* **Shard03** sẽ ưu tiên `mongo-3` làm Primary.

Như vậy, **Primary được phân bổ đều trên ba server**, đảm bảo tài nguyên được sử dụng tối ưu và hệ thống **cân bằng tải tốt hơn**.

---

#### **Bước 4: Tạo Tài khoản Quản trị trên PRIMARY (Tiếp tục dùng Exception)**

**Lưu ý quan trọng:** Vì "localhost exception" chỉ hoạt động khi bạn kết nối từ chính máy đang chạy `mongod`, bạn phải thực hiện lệnh `mongosh` trên đúng máy chủ đang giữ vai trò **PRIMARY** cho mỗi shard.

*   **Tạo user cho `shard01`:**
    Theo cấu hình, `mongo-1` là PRIMARY. **Chạy lệnh này trên máy `mongo-1`**:
    ```bash
    mongosh --port 27011
    use admin
    db.createUser({
      user: "shard1Admin",
      pwd: passwordPrompt(),
      roles: [ { role: "root", db: "admin" } ]
    })
    exit
    ```

*   **Tạo user cho `shard02`:**
    Theo cấu hình, `mongo-2` là PRIMARY. Bạn phải **SSH vào máy `mongo-2`** và chạy lệnh từ đó:
    ```bash
    # Trên máy mongo-2
    mongosh --port 27012
    use admin
    db.createUser({
      user: "shard2Admin",
      pwd: passwordPrompt(),
      roles: [ { role: "root", db: "admin" } ]
    })
    exit
    ```

*   **Tạo user cho `shard03`:**
    Theo cấu hình, `mongo-3` là PRIMARY. Bạn phải **SSH vào máy `mongo-3`** và chạy lệnh từ đó:
    ```bash
    # Trên máy mongo-3
    mongosh --port 27013
    use admin
    db.createUser({
      user: "shard3Admin",
      pwd: passwordPrompt(),
      roles: [ { role: "root", db: "admin" } ]
    })
    exit
    ```

Sau khi người dùng đầu tiên được tạo trong mỗi replica set, "localhost exception" sẽ **tự động đóng lại**. Mọi kết nối sau này đều bắt buộc phải xác thực.

---

#### **Bước 5: Hoàn tất và Kiểm tra**

Hệ thống của bạn hiện đã được cấu hình đầy đủ và bảo mật mà không cần bất kỳ lần khởi động lại nào.

Bạn có thể kiểm tra bằng cách kết nối tới một shard bất kỳ bằng tài khoản vừa tạo từ bất kỳ máy nào có `mongosh`:
```bash
# Thử kết nối vào shard 1 với xác thực
mongosh --host mongo-1 --port 27011 -u "shard1Admin" -p --authenticationDatabase "admin"
```
Hệ thống sẽ yêu cầu mật khẩu, và sau khi nhập đúng, bạn sẽ đăng nhập thành công. Quá trình thiết lập đã hoàn tất.

### **Giai đoạn 5: Dựng Mongos và Hoàn thiện Cluster**

```mermaid
flowchart TD
    A[Create Mongos Config] --> B[Start Mongos Process]
    B --> C[Connect to Config Servers]
    C --> D[Add Shard 1]
    D --> E[Add Shard 2]
    E --> F[Add Shard 3]
    F --> G[Enable Sharding on Database]
    G --> H[Shard Collections]
    H --> I[Test Data Distribution]
    I --> J[Cluster Complete]
    
    style A fill:#fff3e0
    style J fill:#e8f5e8
    
    subgraph "Mongos Configuration"
        M1["Port: 27020<br/>ConfigDB: Rep1/mongo-1:27010,mongo-2:27010,mongo-3:27010"]
    end
    
    B --> M1
```

#### **1. Tạo File Cấu hình Mongos (Trên 1 máy)**

*   **File `/etc/mongos.conf`:**
```yaml
systemLog:
  destination: file
  logAppend: true
  path: /data/mongos.log
 
processManagement:
#  fork: true  # fork and run in background
#  pidFilePath: /data/mongod-mongos.pid  # location of pidfile
  timeZoneInfo: /usr/share/zoneinfo
 
net:
  port: 27020
  bindIp: 0.0.0.0  # Enter 0.0.0.0,:: to bind to all IPv4 and IPv6 addresses or, alternatively, use the net.bindIpAll setting.
security:
  keyFile: /data/mongo-keyfile 
sharding:
  configDB: "Rep1/mongo-1:27010,mongo-2:27010,mongo-3:27010"
```

#### **2. Khởi động Mongos**

```bash
sudo -u mongod /usr/bin/mongos --config /etc/mongos.conf &
tail -f /data/mongos.log # Theo dõi log đến khi thấy "connected to config replica set"
```

#### **3. Thêm các Shard vào Cluster**

⚠️ **BẪY NGƯỜI MỚI - Giai đoạn 5:**
- **`configDB` sai `replSetName`** hoặc **nhầm thứ tự host** → mongos không kết nối nổi
- **Chỉ chạy một `mongos`** trong production → SPOF về truy vấn (nên có nhiều `mongos`)
- **Dùng mật khẩu hard-code trong lệnh** → rò rỉ credential

*   **Thực hiện (Kết nối vào Mongos):**
    ```bash
    mongosh --port 27020 -u mongodba --authenticationDatabase admin
    # Sẽ prompt nhập password an toàn
    ```
    Bên trong mongosh:
    ```javascript
    // Dùng định dạng replica set / seed list, hiệu quả hơn
    sh.addShard("shard01/mongo-1:27011,mongo-2:27011,mongo-3:27011")
    sh.addShard("shard02/mongo-1:27012,mongo-2:27012,mongo-3:27012")
    sh.addShard("shard03/mongo-1:27013,mongo-2:27013,mongo-3:27013")
    ```

#### **4. Kích hoạt Sharding và Test**

1.  **Kiểm tra trạng thái:** `sh.status()` sẽ hiển thị các shard đã được thêm.
2.  **Bật sharding cho database:** `sh.enableSharding("testDB")`
3.  **Shard collection với `hashed` key để phân phối đều:**
    ```javascript
    use testDB
    sh.shardCollection("testDB.myCollection", { "_id": "hashed" } )
    ```
4.  **Insert dữ liệu để kiểm tra:**
    ```javascript
    for (var i = 1; i <= 100000; i++) {
      db.myCollection.insertOne({ name: "test_data_" + i });
    }
    db.myCollection.getShardDistribution() // Xem dữ liệu đã được phân phối đều chưa
    ```

---

### **Giai đoạn 6: Debug, Kiểm tra và Lưu ý Production**

```mermaid
flowchart LR
    A[Check Processes] --> B[Verify Shard Status]
    B --> C[Review Logs]
    C --> D[Test Failover]
    D --> E[Monitor Performance]
    E --> F[Production Readiness]
    
    subgraph "Health Checks"
        H1["ps -ef | grep mongo<br/>4 mongod + 1 mongos"]
        H2["sh.status()<br/>All shards active"]
        H3["tail -f /data/*.log<br/>No ERROR/WARNING"]
    end
    
    subgraph "Production Considerations"
        P1["Replace keyFile with x.509 certificates"]
        P2["Regular config server backups"]
        P3["Monitoring with Atlas/Ops Manager"]
        P4["Multiple mongos instances"]
    end
    
    A --> H1
    B --> H2
    C --> H3
    F --> P1
    F --> P2
    F --> P3
    F --> P4
    
    style F fill:#e8f5e8
```

*   **Kiểm tra tổng thể:**
    *   `ps -ef | grep mongo`: Phải có 4 tiến trình `mongod` và 1 tiến trình `mongos` (trên node chạy mongos).
    *   `sh.status()`: Các shard phải ở trạng thái `active`.
*   **Xem Logs:** `tail -n 100 /data/*.log` để tìm lỗi `ERROR` hoặc `WARNING`.
*   **Test Failover:** Thử kill tiến trình PRIMARY của một shard (`pkill -15 -f shard01.conf`) và dùng `rs.status()` trên port của shard đó để xem một node SECONDARY có được bầu lên làm PRIMARY hay không.
*   **Lưu ý Production:**
    *   **Bảo mật:** Thay thế `keyFile` bằng chứng chỉ **x.509** để mã hóa và xác thực mạnh hơn.
    *   **Backup:** Thường xuyên sao lưu `config server` vì nó chứa toàn bộ metadata của cluster.
    *   **Giám sát:** Sử dụng các công cụ như MongoDB Atlas, Ops Manager, hoặc Prometheus để theo dõi sức khỏe hệ thống.
    *   **Mở rộng:** Có thể thêm các instance `mongos` trên các máy khác để cân bằng tải truy vấn.
    *   **systemd Unit:** Khuyến nghị tạo unit file cho `mongod`/`mongos` thay vì dùng ` &` trong production.

💡 **Mẫu systemd Unit cho Production:**
```ini
# /etc/systemd/system/mongod-shard01.service
[Unit]
Description=MongoDB Shard01
After=network.target disable-transparent-huge-pages.service

[Service]
User=mongod
Group=mongod
ExecStart=/usr/bin/mongod --config /etc/mongod-shard1.conf
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure
LimitNOFILE=64000
LimitNPROC=64000

[Install]
WantedBy=multi-user.target
```
Kích hoạt: `systemctl daemon-reload && systemctl enable --now mongod-shard01`

---

### **Giai đoạn 7: Thao tác và Truy vấn Dữ liệu (Aggregation Framework)**

```mermaid
graph TD
    A[Raw Data in Collections] --> B{Aggregation Pipeline};
    B --> C[Stage 1: $match<br/>Filter documents];
    C --> D[Stage 2: $group<br/>Aggregate values];
    D --> E[Stage 3: $sort<br/>Order results];
    E --> F[Stage 4: $project<br/>Reshape output];
    F --> G[Final Result Set];

    style A fill:#e3f2fd
    style G fill:#e8f5e8
```

Sau khi đã có một cluster hoàn chỉnh, bước tiếp theo là khai thác sức mạnh của dữ liệu. Aggregation Framework là một công cụ cực kỳ mạnh mẽ để thực hiện các phép biến đổi và tính toán phức tạp ngay trên server.

*   **Mục đích:** Thay vì kéo hàng tấn dữ liệu thô về ứng dụng rồi mới xử lý, chúng ta sẽ đẩy logic tính toán về phía MongoDB, giúp giảm tải băng thông mạng và tận dụng tài nguyên của database.
*   **Bẫy người mới:** Thực hiện các logic nối (join), lọc, nhóm (group by) phức tạp ở tầng ứng dụng (application layer). Điều này cực kỳ thiếu hiệu quả, làm chậm ứng dụng và không tận dụng được sức mạnh của MongoDB.
*   **Thực hiện đúng:** Xây dựng một "đường ống" (pipeline) gồm nhiều "giai đoạn" (stages), mỗi giai đoạn sẽ xử lý đầu vào từ giai đoạn trước và đưa kết quả cho giai đoạn tiếp theo.

#### **1. Bài toán 1: Lọc, Sắp xếp và Định hình Dữ liệu**

```mermaid
graph TD
    A[Collection: persons] --> B[Stage 1: $match<br/>{vocation: "ENGINEER"}]
    B --> C[Stage 2: $sort<br/>{dateofbirth: -1}]
    C --> D[Stage 3: $limit<br/>3]
    D --> E[Stage 4: $project<br/>{_id:0, firstname:1, ...}]
    E --> F[Output: 3 kỹ sư trẻ nhất]

    style A fill:#e3f2fd
    style F fill:#e8f5e8
    style B fill:#f3e5f5
    style C fill:#e0f2f1
    style D fill:#fff3e0
    style E fill:#fce4ec
```

*   **Chuẩn bị dữ liệu:**
    ```javascript
    use testDB // Sử dụng lại database từ Giai đoạn 4
    db.persons.insertMany([
      { "person_id": "6392529400", "firstname": "Elise", "lastname": "Smith", "dateofbirth": ISODate("1972-01-13T09:32:07Z"), "vocation": "ENGINEER", "address": { "number": 5625, "street": "Tipa Circle", "city": "Wojzinmoj" } },
      { "person_id": "1723338115", "firstname": "Olive", "lastname": "Ranieri", "dateofbirth": ISODate("1985-05-12T23:14:30Z"), "gender": "FEMALE", "vocation": "ENGINEER", "address": { "number": 9303, "street": "Mele Circle", "city": "Tobihbo" } },
      { "person_id": "8732762874", "firstname": "Toni", "lastname": "Jones", "dateofbirth": ISODate("1991-11-23T16:53:56Z"), "vocation": "POLITICIAN", "address": { "number": 1, "street": "High Street", "city": "Upper Abbeywoodington" } },
      { "person_id": "7363629563", "firstname": "Bert", "lastname": "Gooding", "dateofbirth": ISODate("1941-04-07T22:11:52Z"), "vocation": "FLORIST", "address": { "number": 13, "street": "Upper Bold Road", "city": "Redringtonville" } },
      { "person_id": "1029648329", "firstname": "Sophie", "lastname": "Celements", "dateofbirth": ISODate("1959-07-06T17:35:45Z"), "vocation": "ENGINEER", "address": { "number": 5, "street": "Innings Close", "city": "Basilbridge" } },
      { "person_id": "7363626383", "firstname": "Carl", "lastname": "Simmons", "dateofbirth": ISODate("1998-12-26T13:13:55Z"), "vocation": "ENGINEER", "address": { "number": 187, "street": "Hillside Road", "city": "Kenningford" } }
    ]);
    ```
*   **Yêu cầu:** Trả về 3 người kỹ sư (ENGINEER) trẻ nhất, chỉ hiển thị `firstname`, `lastname`, `dateofbirth`.
*   **Lời giải:**
    ```javascript
    db.persons.aggregate([
      // Giai đoạn 1: Lọc ra những người là ENGINEER
      { $match: { vocation: "ENGINEER" } },
      // Giai đoạn 2: Sắp xếp theo ngày sinh giảm dần (ngày sinh lớn hơn là người trẻ hơn)
      { $sort: { dateofbirth: -1 } },
      // Giai đoạn 3: Giới hạn kết quả chỉ lấy 3 người đầu tiên
      { $limit: 3 },
      // Giai đoạn 4: Định hình lại output, ẩn _id và các trường không cần thiết
      { $project: {
          _id: 0,
          firstname: 1,
          lastname: 1,
          dateofbirth: 1
      }}
    ])
    ```

#### **2. Bài toán 2: Thống kê và Nhóm Dữ liệu**

*   **Chuẩn bị dữ liệu:**
    ```javascript
    db.orders.insertMany([
      { "customer_id": "elise_smith@myemail.com", "orderdate": ISODate("2020-05-30T08:35:52Z"), "value": NumberDecimal("231.43")},
      { "customer_id": "elise_smith@myemail.com", "orderdate": ISODate("2020-01-13T09:32:07Z"), "value": NumberDecimal("99.99")},
      { "customer_id": "oranieri@warmmail.com", "orderdate": ISODate("2020-01-01T08:25:37Z"), "value": NumberDecimal("63.13")},
      { "customer_id": "tj@wheresmyemail.com", "orderdate": ISODate("2019-05-28T19:13:32Z"), "value": NumberDecimal("2.01")},
      { "customer_id": "tj@wheresmyemail.com", "orderdate": ISODate("2020-11-23T22:56:53Z"), "value": NumberDecimal("187.99")},
      { "customer_id": "elise_smith@myemail.com", "orderdate": ISODate("2020-12-26T08:55:46Z"), "value": NumberDecimal("48.50")}
    ]);
    ```
*   **Yêu cầu:** Thống kê dữ liệu khách hàng trong năm 2020, thể hiện: ngày mua hàng đầu tiên, tổng giá trị đơn hàng, và số lần mua.
*   **Lời giải:**
    ```javascript
    db.orders.aggregate([
      // Giai đoạn 1: Lọc các đơn hàng trong năm 2020
      { $match: {
          orderdate: {
            $gte: ISODate("2020-01-01T00:00:00Z"),
            $lt: ISODate("2021-01-01T00:00:00Z")
          }
      }},
      // Giai đoạn 2: Nhóm theo customer_id và tính toán
      { $group: {
          _id: "$customer_id",
          first_purchase_date: { $min: "$orderdate" }, // Lấy ngày mua nhỏ nhất
          total_value: { $sum: "$value" },            // Tính tổng giá trị
          purchase_count: { $sum: 1 }                 // Đếm số lượng document trong nhóm
      }}
    ])
    ```

#### **3. Bài toán 3: Join Dữ liệu giữa các Collection**

*   **Chuẩn bị dữ liệu:**
    ```javascript
    db.products.insertMany([
      { "id": "a1b2c3d4", "name": "Asus Laptop", "category": "ELECTRONICS" },
      { "id": "z9y8x7w6", "name": "The Day Of The Triffids", "category": "BOOKS" },
      { "id": "ff11gg22hh33", "name": "Morphy Richards Food Mixer", "category": "KITCHENWARE" }
    ]);
    // Giả sử collection 'orders' đã có từ bài 2
    ```
*   **Yêu cầu:** Lấy dữ liệu đơn hàng trong năm 2020, nhưng thay vì hiển thị `product_id`, hãy tra cứu và hiển thị `product_name` và `product_category`.
*   **Lời giải:**

    Sơ đồ dưới đây minh họa cách `$lookup` hoạt động như một phép "join" giữa hai collection.

    ```mermaid
    graph TD
        subgraph "Collections"
            Orders["orders<br/>(localField: product_id)"]
            Products["products<br/>(foreignField: id)"]
        end

        subgraph "Aggregation Pipeline"
            Input[Input: Documents từ 'orders'] --> Lookup{"$lookup Stage"}
            Lookup --> Output["Output: Documents từ 'orders'<br/>+ mảng 'product_details' mới"]
        end

        Products -- "Tìm kiếm các document<br/>có 'id' khớp với 'product_id'" --> Lookup
        Input -- "Lấy 'product_id' từ mỗi document" --> Lookup

        style Orders fill:#e3f2fd
        style Products fill:#f3e5f5
        style Lookup fill:#fff3e0
    ```

    ```javascript
    db.orders.aggregate([
      // Giai đoạn 1: Lọc đơn hàng trong năm 2020
      { $match: {
          orderdate: { $gte: ISODate("2020-01-01T00:00:00Z"), $lt: ISODate("2021-01-01T00:00:00Z") }
      }},
      // Giai đoạn 2: Join với collection 'products'
      { $lookup: {
          from: "products",           // Collection để join
          localField: "product_id",   // Trường trong collection 'orders'
          foreignField: "id",       // Trường trong collection 'products'
          as: "product_details"     // Tên mảng chứa kết quả join
      }},
      // Giai đoạn 3: "Làm phẳng" mảng product_details và định hình lại output
      { $project: {
          _id: 0,
          customer_id: 1,
          orderdate: 1,
          value: 1,
          product_name: { $arrayElemAt: ["$product_details.name", 0] },
          product_category: { $arrayElemAt: ["$product_details.category", 0] }
      }}
    ])
    ```

---

### **Giai đoạn 8: Quản trị Bảo mật và Người dùng**

```mermaid
flowchart TD
    subgraph "Authentication Flow in Sharded Cluster"
        CLIENT[Client Application] --> MONGOS[Mongos Router]
        MONGOS --> |"Authenticates with"| CONFIGDB[Config Server]
        CONFIGDB --> |"User credentials stored"| USERDB[(User Database)]
        MONGOS --> |"Forwards authenticated requests"| SHARD1[Shard 1]
        MONGOS --> |"Forwards authenticated requests"| SHARD2[Shard 2]
        MONGOS --> |"Forwards authenticated requests"| SHARD3[Shard 3]
    end
    
    subgraph "Internal Authentication"
        KEYFILE[KeyFile/x.509] --> |"Secures communication"| INTERNAL[Internal Cluster Communication]
        INTERNAL --> CONFIGDB
        INTERNAL --> SHARD1
        INTERNAL --> SHARD2
        INTERNAL --> SHARD3
    end
    
    subgraph "Authorization (RBAC)"
        USERDB --> ROLES[User Roles]
        ROLES --> PRIVILEGES[Specific Privileges]
        PRIVILEGES --> RESOURCES[Database/Collection Resources]
        RESOURCES --> ACTIONS[Allowed Actions]
    end
    
    style CLIENT fill:#e3f2fd
    style MONGOS fill:#fff3e0
    style KEYFILE fill:#ffebee
    style ACTIONS fill:#e8f5e8
```

Một cluster không được bảo mật là một thảm họa. MongoDB cung cấp hệ thống Role-Based Access Control (RBAC) mạnh mẽ để đảm bảo "đúng người, đúng việc".

*   **Mục đích:** Kiểm soát chặt chẽ ai được phép làm gì trên những dữ liệu nào. Xác thực (bạn là ai?) và Ủy quyền (bạn được làm gì?).

⚠️ **BẪY NGƯỜI MỚI - Giai đoạn 8:**
- **Chạy cluster mà không bật `authorization`** → ai cũng có thể truy cập
- **Dùng user `root` cho ứng dụng** → rủi ro lớn khi bị hack
- **Bật `auditLog` trên Community** → mongod không khởi động (chỉ Enterprise/Atlas)
- **Không áp dụng Least Privilege** → user có quyền quá rộng không cần thiết

💡 **NGUYÊN TẮC:** Mỗi user/ứng dụng chỉ nên có những quyền hạn thực sự cần thiết.

#### **1. Tạo User và Gán Role có sẵn**

1.  **Kết nối với quyền admin:** (Như đã làm ở Giai đoạn 3)
    `mongosh --port 27020 sudo -u mongodba --authenticationDatabase admin`
    # Sẽ prompt nhập password an toàn
2.  **Tạo user cho ứng dụng:**
    ```javascript
    use reporting // Chuyển sang DB mà user sẽ thao tác
    db.createUser({
      user: "reportUser",
      pwd: passwordPrompt(), // Sẽ hỏi để nhập mật khẩu an toàn
      roles: [
        { role: "read", db: "reporting" } // Chỉ cho phép đọc trên DB 'reporting'
      ]
    })
    
    use testDB
    db.createUser({
      user: "appUser",
      pwd: passwordPrompt(),
      roles: [
        { role: "readWrite", db: "testDB" } // Cho phép đọc và ghi trên DB 'testDB'
      ]
    })
    ```

#### **2. Tạo Role Tùy chỉnh (Custom Role)**

Đôi khi các role có sẵn (`read`, `readWrite`, `dbAdmin`...) quá rộng. Ta có thể tự tạo role chi tiết hơn.

*   **Yêu cầu:** Tạo một role `inventoryManager` chỉ được phép `find`, `update`, `insert` trên collection `inventory` và chỉ `find` trên collection `orders` trong database `products`.
*   **Thực hiện:**

    Quy trình tạo và gán role tùy chỉnh được minh họa như sau:

    ```mermaid
    flowchart TD
        A["Admin tạo role 'inventoryManager'"] --> B{"Định nghĩa Privileges"}
        B --> P1["Resource: products.inventory<br/>Actions: find, update, insert"]
        B --> P2["Resource: products.orders<br/>Actions: find"]
        
        A --> C["Admin tạo user 'manager01'"]
        C --> D["Gán role 'inventoryManager' cho user"]

        subgraph "Kiểm tra quyền"
            U["User 'manager01' đăng nhập"]
            U --> T1{"Thử: db.inventory.insert()"} --> R1["✅ Thành công"]
            U --> T2{"Thử: db.orders.find()"} --> R2["✅ Thành công"]
            U --> T3{"Thử: db.orders.remove()"} --> R3["❌ Thất bại (Unauthorized)"]
        end

        style A fill:#e3f2fd
        style C fill:#e3f2fd
        style P1 fill:#e8f5e8
        style P2 fill:#e8f5e8
        style R1 fill:#c8e6c9
        style R2 fill:#c8e6c9
        style R3 fill:#ffcdd2
    ```

    ```javascript
    use products
    db.createRole({
      role: "inventoryManager",
      privileges: [
        {
          resource: { db: "products", collection: "inventory" },
          actions: [ "find", "update", "insert" ]
        },
        {
          resource: { db: "products", collection: "orders" },
          actions: [ "find" ]
        }
      ],
      roles: [] // Có thể kế thừa từ các role khác nếu muốn
    })
    
    // Sau khi tạo role, gán nó cho user
    db.createUser({
      user: "manager01",
      pwd: passwordPrompt(),
      roles: [ { role: "inventoryManager", db: "products" } ]
    })
    ```

#### **3. Cấu hình Audit Log (Ghi lại hoạt động)**

⚠️ **CẢNH BÁO MONGODB COMMUNITY EDITION:**
- **Audit Log chỉ khả dụng trong MongoDB Enterprise/Atlas**
- **MongoDB Community KHÔNG hỗ trợ cấu hình `auditLog`**
- **Nếu đang dùng Community, bỏ qua phần này để tránh lỗi cấu hình**

*   **Mục đích:** Ghi lại các sự kiện quan trọng (đăng nhập, thay đổi schema, tạo user...) ra một file log riêng để phục vụ cho việc điều tra an ninh.
*   **Thực hiện đúng (CHỈ DÀNH CHO MONGODB ENTERPRISE/ATLAS):**
    1.  Tạo thư mục cho audit log trên **CẢ 3 MÁY**:
        ```bash
        mkdir /data/audit
        chown mongod:mongod /data/audit
        ```
    2.  Thêm cấu hình `auditLog` vào **tất cả các file config** (`mongod-config.conf`, `mongod-shard1.conf`...):
        ```yaml
        security:
          authorization: enabled
          keyFile: /data/mongo-keyfile
        
        # --- CHỈ THÊM ĐOẠN NÀY NẾU DÙNG ENTERPRISE/ATLAS ---
        auditLog:
          destination: file
          format: JSON
          path: /data/audit/audit.log
        ```
    3.  Khởi động lại tất cả các tiến trình `mongod`. Giờ đây, mọi hành động quan trọng sẽ được ghi lại trong file `/data/audit/audit.log`.

---

### **Giai đoạn 9: Quản trị Nâng cao và Vận hành Replica Set**

```mermaid
sequenceDiagram
    participant User
    participant PRIMARY
    participant Secondary1
    participant Secondary2
    
    User->>PRIMARY: Write Operation
    PRIMARY->>Secondary1: Replicate Oplog
    PRIMARY->>Secondary2: Replicate Oplog
    
    Note right of PRIMARY: PRIMARY fails!
    
    Secondary1->>Secondary2: Request Vote
    Secondary2-->>Secondary1: Vote Granted
    Secondary1->>Secondary1: Becomes New PRIMARY
    
    User->>Secondary1: Write Operation (Success)
```

Dựng replica set chỉ là bước đầu. Vận hành nó trong thực tế đòi hỏi sự hiểu biết về cơ chế bầu cử, failover và các cấu hình đặc biệt.

#### **1. Điều chỉnh Primary (Election)**

```mermaid
flowchart TD
    subgraph "Quy Trình Bầu Cử Replica Set"
        START[Bắt Đầu: Cấu Hình Ưu Tiên Primary] --> CONNECT[Kết Nối Đến Replica Set]
        CONNECT --> GETCONF[Lấy Cấu Hình Hiện Tại]
        GETCONF --> SETPRIORITY[Đặt Ưu Tiên Node]
        SETPRIORITY --> RECONFIG[Cấu Hình Lại Replica Set]
        RECONFIG --> VERIFY[Xác Minh Primary Mới]
        VERIFY --> COMPLETE[Bầu Cử Primary Hoàn Tất]
    end
    
    subgraph "Cấu Hình Ưu Tiên"
        NODE1["mongo-1:27011<br/>Ưu Tiên: 3<br/>(Máy Chủ Mạnh Nhất)"]
        NODE2["mongo-2:27011<br/>Ưu Tiên: 2<br/>(Máy Chủ Trung Bình)"]
        NODE3["mongo-3:27011<br/>Ưu Tiên: 1<br/>(Máy Chủ Yếu Nhất)"]
    end
    
    subgraph "Kịch Bản Bầu Cử"
        SCENARIO1["Hoạt Động Bình Thường<br/>mongo-1 là PRIMARY"]
        SCENARIO2["mongo-1 Hỏng<br/>mongo-2 trở thành PRIMARY"]
        SCENARIO3["mongo-1 & mongo-2 Hỏng<br/>mongo-3 trở thành PRIMARY"]
    end
    
    subgraph "Lệnh"
        CMD1["cfg = rs.conf()"]
        CMD2["cfg.members[0].priority = 3"]
        CMD3["rs.reconfig(cfg)"]
        CMD4["rs.status()"]
    end
    
    subgraph "⚠️ Bẫy Phổ Biến"
        PITFALL1["Tất cả node cùng ưu tiên<br/>❌ Node yếu có thể trở thành PRIMARY"]
        PITFALL2["Ưu tiên quá cao<br/>❌ Có thể gây split-brain"]
        PITFALL3["Quên xác minh<br/>❌ Cấu hình không được áp dụng"]
    end
    
    SETPRIORITY --> CMD1
    SETPRIORITY --> CMD2
    RECONFIG --> CMD3
    VERIFY --> CMD4
    
    style NODE1 fill:#e8f5e8
    style NODE2 fill:#fff3e0
    style NODE3 fill:#ffebee
    style SCENARIO1 fill:#e3f2fd
    style PITFALL1 fill:#ffebee
    style PITFALL2 fill:#ffebee
    style PITFALL3 fill:#ffebee
```

*   **Mục đích:** Đảm bảo node máy chủ mạnh nhất, có độ trễ mạng thấp nhất sẽ được ưu tiên làm `PRIMARY` để tối ưu hiệu năng ghi.
*   **Bẫy người mới:** Để tất cả các node có priority bằng nhau, dẫn đến việc một node yếu có thể được bầu làm `PRIMARY`, ảnh hưởng đến toàn bộ replica set.
*   **Thực hiện đúng:** Kết nối vào replica set (ví dụ shard01) và điều chỉnh `priority`.
    ```bash
    # Kết nối vào một member bất kỳ của shard01
    mongosh --port 27011
    
    # Lấy cấu hình hiện tại
    cfg = rs.conf()
    
    # Giả sử mongo-1 là máy mạnh nhất
    cfg.members[0].priority = 3 // host: "mongo-1:27011"
    cfg.members[1].priority = 2 // host: "mongo-2:27011"
    cfg.members[2].priority = 1 // host: "mongo-3:27011"
    
    # Áp dụng lại cấu hình
    rs.reconfig(cfg)
    ```    Node có `priority` cao nhất sẽ được ưu tiên trong các cuộc bầu cử. Node có `priority: 0` sẽ không bao giờ trở thành `PRIMARY`.

#### **2. Cấu hình Hidden Node**

*   **Mục đích:** Tạo một member ẩn, không được ứng dụng nhìn thấy và không thể trở thành `PRIMARY`. Nó chuyên dùng cho các tác vụ như backup, phân tích dữ liệu mà không ảnh hưởng đến tải của các node chính.
*   **Thực hiện đúng:**

    Kiến trúc với Hidden Node:

    ```mermaid
    graph TD
        subgraph "Replica Set with Hidden Node"
            P[Primary]
            S1[Secondary]
            H[Hidden Secondary<br/>(priority: 0, hidden: true)]
            B[Backup Tool / BI]
        end

        subgraph "Application"
            App[Client App]
        end

        App -- "Read/Write" --> P
        App -- "Read (optional)" --> S1
        App -.->|Không thấy| H

        P -- "Replication" --> S1
        P -- "Replication" --> H

        B -- "Chỉ đọc từ node này" --> H

        style H fill:#f3e5f5,stroke:#7b1fa2
        style B fill:#fff3e0
    end
    ```

    ```javascript
    cfg = rs.conf()
    // Giả sử muốn ẩn node mongo-3
    cfg.members[2].priority = 0
    cfg.members[2].hidden = true
    rs.reconfig(cfg)
    ```

#### **3. Điều chỉnh Kích thước Oplog**

*   **Mục đích:** Oplog (operations log) là một collection đặc biệt lưu lại tất cả các thao tác ghi. Kích thước oplog quyết định "cửa sổ thời gian" mà một node `SECONDARY` có thể offline mà không cần phải resync toàn bộ dữ liệu.
*   **Bẫy người mới:** Để kích thước oplog mặc định. Với hệ thống ghi nhiều, oplog sẽ bị ghi đè rất nhanh, khiến node `SECONDARY` bị lỗi và phải resync, gây tốn tài nguyên.
*   **Thực hiện đúng:** (Làm trên `PRIMARY` của replica set)
    ```javascript
    // Kiểm tra kích thước hiện tại (bytes)
    use local
    db.oplog.rs.stats().maxSize
    
    // Thay đổi kích thước oplog thành 20GB (20000 MB)
    db.adminCommand({ replSetResizeOplog: 1, size: 20000 })
    ```

---

### **Giai đoạn 10: Sao lưu, Phục hồi và Giám sát**

```mermaid
flowchart LR
    subgraph "Backup Strategy"
        A[Production Database] --> B[Full Backup<br/>Daily/Weekly]
        A --> C[Incremental Oplog<br/>Every 15 mins]
        A --> D[Snapshot Backup<br/>Storage Level]
    end
    
    subgraph "Recovery Scenarios"
        E[Complete Disaster] --> F[Full Restore<br/>+ Latest Oplog]
        G[Data Corruption] --> H[Point-in-Time Recovery<br/>PITR]
        I[Accidental Deletion] --> J[Selective Restore<br/>+ Oplog Replay]
    end
    
    subgraph "Monitoring Tools"
        K[mongostat] --> L["Real-time Operations<br/>Insert/Query/Update/Delete"]
        M[mongotop] --> N["Collection-level<br/>Read/Write Times"]
        O[Database Profiler] --> P["Slow Query Analysis<br/>Performance Tuning"]
        Q[currentOp()] --> R["Active Operations<br/>Long-running Queries"]
    end
    
    subgraph "Health Checks"
        S[Process Status] --> T["4 mongod + 1 mongos<br/>per node"]
        U[Replica Set Status] --> V["PRIMARY/SECONDARY<br/>Election Health"]
        W[Shard Status] --> X["Active Shards<br/>Balanced Distribution"]
        Y[Log Analysis] --> Z["ERROR/WARNING<br/>Pattern Detection"]
    end
    
    B --> F
    C --> H
    C --> J
    D --> F
    
    style A fill:#e3f2fd
    style F fill:#e8f5e8
    style H fill:#fff3e0
    style R fill:#ffebee
```

Dữ liệu là tài sản quý giá nhất. Một chiến lược sao lưu và giám sát hiệu quả là bắt buộc.

#### **1. Sao lưu và Phục hồi (`mongodump` / `mongorestore`)**

*   **Mục đích:** Tạo bản sao lưu logic của database để phòng trường hợp xảy ra sự cố.
*   **Thực hiện đúng:**
    *   **Backup toàn bộ database `testDB` (chạy từ một máy client có cài mongo tools):**
        ```bash
        mongodump --host=mongo-1 --port=27020 \
                  sudo -u mongodba --authenticationDatabase admin \
                  --db=testDB --out=/backup/testDB_`date +%F`
        # Sẽ prompt nhập password an toàn
        ```
    *   **Restore database `testDB`:**
        ```bash
        mongorestore --host=mongo-1 --port=27020 \
                     sudo -u mongodba --authenticationDatabase admin \
                     --db=testDB /backup/testDB_YYYY-MM-DD
        # Sẽ prompt nhập password an toàn
        ```

#### **2. Phục hồi tại một thời điểm (Point-in-Time Recovery)**

```mermaid
sequenceDiagram
    participant Admin as Administrator
    participant DB as Database
    participant Backup as Backup System
    participant Oplog as Oplog
    
    Note over Admin, Oplog: Normal Operation
    Admin->>DB: Regular Operations
    DB->>Oplog: Write to Oplog
    Backup->>Oplog: Backup Oplog (Hourly)
    
    Note over Admin, Oplog: Disaster Scenario
    Admin->>DB: Accidental DELETE/UPDATE
    Note right of DB: Data Corruption at 10:30 AM
    
    Admin->>Backup: Identify Last Good Backup
    Backup-->>Admin: Full Backup (9:00 AM)
    
    Admin->>Oplog: Find Timestamp Before Error
    Oplog-->>Admin: Timestamp: 1729073314:3
    
    Admin->>DB: Restore Full Backup
    Admin->>DB: Replay Oplog to Timestamp
    DB-->>Admin: Data Restored to 10:29 AM
    
    Note over Admin, Oplog: Recovery Complete
```

```mermaid
flowchart TD
    subgraph "PITR Process Flow"
        START[Disaster Detected] --> IDENTIFY[Identify Error Time]
        IDENTIFY --> FIND[Find Last Good Backup]
        FIND --> RESTORE[Restore Full Backup]
        RESTORE --> REPLAY[Replay Oplog to Timestamp]
        REPLAY --> VERIFY[Verify Data Integrity]
        VERIFY --> COMPLETE[Recovery Complete]
    end
    
    subgraph "Backup Strategy"
        DAILY[Daily Full Backup<br/>9:00 AM]
        HOURLY[Hourly Oplog Backup<br/>Every Hour]
        CONTINUOUS[Continuous Oplog<br/>Real-time]
    end
    
    subgraph "Recovery Scenarios"
        SCENARIO1[Error at 10:30 AM<br/>Restore to 10:29 AM]
        SCENARIO2[Error at 2:00 PM<br/>Restore to 1:59 PM]
        SCENARIO3[Error at 8:00 AM<br/>Restore to 7:59 AM]
    end
    
    subgraph "Commands"
        CMD1["mongodump --oplog"]
        CMD2["mongorestore --oplogReplay"]
        CMD3["--oplogLimit=timestamp"]
    end
    
    subgraph "⚠️ Common Pitfalls"
        PITFALL1["No Oplog Backup<br/>❌ Can't do PITR"]
        PITFALL2["Wrong Timestamp<br/>❌ Data still corrupted"]
        PITFALL3["Incomplete Oplog<br/>❌ Partial recovery"]
    end
    
    FIND --> DAILY
    REPLAY --> HOURLY
    REPLAY --> CONTINUOUS
    
    style SCENARIO1 fill:#e3f2fd
    style SCENARIO2 fill:#fff3e0
    style SCENARIO3 fill:#e8f5e8
    style PITFALL1 fill:#ffebee
    style PITFALL2 fill:#ffebee
    style PITFALL3 fill:#ffebee
```

*   **Mục đích:** Cứu dữ liệu khi có người lỡ tay `DELETE` hoặc `UPDATE` sai. Kỹ thuật này cho phép khôi phục lại trạng thái của database *ngay trước* khi sự cố xảy ra.
*   **Bẫy người mới:** Chỉ có backup hàng đêm. Nếu sai sót xảy ra lúc 9 giờ sáng, bạn sẽ mất toàn bộ dữ liệu từ đêm hôm trước.
*   **Thực hiện đúng (Quy trình):**
    1.  **Luôn có một bản backup Oplog gần nhất:**
        ```bash
        # Lệnh này nên được chạy định kỳ (ví dụ mỗi giờ)
        mongodump --host=mongo-1 --port=27011 \
                  -d local -c oplog.rs --out /backup/oplogs
        ```    2.  **Khi có sự cố (ví dụ: xóa nhầm lúc 10:30:00 AM):**
        *   Tìm `timestamp` của thời điểm ngay trước khi xóa trong bản backup oplog.
    3.  **Restore:**
        *   Khôi phục từ bản backup đầy đủ gần nhất.
        *   Dùng `mongorestore` với cờ `--oplogReplay` và `--oplogLimit` để áp dụng lại các thay đổi từ oplog cho đến thời điểm mong muốn.
        ```bash
        # Ví dụ restore tới timestamp 1729073314:3
        mongorestore --port 27017 --oplogReplay \
                     --oplogLimit=1729073314:3 \
                     /backup/mongo/local/oplog.rs.bson
        ```

#### **3. Giám sát Hiệu năng**

Sơ đồ dưới đây tóm tắt các công cụ giám sát dòng lệnh và mục đích sử dụng của chúng.

```mermaid
graph TD
    subgraph "Real-time Monitoring Tools"
        A[mongostat] --> B["- insert, query, update, delete rates<br/>- faults, locked %<br/>- network traffic, connections"]
        C[mongotop] --> D["- Time spent on read/write<br/>- Per collection basis<br/>- Identifies hot collections"]
    end

    subgraph "Deep Dive & Troubleshooting"
        E[Database Profiler] --> F["- Captures slow operations (>N ms)<br/>- Detailed execution stats<br/>- Helps find queries needing indexes"]
        G[db.currentOp()] --> H["- Shows all active operations<br/>- Identifies long-running queries<br/>- Can be used to kill operations"]
    end

    style A fill:#e3f2fd
    style C fill:#e3f2fd
    style E fill:#fff3e0
    style G fill:#fff3e0
end
```

*   **Mục đích:** Theo dõi sức khỏe hệ thống, phát hiện các "điểm nóng" và truy vấn chậm.
*   **Công cụ dòng lệnh:**
    *   `mongostat`: Cung cấp cái nhìn tổng quan theo thời gian thực về các hoạt động (inserts, queries, updates, deletes...), lỗi, và hàng đợi.
        ```bash
        mongostat --host mongo-1 --port 27020 sudo -u mongodba --authenticationDatabase admin
        # Sẽ prompt nhập password an toàn
        ```
    *   `mongotop`: Hiển thị thời gian đọc/ghi trên từng collection, giúp bạn biết collection nào đang hoạt động nhiều nhất.
        ```bash
        mongotop --host mongo-1 --port 27020 sudo -u mongodba --authenticationDatabase admin
        # Sẽ prompt nhập password an toàn
        ```
*   **Database Profiler (Tìm truy vấn chậm):**
    1.  **Bật profiler:** Ghi lại các truy vấn chạy chậm hơn 100ms.
        ```javascript
        use testDB
        db.setProfilingLevel(1, { slowms: 100 })
        ```
    2.  **Xem các truy vấn chậm:**
        ```javascript
        db.system.profile.find().pretty()
        ```
    3.  **Tắt profiler:** `db.setProfilingLevel(0)`

---

### **Giai đoạn 11: Tối ưu Hiệu năng Truy vấn với Index**

```mermaid
graph TD
    subgraph NoIndex ["🔍 Truy vấn KHÔNG có Index"]
        Q1["📝 Truy vấn"] --> P1{"🧠 Query Planner"}
        P1 --> S1["📚 Collection Scan<br/>💔 Đọc TOÀN BỘ collection<br/>⏱️ Chậm và tốn tài nguyên"]
        S1 --> R1["🐌 Kết quả chậm"]
    end
    
    subgraph WithIndex ["⚡ Truy vấn CÓ Index"]
        Q2["📝 Truy vấn"] --> P2{"🧠 Query Planner"}
        P2 --> S2["🎯 Index Scan<br/>✨ Tra cứu và nhảy thẳng đến dữ liệu<br/>🚀 Nhanh và hiệu quả"]
        S2 --> R2["⚡ Kết quả nhanh"]
    end
    
    %% So sánh trực tiếp
    Q1 -.->|"So sánh hiệu suất"| Q2
    R1 -.->|"Chênh lệch tốc độ"| R2
    
    %% Styling cho nodes
    style Q1 fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    style Q2 fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    style P1 fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style P2 fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style S1 fill:#ffebee,stroke:#d32f2f,stroke-width:3px
    style S2 fill:#e8f5e8,stroke:#388e3c,stroke-width:3px
    style R1 fill:#ffcdd2,stroke:#c62828,stroke-width:2px
    style R2 fill:#c8e6c9,stroke:#2e7d32,stroke-width:2px
    
    %% Styling cho subgraphs
    style NoIndex fill:#fce4ec,stroke:#ad1457,stroke-width:2px
    style WithIndex fill:#e0f2f1,stroke:#00695c,stroke-width:2px
```

Nếu Sharding là giải pháp cho bài toán *dung lượng* (scale-out), thì Index (chỉ mục) là giải pháp cho bài toán *tốc độ* truy vấn. Một truy vấn không có index sẽ phải quét toàn bộ collection (Full Collection Scan), cực kỳ chậm và tốn tài nguyên.

*   **Mục đích:** Tạo ra các cấu trúc dữ liệu đặc biệt (thường là B-Tree) giúp MongoDB nhanh chóng định vị các document cần thiết mà không cần phải đọc qua từng document một. Tương tự như mục lục của một cuốn sách.
*   **Bẫy người mới:**
    *   Bỏ qua việc tạo index, dẫn đến hiệu năng thảm họa khi dữ liệu lớn dần.
    *   Tạo index một cách bừa bãi trên mọi trường. Mỗi index đều tốn RAM và làm chậm các thao tác ghi (insert, update, delete).
*   **Thực hiện đúng:** Phân tích các truy vấn thường xuyên của ứng dụng và chỉ tạo index để phục vụ cho các truy vấn đó.

#### **1. Các loại Index cơ bản và cách tạo**

*   **Kết nối vào Mongos để thực hiện:**
    ```bash
    mongosh --port 27020 sudo -u mongodba --authenticationDatabase admin
    # Sẽ prompt nhập password an toàn
    use testDB
    ```

*   **Single Field Index (Chỉ mục trên một trường):**
    ```javascript
    // Tạo index trên trường 'vocation' của collection 'persons'
    // 1: sắp xếp tăng dần, -1: sắp xếp giảm dần
    db.persons.createIndex({ "vocation": 1 })
    ```
    *Phục vụ cho các truy vấn như:* `db.persons.find({ vocation: "ENGINEER" })`

*   **Compound Index (Chỉ mục phức hợp trên nhiều trường):**
    ```javascript
    // Tạo index trên 'vocation' và 'lastname'
    db.persons.createIndex({ "vocation": 1, "lastname": 1 })
    ```
    *Quan trọng:* Thứ tự các trường trong index rất quan trọng. Index này sẽ phục vụ tốt cho:
    1.  `db.persons.find({ vocation: "ENGINEER" })`
    2.  `db.persons.find({ vocation: "ENGINEER", lastname: "Smith" })`
    *Nhưng sẽ **không** phục vụ tốt cho:* `db.persons.find({ lastname: "Smith" })`

*   **Text Index (Chỉ mục văn bản):** Dùng cho tìm kiếm toàn văn bản (full-text search).
    ```javascript
    // Chuẩn bị dữ liệu
    db.articles.insertOne({ title: "MongoDB Performance Tuning", content: "Indexing is a key concept for tuning databases." })
    db.articles.insertOne({ title: "Sharding Guide", content: "A guide to horizontal scaling with MongoDB." })
    
    // Tạo text index trên trường 'content'
    db.articles.createIndex({ "content": "text" })
    
    // Tìm kiếm
    db.articles.find({ $text: { $search: "tuning scaling" } }) // Sẽ tìm các document chứa 'tuning' hoặc 'scaling'
    ```

#### **2. Phân tích Kế hoạch Thực thi (Execution Plan)**

Làm sao để biết một truy vấn có đang sử dụng index hay không? Hãy dùng lệnh `explain()`.

*   **Thực hiện:**
    ```javascript
    db.persons.find({ vocation: "ENGINEER", lastname: "Smith" }).explain("executionStats")
    ```*   **Phân tích kết quả:**
    Bạn cần tìm đến mục `executionStats.winningPlan`.
    *   **TỐT (`IXSCAN`):** Nếu `stage` là `IXSCAN` (Index Scan), nghĩa là truy vấn của bạn đã sử dụng index.
        ```json
        "winningPlan": {
          "stage": "FETCH",
          "inputStage": {
            "stage": "IXSCAN", // <-- Tuyệt vời!
            "indexName": "vocation_1_lastname_1",
            // ...
          }
        }
        ```
    *   **TỆ (`COLLSCAN`):** Nếu `stage` là `COLLSCAN` (Collection Scan), nghĩa là MongoDB đã phải đọc toàn bộ collection. Bạn cần phải xem lại và tạo index phù hợp.
        ```json
        "winningPlan": {
          "stage": "COLLSCAN", // <-- Cảnh báo hiệu năng!
          "filter": {
             // ...
          }
        }
        ```

---

### **Giai đoạn 12: Chiến lược Sharding và Phân phối Dữ liệu**

```mermaid
flowchart TD
    subgraph "Quy Trình Phân Bố Dữ Liệu"
        A[Tài Liệu Mới] --> B{"Phân Tích Khóa Phân Mảnh"}
        B --> C["Tính Toán Chunk Đích"]
        C --> D["Định Tuyến Đến Shard Phù Hợp"]
        D --> E["Tài Liệu Được Lưu Trữ"]
    end
    
    subgraph "Quản Lý Chunk"
        F["Giám Sát Kích Thước Chunk"] --> G{"Chunk > 64MB?"}
        G -->|Có| H["Chia Nhỏ Chunk"]
        G -->|Không| I["Tiếp Tục Giám Sát"]
        H --> J["Tạo Chunk Mới"]
        J --> K["Cập Nhật Máy Chủ Cấu Hình"]
    end
    
    subgraph "Quy Trình Cân Bằng"
        L["Dịch Vụ Cân Bằng"] --> M{"Kiểm Tra Phân Bố Shard"}
        M --> N{"Mất Cân Bằng?"}
        N -->|Có| O["Chọn Chunks Để Di Chuyển"]
        N -->|Không| P["Chờ Chu Kỳ Tiếp Theo"]
        O --> Q["Di Chuyển Chunks"]
        Q --> R["Cập Nhật Siêu Dữ Liệu"]
        R --> S["Cân Bằng Được Khôi Phục"]
    end
    
    subgraph "Chiến Lược Khóa Phân Mảnh"
        T["Phân Mảnh Băm"] --> U["Phân Bố Đều<br/>Truy Cập Ngẫu Nhiên"]
        V["Phân Mảnh Theo Dải"] --> W["Truy Vấn Có Mục Tiêu<br/>Rủi Ro Điểm Nóng"]
    end
    
    E --> F
    K --> L
    S --> P
    
    style A fill:#e3f2fd
    style E fill:#e8f5e8
    style O fill:#fff3e0
    style S fill:#c8e6c9
    style U fill:#e1f5fe
    style W fill:#fff8e1
```

Chúng ta đã dựng cluster sharding, nhưng việc phân chia dữ liệu diễn ra như thế nào? "Bộ não" của quá trình này nằm ở **Shard Key** và tiến trình **Balancer**.

*   **Mục đích:** Chọn một "khóa phân mảnh" (Shard Key) tốt để dữ liệu được phân phối đều trên các Shard, tránh tình trạng "shard nóng, shard lạnh" (hotspot), và tối ưu hóa cả việc ghi và đọc.
*   **Các khái niệm cốt lõi:**
    *   **Shard Key:** Một hoặc nhiều trường trong document được dùng để quyết định document đó sẽ thuộc về "vùng" dữ liệu nào. **Một khi đã chọn, Shard Key không thể thay đổi.**
    *   **Chunk:** Một dải liên tiếp các giá trị của Shard Key. MongoDB sẽ tự động chia dữ liệu thành các chunk (mặc định 64MB).
    *   **Balancer:** Một tiến trình chạy ngầm, theo dõi sự phân bố của các chunk trên các shard. Nếu một shard có quá nhiều chunk so với các shard khác, Balancer sẽ tự động di chuyển (migrate) chunk để cân bằng lại.

#### **1. Lựa chọn Chiến lược Shard Key**

Đây là quyết định quan trọng nhất khi thiết kế một hệ thống sharding.

*   **Hashed Sharding (Phân mảnh Băm):**
    *   **Cách hoạt động:** MongoDB tính toán một giá trị băm (hash) của Shard Key và dùng giá trị này để phân chia dữ liệu.
    *   **Ví dụ:** `sh.shardCollection("testDB.myCollection", { "_id": "hashed" } )` (Như đã làm ở Giai đoạn 4)
    *   **Ưu điểm:** Đảm bảo dữ liệu ghi được phân phối **cực kỳ đều** trên tất cả các shard. Rất tốt cho các workload ghi nặng (heavy write).
    *   **Nhược điểm:** Các truy vấn theo dải (range query) sẽ không hiệu quả, vì các document có giá trị `_id` gần nhau lại nằm trên các shard khác nhau. `mongos` sẽ phải hỏi tất cả các shard.

*   **Ranged Sharding (Phân mảnh theo Dải):**
    *   **Cách hoạt động:** MongoDB chia dữ liệu dựa trên giá trị thực của Shard Key. Các document có giá trị key gần nhau sẽ nằm chung một chunk và trên cùng một shard.
    *   **Ví dụ:** `sh.shardCollection("events.logs", { "timestamp": 1 } )`
    *   **Ưu điểm:** Rất hiệu quả cho các truy vấn theo dải. Ví dụ: "lấy tất cả log trong 1 giờ qua" sẽ chỉ cần hỏi một hoặc hai shard.
    *   **Nhược điểm:** **Rủi ro tạo ra Hotspot.** Nếu Shard Key là một trường tăng đơn điệu (như `timestamp` hoặc `_id` mặc định), tất cả các lượt ghi mới sẽ luôn đi vào chunk cuối cùng trên một shard duy nhất, gây quá tải cho shard đó.

#### **2. Bẫy người mới khi chọn Shard Key**

⚠️ **BẪY NGƯỜI MỚI - Giai đoạn 12:**
- **Chọn `_id` mặc định với Ranged Sharding** → hot shard (đây là lỗi kinh điển)
- **Chọn key cardinality thấp** (ví dụ: `country` khi 90% là Việt Nam) → jumbo chunk
- **Khẳng định chunk size cứng 64MB** → khác theo version
- **Quên rằng Shard Key là bất biến** → không thể thay đổi sau khi sharding

💡 **MẸO:** Kiểm tra `maxChunkSizeBytes` hoặc tài liệu version đang chạy thay vì giả định.

Sơ đồ dưới đây minh họa vấn đề "hot shard" kinh điển khi sử dụng Ranged Sharding trên một khóa tăng đơn điệu.

```mermaid
graph TD
    subgraph "Ranged Sharding on Monotonic Key (e.g., Timestamp)"
        direction LR
        S1[Shard 1<br/>Chunks for Jan-Mar]
        S2[Shard 2<br/>Chunks for Apr-Jun]
        S3[Shard 3<br/>Chunks for Jul-Sep]
        S4["🔥 Hot Shard 4<br/>Chunks for Oct-Dec"]
    end

    subgraph "Write Operations"
        W1["New write (Oct 1)"] --> S4
        W2["New write (Oct 2)"] --> S4
        W3["New write (Nov 5)"] --> S4
    end

    Note over S4: "Tất cả các lượt ghi mới<br/>đều dồn vào đây!"

    style S4 fill:#ffcdd2,stroke:#c62828
end
```

*   **Chọn `_id` mặc định với Ranged Sharding:** Đây là lỗi kinh điển. `_id` của MongoDB có chứa timestamp và luôn tăng. Kết quả là tạo ra một "hot shard" hứng chịu toàn bộ lưu lượng ghi.
*   **Chọn một key có số lượng giá trị thấp (Low Cardinality):** Ví dụ, sharding collection người dùng theo trường `country` trong khi 90% người dùng đến từ "Việt Nam". Điều này sẽ tạo ra một chunk khổng lồ không thể chia tách (jumbo chunk) và không thể cân bằng.
*   **Quên rằng Shard Key là bất biến:** Không thể thay đổi Shard Key của một collection sau khi đã sharding. Nếu chọn sai, cách duy nhất để sửa là tạo một collection mới, sharding lại với key đúng, và di chuyển toàn bộ dữ liệu sang.

#### **3. Kiểm tra Phân phối Dữ liệu**

*   **Kết nối vào `mongos` và chạy các lệnh sau:**
    ```javascript
    use testDB
    
    // Xem thống kê phân phối của collection
    db.myCollection.getShardDistribution()
    
    // Xem trạng thái tổng quan của cluster, bao gồm các chunk và balancer
    sh.status()
    ```
    Hãy chú ý đến số lượng chunk trên mỗi shard trong kết quả của `sh.status()`. Nếu chúng chênh lệch quá nhiều, có thể Shard Key của bạn chưa tối ưu.

---

### **Giai đoạn 13: Giám sát Nâng cao và Tự động hóa Vận hành**

```mermaid
flowchart TD
    A["👨‍💼 Quản Trị Viên<br/>🔧 Người quản lý hệ thống"] -->|"⚡ Chạy script giám sát"| B{"📊 db.currentOp()<br/>🔍 Kiểm tra tác vụ hiện tại"}
    
    B --> C{"🧐 Phân tích tác vụ<br/>⏱️ Đánh giá thời gian thực thi"}
    
    C -->|"✅ Tác vụ bình thường < 60s"| D["✨ Trạng thái OK<br/>📝 Ghi log bình thường"]
    
    C -->|"⚠️ Phát hiện truy vấn chậm > 60s"| E["🎯 Lấy Operation ID<br/>📋 Thu thập thông tin opid"]
    
    E --> F{"💀 db.killOp(opid)<br/>🔪 Thực hiện hạ sát tác vụ"}
    
    F -->|"✅ Thành công"| G["🎉 Hạ sát thành công<br/>📊 Cập nhật metrics"]
    
    F -->|"❌ Thất bại"| H["⚠️ Không thể hạ sát<br/>🚨 Cảnh báo Quản Trị Viên"]
    
    G --> I["📝 Ghi log thành công<br/>📈 Cập nhật dashboard"]
    H --> J["🔔 Gửi cảnh báo<br/>📞 Thông báo khẩn cấp"]
    
    D --> K["🔄 Tiếp tục giám sát"]
    I --> K
    J --> K
    
    K -->|"🕐 Chu kỳ giám sát"| B
    
    %% Styling cho các nodes chính
    style A fill:#e3f2fd,stroke:#1976d2,stroke-width:3px
    style B fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style C fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    
    %% Styling cho kết quả bình thường
    style D fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    style K fill:#e0f2f1,stroke:#00695c,stroke-width:2px
    
    %% Styling cho trường hợp cảnh báo
    style E fill:#fff8e1,stroke:#ffa000,stroke-width:3px
    style F fill:#ffebee,stroke:#d32f2f,stroke-width:3px
    
    %% Styling cho kết quả
    style G fill:#c8e6c9,stroke:#2e7d32,stroke-width:2px
    style H fill:#ffcdd2,stroke:#c62828,stroke-width:3px
    style I fill:#e1f5fe,stroke:#0277bd,stroke-width:2px
    style J fill:#fce4ec,stroke:#ad1457,stroke-width:3px
```

`mongostat` và `mongotop` cho ta cái nhìn tổng quan, nhưng khi một sự cố hiệu năng xảy ra, ta cần các công cụ "phẫu thuật" để xem chính xác *việc gì* đang diễn ra bên trong và can thiệp nếu cần.

*   **Mục đích:** Chủ động kiểm tra các tác vụ đang chạy trong thời gian thực, xác định các truy vấn "treo" hoặc quá chậm đang chiếm giữ tài nguyên, và có khả năng "hạ sát" (kill) chúng một cách an toàn để giải cứu hệ thống.
*   **Bẫy người mới:** Chỉ phản ứng khi hệ thống đã "đứng hình". Không biết cách xem các session đang hoạt động hoặc cho rằng cách duy nhất để xử lý truy vấn treo là khởi động lại `mongod`, gây gián đoạn dịch vụ không cần thiết.
*   **Thực hiện đúng:** Sử dụng `db.currentOp()` thường xuyên và có kịch bản (script) sẵn sàng để xử lý các tình huống khẩn cấp.

#### **1. Phân tích các Tác vụ đang chạy (`db.currentOp`)**

Lệnh này giống như `show processlist` trong MySQL hay `top` trên Linux, nó cho bạn thấy mọi thứ đang được thực thi bởi server ngay tại thời điểm đó.

*   **Yêu cầu:** Tìm tất cả các truy vấn từ user ứng dụng đang chạy quá 10 giây.
*   **Thực hiện (kết nối vào `mongos`):**
    ```javascript
    // Script: get_sql_running.js
    var c=1;
    db.currentOp({
      "active": true,
      "secs_running": { "$gt": 10 },
      // "op": "query", // Có thể lọc cụ thể hơn theo loại tác vụ
      // "ns": "testDB.myCollection" // Lọc theo collection
    }).inprog.forEach(function(op) {
      print("--- Slow Op " + c + " ---");
      print("opid: " + op.opid);
      print("active_secs: " + op.secs_running);
      print("namespace: " + op.ns);
      print("operation: " + op.op);
      printjson(op.command);
      c++;
    })
    ```
    Bạn có thể lưu script trên vào file `get_sql_running.js` và chạy bằng `mongosh --port 27020 -u ... -f get_sql_running.js`.

#### **2. "Hạ sát" các Truy vấn Gây hại (`db.killOp`)**

Khi bạn đã xác định được `opid` (Operation ID) của một truy vấn gây hại, bạn có thể chấm dứt nó.

*   **CẢNH BÁO:** Đây là một hành động mạnh. Cần cân nhắc kỹ lưỡng, đặc biệt là khi kill các thao tác ghi. Việc kill một thao tác ghi có thể để lại dữ liệu ở trạng thái không nhất quán tạm thời trước khi được rollback. Luôn ưu tiên tối ưu truy vấn và index hơn là phải đi kill chúng.

*   **Thực hiện đúng (Script an toàn):** Tài liệu của bạn cung cấp một script rất hay, nó gói logic này vào một hàm an toàn.
    ```javascript
    // Script: killLongRunningOps.js
    function killLongRunningOps(maxSecsRunning) {
      print("Searching for ops running longer than " + maxSecsRunning + " seconds...");
      let opsToKill = db.currentOp({
        "active": true,
        "secs_running": { "$gt": maxSecsRunning },
        "op": { "$in": ["query", "getmore"] } // Chỉ tập trung vào các truy vấn đọc
      }).inprog;
    
      for (let op of opsToKill) {
        // Cực kỳ quan trọng: Không kill các tác vụ hệ thống
        if (!op.ns.startsWith("local") && !op.ns.startsWith("config")) {
          print(`Killing opId: ${op.opid} on ${op.ns}, running for ${op.secs_running}s`);
          db.killOp(op.opid);
        }
      }
    };
    
    // Ví dụ: Kill tất cả truy vấn đọc chạy quá 60 giây
    killLongRunningOps(60);
    ```

#### **3. Tự động hóa Giám sát Tình trạng Replica Set**

*   **Mục đích:** Viết các script nhỏ để kiểm tra các chỉ số sức khỏe quan trọng như "replication lag" (độ trễ sao chép) và tình trạng của các node secondary. Các script này có thể được tích hợp vào các hệ thống giám sát như Zabbix, Nagios, hoặc một cron job đơn giản.
*   **Script kiểm tra Replication Lag:**
    ```javascript
    // Script: check_replica_lag.js
    // Chạy script này trên từng PRIMARY của mỗi shard
    // Ví dụ: mongosh --port 27011 -f check_replica_lag.js
    printjson(rs.printSecondaryReplicationInfo());
    ```
    *Kết quả sẽ cho bạn biết mỗi node secondary đang "chậm chân" hơn primary bao nhiêu giây. Nếu con số này tăng đột biến, đó là dấu hiệu của sự cố mạng hoặc quá tải trên secondary.*
*   **Script đếm số lượng node Secondary:**
    ```javascript
    // Script: check_secondary_count.js
    // Chạy trên một member bất kỳ của replica set
    var secondaryCount = 0;
    var status = rs.status();
    for (var i = 0; i < status.members.length; i++) {
      if (status.members[i].stateStr === "SECONDARY") {
        secondaryCount++;
      }
    }
    print("Number of active secondaries: " + secondaryCount);
    ```
    *Nếu số lượng secondary ít hơn mong đợi, hệ thống của bạn đã mất đi một phần khả năng chịu lỗi (failover).*

---

### **Giai đoạn 14: Kỹ thuật Phục hồi Nâng cao - Point-in-Time Recovery (PITR)**

⚠️ **CẢNH BÁO SHARDED CLUSTER:**
- **`mongorestore --oplogReplay` chỉ áp dụng cho MỘT replica set**
- **Với sharded cluster, cần đồng bộ TỪNG SHARD hoặc dùng giải pháp chuyên dụng**
- **ĐỪNG dump oplog từ `mongos` rồi kỳ vọng replay cho cả cụm**

```mermaid
graph TD
    subgraph "⚠️ PITR in Sharded Cluster is Complex!"
        A["mongorestore --oplogReplay"] -- "Chỉ hoạt động trên" --> B["MỘT Replica Set (một shard)"]
        B -.-> C["Không thể áp dụng<br/>cho toàn bộ cluster<br/>cùng một lúc"]
        C --> D{"Yêu cầu<br/>- Backup đồng bộ từ tất cả các shard<br/>- Phối hợp timestamp chính xác<br/>- Hoặc dùng công cụ chuyên dụng"}
    end

    style A fill:#fff3e0
    style B fill:#e8f5e8
    style C fill:#ffcdd2,stroke:#c62828
    style D fill:#e3f2fd
end
```

💡 **PITR SHARDED CLUSTER đúng cách:**
- Full backup đồng bộ + oplog từng shard
- Kiểm soát timestamp phối hợp
- Hoặc dùng MongoDB Atlas/Ops Manager backup

```mermaid
sequenceDiagram
    participant Admin
    participant BackupServer
    participant TempMongo
    participant ProductionMongo
    
    Admin->>ProductionMongo: Xóa nhầm dữ liệu lúc 10:30:00
    Note over Admin: "Hoảng hốt!"
    
    Admin->>BackupServer: Lấy bản full backup (2h sáng)
    Admin->>BackupServer: Lấy bản backup Oplog gần nhất
    
    Admin->>TempMongo: Restore bản full backup
    Admin->>TempMongo: Dùng mongorestore --oplogReplay<br/>--oplogLimit="10:29:59"
    Note over TempMongo: Dữ liệu được khôi phục<br/>tại thời điểm 10:29:59
    
    Admin->>TempMongo: Kiểm tra và xác thực dữ liệu
    Admin->>ProductionMongo: Lên kế hoạch<br/>chuyển dữ liệu đã cứu về lại
```

Đây là kỹ năng "cứu hoả" tối thượng. Khi một lập trình viên lỡ tay `DROP collection` hoặc chạy một lệnh `UPDATE` sai làm hỏng toàn bộ dữ liệu, việc restore từ bản backup đêm qua có thể làm mất hàng giờ dữ liệu quý giá. PITR cho phép bạn quay ngược thời gian về đúng *giây* trước khi thảm hoạ xảy ra.

*   **Mục đích:** Khôi phục database về một thời điểm cụ thể trong quá khứ, giảm thiểu tối đa việc mất dữ liệu (Recovery Point Objective ~ 0).
*   **Bẫy người mới:** Không có chiến lược backup oplog. Chỉ khi sự cố xảy ra mới nhận ra rằng mình không có nguyên liệu để thực hiện PITR.
*   **Thực hiện đúng:** Xây dựng một quy trình chặt chẽ bao gồm: sao lưu đầy đủ định kỳ và sao lưu oplog liên tục (hoặc rất thường xuyên).

#### **Quy trình Cứu dữ liệu khi Xóa nhầm**

**Kịch bản:** Vào lúc **10:45:15 sáng**, một collection quan trọng đã bị xóa nhầm. Bản full backup gần nhất là lúc **2:00 sáng**. Bạn có một cronjob chạy `mongodump` để backup oplog mỗi 15 phút.

1.  **HÀNH ĐỘNG ĐẦU TIÊN: BÌNH TĨNH VÀ CÔ LẬP.**
    *   Ngăn chặn ngay lập tức mọi kết nối từ ứng dụng tới database để tránh các sai sót phát sinh thêm.

2.  **CHUẨN BỊ MÔI TRƯỜNG PHỤC HỒI:**
    *   Dựng một instance MongoDB tạm thời trên một server khác. **Tuyệt đối không restore đè lên server production đang gặp sự cố.**

3.  **PHỤC HỒI TỪ BẢN FULL BACKUP:**
    *   Trên server tạm, chạy lệnh restore từ bản backup lúc 2:00 sáng.
        ```bash
        mongorestore --host=localhost --port=27017 /path/to/full_backup_0200AM
        ```

4.  **TÌM TIMESTAMP CỦA "TỘI ÁC":**
    *   Chúng ta cần tìm timestamp chính xác tương ứng với thời điểm ngay trước 10:45:15.
    *   Lấy file backup oplog gần nhất (ví dụ bản lúc 11:00, chứa các log từ 10:45 đến 11:00).
    *   Dùng `bsondump` để chuyển file BSON của oplog sang định dạng JSON có thể đọc được.
        ```bash
        bsondump /backup/mongo/local/oplog.rs.bson > oplog.json
        ```
    *   Mở file `oplog.json` và tìm kiếm. Mỗi entry sẽ có một trường `"ts"` (timestamp) dạng `Timestamp(1678852000, 1)`. Con số đầu tiên là Unix timestamp. Bạn cần tìm entry ngay trước thời điểm xảy ra sự cố. Giả sử bạn tìm được timestamp giới hạn là `1729073314:3`.

5.  **ÁP DỤNG OPLOG (Bước Ma thuật):**
    *   Bây giờ, trên server tạm, bạn sẽ "chiếu lại" các thay đổi từ oplog, bắt đầu từ thời điểm của bản full backup và dừng lại *ngay trước* khi lệnh xóa được thực thi.
        ```bash
        # Tạo user restore đặc biệt nếu cần (như trong tài liệu)
        # ...
        
        # Chạy lệnh restore oplog
        mongorestore --port 27017 \
                     --oplogReplay --oplogLimit=1729073314:3 \
                     /backup/mongo/local/oplog.rs.bson
        ```
        *   `--oplogReplay`: Yêu cầu `mongorestore` áp dụng các entry trong file oplog.
        *   `--oplogLimit`: Đây chính là chiếc "cỗ máy thời gian". Nó sẽ dừng lại ở timestamp được chỉ định.

6.  **XÁC THỰC VÀ HOÀN TẤT:**
    *   Kết nối vào server tạm, kiểm tra kỹ lưỡng xem collection đã quay trở lại và dữ liệu đã đúng trạng thái tại thời điểm 10:45:14 hay chưa.
    *   Khi đã chắc chắn 100%, bạn có thể lên kế hoạch để di chuyển dữ liệu đã được cứu này về lại môi trường production.

Quy trình này phức tạp nhưng là cứu cánh trong những tình huống nguy cấp nhất. Việc thực hành nó trong môi trường test là bắt buộc để bạn luôn sẵn sàng khi cần.

Bạn đã đi đến phần cuối cùng của những kiến thức cốt lõi có trong tài liệu thực hành rồi đấy. Chúng ta đã tích hợp gần như toàn bộ các bài thực hành từ Day 1 đến Day 5, từ cài đặt, truy vấn, bảo mật, vận hành replica set, sharding, cho đến các kịch bản giám sát và phục hồi.

Chủ đề cuối cùng được đề cập trong tài liệu của bạn là một phần tinh chỉnh hiệu năng cực kỳ quan trọng, liên quan trực tiếp đến cách MongoDB sử dụng bộ nhớ (RAM). Hãy hoàn thiện nó với giai đoạn cuối cùng này.

---

### **Giai đoạn 15: Tinh chỉnh Tầng Lưu trữ (Storage Engine Tuning)**

```mermaid
graph TD
    subgraph "Server RAM"
        A[OS File System Cache]
        B[WiredTiger Internal Cache]
        C[Other Processes]
    end

    style B fill:#e3f2fd

    D(Working Set - Dữ liệu & Index thường xuyên truy cập)
    E(Truy vấn) --> |Cần dữ liệu| D
    D -->|Data in RAM?| F{Cache Check}
    F -->|Yes| B
    F -->|No| A
    B --> |Data found| G[Fast Response]
    A --> |Data found| H[Slightly Slower Response]
```

MongoDB, cụ thể là storage engine WiredTiger, sử dụng hai cơ chế cache chính để tăng tốc độ: cache nội bộ của chính nó (WiredTiger internal cache) và cache của hệ điều hành (OS filesystem cache). Việc cân bằng dung lượng RAM cho hai cơ chế này là chìa khóa để đạt hiệu năng tối đa.

*   **Mục đích:** Phân bổ bộ nhớ RAM một cách hợp lý để đảm bảo "working set" (dữ liệu và index được truy cập thường xuyên nhất) có thể nằm vừa trong RAM, giảm thiểu việc phải đọc từ ổ đĩa (disk I/O) - vốn là tác nhân gây chậm hiệu năng nhất.
*   **Bẫy người mới:**
    *   Để mặc định mà không hiểu nó hoạt động ra sao. Mặc định của MongoDB là `(RAM - 1GB) / 2`, thường là một khởi đầu tốt nhưng không phải lúc nào cũng tối ưu.
    *   Phân bổ quá nhiều RAM cho WiredTiger cache. Điều này sẽ "bóp nghẹt" OS cache, làm cho các thao tác đọc dữ liệu không nằm trong working set trở nên rất chậm.
*   **Thực hiện đúng:** Hiểu rõ workload của bạn. Nếu hệ thống của bạn có chỉ số cache hit ratio thấp (phải đọc từ disk nhiều), việc tinh chỉnh này là cần thiết.

#### **1. Cấu hình Kích thước WiredTiger Cache**

WiredTiger cache chủ yếu được dùng để giữ các dữ liệu *chưa được nén* (uncompressed data). Trong khi đó, OS filesystem cache lại giữ các file dữ liệu *đã được nén* (compressed data blocks) của MongoDB.

*   **Quy tắc chung:**
    *   **RAM > Working Set:** Nếu bạn có thừa RAM để chứa toàn bộ working set, hãy tăng WiredTiger cache lên.
    *   **RAM < Working Set:** Nếu RAM của bạn eo hẹp hơn, hãy giữ WiredTiger cache ở mức nhỏ hơn để nhường phần lớn RAM cho OS cache quản lý.

*   **Cách cấu hình:**
    1.  Mở file cấu hình của một tiến trình `mongod` (ví dụ: `/etc/mongod-shard1.conf`).
    2.  Thêm hoặc chỉnh sửa tham số `cacheSizeGB` trong mục `storage`.
        ```yaml
        storage:
          dbPath: /data/shard1
          journal:
            enabled: true # Luôn bật journal trong production
          engine: wiredTiger # Mặc định, nhưng ghi rõ ràng là một thói quen tốt
          wiredTiger:
            engineConfig:
              # --- Dòng quan trọng là đây ---
              cacheSizeGB: 8 # Ví dụ: Cấp phát 8GB RAM cho WiredTiger cache
        ```
    3.  **Ví dụ thực tế:** Trên một máy chủ có 64GB RAM.
        *   **Mặc định:** MongoDB sẽ lấy `(64 - 1) / 2 = 31.5 GB` cho WiredTiger cache.
        *   **Tùy chỉnh:** Nếu bạn biết working set của mình khoảng 40GB và đã được nén tốt, bạn có thể giảm `cacheSizeGB` xuống còn 16GB, để lại `64 - 16 = 48GB` cho OS cache, đủ sức chứa toàn bộ working set đã nén.
    4.  Khởi động lại tiến trình `mongod` để áp dụng thay đổi.

---

### **Tổng kết và Con đường Tiếp theo**

Chúc mừng bạn! Bằng cách kết hợp tài liệu hướng dẫn gốc và các bài thực hành chi tiết, bạn đã đi qua một hành trình hoàn chỉnh:

1.  **Chuẩn bị Môi trường:** Từ A-Z để có một nền tảng Linux vững chắc.
2.  **Dựng Cluster Sharding:** Hiểu rõ và xây dựng từng thành phần: Config Server, Shard, Mongos.
3.  **Thao tác Dữ liệu:** Nắm vững Aggregation Framework để xử lý dữ liệu phức tạp.
4.  **Bảo mật:** Quản lý người dùng, phân quyền chi tiết với RBAC và ghi log audit.
5.  **Vận hành Replica Set:** Tinh chỉnh bầu cử, cấu hình node ẩn và quản lý Oplog.
6.  **Tối ưu Hiệu năng:** Sử dụng Index, phân tích `explain()`, và tinh chỉnh Storage Engine.
7.  **Giám sát & Phục hồi:** Chủ động theo dõi hệ thống và nắm trong tay các kỹ thuật cứu dữ liệu tối thượng như Point-in-Time Recovery.

Bạn không chỉ biết "làm theo", mà còn hiểu được "tại sao" và biết cách tránh những "cái bẫy" phổ biến. Đây là một nền tảng cực kỳ vững chắc.

---

Hệ sinh thái MongoDB rất rộng lớn. Với những kiến thức này, bạn đã sẵn sàng để khám phá các chủ đề nâng cao hơn:

*   **Schema Design Patterns:** Cách thiết kế cấu trúc document (schema) hiệu quả cho các loại ứng dụng khác nhau (ví dụ: áp dụng ánh xạ, ánh xạ mở rộng, tập con, cây...).
*   **Change Streams:** Theo dõi sự thay đổi dữ liệu trong collection theo thời gian thực để xây dựng các hệ thống phản ứng (reactive systems).
*   **Multi-Document Transactions:** Cách đảm bảo tính toàn vẹn ACID khi phải thực hiện các thao tác ghi trên nhiều document hoặc nhiều collection.
*   **MongoDB Atlas:** Trải nghiệm phiên bản cloud của MongoDB, nơi rất nhiều tác vụ vận hành (backup, scaling, monitoring) đã được tự động hóa, giúp bạn tập trung hơn vào việc phát triển ứng dụng.
*   **Bảo mật Chuyên sâu:** Triển khai xác thực qua chứng chỉ x.509, tích hợp với LDAP/Kerberos.

Chúc mừng bạn một lần nữa vì đã hoàn thành một chặng đường rất dài và chuyên sâu. Chúc bạn thành công trên con đường làm chủ MongoDB

---

## **Phần III: Vận Hành Production và Best Practices**

### **Giai đoạn 16: Production Readiness Checklist**

```mermaid
flowchart TD
    A["🎯 Triển Khai Production"] --> B["🔒 Tăng Cường Bảo Mật"]
    B --> C["📊 Thiết Lập Giám Sát"]
    C --> D["🔄 Chiến Lược Sao Lưu"]
    D --> E["⚖️ Tinh Chỉnh Hiệu Suất"]
    E --> F["🚨 Cấu Hình Cảnh Báo"]
    F --> G["📋 Tài Liệu"]
    G --> H["✅ Sẵn Sàng Production"]
    
    subgraph "Danh Sách Kiểm Tra Bảo Mật"
        S1["Thay Thế KeyFile bằng x.509"]
        S2["Bật SSL/TLS"]
        S3["Cấu Hình Quy Tắc Tường Lửa"]
        S4["Triển Khai Phân Đoạn Mạng"]
        S5["Thiết Lập Ghi Nhật Ký Kiểm Toán"]
        S6["Cập Nhật Bảo Mật Định Kỳ"]
    end
    
    subgraph "Danh Sách Kiểm Tra Giám Sát"
        M1["Triển Khai MongoDB Ops Manager"]
        M2["Cấu Hình Prometheus/Grafana"]
        M3["Thiết Lập Tổng Hợp Log"]
        M4["Cấu Hình Kiểm Tra Sức Khỏe"]
        M5["Triển Khai Các Chỉ Số Tùy Chỉnh"]
        M6["Thiết Lập Phòng Chống Cảnh Báo Quá Tải"]
    end
    
    B --> S1
    C --> M1
    
    style A fill:#e3f2fd
    style H fill:#e8f5e8
    style S1 fill:#fff3e0
    style M1 fill:#f3e5f5
```

#### **1. Security Hardening cho Production**

**Thay thế KeyFile bằng x.509 Certificates:**

```bash
# Tạo CA certificate
openssl genrsa -out ca-key.pem 4096
openssl req -new -x509 -days 365 -key ca-key.pem -out ca.pem -subj "/CN=MongoDB-CA"

# Tạo server certificates cho mỗi node
for host in mongo-1 mongo-2 mongo-3; do
    openssl genrsa -out ${host}-key.pem 4096
    openssl req -new -key ${host}-key.pem -out ${host}.csr -subj "/CN=${host}"
    openssl x509 -req -in ${host}.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out ${host}.pem -days 365
    cat ${host}.pem ${host}-key.pem > ${host}-combined.pem
done
```

**Cấu hình SSL/TLS trong MongoDB:**

```yaml
# Cập nhật mongod configuration
net:
  port: 27017
  bindIp: 0.0.0.0
  ssl:
    mode: requireSSL
    PEMKeyFile: /etc/mongodb/ssl/mongo-1-combined.pem
    CAFile: /etc/mongodb/ssl/ca.pem
    clusterFile: /etc/mongodb/ssl/mongo-1-combined.pem
    allowConnectionsWithoutCertificates: false
    allowInvalidHostnames: false

security:
  clusterAuthMode: x509
  authorization: enabled
```

#### **2. Systemd Service Files cho Production**

**Template cho MongoDB Services:**

```ini
# /etc/systemd/system/mongod-config.service
[Unit]
Description=MongoDB Config Server
After=network.target disable-transparent-huge-pages.service
Documentation=https://docs.mongodb.org/manual

[Service]
User=mongod
Group=mongod
Environment="OPTIONS=-f /etc/mongod-config.conf"
ExecStart=/usr/bin/mongod $OPTIONS
ExecStartPre=/usr/bin/mkdir -p /var/run/mongodb
ExecStartPre=/usr/bin/chown mongod:mongod /var/run/mongodb
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure
RestartSec=10
KillMode=mixed
KillSignal=SIGTERM
TimeoutStopSec=120
LimitFSIZE=infinity
LimitCPU=infinity
LimitAS=infinity
LimitNOFILE=64000
LimitNPROC=64000
LimitMEMLOCK=infinity
TasksMax=infinity
TasksAccounting=false

[Install]
WantedBy=multi-user.target
```

#### **3. Advanced Monitoring với Prometheus và Grafana**

**MongoDB Exporter Configuration:**

```yaml
# docker-compose.yml for monitoring stack
version: '3.8'
services:
  mongodb-exporter:
    image: percona/mongodb_exporter:latest
    ports:
      - "9216:9216"
    environment:
      - MONGODB_URI=mongodb://monitor_user:password@mongo-1:27020,mongo-2:27020,mongo-3:27020/?authSource=admin
    command:
      - '--mongodb.uri=mongodb://monitor_user:password@mongo-1:27020,mongo-2:27020,mongo-3:27020/?authSource=admin'
      - '--collect-all'
      - '--compatible-mode'
  
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
  
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin123
    volumes:
      - grafana-storage:/var/lib/grafana

volumes:
  grafana-storage:
```

### **Giai đoạn 17: Advanced Operational Procedures**

#### **1. Zero-Downtime Maintenance Procedures**

```mermaid
sequenceDiagram
    participant QuảnTrịViên
    participant Chính
    participant Phụ1
    participant Phụ2
    participant BộCânBằngTải
    
    Note over QuảnTrịViên,BộCânBằngTải: Chiến Lược Bảo Trì Liên Tục
    
    QuảnTrịViên->>BộCânBằngTải: Loại bỏ Phụ1 khỏi vòng quay
    QuảnTrịViên->>Phụ1: Dừng dịch vụ mongod
    QuảnTrịViên->>Phụ1: Thực hiện bảo trì (cập nhật OS, v.v)
    QuảnTrịViên->>Phụ1: Khởi động dịch vụ mongod
    QuảnTrịViên->>Phụ1: Xác minh trạng thái replica set
    QuảnTrịViên->>BộCânBằngTải: Thêm Phụ1 trở lại vòng quay
    
    Note over QuảnTrịViên,BộCânBằngTải: Lặp lại cho Phụ2
    
    QuảnTrịViên->>Chính: Kích hoạt chuyển đổi
    Chính->>Phụ1: Quy trình bầu chọn
    Phụ1->>Phụ1: Trở thành Chính mới
    QuảnTrịViên->>Chính: Thực hiện bảo trì trên Chính cũ
    QuảnTrịViên->>Chính: Khởi động lại với vai trò Phụ
```

**Script để thực hiện Rolling Maintenance:**

```bash
#!/bin/bash
# rolling-maintenance.sh

SERVERS=("mongo-1" "mongo-2" "mongo-3")
PORTS=(27011 27012 27013)  # Shard ports

for i in "${!SERVERS[@]}"; do
    SERVER=${SERVERS[$i]}
    PORT=${PORTS[$i]}
    
    echo "Starting maintenance on $SERVER:$PORT"
    
    # Check if this is primary
    IS_PRIMARY=$(mongosh --host $SERVER --port $PORT --quiet --eval "db.hello().isWritablePrimary")
    
    if [ "$IS_PRIMARY" == "true" ]; then
        echo "$SERVER is PRIMARY, triggering step-down"
        mongosh --host $SERVER --port $PORT --eval "db.adminCommand({replSetStepDown: 60})"
        sleep 70  # Wait for election to complete
    fi
    
    echo "Stopping mongod on $SERVER"
    ssh $SERVER "systemctl stop mongod-shard1"
    
    echo "Performing maintenance on $SERVER"
    ssh $SERVER "yum update -y && reboot"
    
    # Wait for server to come back online
    echo "Waiting for $SERVER to come back online..."
    while ! ping -c 1 $SERVER &> /dev/null; do
        sleep 10
    done
    
    sleep 60  # Additional wait for services to start
    
    echo "Verifying replica set status"
    mongosh --host $SERVER --port $PORT --eval "rs.status()"
    
    echo "Maintenance completed on $SERVER"
    echo "Waiting before next server..."
    sleep 120
done

echo "Rolling maintenance completed for all servers"
```

#### **2. Disaster Recovery Procedures**

Quy trình phục hồi sau thảm họa cho toàn bộ cluster:

```mermaid
flowchart TD
    A[Thảm Họa Xảy Ra!] --> B["Dừng tất cả các tiến trình MongoDB<br/>(mongos, mongod)"]
    B --> C["Khôi phục Dữ liệu Máy Chủ Cấu Hình<br/>(từ bản sao lưu)"]
    C --> D["Khởi động Máy Chủ Cấu Hình<br/>Chờ bầu chọn"]
    D --> E["Khôi phục Dữ liệu TỪNG Shard<br/>(từ bản sao lưu)"]
    E --> F["Khởi động Máy Chủ Shard<br/>Chờ bầu chọn"]
    F --> G["Khởi động Bộ Định Tuyến Mongos"]
    G --> H["Xác minh Sức khỏe Cluster<br/>(sh.status())"]
    H --> I[Hoàn Tất Phục Hồi]

    style A fill:#ffebee,stroke:#d32f2f
    style I fill:#e8f5e8,stroke:#388e3c
end
```

**Complete Cluster Recovery từ Backup:**

```bash
#!/bin/bash
# disaster-recovery.sh

BACKUP_DIR="/backup/mongodb-disaster-recovery"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
RECOVERY_LOG="/var/log/mongodb-recovery-${TIMESTAMP}.log"

log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a $RECOVERY_LOG
}

log "Starting MongoDB Cluster Disaster Recovery"

# Step 1: Stop all MongoDB processes
log "Stopping all MongoDB processes"
for server in mongo-1 mongo-2 mongo-3; do
    ssh $server "systemctl stop mongod-config mongod-shard1 mongod-shard2 mongod-shard3 mongos"
done

# Step 2: Restore Config Server
log "Restoring Config Server data"
mongorestore --host mongo-1:27010 --dir $BACKUP_DIR/config-server/ --drop

# Step 3: Restore each Shard
for shard in shard1 shard2 shard3; do
    log "Restoring $shard data"
    mongorestore --host mongo-1:2701${shard: -1} --dir $BACKUP_DIR/$shard/ --drop
done

# Step 4: Start services in correct order
log "Starting Config Servers"
for server in mongo-1 mongo-2 mongo-3; do
    ssh $server "systemctl start mongod-config"
done

log "Waiting for Config Server election"
sleep 30

log "Starting Shards"
for server in mongo-1 mongo-2 mongo-3; do
    ssh $server "systemctl start mongod-shard1 mongod-shard2 mongod-shard3"
done

log "Waiting for Shard elections"
sleep 60

log "Starting Mongos"
ssh mongo-1 "systemctl start mongos"

# Step 5: Verify cluster health
log "Verifying cluster health"
mongosh --port 27020 --eval "sh.status()" | tee -a $RECOVERY_LOG

log "Disaster recovery completed. Check $RECOVERY_LOG for details."
```

### **Giai đoạn 18: Performance Optimization và Capacity Planning**

#### **1. Advanced Performance Monitoring**

```javascript
// Comprehensive performance monitoring script
// performance-monitor.js

function collectPerformanceMetrics() {
    var metrics = {
        timestamp: new Date(),
        serverStatus: db.serverStatus(),
        replSetStatus: rs.status(),
        currentOp: db.currentOp({"active": true}),
        shardingStats: sh.status(),
        indexStats: []
    };
    
    // Collect index usage statistics
    db.adminCommand("listCollections").cursor.firstBatch.forEach(function(collection) {
        var collName = collection.name;
        if (!collName.startsWith("system.")) {
            try {
                var indexStats = db[collName].aggregate([
                    {$indexStats: {}}
                ]).toArray();
                metrics.indexStats.push({
                    collection: collName,
                    indexes: indexStats
                });
            } catch (e) {
                // Skip collections that don't support indexStats
            }
        }
    });
    
    return metrics;
}

// Usage: mongosh --port 27020 < performance-monitor.js
var metrics = collectPerformanceMetrics();
printjson(metrics);
```

#### **2. Capacity Planning Guidelines**

Sơ đồ quy trình lập kế hoạch dung lượng cho các tài nguyên chính:

```mermaid
graph TD
    subgraph "Lập Kế Hoạch CPU"
        A["Giám sát Sử Dụng CPU<br/>(top, iostat)"] --> B{"Duy Trì > 80%?"}
        B -- Có --> C["Mở Rộng Ra<br/>(Thêm shard/node)"]
        B -- Không --> D["OK"]
    end

    subgraph "Lập Kế Hoạch Bộ Nhớ"
        E["Phân Tích Kích Thước Working Set"] --> F["Tính Toán Kích Thước<br/>Bộ Nhớ Cache WiredTiger Tối Ưu"]
        F --> G{"RAM < Working Set?"}
        G -- Có --> H["Giảm Kích Thước Cache<br/>(Ưu Tiên Cache OS)"]
        G -- Không --> I["Tăng Kích Thước Cache<br/>(Ưu Tiên Cache WiredTiger)"]
    end

    subgraph "Lập Kế Hoạch Lưu Trữ"
        J["Giám Sát Sử Dụng Đĩa<br/>(df -h, db.stats())"] --> K["Phân Tích Tốc Độ Tăng Trưởng"]
        K --> L{"Sử Dụng > 85%?"}
        L -- Có --> M["Thêm Dung Lượng Lưu Trữ<br/>(Thêm đĩa, thêm shard)"]
        L -- Không --> N["OK"]
    end

    style C fill:#fff3e0
    style H fill:#fff3e0
    style M fill:#fff3e0
end
```

**CPU Capacity Planning:**
- Monitor CPU utilization during peak hours
- Target 70% average utilization untuk normal operations
- Scale out when sustained >80% utilization

**Memory Capacity Planning:**
```bash
# Script to calculate optimal WiredTiger cache size
#!/bin/bash

TOTAL_RAM=$(free -g | awk 'NR==2{print $2}')
OS_RESERVED=2  # GB reserved for OS
OTHER_PROCESSES=1  # GB for other processes

AVAILABLE_RAM=$((TOTAL_RAM - OS_RESERVED - OTHER_PROCESSES))
WIREDTIGER_CACHE=$((AVAILABLE_RAM * 50 / 100))  # 50% of available RAM

echo "Total RAM: ${TOTAL_RAM}GB"
echo "Available for MongoDB: ${AVAILABLE_RAM}GB"
echo "Recommended WiredTiger Cache: ${WIREDTIGER_CACHE}GB"
echo ""
echo "Add to mongod.conf:"
echo "storage:"
echo "  wiredTiger:"
echo "    engineConfig:"
echo "      cacheSizeGB: ${WIREDTIGER_CACHE}"
```

**Storage Capacity Planning:**
```javascript
// Storage growth analysis
function analyzeStorageGrowth() {
    var stats = db.stats();
    var collections = [];
    
    db.adminCommand("listCollections").cursor.firstBatch.forEach(function(coll) {
        if (!coll.name.startsWith("system.")) {
            var collStats = db[coll.name].stats();
            collections.push({
                name: coll.name,
                size: collStats.size,
                storageSize: collStats.storageSize,
                indexSize: collStats.totalIndexSize,
                documents: collStats.count
            });
        }
    });
    
    return {
        database: db.getName(),
        totalSize: stats.dataSize,
        totalStorageSize: stats.storageSize,
        totalIndexSize: stats.indexSize,
        collections: collections.sort((a, b) => b.storageSize - a.storageSize)
    };
}

// Run on each shard to get complete picture
var growth = analyzeStorageGrowth();
printjson(growth);
```

### **Giai đoạn 19: Troubleshooting Common Issues**

#### **1. Balancer Issues**

Sơ đồ chẩn đoán các vấn đề liên quan đến Balancer:

```mermaid
flowchart TD
    A[Trình Cân Bằng không hoạt động?] --> B{"1. Trình Cân Bằng có được bật không?<br/>sh.getBalancerState()"}
    B -- "Không (false)" --> C["Bật trình cân bằng<br/>sh.startBalancer()"]
    B -- "Có (true)" --> D{"2. Trình Cân Bằng có đang chạy không?<br/>sh.isBalancerRunning()"}
    D -- "Không" --> E{"3. Kiểm tra khóa<br/>db.locks.find({_id: 'balancer'})"}
    E -- "Có khóa" --> F["Xóa khóa<br/>db.locks.remove({_id: 'balancer'})"]
    E -- "Không có khóa" --> G["Kiểm tra log của mongos/máy chủ cấu hình<br/>để tìm lỗi chi tiết"]
    D -- "Có" --> H["Trình Cân Bằng đang chạy, hãy kiên nhẫn<br/>hoặc kiểm tra log để xem tiến trình"]

    style C fill:#e8f5e8
    style F fill:#e8f5e8
    style H fill:#e3f2fd
end
```

```javascript
// Comprehensive balancer diagnostic
function diagnoseBalancer() {
    print("=== Balancer Diagnostic Report ===");
    
    // Check balancer status
    var balancerStatus = sh.getBalancerState();
    print("Balancer Enabled: " + balancerStatus);
    
    // Check if balancer is running
    var balancerRunning = sh.isBalancerRunning();
    print("Balancer Running: " + balancerRunning);
    
    // Check balancer window
    var balancerWindow = db.settings.findOne({_id: "balancer"});
    if (balancerWindow) {
        print("Balancer Window: " + JSON.stringify(balancerWindow));
    } else {
        print("Balancer Window: Not configured (runs 24/7)");
    }
    
    // Check chunk distribution
    print("\n=== Chunk Distribution ===");
    var shards = db.shards.find().toArray();
    shards.forEach(function(shard) {
        var chunkCount = db.chunks.count({shard: shard._id});
        print("Shard " + shard._id + ": " + chunkCount + " chunks");
    });
    
    // Check for jumbo chunks
    print("\n=== Jumbo Chunks ===");
    var jumboChunks = db.chunks.find({jumbo: true}).toArray();
    if (jumboChunks.length > 0) {
        print("Found " + jumboChunks.length + " jumbo chunks:");
        jumboChunks.forEach(function(chunk) {
            print("  Namespace: " + chunk.ns + ", Shard: " + chunk.shard);
        });
    } else {
        print("No jumbo chunks found");
    }
    
    // Check recent balancer operations
    print("\n=== Recent Balancer Operations ===");
    var recentOps = db.changelog.find({what: /moveChunk/}).sort({time: -1}).limit(5).toArray();
    recentOps.forEach(function(op) {
        print(op.time + ": " + op.what + " - " + op.ns + " from " + op.details.from + " to " + op.details.to);
    });
}

// Run diagnostic
diagnoseBalancer();
```

#### **2. Replica Set Election Issues**

Sơ đồ chẩn đoán các vấn đề liên quan đến bầu cử trong Replica Set:

```mermaid
flowchart TD
    A[Replica set không bầu được PRIMARY?] --> B{"1. Đa số node có hoạt động không?<br/>(>50% members HOẠT ĐỘNG)"}
    B -- "Không" --> C["Khởi động lại các node bị down.<br/>Cần đa số để bầu cử."]
    B -- "Có" --> D{"2. Kết nối mạng giữa các node có tốt không?<br/>(ping, telnet port)"}
    D -- "Không" --> E["Kiểm tra firewall, cấu hình mạng.<br/>Các node phải thấy nhau."]
    D -- "Có" --> F{"3. Oplog có bị quá cũ không?<br/>(rs.printReplicationInfo())"}
    F -- "Có (lag > cửa sổ oplog)" --> G["Node bị lag quá sẽ không được bầu.<br/>Cần đồng bộ lại node đó."]
    F -- "Không" -- > H{"4. Cấu hình priority/votes có vấn đề?<br/>(rs.conf())"}
    H -- "Có" --> I["Kiểm tra lại cấu hình,<br/>đảm bảo có đủ node có thể bầu chọn."]
    H -- "Không" --> J["Kiểm tra log của các node<br/>để tìm lỗi chi tiết (ví dụ: keyfile không khớp)."]

    style C fill:#ffebee
    style E fill:#ffebee
    style G fill:#fff3e0
    style I fill:#fff3e0
end
```

```bash
#!/bin/bash
# replica-set-diagnostic.sh

PORT=$1
if [ -z "$PORT" ]; then
    echo "Usage: $0 <port>"
    echo "Example: $0 27011"
    exit 1
fi

echo "=== Replica Set Diagnostic for Port $PORT ==="

# Check replica set status
echo "--- Replica Set Status ---"
mongosh --port $PORT --quiet --eval "rs.status()"

# Check replica set configuration
echo "--- Replica Set Configuration ---"
mongosh --port $PORT --quiet --eval "rs.conf()"

# Check server status
echo "--- Server Status ---"
mongosh --port $PORT --quiet --eval "db.serverStatus().repl"

# Check oplog status
echo "--- Oplog Status ---"
mongosh --port $PORT --quiet --eval "
    use local;
    var first = db.oplog.rs.find().sort({ts: 1}).limit(1).next();
    var last = db.oplog.rs.find().sort({ts: -1}).limit(1).next();
    var timeDiff = (last.ts.getTime() - first.ts.getTime()) / 1000 / 3600;
    print('Oplog time window: ' + timeDiff.toFixed(2) + ' hours');
    print('Oplog size: ' + (db.oplog.rs.stats().size / 1024 / 1024 / 1024).toFixed(2) + ' GB');
"

# Check for election issues
echo "--- Election Logs ---"
journalctl sudo -u mongod-shard$(echo $PORT | tail -c 2) --since="1 hour ago" | grep -i "election\|primary\|secondary"

echo "=== Diagnostic Complete ==="
```

### **Kết luận: MongoDB Production Excellence**

Việc triển khai và vận hành một MongoDB Sharded Cluster trong môi trường production đòi hỏi sự kết hợp giữa:

1. **Kiến thức lý thuyết vững chắc** về kiến trúc phân tán
2. **Kỹ năng thực hành** trong việc cấu hình và triển khai
3. **Quy trình vận hành** có hệ thống và automation
4. **Monitoring và alerting** chủ động
5. **Disaster recovery planning** chi tiết

Thông qua hướng dẫn này, bạn đã có được một nền tảng vững chắc để xây dựng và vận hành các hệ thống MongoDB quy mô lớn một cách an toàn và hiệu quả.