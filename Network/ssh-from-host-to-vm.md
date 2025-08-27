# 🔗 SSH từ Host đến Virtual Machine

## 📖 Tổng quan

Hướng dẫn cấu hình SSH để kết nối từ máy host đến các máy ảo (VM) một cách an toàn và hiệu quả.

## 🏗️ Kiến trúc kết nối

```mermaid
flowchart LR
    subgraph Host["💻 Máy Host"]
        SSH_Client["🔌 SSH Client"]
        Private_Key["🔑 Private Key"]
    end
    
    subgraph Network["🌐 Lớp Mạng"]
        Bridge["🌉 Bridge/NAT"]
        Firewall["🔥 Tường Lửa"]
    end
    
    subgraph VM["📦 Máy Ảo"]
        SSH_Server["🖥️ SSH Server"]
        Auth["🔐 Xác Thực"]
        Shell["💻 Truy Cập Shell"]
    end
    
    SSH_Client -->|"⚡ Port 22/Tùy Chỉnh"| Bridge
    Private_Key -.->|"🔒 Xác Thực"| SSH_Client
    Bridge --> Firewall
    Firewall --> SSH_Server
    SSH_Server --> Auth
    Auth -->|"✅ Thành Công"| Shell
    
    classDef hostStyle fill:#e1f5fe,stroke:#01579b,stroke-width:3px,color:#000
    classDef networkStyle fill:#f3e5f5,stroke:#4a148c,stroke-width:3px,color:#000
    classDef vmStyle fill:#e8f5e8,stroke:#1b5e20,stroke-width:3px,color:#000
    classDef keyStyle fill:#fff3e0,stroke:#e65100,stroke-width:2px,color:#000
    classDef securityStyle fill:#ffebee,stroke:#c62828,stroke-width:2px,color:#000
    
    class SSH_Client,Private_Key hostStyle
    class Bridge,Firewall networkStyle
    class SSH_Server,Auth,Shell vmStyle
    class Private_Key keyStyle
    class Auth,Firewall securityStyle
```

## 🔄 Các phương thức kết nối

### 1. 🌉 Bridge Network Mode

```mermaid
sequenceDiagram
    participant H as 💻 Host
    participant B as 🌉 Bridge
    participant V as 📦 VM
    
    Note over H,V: 🌉 Bridge Network Connection
    H->>B: 📡 SSH Request (VM_IP:22)
    activate B
    B->>V: ⏩ Forward Request
    activate V
    V->>B: 📡 SSH Response
    B->>H: ✅ Connection Established
    deactivate V
    deactivate B
    
    Note over H,V: 🎯 Direct IP Access
```

### 2. 🔄 NAT với Port Forwarding

```mermaid
sequenceDiagram
    participant H as 💻 Host
    participant N as 🔄 NAT
    participant V as 📦 VM
    
    Note over H,V: 🔄 NAT + Port Forwarding
    H->>N: 📡 SSH localhost:2222
    activate N
    Note right of N: 🔀 Port 2222 → VM:22
    N->>V: ⏩ Forward to VM:22
    activate V
    V->>N: 📡 SSH Response
    N->>H: ✅ Connection via localhost:2222
    deactivate V
    deactivate N
```

## ⚙️ Cấu hình SSH Server trên VM

### 📥 Bước 1: Cài đặt SSH Server

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install openssh-server

# CentOS/RHEL
sudo yum install openssh-server
# hoặc với dnf
sudo dnf install openssh-server
```

### ⚙️ Bước 2: Cấu hình SSH

```bash
# Chỉnh sửa file cấu hình
sudo nano /etc/ssh/sshd_config
```

**📋 Cấu hình khuyến nghị:**

```bash
# 🔄 Đổi port mặc định (tùy chọn)
Port 2222

# 🚫 Cho phép đăng nhập root (không khuyến nghị)
PermitRootLogin no

# 🔑 Sử dụng key authentication
PubkeyAuthentication yes
PasswordAuthentication no

# ⏰ Giới hạn thời gian kết nối
ClientAliveInterval 300
ClientAliveCountMax 2

