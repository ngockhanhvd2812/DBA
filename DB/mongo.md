# Lộ trình MongoDB

> Quy ước:
>
> * Dùng `mongosh` để thao tác.
> * Hệ điều hành ví dụ: Ubuntu 22.04 / macOS (ghi chú khi cần).
> * Dữ liệu mẫu: dùng `mongoimport`/`mongorestore` hoặc **sample datasets**.

---

## Day 1 — Giới thiệu & Cài đặt nhanh

**Lý thuyết gọn:** MongoDB là NoSQL dạng document, lưu JSON/BSON, thao tác qua shell/driver. Kết nối bằng **connection string** chuẩn hoặc `mongodb+srv` (SRV DNS).

### Lab

1. **Cài MongoDB Community** (Ubuntu):

```bash
# 1) Import GPG & repo (theo hướng dẫn chính thức cho bản hiện tại)
sudo apt-get update && sudo apt-get install -y gnupg curl
# (tham khảo trang cài đặt bản đang dùng)

# 2) Cài server & tools
sudo apt-get install -y mongodb-org
```

2. **Start & kiểm tra:**

```bash
sudo systemctl enable --now mongod
sudo systemctl status mongod
```

3. **Cài mongosh & Database Tools** (nếu chưa có):

* `mongosh` cài theo docs hiện hành.
* **Tools** (mongodump, mongoimport…): tải từ trang **Database Tools**.

1. **Kết nối thử:**

```bash
mongosh "mongodb://localhost:27017"
```

5. **Nạp dữ liệu mẫu** (CSV/JSON):

```bash
# ví dụ nạp JSON
mongoimport --db shop --collection products --file products.json --jsonArray
```

* `mongoimport` hỗ trợ JSON/CSV/TSV.

**Tips & Best Practices**

* Dùng `mongodb+srv://...` khi kết nối Atlas/cluster có DNS SRV. 
* Luôn cài **Database Tools** tách rời (từ MongoDB 4.4). 

---

## Day 2 — Kết nối & CRUD cơ bản

**Lý thuyết gọn:** CRUD = Create/Read/Update/Delete. Dùng `insertOne/Many`, `find`, `updateOne/Many`, `deleteOne/Many`. 

### Lab

```javascript
use shop
db.products.insertMany([
  { _id: 1, name: "iPhone 15", price: 999, tags: ["phone","apple"], stock: 20 },
  { _id: 2, name: "Pixel 8",  price: 799, tags: ["phone","google"], stock: 15 }
])

// Read
db.products.find({ price: { $gte: 800 } }, { name: 1, price: 1 })

// Update
db.products.updateOne({ _id: 2 }, { $inc: { stock: 5 } })

// Delete
db.products.deleteOne({ _id: 1 })
```

**Tips & Best Practices**

* Luôn chỉ định **projection** để giảm băng thông: `{ field: 1 }`.
* Gắn `_id` có ý nghĩa (hoặc để mặc định ObjectId).

---

## Day 3 — Truy vấn hiệu quả & phân trang

**Lý thuyết gọn:** Kết hợp `filter + projection + sort + limit + skip`. 

### Lab

```javascript
// Phân trang (page=2, size=5):
const page=2, size=5
db.products.find({}, {name:1, price:1})
           .sort({price:-1})
           .skip((page-1)*size)
           .limit(size)
```

**Tips & Best Practices**

* Tránh `skip` quá lớn: cân nhắc **range paging** bằng điều kiện `_id`/trường đã sort.

---

## Day 4 — Thiết kế schema & **Schema Validation**

**Lý thuyết gọn:** MongoDB linh hoạt schema, nhưng có thể ràng buộc bằng **JSON Schema validator** để tránh dữ liệu “bẩn”. 

### Lab

```javascript
db.createCollection("orders", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["userId","items","total"],
      properties: {
        userId: { bsonType: "objectId" },
        items:  { bsonType: "array", items: { 
                    bsonType: "object",
                    required: ["sku","qty","price"]
                  } },
        total:  { bsonType: "double", minimum: 0 }
      }
    }
  }
})
```

