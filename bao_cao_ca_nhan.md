# BÁO CÁO CÁ NHÂN – PHẦN CÔNG VIỆC KHÓ / PHỨC TẠP / Ý TƯỞNG MỚI

> **Dự án:** Hệ thống Quản lý Du lịch Trực tuyến (B2B SaaS)  
> **Môn học:** Phân tích Yêu cầu Phần mềm  
> **Giảng viên:** Đào Ngọc Phong  
> **Lớp:** E22CNPM04 – Học viện Công nghệ Bưu chính Viễn thông  
> **Ngày:** 08/06/2026

---

## MỤC LỤC

1. [Lý do chọn đề tài](#1-lý-do-chọn-đề-tài)
2. [Văn bản pháp lý liên quan](#2-văn-bản-pháp-lý-liên-quan)
3. [Yêu cầu phi chức năng (NFR)](#3-yêu-cầu-phi-chức-năng-nfr)
4. [Tích hợp và dịch chuyển dữ liệu](#4-tích-hợp-và-dịch-chuyển-dữ-liệu)
5. [Phần khó – Phức tạp – Ý tưởng mới nổi bật](#5-phần-khó--phức-tạp--ý-tưởng-mới-nổi-bật)

---

## 1. Lý do chọn đề tài

### 1.1. Bối cảnh thị trường – Cơ sở thực tiễn

Việc chọn đề tài **"Hệ thống Quản lý Du lịch Trực tuyến"** không phải ngẫu nhiên mà xuất phát từ phân tích bài bản về khoảng trống thị trường:

| Chỉ số | Giá trị | Nguồn |
|---|---|---|
| Doanh thu du lịch trực tuyến toàn cầu (2023) | ~750 tỷ USD | Statista, Phocuswright |
| Tốc độ tăng trưởng kép CAGR (2023–2028) | 9,0%/năm | Allied Market Research |
| Lượt khách quốc tế đến Việt Nam (2024) | 18 triệu | Tổng cục Du lịch VN |
| Tổng doanh thu du lịch Việt Nam (2024) | ~840.000 tỷ VNĐ (~34 tỷ USD) | Tổng cục Thống kê |
| Doanh nghiệp lữ hành có giấy phép tại VN | ~3.800 doanh nghiệp | Sở Du lịch các tỉnh |
| Tỷ lệ đặt tour trực tuyến (2024) | ~42% | Vietnam E-commerce Report |

### 1.2. Bài toán thực tế – Pain Points của doanh nghiệp lữ hành SME

Qua khảo sát, hơn **80% doanh nghiệp lữ hành Việt Nam là SME** đang gặp phải:

| STT | Vấn đề | Mô tả thực tế | Hậu quả |
|---|---|---|---|
| 1 | Quản lý tour bằng Excel | Lịch trình, bảng giá, danh sách khách lưu trong nhiều file rời rạc | Dữ liệu phân tán, dễ mất |
| 2 | Tiếp nhận đặt chỗ qua Zalo/điện thoại | Nhân viên ghi chép thủ công | Sai sót, đặt trùng/quá chỗ |
| 3 | Không kiểm soát kho chỗ real-time | Không biết còn bao nhiêu chỗ trống | Overbooking hoặc bỏ lỡ cơ hội bán |
| 4 | Điều phối HDV/Xe thủ công | Gán hướng dẫn viên bằng nhớ hoặc Excel | Trùng lịch, thiếu nhân sự ngày cao điểm |
| 5 | Không có báo cáo kinh doanh | Không tổng hợp được doanh thu, tỷ lệ lấp đầy | Quyết định kinh doanh dựa trên cảm tính |
| 6 | Bảo mật thông tin kém | Thông tin Passport/CCCD lưu trữ không mã hóa | **Vi phạm Nghị định 13/2023/NĐ-CP** |

### 1.3. Khoảng trống thị trường – Lý do nhóm đề xuất hệ thống mới

| Tiêu chí | Traveloka / iVIVU | Sabre / Amadeus | Giải pháp nhóm |
|---|---|---|---|
| Mô hình | B2C – Sàn bán cho khách lẻ | B2B – Hệ thống GDS quốc tế | **B2B – Quản lý nội bộ cho DN lữ hành VN** |
| Chi phí | Hoa hồng 15–25% | Phí license rất cao | **Phù hợp với SME Việt Nam** |
| Quản lý tour tùy chỉnh | Không hỗ trợ | Phức tạp, không phù hợp VN | **Có – từ A đến Z** |
| Điều phối HDV/Xe | Không hỗ trợ | Hạn chế | **Có – module vận hành đầy đủ** |
| Tuân thủ Luật Du lịch VN | Một phần | Chỉ chuẩn quốc tế | **Cao – Nghị định 168/2017, 13/2023** |
| Đa doanh nghiệp (Multi-tenancy) | Không | Có | **Có – dữ liệu cô lập theo OrgID** |

> **Kết luận:** Trên thị trường Việt Nam hiện tại chưa có một giải pháp phần mềm nào đồng thời đáp ứng đủ 3 yếu tố: **(1) Quản lý toàn bộ vòng đời tour, (2) Chi phí phù hợp SME, (3) Tuân thủ đầy đủ pháp lý Việt Nam**. Đây chính là khoảng trống thị trường mà hệ thống hướng tới lấp đầy.

---

## 2. Văn bản pháp lý liên quan

Đây là phần **khó và tốn nhiều công nghiên cứu nhất** trong dự án. Nhóm phải đọc và ánh xạ từng điều khoản luật vào các Business Rules cụ thể của hệ thống.

### 2.1. Danh mục văn bản pháp lý đã tra cứu

| STT | Văn bản | Nội dung liên quan | Ánh xạ vào hệ thống |
|---|---|---|---|
| 1 | **Luật Du lịch 2017** | Quy định về hoạt động kinh doanh lữ hành, hướng dẫn viên, hợp đồng lữ hành | UC03 (Tạo tour), UC07 (Điều phối HDV), UC08 (Danh sách hành khách) |
| 2 | **Nghị định 168/2017/NĐ-CP** | Quy định chi tiết về kinh doanh lữ hành và hướng dẫn du lịch | CBR-14 (Lịch HDV không trùng), UC07 |
| 3 | **Nghị định 85/2021/NĐ-CP** | Điều 14: Đặt cọc tối thiểu 30%; Chính sách hoàn/hủy theo mốc thời gian | CBR-03, CBR-05, CBR-06, CBR-07, CBR-08, CBR-17 |
| 4 | **Nghị định 13/2023/NĐ-CP** | Bảo vệ dữ liệu cá nhân: mã hóa Passport/CCCD, lưu trữ tối thiểu 5 năm, ghi Audit Log | CBR-11, CBR-15, CBR-19, NFR-Security |
| 5 | **Luật Bảo vệ Người tiêu dùng 2023** | Giá phải được khóa cố định trong quá trình thanh toán, không thay đổi đột ngột | CBR-25 (Price Lock 15 phút) |
| 6 | **Luật Giá 2023** | Giới hạn biên độ thay đổi giá động | CBR-26 (Dynamic Pricing: 50%–150% giá gốc) |
| 7 | **Quy định NHNN về Cho vay tiêu dùng** | Quy trình phê duyệt BNPL, hoàn tiền khoản vay qua nhà cung cấp | CBR-21 (Hoàn BNPL qua provider) |
| 8 | **Quyết định 147/QĐ-TTg** | Chiến lược phát triển du lịch Việt Nam đến 2030 | Cơ sở phân tích thị trường, mục tiêu hệ thống |

### 2.2. Cách ánh xạ pháp lý vào Business Rules (CBR) – Điểm phức tạp

Thách thức không chỉ là đọc luật, mà là **dịch ngôn ngữ pháp lý thành logic hệ thống**. Dưới đây là ví dụ điển hình:

#### Nghị định 85/2021 → CBR-03 đến CBR-08: Chính sách đặt cọc & hủy tour

```
Nghị định 85/2021, Điều 14:
  "Bên đặt tour phải đặt cọc tối thiểu 30% tổng giá trị hợp đồng"
  "Hủy trước 15 ngày: hoàn 100%"
  "Hủy 8–14 ngày: phạt 30%"
  "Hủy 3–7 ngày: phạt 50%"
  "Hủy < 3 ngày hoặc No-show: phạt 100%"
```

↓ **Ánh xạ thành Business Rules trong hệ thống:**

| ID | Rule | Cơ sở pháp lý |
|---|---|---|
| CBR-03 | Đặt cọc tối thiểu 30% tổng đơn hàng khi xác nhận booking | Nghị định 85/2021, Điều 14 |
| CBR-04 | Thanh toán đủ 100% trước 7 ngày khởi hành | Quy định nội bộ (bổ sung) |
| CBR-05 | Hủy ≥ 15 ngày: hoàn 100% đặt cọc | Nghị định 85/2021 |
| CBR-06 | Hủy 8–14 ngày: phạt 30% tổng đơn | Nghị định 85/2021 |
| CBR-07 | Hủy 3–7 ngày: phạt 50% tổng đơn | Nghị định 85/2021 |
| CBR-08 | Hủy < 3 ngày hoặc No-show: mất 100% | Nghị định 85/2021 |

> **Điểm khó:** Các rule CBR-03 đến CBR-08 được thiết kế có thể cấu hình linh hoạt theo từng doanh nghiệp (qua module Enterprise Configuration – UC10), vừa tuân thủ pháp lý vừa đảm bảo tính linh hoạt thương mại.

#### Nghị định 13/2023 → CBR-11, CBR-15, CBR-19: Bảo vệ dữ liệu cá nhân

- **CBR-11:** Số Passport/CCCD **bắt buộc mã hóa AES-256** tại tầng database. Chỉ Operator và Manager mới xem được thông tin đầy đủ; các vai trò khác thấy dạng che `P****123`.
- **CBR-15:** Thông tin cá nhân khách hàng phải lưu trữ **tối thiểu 5 năm** từ giao dịch cuối cùng.
- **CBR-19:** Mọi hành động tạo/sửa/xóa dữ liệu nhạy cảm phải ghi **Audit Log** kèm: tài khoản, thời gian, địa chỉ IP, nội dung thay đổi. Log lưu tối thiểu **12 tháng**.

---

## 3. Yêu cầu phi chức năng (NFR)

Phần NFR được phân thành **7 nhóm** chính, mỗi nhóm gắn với bối cảnh kỹ thuật và pháp lý cụ thể.

### 3.1. Bảo mật (Security) – NFR phức tạp nhất

| Yêu cầu | Chỉ số / Tiêu chuẩn | Lý do / Cơ sở |
|---|---|---|
| Mã hóa kết nối | HTTPS/TLS 1.2+ bắt buộc toàn bộ | Bảo vệ dữ liệu truyền tải |
| Xác thực API bên thứ 3 | API Key + HMAC-SHA256 signature | Tích hợp VNPay, MoMo |
| Chống tấn công | SQL Injection, XSS, CSRF prevention + Rate Limiting | OWASP Top 10 |
| Brute-force login | Tối đa 5 lần thất bại → khóa tài khoản 15 phút | Bảo mật tài khoản người dùng |
| Mã hóa dữ liệu nhạy cảm | **AES-256** tại tầng database cho Passport/CCCD, hồ sơ tín dụng BNPL, ảnh eKYC | Nghị định 13/2023 |
| Audit Log | Toàn bộ hành động nhạy cảm: tài khoản + thời gian + IP + nội dung, lưu **≥ 12 tháng** | Nghị định 13/2023 |
| Che thông tin | Credit contract số dạng `CN****5678`, Passport dạng `P****123` | Phân quyền theo vai trò |

### 3.2. Hiệu năng & Khả năng mở rộng (Performance & Scalability)

| Yêu cầu | Chỉ số cụ thể |
|---|---|
| Response time API thông thường | < 2 giây dưới tải bình thường |
| Response time API thanh toán | ≤ 5 giây |
| Throughput tối thiểu | **500 request đồng thời/giây** không gián đoạn |
| Số người dùng đồng thời | Tối thiểu **200 tổ chức + 10.000 end users** cùng lúc |
| Kiến trúc mở rộng ngang | Application Server **stateless** → thêm instance dễ dàng sau Load Balancer |
| Caching | Redis TTL cho danh mục tour, bảng giá công khai |
| Tối ưu database | Index đầy đủ trên `OrgID`, `TourID`, `DepartureDate`; partition bảng Booking theo năm khi > 1 triệu bản ghi |
| Xử lý bất đồng bộ | Xuất báo cáo Excel/PDF lớn → Job Queue (không block main thread) |
| **Dynamic Pricing latency** | Tính lại giá theo tỷ lệ lấp đầy và ngày khởi hành: **< 500ms/request** |
| **Cache eviction cho giá động** | Khi giao dịch tour xác nhận → invalidate cache giá trong **< 1 giây** |

> ⚡ **Điểm khó:** Yêu cầu về Dynamic Pricing latency (< 500ms) và cache eviction (< 1 giây) đòi hỏi thiết kế Redis Cluster với chiến lược active cache invalidation, không phải TTL thụ động thông thường.

### 3.3. Khả dụng (Availability)

| Yêu cầu | Chỉ số |
|---|---|
| Uptime cam kết | **≥ 99,9%** (tương đương ≤ 8,7 giờ downtime/năm) |
| Failover tự động | Active-Passive hoặc Active-Active database → chuyển đổi tự động **< 30 giây** |
| Backup dữ liệu | Full Backup hàng ngày + Incremental mỗi 4 giờ, lưu offsite ≥ 30 ngày |
| Phục hồi thảm họa | **RTO ≤ 4 giờ**, **RPO ≤ 1 giờ** |
| Bảo trì có kế hoạch | Thông báo trước 24h, thực hiện 2:00–5:00 SA, tối đa 2 giờ/phiên |
| Monitoring | Prometheus + Grafana: CPU, RAM, API latency, error rate → alert tự động khi vượt ngưỡng |

### 3.4. Tính di động (Portability)

- **Trình duyệt:** Chrome v100+, Firefox v100+, Edge v100+, Safari v15+
- **Thiết bị:** Desktop ≥1280px, Tablet ≥768px, Mobile ≥375px (Responsive Design)
- **Hệ điều hành backend:** Không phụ thuộc OS – triển khai được trên Linux/Windows Server/Docker
- **Cloud:** AWS, Google Cloud, Azure mà **không cần thay đổi source code**
- **Database:** ORM → hỗ trợ migrate giữa MySQL và PostgreSQL

### 3.5. Khả năng bảo trì (Maintainability)

- Kiến trúc phân tầng rõ ràng: **Presentation → Business Logic → Data Access**
- API documentation: **Swagger/OpenAPI 3.0** đầy đủ cho mọi endpoint
- Log phân cấp JSON format + **Correlation ID** để trace xuyên suốt các module
- Unit test coverage **≥ 70%** cho các module cốt lõi (Booking, Payment, Inventory)
- Quản lý version: **GitFlow** + **Semantic Versioning (MAJOR.MINOR.PATCH)**

### 3.6. Khả năng sử dụng (Usability) – Điểm mới

| Yêu cầu | Chi tiết |
|---|---|
| Thời gian làm quen | Người dùng cơ bản có thể thực hiện nghiệp vụ chính **trong ≤ 2 giờ** |
| Thông báo lỗi | Tiếng Việt, mô tả rõ nguyên nhân và hướng xử lý |
| Đa ngôn ngữ (i18n) | Tiếng Việt (mặc định) + Tiếng Anh; source code dùng locale files |
| Accessibility | **WCAG 2.1 Level AA**: keyboard navigation, color contrast ≥ 4,5:1, alt text, label |
| **Dynamic price transparency** | Khi giá thay đổi trong lúc tìm kiếm → hiển thị banner giải thích lý do. Khi bắt đầu checkout → banner "Giá được khóa trong 15 phút" |
| **BNPL loan status** | Progress bar từng bước: Đã nộp đơn → Đang xét duyệt → Được duyệt/Từ chối, kèm thông báo ngôn ngữ địa phương |

### 3.7. Ràng buộc thiết kế (Design Constraints)

- Kiến trúc **3-tier** (Client – API Server – Database)
- Backend: **Java Spring Boot** (nghiệp vụ lõi) + **Node.js/NestJS hoặc Go** (dịch vụ xử lý đồng thời cao)
- Database: **PostgreSQL/MySQL** (quan hệ) + **Redis Cluster** (caching & distributed lock) + **Elasticsearch** (tìm kiếm)
- Message Queue: **Apache Kafka hoặc RabbitMQ**
- Tiêu chuẩn thanh toán: **PCI DSS** (bảo mật giao dịch thẻ)
- Container: **Docker** + **Kubernetes**

---

## 4. Tích hợp và Dịch chuyển Dữ liệu

### 4.1. Tích hợp cổng thanh toán (VNPay & MoMo)

**Luồng tích hợp đầy đủ:**

```
Khách hàng xác nhận thanh toán
        ↓
Hệ thống tạo Payment Request (unique order code)
        ↓
Redirect sang trang xác thực của cổng thanh toán
  (Khách không nhập thông tin thẻ trên hệ thống)
        ↓
[Giao dịch thành công] → Cổng thanh toán gửi IPN Webhook
        ↓
Hệ thống xác minh chữ ký HMAC-SHA256
        ↓
Cập nhật trạng thái đơn → "Đã thanh toán"
Trừ slot đã khóa (Hard-Lock)
Tự động phát hành E-Voucher qua Email
        ↓
[Giao dịch thất bại / Timeout] → Hủy phiên, trả chỗ về kho
```

**Điểm kỹ thuật đáng chú ý:**
- Toàn bộ giao tiếp qua **RESTful API + HTTPS + HMAC-SHA256**
- Khách hàng **không bao giờ nhập thông tin thẻ trên hệ thống** → giảm phạm vi PCI DSS
- IPN Webhook phải được xác minh chữ ký trước khi cập nhật trạng thái để chống giả mạo

### 4.2. Tích hợp BNPL (Fundiin, Kredivo)

**Luồng eKYC + Phê duyệt tín dụng:**

```
Khách chọn "Mua trước trả sau"
        ↓
Hệ thống gửi API request + HMAC-SHA256 đến BNPL Provider
  (Kèm thông tin eKYC: OCR CCCD + nhận diện khuôn mặt)
        ↓
Redirect sang portal của BNPL Provider
  → Chạm điểm tín dụng real-time (Credit Scoring)
        ↓
[Duyệt] → Provider gọi IPN Webhook về hệ thống
Hệ thống đánh dấu đơn "Paid-BNPL"
Tự động tạo hợp đồng tín dụng mini + E-Voucher
        ↓
[Hủy đơn BNPL] → Hoàn tiền qua cancellation API của Provider
  (Hoàn về tài khoản Provider, KHÔNG hoàn tiền mặt cho khách)
  → CBR-21 tuân thủ quy định NHNN về cho vay tiêu dùng
```

**Quy trình eKYC:**
1. Thu thập: Số điện thoại + CCCD/CMND
2. Nhận diện điện tử: OCR trích xuất dữ liệu giấy tờ tùy thân
3. Đối khớp sinh trắc học: Liveness Detection (chống ảnh giả)
4. Credit Scoring: Phân tích lịch sử giao dịch + hành vi tiêu dùng → cấp hạn mức

### 4.3. Tích hợp Dynamic Pricing Engine (DPE)

**Công thức tính giá động:**

$$\text{Giá điều chỉnh} = (\text{Nhu cầu thị trường} \times 0{,}5) + (\text{Tỷ lệ lấp đầy còn lại} \times 0{,}3) + (\text{Biên lợi nhuận} \times 0{,}2)$$

**Kiến trúc tích hợp DPE:**

```
Booking Service → RESTful endpoint → Dynamic Pricing Engine
                                             ↓
                                   Tính giá (< 500ms)
                                             ↓
                                   Lưu Redis Cache (TTL = 15 phút)
                                             ↓
                        Giá được khóa cho phiên của khách hàng
                                             ↓
                        Khi giao dịch xác nhận → Active Eviction
                        (Invalidate cache trong < 1 giây)
```

**Ràng buộc pháp lý trên DPE:**
- Giá chỉ được điều chỉnh trong biên độ **50% – 150%** giá gốc cấu hình trong PricePlan (CBR-26 – Luật Giá 2023)
- Giá **bị khóa cố định** ngay khi khách bắt đầu luồng checkout, không thể thay đổi trong 15 phút (CBR-25 – Luật Bảo vệ NTD 2023)

### 4.4. Dịch chuyển dữ liệu (Data Migration)

**Bài toán:** Hỗ trợ doanh nghiệp lữ hành chuyển đổi từ Excel/Zalo sang hệ thống mới mà không mất dữ liệu lịch sử.

**Quy trình:**

```
1. Hệ thống cung cấp Excel Template chuẩn (.xlsx)
   Các cột bắt buộc:
   - Họ tên, Số điện thoại, Email, Ngày sinh
   - Số Passport/CCCD
   - Lịch sử đặt tour

2. Doanh nghiệp điền dữ liệu → Upload file

3. Hệ thống tự động Validate từng dòng:
   - Kiểm tra định dạng, trường bắt buộc
   - Đánh dấu bản ghi lỗi (highlight màu đỏ)
   - Sinh báo cáo lỗi chi tiết để người dùng sửa

4. Chỉ import các dòng hợp lệ vào database
   - Không ảnh hưởng dữ liệu hiện có
   - Rollback nếu phát hiện lỗi hệ thống

5. Confirm và hoàn tất migration
```

**Điểm đảm bảo chất lượng:**
- Validate **trước khi ghi vào database** → không có dữ liệu bẩn
- Hỗ trợ **idempotent import** (import lại file không tạo bản ghi trùng)
- Báo cáo lỗi chi tiết → người dùng biết chính xác cần sửa gì

---

## 5. Phần khó – Phức tạp – Ý tưởng mới nổi bật

### 5.1. ⚡ Xử lý Race Condition trong đặt chỗ – Vấn đề khó nhất

**Bài toán:** Khi có flash sale hoặc nhiều người cùng đặt chỗ cuối cùng trong cùng một thời điểm, hệ thống có thể xác nhận quá số chỗ thực tế (Race Condition).

**Giải pháp 3 tầng:**

```
Tầng 1: API Gateway – Token Bucket / Leaky Bucket
  → Lọc lưu lượng, đẩy request vượt tải vào Virtual Queue (Redis/RabbitMQ)
  → Giao diện hiển thị: "Bạn đang ở vị trí thứ 1.234..."

Tầng 2: Redis Lua Script – Atomic Decrement (Soft-Lock)
  → Khi khách nhấn "Đặt ngay": Redis Lua Script giảm số chỗ khả dụng một cách nguyên tử
  → Chỉ 1 người giành được slot cuối cùng (loại bỏ Race Condition hoàn toàn)
  → Trạng thái: PENDING_PAYMENT (Soft-Lock 15 phút)

Tầng 3: Kafka Delayed Message – Auto Release
  → Sau 15 phút không nhận được IPN webhook thanh toán
  → Job tự động giải phóng khóa, hoàn trả chỗ về kho
```

> **Tại sao dùng Lua Script thay vì transaction thông thường?**  
> Redis Lua Script đảm bảo tính **nguyên tử (Atomic)** – tức là kiểm tra và giảm số chỗ xảy ra trong một bước duy nhất, không thể bị interrupt giữa chừng bởi request khác. Đây là kỹ thuật được các hệ thống như Ticketmaster, Booking.com sử dụng.

### 5.2. 🧩 Saga Pattern – Giao dịch phân tán trong Microservices

**Bài toán:** Đặt tour liên quan đến nhiều dịch vụ độc lập. Nếu một bước thất bại sau khi các bước trước đã thành công, cần rollback nhất quán.

**Ví dụ scenario thất bại:**

```
Bước 1: Inventory Service → Giữ chỗ ✅
Bước 2: Pricing Service   → Chốt giá  ✅
Bước 3: Payment Gateway   → Trừ tiền  ✅
Bước 4: Loyalty Service   → Cộng điểm ❌ (THẤT BẠI)
Bước 5: Notification      → (chưa chạy)
```

**Giải pháp Saga Orchestrator:**

```
→ Phát hiện Bước 4 thất bại
→ Tự động kích hoạt Compensating Transactions:
   - Bước 3 bù trừ: Hoàn tiền về tài khoản khách
   - Bước 1 bù trừ: Nhả chỗ về kho
   → Đảm bảo KHÔNG bao giờ xảy ra "mất tiền mà không có chỗ"
```

> **Tại sao không dùng ACID transaction truyền thống?** Trong Microservices, mỗi service có database riêng, không thể dùng 2-Phase Commit vì quá chậm và có điểm thất bại đơn. Saga Pattern giải quyết bằng cách chain các local transaction kèm compensating transaction.

### 5.3. 🏨 Thuật toán Tetris xếp phòng (Bin Packing Problem)

**Bài toán (NP-Hard):** Tour/phòng bị phân mảnh lịch trình → khoảng trống nhỏ lẻ không thể bán được cho khách dài ngày.

**Ví dụ:**
```
Phòng 302:
  Khách A: 1/6 → 3/6
  Khoảng trống: 4/6 (1 ngày) ← Không thể bán cho khách muốn 3+ ngày
  Khách B: 5/6 → 7/6
```

**Giải pháp:** Cron Job chạy ngầm hàng đêm, tự động xáo trộn gán phòng vật lý theo thuật toán Tetris:
- Gom các đặt phòng của cùng loại phòng vào một số phòng vật lý ít nhất
- Tạo ra các "khoảng trống dài liên tục" ở các phòng còn lại
- Các khoảng trống dài → bán được cho khách Group Booking dài ngày

### 5.4. 💳 Hệ thống Credit Scoring tự động cho BNPL

**Pipeline chấm điểm tín dụng:**

```
Input Data:
  - Lịch sử giao dịch nội bộ (booking history)
  - Kết quả eKYC (OCR + Face Matching)
  - Thói quen tiêu dùng (behavioral signals)
  - (Tùy chọn) Điểm tín dụng CIC quốc gia
          ↓
ML Model: XGBoost / LightGBM (phân loại rủi ro)
          ↓
Output: Credit Score → Hạn mức tín dụng (VD: 10 triệu VNĐ)
          ↓
Trạng thái BNPL:
  Inactive → Under Review → Active / Rejected
  Active → Suspended (nếu trễ thanh toán)
  Active → Blocked (nếu phát hiện gian lận / nợ xấu > 90 ngày)
```

**Điểm mới:** Hạn mức được **cấp ngay lập tức trên giao diện** (real-time scoring), không cần chờ duyệt thủ công như mô hình ngân hàng truyền thống.

### 5.5. 📊 Dynamic Pricing Engine – Tính giá thời gian thực

**Điểm phức tạp:** Phải cân bằng giữa 3 yêu cầu mâu thuẫn:

| Yêu cầu | Ràng buộc |
|---|---|
| Giá thay đổi liên tục theo thị trường | Phải < 500ms mỗi lần tính |
| Giá phải minh bạch với khách | Phải lock giá trong 15 phút khi checkout |
| Giá không được vô lý | Biên độ 50%–150% giá gốc (Luật Giá 2023) |

**Giải pháp:**
- Dùng mô hình LSTM (chuỗi thời gian) + công thức Rule-based kết hợp
- Cache kết quả trong Redis (TTL = 15 phút = Seat Lock duration)
- Active eviction khi giao dịch xác nhận (< 1 giây)
- Price Lock bắt đầu từ khi khách vào luồng checkout (CBR-25)

### 5.6. 🔐 Multi-tenancy với Data Isolation hoàn toàn

**Thách thức:** Nhiều doanh nghiệp lữ hành dùng chung một hệ thống (SaaS), nhưng dữ liệu phải hoàn toàn tách biệt – doanh nghiệp A không bao giờ nhìn thấy dữ liệu của doanh nghiệp B.

**Giải pháp:**
- Mọi bảng dữ liệu đều có cột `OrgID` làm **tenant discriminator**
- Index đầy đủ trên `OrgID` để đảm bảo performance
- Mã tour `TourCode` chỉ unique **trong phạm vi `OrgID`** (CBR-13) – hai doanh nghiệp khác nhau có thể trùng mã, nhưng không có xung đột dữ liệu

---

## Tổng kết

| Phần công việc | Mức độ khó | Điểm mới |
|---|---|---|
| Nghiên cứu & ánh xạ văn bản pháp lý vào Business Rules | ⭐⭐⭐⭐⭐ | Xây dựng bộ 26 CBR tuân thủ 6 văn bản pháp luật |
| Race Condition – Redis Lua Script + Virtual Queue | ⭐⭐⭐⭐⭐ | Kết hợp Token Bucket + Atomic Lua + Kafka Delayed Message |
| Saga Pattern cho Microservices | ⭐⭐⭐⭐ | Compensating Transaction tự động, không mất dữ liệu |
| Dynamic Pricing Engine + Cache Eviction | ⭐⭐⭐⭐ | Cân bằng real-time pricing với price lock trong checkout |
| BNPL + eKYC + Credit Scoring tự động | ⭐⭐⭐⭐ | Pipeline ML chấm điểm real-time, duyệt tức thì |
| Thuật toán Tetris xếp phòng (Bin Packing) | ⭐⭐⭐ | Tối ưu khoảng trống lịch trình bằng Cron Job hàng đêm |
| NFR – Performance + Availability + Security | ⭐⭐⭐⭐ | Chỉ số cụ thể: 99,9% uptime, < 500ms DPE, AES-256 |
| Data Migration từ Excel sang hệ thống | ⭐⭐⭐ | Validate trước import, báo cáo lỗi chi tiết, idempotent |

---

*Tài liệu này tổng hợp từ bộ SRS của nhóm: `design_analysis.txt`, `bao_cao_phan_tich_nhom.md`, `market_analysis.txt`, và file DOCX phân tích thiết kế.*