# 👤 Chỉ cho phép các user cụ thể
AllowUsers your_username
```

### 🚀 Bước 3: Khởi động SSH Service

```bash
# 🚀 Khởi động và enable SSH
sudo systemctl start ssh
sudo systemctl enable ssh

# 📊 Kiểm tra trạng thái
sudo systemctl status ssh
```

## 🔐 Quá trình xác thực SSH

```mermaid
flowchart LR
    Start(["🚀 Bắt Đầu Kết Nối SSH"]) --> CheckKey{"🔍 Kiểm Tra Private Key"}
    
    CheckKey -->|"🔑 Tìm Thấy Key"| LoadKey["📥 Tải Private Key"]
    CheckKey -->|"❌ Không Có Key"| Password["🔒 Xác Thực Mật Khẩu"]
    
    LoadKey --> Handshake["🤝 SSH Handshake"]
    Password --> Handshake
    
    Handshake --> Verify{"✅ Xác Minh Danh Tính"}
    
    Verify -->|"🎯 Thành Công"| Connected["🎉 Đã Kết Nối"]
    Verify -->|"⛔ Thất Bại"| Denied["❌ Từ Chối Truy Cập"]
    
    Connected --> Session["💻 Phiên SSH"]
    
    classDef startStyle fill:#e8f5e8,stroke:#2e7d32,stroke-width:3px,color:#000
    classDef processStyle fill:#e3f2fd,stroke:#1976d2,stroke-width:2px,color:#000
    classDef successStyle fill:#e8f5e8,stroke:#388e3c,stroke-width:3px,color:#000
    classDef errorStyle fill:#ffebee,stroke:#d32f2f,stroke-width:3px,color:#000
    classDef securityStyle fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    
    class Start startStyle
    class LoadKey,Handshake,Session processStyle
    class Connected,Session successStyle
    class Denied errorStyle
    class CheckKey,Verify,Password securityStyle
```

## 🔑 Tạo và cấu hình SSH Keys

### 🔧 Bước 1: Tạo SSH Key Pair trên Host

```bash
# 🔑 Tạo SSH key pair
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# 🔐 Hoặc sử dụng ed25519 (khuyến nghị)
ssh-keygen -t ed25519 -C "your_email@example.com"
```

### 📤 Bước 2: Copy Public Key đến VM

```bash
# 📤 Sử dụng ssh-copy-id
ssh-copy-id username@vm_ip_address

# 📋 Hoặc copy thủ công
cat ~/.ssh/id_rsa.pub | ssh username@vm_ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### 🔄 Quá trình quản lý keys

```mermaid
flowchart LR
    subgraph Host["💻 Máy Host"]
        Generate["🔧 Tạo Keys"]
        PrivateKey["🔐 Private Key<br/>📁 ~/.ssh/id_rsa"]
        PublicKey["🔑 Public Key<br/>📁 ~/.ssh/id_rsa.pub"]
    end
    
    subgraph VM["📦 Máy Ảo"]
        AuthKeys["📋 Authorized Keys<br/>📁 ~/.ssh/authorized_keys"]
        SSHAuth["🔓 Xác Thực SSH"]
    end
    
    Generate --> PrivateKey
    Generate --> PublicKey
    PublicKey -->|"📤 ssh-copy-id"| AuthKeys
    PrivateKey -.->|"🔐 Dùng Để Đăng Nhập"| SSHAuth
    AuthKeys --> SSHAuth
    
    classDef hostStyle fill:#e1f5fe,stroke:#01579b,stroke-width:3px,color:#000
    classDef vmStyle fill:#e8f5e8,stroke:#1b5e20,stroke-width:3px,color:#000
    classDef keyStyle fill:#fff3e0,stroke:#e65100,stroke-width:2px,color:#000
    classDef authStyle fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000
    
    class Generate,PrivateKey,PublicKey hostStyle
    class AuthKeys,SSHAuth vmStyle
    class PrivateKey,PublicKey,AuthKeys keyStyle
    class SSHAuth authStyle
```