**Tips & Best Practices**

* Bắt buộc những trường tối thiểu cho nghiệp vụ; cho phép linh hoạt ở phần mở rộng (extra fields) nếu cần.

---

## Day 5 — **Index** cơ bản (single, compound, TTL, unique)

**Lý thuyết gọn:** Index giúp truy vấn nhanh, nên tạo theo **mẫu truy vấn**. Có `single`, `compound`, `unique`, `TTL`, `partial`. Dùng `explain()` để kiểm chứng. 

### Lab

```javascript
// Single
db.products.createIndex({ price: 1 })

// Compound (lọc theo category + sort theo price desc)
db.products.createIndex({ category: 1, price: -1 })

// Unique
db.users.createIndex({ email: 1 }, { unique: true })

// TTL: logs tự xóa sau 7 ngày (604800s)
db.logs.createIndex({ createdAt: 1 }, { expireAfterSeconds: 604800 })

// Partial Index: chỉ index documents còn hàng
db.products.createIndex({ name: 1 }, { partialFilterExpression: { stock: { $gt: 0 } } })

// Kiểm tra plan
db.products.find({ category: "phone" }).sort({ price: -1 }).explain("executionStats")
```

**Tips & Best Practices**

* Thứ tự trường trong **compound index** nên: *filter → sort → projection*.
* **Đừng lạm dụng** index — mỗi index tốn RAM/ghi thêm khi write.
* Dùng `explain("executionStats")` để xem **nReturned**, **totalDocsExamined**. 

---

## Day 6 — **Aggregation** căn bản

**Lý thuyết gọn:** Pipeline gồm nhiều stage: `$match → $project → $group → $sort → $limit …`; `$lookup` để join; `$unwind` flatten mảng. 

### Lab

```javascript
// Top 5 sản phẩm bán chạy theo category
db.orderItems.aggregate([
  { $match: { status: "paid" } },
  { $group: { _id: { category: "$category", sku: "$sku" }, qty: { $sum: "$qty" } } },
  { $sort: { qty: -1 } },
  { $group: { _id: "$_id.category", top: { $push: { sku: "$_id.sku", qty: "$qty" } } } },
  { $project: { top: { $slice: ["$top", 5] } } }
])
```

**Tips & Best Practices**

* Đưa `$match` sớm để giảm dữ liệu chảy qua pipeline. 
* Tạo index hỗ trợ `$match`/`$lookup` nếu cần.

---

## Day 7 — **Bảo mật & Phân quyền (RBAC), TLS, Internal Auth**

**Lý thuyết gọn:**

* Bật **authorization** rồi tạo user/role theo RBAC. 
* **Replica set** nên bật **internal authentication** (keyFile) để node tin nhau. 
* Dùng **TLS/SSL** để mã hóa kết nối `mongod`/`mongos`. 

### Lab

1. **Bật auth** trong `/etc/mongod.conf`:

```yaml
security:
  authorization: enabled
```

2. **Tạo user admin**:

```javascript
use admin
db.createUser({
  user: "root",
  pwd:  "strongpass",
  roles: ["userAdminAnyDatabase","dbAdminAnyDatabase","readWriteAnyDatabase","clusterAdmin"]
})
```

3. **(Replica set) Internal auth bằng keyFile**:

```yaml
security:
  keyFile: /etc/mongo-keyfile
```

Sinh key, phân quyền 400, restart. 
4\) **Bật TLS**: cấu hình `net.tls.certificateKeyFile`, `CAFile`, và bắt buộc TLS nếu yêu cầu. 

**Tips & Best Practices**

* Theo **RBAC**: cấp đúng vai trò, nguyên tắc **least privilege**. 
* Luôn mã hóa lưu thông giữa client–server & giữa các member. 

---

## Day 8 — **Tối ưu cơ bản**: Explain, Profiler, Read/Write Concern

**Lý thuyết gọn:**

