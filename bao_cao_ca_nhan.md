# BÁO CÁO CÁ NHÂN – PHẦN CÔNG VIỆC KHÓ / PHỨC TẠP / Ý TƯỞNG MỚI

> **Dự án:** Hệ thống Quản lý Du lịch Trực tuyến (B2B SaaS)  
> **Môn học:** Phân tích Yêu cầu Phần mềm  
> **Giảng viên:** Đào Ngọc Phong  
> **Lớp:** E22CNPM04 – Học viện Công nghệ Bưu chính Viễn thông  
> **Ngày:** 08/06/2026

---

## 1. Lý do chọn đề tài

### 1.1. Bối cảnh thị trường

| Chỉ số | Giá trị |
|---|---|
| Doanh thu du lịch trực tuyến toàn cầu (2023) | ~750 tỷ USD |
| Tốc độ tăng trưởng CAGR (2023–2028) | 9,0%/năm |
| Lượt khách quốc tế đến Việt Nam (2024) | 18 triệu |
| Doanh nghiệp lữ hành có giấy phép tại VN | ~3.800 doanh nghiệp |
| Tỷ lệ đặt tour trực tuyến (2024) | ~42% |

### 1.2. Pain Points của doanh nghiệp lữ hành SME

Hơn **80% doanh nghiệp lữ hành Việt Nam là SME** đang gặp phải:

- **Quản lý thủ công bằng Excel/Zalo:** Dữ liệu phân tán, dễ mất, sai sót khi đặt chỗ
- **Không kiểm soát kho chỗ real-time:** Overbooking hoặc bỏ lỡ cơ hội bán
- **Điều phối HDV/Xe thủ công:** Trùng lịch, thiếu nhân sự ngày cao điểm
- **Không có báo cáo kinh doanh:** Quyết định dựa trên cảm tính
- **Bảo mật kém:** Lưu trữ Passport/CCCD không mã hóa – **vi phạm Nghị định 13/2023**

### 1.3. Khoảng trống thị trường

| Tiêu chí | Traveloka/iVIVU | Sabre/Amadeus | **Giải pháp nhóm** |
|---|---|---|---|
| Mô hình | B2C | B2B quốc tế | **B2B – SME Việt Nam** |
| Chi phí | Hoa hồng 15–25% | Phí license cao | **Phù hợp SME** |
| Quản lý tour tùy chỉnh | Không | Phức tạp | **Có – toàn diện** |
| Tuân thủ pháp lý VN | Một phần | Chỉ chuẩn quốc tế | **Cao – NĐ 168/2017, 13/2023** |
| Multi-tenancy | Không | Có | **Có – cô lập theo OrgID** |

> **Kết luận:** Thị trường VN chưa có giải pháp đồng thời: **(1) Quản lý toàn bộ vòng đời tour, (2) Chi phí phù hợp SME, (3) Tuân thủ đầy đủ pháp lý Việt Nam.**

---

## 2. Văn bản pháp lý liên quan

### 2.1. Danh mục văn bản đã tra cứu

| Văn bản | Nội dung liên quan | Ánh xạ vào hệ thống |
|---|---|---|
| Luật Du lịch 2017 | Kinh doanh lữ hành, hợp đồng, HDV | UC03, UC07, UC08 |
| Nghị định 85/2021/NĐ-CP | Đặt cọc tối thiểu 30%; chính sách hoàn/hủy | CBR-03 đến CBR-08, CBR-17 |
| Nghị định 13/2023/NĐ-CP | Bảo vệ dữ liệu cá nhân, AES-256, lưu 5 năm, Audit Log | CBR-11, CBR-15, CBR-19 |
| Luật Bảo vệ NTD 2023 | Khóa giá cố định trong quá trình thanh toán | CBR-25 (Price Lock 15 phút) |
| Luật Giá 2023 | Giới hạn biên độ giá động | CBR-26 (50%–150% giá gốc) |
| Quy định NHNN | Quy trình phê duyệt BNPL, hoàn tiền qua provider | CBR-21 |

### 2.2. Ánh xạ pháp lý vào Business Rules – Điểm phức tạp

Thách thức là **dịch ngôn ngữ pháp lý thành logic hệ thống**:

**Nghị định 85/2021 → Chính sách đặt cọc & hủy tour:**

| ID | Rule | Cơ sở |
|---|---|---|
| CBR-03 | Đặt cọc tối thiểu 30% khi xác nhận booking | NĐ 85/2021, Điều 14 |
| CBR-05 | Hủy ≥ 15 ngày: hoàn 100% | NĐ 85/2021 |
| CBR-06 | Hủy 8–14 ngày: phạt 30% | NĐ 85/2021 |
| CBR-07 | Hủy 3–7 ngày: phạt 50% | NĐ 85/2021 |
| CBR-08 | Hủy < 3 ngày / No-show: mất 100% | NĐ 85/2021 |