## 🌐 Cấu hình Network cho VM

### 🖥️ VMware Workstation

```bash
# 🌉 Bridge Mode - VM có IP riêng trong mạng
# VM Settings → Network Adapter → Bridge

# 🔄 NAT Mode với Port Forwarding
# Virtual Network Editor → NAT Settings → Port Forwarding
# Host Port: 2222 → VM IP: 22
```

### 📦 VirtualBox

```bash
# 🌉 Bridge Mode
# VM Settings → Network → Attached to: Bridged Adapter

# 🔄 NAT với Port Forwarding
# VM Settings → Network → Advanced → Port Forwarding
# Host Port: 2222 → Guest Port: 22
```

## 💻 Cấu hình SSH Client trên Host

### 📝 SSH Config File

```bash
# 📁 Tạo/chỉnh sửa ~/.ssh/config
mkdir -p ~/.ssh
nano ~/.ssh/config
```

**📄 Nội dung file config:**

```bash
# 🖥️ VM Development Server
Host vm-dev
    HostName 192.168.1.100
    User developer
    Port 22
    IdentityFile ~/.ssh/id_rsa
    ServerAliveInterval 60
    ServerAliveCountMax 3

# 🔄 VM with NAT Port Forwarding
Host vm-nat
    HostName localhost
    User developer
    Port 2222
    IdentityFile ~/.ssh/id_rsa

# 🏭 Production VM
Host vm-prod
    HostName 10.0.0.50
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
    StrictHostKeyChecking yes
```

## 🔧 Troubleshooting

### 🔍 Quy trình xử lý sự cố

```mermaid
flowchart TD
    Problem(["⚠️ Sự Cố Kết Nối SSH"]) --> CheckBasic{"🔍 Kiểm Tra Cơ Bản"}
    
    CheckBasic -->|"🌐 Mạng"| Network["🌐 Kiểm Tra Mạng<br/>• 📡 Ping VM<br/>• 🔥 Kiểm tra Firewall<br/>• 🏠 Xác minh IP"]
    CheckBasic -->|"🛠️ Dịch Vụ SSH"| Service["⚙️ Kiểm Tra SSH Service<br/>• 📊 systemctl status<br/>• 🔌 Port listening<br/>• 📝 Config syntax"]
    CheckBasic -->|"🔐 Xác Thực"| Auth["🔐 Kiểm Tra Auth<br/>• 🔑 Quyền key<br/>• 📋 authorized_keys<br/>• ⚙️ SSH config"]
    
    Network --> NetworkOK{"🌐 Mạng OK?"}
    Service --> ServiceOK{"🛠️ Service OK?"}
    Auth --> AuthOK{"🔐 Auth OK?"}
    
    NetworkOK -->|"❌ Không"| FixNetwork["🔧 Sửa Mạng<br/>• 🌉 Cấu hình bridge/NAT<br/>• 🔓 Mở firewall<br/>• 🔍 Kiểm tra mạng VM"]
    ServiceOK -->|"❌ Không"| FixService["🔧 Sửa Service<br/>• ▶️ Start SSH daemon<br/>• 🐛 Sửa lỗi config<br/>• 🔌 Kiểm tra port binding"]
    AuthOK -->|"❌ Không"| FixAuth["🔧 Sửa Auth<br/>• 🔑 Sửa quyền key<br/>• 📤 Copy lại public key<br/>• ⚙️ Kiểm tra SSH config"]
    
    NetworkOK -->|"✅ Có"| Success["✅ Đã Kết Nối"]
    ServiceOK -->|"✅ Có"| Success
    AuthOK -->|"✅ Có"| Success
    
    FixNetwork --> Retry["🔄 Thử Lại Kết Nối"]
    FixService --> Retry
    FixAuth --> Retry
    
    Retry --> Problem
    
    classDef problemStyle fill:#ffebee,stroke:#c62828,stroke-width:3px,color:#000
    classDef checkStyle fill:#e3f2fd,stroke:#1976d2,stroke-width:2px,color:#000
    classDef fixStyle fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#000
    classDef successStyle fill:#e8f5e8,stroke:#388e3c,stroke-width:3px,color:#000
    
    class Problem problemStyle
    class CheckBasic,Network,Service,Auth,NetworkOK,ServiceOK,AuthOK checkStyle
    class FixNetwork,FixService,FixAuth,Retry fixStyle
    class Success successStyle
```