* `explain()` giúp nhìn kế hoạch truy vấn. 
* **Database Profiler** ghi lại truy vấn chậm để soi. 
* **Write Concern**/**Read Concern** để cân bằng an toàn vs. hiệu năng. 

### Lab

```javascript
// Profiler: ghi query >50ms
db.setProfilingLevel(1, { slowms: 50 })
db.system.profile.find().sort({ ts: -1 }).limit(3)

// Read Concern "majority"
db.getMongo().setReadConcern("majority")

// Write Concern majority, wtimeout
db.runCommand({insert:"audit", documents:[{t:new Date()}], writeConcern:{w:"majority", wtimeout:2000}})
```

**Tips & Best Practices**

* Tắt profiler khi không cần; dùng **sampling**/APM ở môi trường lớn.
* Hiểu rõ **majority** có chi phí nhưng cho tính nhất quán cao hơn. 

---

## Day 9 — **Replica Set**: dựng & failover

**Lý thuyết gọn:** Replica set = 1 primary + n secondary (bầu cử tự động). Khởi tạo bằng `rs.initiate()`; thêm member rồi kiểm tra `rs.status()`. 

### Lab (3 node local)

```bash
# start 3 mongod với --replSet rs1 --port 27017/8/9 --dbpath riêng
```

```javascript
// mongosh vào 27017
rs.initiate({
  _id: "rs1",
  members: [
    { _id: 0, host: "localhost:27017" },
    { _id: 1, host: "localhost:27018" },
    { _id: 2, host: "localhost:27019" }
  ]
})
rs.status()
```

**Kiểm tra oplog & độ trễ:**

```javascript
rs.printReplicationInfo()
rs.printSecondaryReplicationInfo()
```

(2 lệnh này in báo cáo dạng text, dùng xem tay; muốn script hóa thì xài `db.getReplicationInfo()`/`rs.status()`). 

**Tips & Best Practices**

* Dùng **DNS hostname** thay vì IP để tránh phải reconfig khi IP đổi. 
* Theo dõi độ trễ secondary bằng `rs.printSecondaryReplicationInfo()`. 

---

## Day 10 — Replica Set nâng cao: **priority, hidden, delayed, oplog**

**Lý thuyết gọn:**

* **Priority** điều khiển ứng viên primary; **non-voting** để thêm node đọc/backup. 
* **Hidden**: không nhận đọc từ client, thích hợp cho backup/BI. 
* **Delayed**: secondary trễ X giây → “ảnh chụp quá khứ”. Cần **oplog window** đủ dài. 
* **Resize oplog** runtime bằng `replSetResizeOplog`. 

### Lab

```javascript
// Tăng priority cho node A
cfg = rs.conf()
cfg.members[0].priority = 2
rs.reconfig(cfg)

// Member hidden + non-voting
cfg = rs.conf()
cfg.members[2].hidden = true
cfg.members[2].priority = 0
cfg.members[2].votes = 0
rs.reconfig(cfg)

// Member delayed 1 giờ
cfg = rs.conf()
cfg.members[1].priority = 0
cfg.members[1].secondaryDelaySecs = 3600
rs.reconfig(cfg)

// Resize oplog lên 20GB
db.adminCommand({ replSetResizeOplog: 1, size: 20480 })
```

**Tips & Best Practices**

* `rs.reconfig()` có thể làm primary **step down** (mất kết nối tạm 10–20s) → nên làm lúc bảo trì. 
* Đặt **secondaryDelaySecs** < **cửa sổ oplog**, không thì node delayed sẽ rơi khỏi sync. 

---

## Day 11 — **Sharding** căn bản

**Lý thuyết gọn:** Sharding = chia dữ liệu theo **shard key** trên nhiều shard. Chọn shard key cực kỳ quan trọng. 

### Lab (khái quát thao tác trên `mongos`)

```javascript
// (MongoDB >= 6.0 có thể shard trực tiếp bằng shardCollection)
// Tạo shard key & shard collection
sh.shardCollection("shop.orders", { userId: 1, createdAt: 1 }) // range
// hoặc hashed
sh.shardCollection("shop.sessions", { _id: "hashed" })

// Xem trạng thái
db.printShardingStatus()
```