**Nghị định 13/2023 → Bảo vệ dữ liệu cá nhân:**
- **CBR-11:** Passport/CCCD mã hóa **AES-256**; hiển thị dạng `P****123` với vai trò thấp
- **CBR-15:** Lưu trữ thông tin cá nhân **tối thiểu 5 năm**
- **CBR-19:** **Audit Log** mọi thao tác nhạy cảm (tài khoản + IP + thời gian), lưu **≥ 12 tháng**

---

## 3. Yêu cầu phi chức năng (NFR)

### 3.1. Bảo mật

| Yêu cầu | Chỉ số / Tiêu chuẩn |
|---|---|
| Kết nối | HTTPS/TLS 1.2+ bắt buộc |
| Xác thực API | API Key + HMAC-SHA256 |
| Chống tấn công | OWASP Top 10: SQL Injection, XSS, CSRF + Rate Limiting |
| Brute-force | 5 lần thất bại → khóa 15 phút |
| Mã hóa DB | AES-256 cho Passport/CCCD, hồ sơ BNPL, ảnh eKYC |
| Audit Log | Lưu ≥ 12 tháng – tuân thủ NĐ 13/2023 |

### 3.2. Hiệu năng & Khả năng mở rộng

| Yêu cầu | Chỉ số |
|---|---|
| Response time API | < 2 giây (thường), ≤ 5 giây (thanh toán) |
| Throughput | 500 request đồng thời/giây |
| Người dùng đồng thời | 200 tổ chức + 10.000 end-users |
| Dynamic Pricing latency | **< 500ms/request** |
| Cache eviction giá động | **< 1 giây** sau giao dịch xác nhận |

### 3.3. Khả dụng & Khả năng bảo trì

| Yêu cầu | Chỉ số |
|---|---|
| Uptime | ≥ 99,9% (≤ 8,7 giờ downtime/năm) |
| Failover | Tự động < 30 giây |
| RTO / RPO | ≤ 4 giờ / ≤ 1 giờ |
| Unit test coverage | ≥ 70% module cốt lõi |
| API docs | Swagger/OpenAPI 3.0 đầy đủ |

### 3.4. Ràng buộc thiết kế

- **Kiến trúc:** 3-tier (Client – API Server – Database)
- **Backend:** Java Spring Boot + Node.js/Go cho dịch vụ tải cao
- **Database:** PostgreSQL/MySQL + Redis Cluster + Elasticsearch
- **Message Queue:** Apache Kafka hoặc RabbitMQ
- **Deploy:** Docker + Kubernetes; Cloud-agnostic (AWS/GCP/Azure)

---

## 4. Tích hợp và Dịch chuyển Dữ liệu

### 4.1. Cổng thanh toán (VNPay & MoMo)

```
Khách xác nhận → Tạo Payment Request (unique order code)
→ Redirect sang cổng thanh toán (khách KHÔNG nhập thẻ trên hệ thống)
→ [Thành công] Cổng gửi IPN Webhook → Xác minh HMAC-SHA256
→ Cập nhật "Đã thanh toán" + Hard-Lock slot + Phát E-Voucher
→ [Thất bại/Timeout] Hủy phiên, hoàn chỗ về kho
```

### 4.2. BNPL (Fundiin, Kredivo) + eKYC

```
Khách chọn BNPL → Gửi API + HMAC-SHA256 đến Provider (kèm eKYC: OCR CCCD + nhận diện khuôn mặt)
→ Credit Scoring real-time tại Provider
→ [Duyệt] IPN Webhook → Đánh dấu "Paid-BNPL" + Tạo hợp đồng tín dụng + E-Voucher
→ [Hủy] Hoàn qua cancellation API Provider (KHÔNG hoàn tiền mặt – CBR-21)
```

### 4.3. Dynamic Pricing Engine (DPE)

**Công thức:** `Giá = (Nhu cầu × 0,5) + (Lấp đầy × 0,3) + (Biên lợi nhuận × 0,2)`

- Tính giá < 500ms → Cache Redis 15 phút → Khóa giá cho phiên checkout
- Khi giao dịch xác nhận → Active eviction cache trong < 1 giây
- Biên độ 50%–150% giá gốc (CBR-26 – Luật Giá 2023)

### 4.4. Data Migration (Excel → Hệ thống)

1. Cung cấp Excel Template chuẩn → Doanh nghiệp điền & upload
2. Validate từng dòng (định dạng, trường bắt buộc) → Báo lỗi chi tiết
3. Chỉ import dòng hợp lệ; rollback nếu lỗi hệ thống
4. **Idempotent import** – import lại không tạo bản ghi trùng

---

## 5. Phần khó – Phức tạp – Ý tưởng mới