### 🛠️ Lệnh kiểm tra thông dụng

```bash
# 🌐 Kiểm tra kết nối mạng
ping vm_ip_address
telnet vm_ip_address 22

# 🖥️ Kiểm tra SSH service trên VM
sudo systemctl status ssh
sudo netstat -tlnp | grep :22
sudo ss -tlnp | grep :22

# ⚙️ Kiểm tra SSH config
sudo sshd -t
sudo sshd -T

# 🔍 Debug SSH connection
ssh -v username@vm_ip
ssh -vv username@vm_ip  # More verbose
ssh -vvv username@vm_ip # Most verbose

# 🔑 Kiểm tra SSH keys
ls -la ~/.ssh/
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 700 ~/.ssh/
```

## 🔒 Bảo mật SSH

### 🛡️ Các biện pháp bảo mật

```mermaid
graph LR
    subgraph Security["🔒 Bảo Mật SSH"]
        subgraph Auth["🔐 Xác Thực"]
            A1["🔑 Xác thực bằng Key"]
            A2["🚫 Tắt Password"]
            A3["🎯 Chỉ User cụ thể"]
            A4["⏰ Timeout phiên"]
        end
        
        subgraph Net["🌐 Mạng"]
            N1["🔥 Quy tắc Firewall"]
            N2["🏠 Hạn chế IP"]
            N3["🔀 Custom Ports"]
            N4["🌐 Truy cập VPN"]
        end
        
        subgraph Monitor["📊 Giám Sát"]
            M1["📊 Phân tích Log"]
            M2["🚨 Cảnh báo Login lỗi"]
            M3["📈 Giám sát kết nối"]
            M4["🔍 Phát hiện xâm nhập"]
        end
        
        subgraph Sys["🛠️ Hệ Thống"]
            S1["🛡️ Hardening OS"]
            S2["🔄 Cập nhật định kỳ"]
            S3["👤 Quản lý User"]
            S4["📝 Audit Trails"]
        end
    end
    
    Auth --> Net
    Net --> Monitor
    Monitor --> Sys
    
    classDef authStyle fill:#e8f5e8,stroke:#2e7d32,stroke-width:3px,color:#000
    classDef netStyle fill:#e3f2fd,stroke:#1976d2,stroke-width:3px,color:#000
    classDef monitorStyle fill:#fff3e0,stroke:#f57c00,stroke-width:3px,color:#000
    classDef sysStyle fill:#f3e5f5,stroke:#7b1fa2,stroke-width:3px,color:#000
    
    class A1,A2,A3,A4 authStyle
    class N1,N2,N3,N4 netStyle
    class M1,M2,M3,M4 monitorStyle
    class S1,S2,S3,S4 sysStyle
```

### 🔥 Cấu hình Firewall

```bash
# 🔥 UFW (Ubuntu)
sudo ufw allow ssh
sudo ufw allow 2222/tcp  # Custom SSH port
sudo ufw enable

# 🛡️ iptables
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 2222 -j ACCEPT

# 🔥 firewalld (CentOS/RHEL)
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --reload
```

## 🎯 Kết luận

Việc cấu hình SSH từ host đến VM đòi hỏi sự kết hợp của:

1. **🌐 Cấu hình mạng** phù hợp (Bridge/NAT)
2. **🖥️ SSH Server** được cấu hình đúng cách
3. **🔐 Authentication** an toàn với SSH keys
4. **🔥 Firewall** và các biện pháp bảo mật
5. **📊 Monitoring** và troubleshooting

Với hướng dẫn này, bạn có thể thiết lập kết nối SSH an toàn và hiệu quả giữa host và VM của mình. 🎉