`db.printShardingStatus()` để đọc tay, không trả JSON. 

**Tips & Best Practices**

* Shard key tốt = **phân bố đều + hỗ trợ query**; tránh khóa đơn điệu (tăng dần). 
* Từ **6.0.3**, **auto-splitting** không còn thực hiện; cân nhắc chính sách cân bằng mới. 

---

## Day 12 — Sharding nâng cao: **Balancer & đánh giá shard key**

**Lý thuyết gọn:**

* Quản lý Balancer: `sh.getBalancerState()`, `sh.isBalancerRunning()`, `sh.startBalancer()`, `db.adminCommand({balancerStatus:1})`. 
* Đánh giá shard key bằng **analyzeShardKey** & **query analyzer**. 

### Lab

```javascript
// Check balancer
sh.getBalancerState()       // true/false
sh.isBalancerRunning()      // trạng thái hiện thời
db.adminCommand({ balancerStatus: 1 })

// Bật sampling để đánh giá candidate shard key
db.collection.configureQueryAnalyzer({ mode: "full", samplesPerSecond: 5 })
db.collection.analyzeShardKey({ userId: 1, createdAt: 1 }, { keyCharacteristics: true, readWriteDistribution: true })
```

**Tips & Best Practices**

* Khi **reshard/refine** khóa, test kỹ lưu lượng & thời gian; reshard tốn tài nguyên và có thể kéo dài.

---

## Day 13 — **High Availability** thực chiến

**Lý thuyết gọn:** HA dựa vào **replica set election** (ưu tiên, votes, failure domain). Non-voting phải có `priority:0`. 

### Lab (tình huống failover)

* Tắt node primary, quan sát bầu cử → node khác lên primary.
* Kiểm tra `rs.status()`, update app/driver dùng replica set URI để tự failover.

**Tips & Best Practices**

* Trải node khác AZ/zone.
* Theo dõi **oplog window** đủ cho backup/khôi phục.

---

## Day 14 — **Backup & Restore** (cơ bản + trên sharded)

**Lý thuyết gọn:**

* `mongodump` tạo dump; thêm `--oplog` để chụp **điểm-thời-gian** (PIT) khi khôi phục cùng `mongorestore --oplogReplay`. 
* Sharded cluster: dừng **balancer**, sao lưu `config` + từng shard. 

### Lab

```bash
# Replica set backup “nóng”:
mongodump --uri="mongodb://rs1/?replicaSet=rs1" --out /backup/rs1-$(date +%F) --oplog

# Khôi phục đầy đủ + oplog
mongorestore --dir /backup/rs1-2025-08-22 --oplogReplay
```

**Tips & Best Practices**

* Trên **sharded**, dừng balancer trước khi dump để tránh lệch dữ liệu. 
* Điểm-thời-gian với dump yêu cầu đã dump kèm `--oplog`. 

---

## Day 15 — **Siêu khó**: Khôi phục khi **xóa nhầm** (PITR/oplog)

**Lý thuyết gọn:** Có thể **replay oplog** đến thời điểm trước “lệnh xóa/ghi nhầm”. Cần dump có `--oplog` **hoặc** có bản sao oplog phù hợp; hoặc dùng giải pháp **Ops Manager/Atlas** cho PITR tự động. 

### Lab (quy trình thực chiến – môi trường staging riêng)

1. **Khôi phục full** đến thời điểm dump:

```bash
mongorestore --dir /backup/full --oplogReplay
```

2. **Giới hạn đến thời điểm trước sự cố** bằng `--oplogLimit time_t:ordinal` (lấy từ mục tiêu trong `oplog.bson`):

```bash
mongorestore --oplogReplay --oplogLimit 1692691200:1 /backup/full
```

3. **Nếu chỉ cần “lọc” thao tác** (ví dụ chỉ hoàn tác `delete` 1 collection), extract & xử lý `oplog` thủ công (aggregate trên `local.oplog.rs` ở node khác), hoặc làm theo hướng dẫn chi tiết cộng đồng. ([Stack Overflow][36], [Snap DBA][37], [Tarun Batra][38])