### 5.1. ⚡ Xử lý Race Condition trong đặt chỗ

**Bài toán:** Flash sale / nhiều người cùng đặt chỗ cuối → Overbooking.

**Giải pháp 3 tầng:**

| Tầng | Kỹ thuật | Mục đích |
|---|---|---|
| 1 – API Gateway | Token Bucket → Virtual Queue (Redis/RabbitMQ) | Lọc tải, hiển thị vị trí hàng chờ |
| 2 – Redis Lua Script | Atomic Decrement (Soft-Lock 15 phút) | Loại bỏ Race Condition hoàn toàn |
| 3 – Kafka Delayed Message | Auto-release sau 15 phút không thanh toán | Hoàn chỗ về kho tự động |

> Lua Script đảm bảo **nguyên tử** – kiểm tra & giảm chỗ trong 1 bước, không thể bị interrupt (kỹ thuật dùng bởi Ticketmaster, Booking.com).

### 5.2. 🧩 Saga Pattern – Giao dịch phân tán Microservices

**Bài toán:** Booking qua nhiều service độc lập; nếu 1 bước thất bại cần rollback nhất quán.

**Ví dụ thất bại:** Inventory ✅ → Pricing ✅ → Payment ✅ → Loyalty ❌

**Giải pháp Saga Orchestrator:**
- Phát hiện bước thất bại → Kích hoạt **Compensating Transactions** tự động
- Hoàn tiền → Nhả chỗ → Đảm bảo không "mất tiền mà không có chỗ"

> Không dùng ACID 2-Phase Commit vì quá chậm và có điểm thất bại đơn trong Microservices.

### 5.3. 🏨 Thuật toán Tetris xếp phòng (Bin Packing)

**Bài toán (NP-Hard):** Lịch đặt phòng phân mảnh → khoảng trống nhỏ lẻ không bán được.

**Giải pháp:** Cron Job chạy hàng đêm, gom đặt phòng vào số phòng vật lý ít nhất, tạo khoảng trống dài liên tục → bán được cho khách Group Booking dài ngày.

### 5.4. 💳 Credit Scoring tự động cho BNPL

```
Input: Lịch sử giao dịch + eKYC (OCR + Face Matching) + Hành vi tiêu dùng
→ ML Model: XGBoost / LightGBM
→ Output: Credit Score → Hạn mức tín dụng (VD: 10 triệu VNĐ)
→ Cấp hạn mức real-time, không cần duyệt thủ công
```

Trạng thái: `Inactive → Under Review → Active/Rejected → Suspended/Blocked`

### 5.5. 📊 Dynamic Pricing Engine – Cân bằng 3 yêu cầu mâu thuẫn

| Yêu cầu | Ràng buộc |
|---|---|
| Giá thay đổi theo thị trường | < 500ms/lần tính |
| Minh bạch với khách | Khóa giá 15 phút khi checkout (CBR-25) |
| Giá không vô lý | Biên độ 50%–150% giá gốc (CBR-26) |

**Giải pháp:** LSTM + Rule-based → Redis Cache (TTL 15 phút) → Active eviction khi giao dịch xác nhận

### 5.6. 🔐 Multi-tenancy với Data Isolation hoàn toàn

- Mọi bảng có cột `OrgID` làm **tenant discriminator**; Index đầy đủ trên `OrgID`
- `TourCode` unique **trong phạm vi OrgID** (CBR-13) – hai doanh nghiệp có thể trùng mã, không xung đột

---

## Tổng kết

| Phần công việc | Mức độ khó | Điểm nổi bật |
|---|---|---|
| Ánh xạ pháp lý vào Business Rules | ⭐⭐⭐⭐⭐ | 26 CBR tuân thủ 6 văn bản pháp luật |
| Race Condition – Redis Lua + Kafka | ⭐⭐⭐⭐⭐ | Token Bucket + Atomic Lua + Delayed Release |
| Saga Pattern Microservices | ⭐⭐⭐⭐ | Compensating Transaction tự động |
| Dynamic Pricing + Cache Eviction | ⭐⭐⭐⭐ | Real-time pricing + price lock checkout |
| BNPL + eKYC + Credit Scoring ML | ⭐⭐⭐⭐ | Duyệt hạn mức real-time |
| Tetris xếp phòng (Bin Packing) | ⭐⭐⭐ | Tối ưu khoảng trống lịch trình |
| NFR – Performance + Security | ⭐⭐⭐⭐ | 99,9% uptime, <500ms DPE, AES-256 |
| Data Migration từ Excel | ⭐⭐⭐ | Validate trước import, idempotent |

---

*Tài liệu tổng hợp từ: `design_analysis.txt`, `bao_cao_phan_tich_nhom.md`, `market_analysis.txt`*