**Warnings**

* Oplog **có cửa sổ giới hạn**; nếu đã trôi thì **bó tay** nếu không có backup khác. 
* `--oplogReplay` yêu cầu **full restore**, không mix khôi phục từng phần. ([Stack Overflow][40])

**Tips & Best Practices**

* Lập lịch backup + kiểm thử **khôi phục định kỳ**; cân nhắc PITR qua **Ops Manager/Atlas**. 

---

## Day 16 — **Giám sát**: `mongostat`, `mongotop`, `serverStatus`, FTDC

**Lý thuyết gọn:**

* `mongostat` xem tổng quan throughput/conn/locks; `mongotop` xem thời gian đọc/ghi theo collection. 
* `serverStatus` trả về tài liệu tổng hợp metrics để giám sát định kỳ. 
* **FTDC** (diagnostic.data) thu thập dữ liệu chẩn đoán liên tục, **bật mặc định**. 

### Lab

```bash
mongostat --uri "mongodb://localhost" 1
mongotop 5

# serverStatus (lọc bớt trường)
mongosh --eval 'db.runCommand({serverStatus:1, metrics:1, connections:1})'
```

**Tips & Best Practices**

* Dùng `clusterMonitor` role để chạy tools trên môi trường có auth. 
* FTDC rất hữu ích khi mở case hỗ trợ/điều tra sự cố → đừng tắt bừa. 

---

## Day 17 — **Performance Tuning** (chuyên sâu)

**Lý thuyết gọn:**

* Tối ưu **index + truy vấn** là đòn bẩy lớn nhất (đã làm ở Day 5–6–8).
* **OS tuning**: Lưu ý **Transparent Huge Pages (THP)** thay đổi theo phiên bản MongoDB:

  * **MongoDB 8.0+**: **khuyến nghị BẬT THP** (liên quan nâng cấp TCMalloc). 
  * **MongoDB 7.0 trở về trước**: **khuyến nghị TẮT THP**. 

### Lab

* So sánh plan trước/sau khi thêm index (đo `nReturned`, `totalDocsExamined`).
* Bật/tắt THP theo đúng version OS+MongoDB đang dùng (systemd service theo docs).

**Tips & Best Practices**

* Theo dõi **working set** có vừa RAM không; thiếu RAM ⇒ page faults ⇒ chậm.
* Kiểm chứng từng tối ưu bằng số liệu (profiler/APM/latency at P95).
* Kiểm tra version trước khi đụng **THP** để tránh tối ưu ngược. 

---

## Day 18 — **Khắc phục sự cố** thường gặp

**Tình huống 1: Replication lag**

* Đo lag bằng `rs.printSecondaryReplicationInfo()` và `rs.status()`; kiểm tra I/O, mạng, lock, oplog đầy/chật. 
* Nếu thiếu **oplog window**, cân nhắc tăng bằng `replSetResizeOplog`. 

**Tình huống 2: Balancer đứng im**

* Kiểm tra `sh.getBalancerState()` (bật/tắt) và `sh.isBalancerRunning()` (đang chạy?). Dùng `db.adminCommand({balancerStatus:1})` để biết chi tiết hơn. 

**Tình huống 3: Query chậm**

* Bật profiler mức 1 (slowms hợp lý), đọc `db.system.profile`, dùng `explain()` và thêm index đúng chỗ. 

**Tips & Best Practices**

* Ghi nhật ký thay đổi cấu hình (priority, votes, balancer) và làm trong **maintenance window**. 
* Với sharding, nhớ từ **6.0.3** trở đi không còn auto-split ⇒ theo dõi chunk/balancer. 

---

# Phụ lục nhanh

* **Kết nối** (SRV/Standard) & tham số URI. 
* **Tools**: `mongodump/mongorestore/mongoimport/mongostat/mongotop`. 
* **Tài liệu chính thức MongoDB**. 