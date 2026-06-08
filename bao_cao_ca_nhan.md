# BÁO CÁO CÁ NHÂN – PHẦN CÔNG VIỆC KHÓ / PHỨC TẠP / Ý TƯỞNG MỚI

> **Dự án:** Hệ thống Quản lý Du lịch Trực tuyến (B2B SaaS)  
> **Môn học:** Phân tích Yêu cầu Phần mềm  
> **Giảng viên:** Đào Ngọc Phong  
> **Lớp:** E22CNPM04 – Học viện Công nghệ Bưu chính Viễn thông  
> **Ngày:** 08/06/2026

---

## 1. Tìm hiểu lý do chọn đề tài

### Điều thấy khó / phức tạp

Khi bắt đầu, nhóm chỉ có ý tưởng chung là làm hệ thống du lịch. Phần khó là phải **chứng minh được đề tài này thực sự cần thiết** – không phải chỉ nói "thị trường du lịch lớn" mà phải chỉ ra được khoảng trống mà các giải pháp hiện tại chưa lấp đầy.

Tôi phải tìm hiểu và so sánh:
- **Traveloka/iVIVU** – hướng đến khách lẻ (B2C), không hỗ trợ doanh nghiệp quản lý nội bộ
- **Sabre/Amadeus** – dành cho hãng hàng không quốc tế, phí license cao, không phù hợp SME Việt Nam
- **Excel/Zalo** – thực tế >80% doanh nghiệp lữ hành SME đang dùng, gây ra hàng loạt vấn đề: đặt trùng chỗ, mất dữ liệu, vi phạm pháp lý

### Ý tưởng mới nhận ra

Thị trường Việt Nam đang thiếu một giải pháp **đồng thời** đáp ứng 3 yếu tố: quản lý toàn bộ vòng đời tour + chi phí phù hợp SME + tuân thủ pháp lý Việt Nam. Đây là lý do chính để nhóm đề xuất hệ thống B2B SaaS dành riêng cho doanh nghiệp lữ hành nội địa.

---

## 2. Tìm kiếm văn bản pháp lý liên quan

### Điều thấy khó / phức tạp

Đây là phần **tốn nhiều công nhất**. Ban đầu tôi nghĩ chỉ cần tra Luật Du lịch là đủ, nhưng thực tế hệ thống liên quan đến nhiều lĩnh vực pháp lý khác nhau mà phải tìm từng cái:

| Văn bản | Lý do liên quan (phát hiện trong quá trình làm) |
|---|---|
| Luật Du lịch 2017 | Quy định về kinh doanh lữ hành, hợp đồng tour, hướng dẫn viên |
| Nghị định 85/2021/NĐ-CP | Phát hiện khi thiết kế tính năng đặt cọc & hủy tour – có quy định cụ thể về mức phạt |
| Nghị định 13/2023/NĐ-CP | Phát hiện khi nhóm bàn về lưu trữ Passport/CCCD – phải mã hóa AES-256, lưu 5 năm |
| Luật Bảo vệ NTD 2023 | Phát hiện khi thiết kế Dynamic Pricing – giá phải khóa cố định trong checkout |
| Luật Giá 2023 | Bổ sung sau – giới hạn biên độ giá động 50%–150% |
| Quy định NHNN | Phát hiện khi tích hợp BNPL – hoàn tiền phải qua provider, không trả tiền mặt |

**Phần khó nhất không phải là tìm luật, mà là dịch ngôn ngữ pháp lý thành logic hệ thống.** Ví dụ, Nghị định 85/2021 viết:

> *"Hủy trước 15 ngày: hoàn 100%; Hủy 8–14 ngày: phạt 30%; Hủy 3–7 ngày: phạt 50%; Hủy dưới 3 ngày hoặc No-show: phạt 100%"*

→ Phải tách thành 5 Business Rules riêng (CBR-03 đến CBR-08), xác định rõ: tính từ ngày nào, phạt trên tổng đơn hay đặt cọc, cần cấu hình linh hoạt theo từng doanh nghiệp hay cố định.

### Điều rút ra

Cần đọc luật **từ góc độ lập trình viên**: mỗi điều khoản là một điều kiện `if-else`, mỗi mức phạt là một con số cần lưu vào database. Nếu chỉ đọc lướt qua thì rất dễ bỏ sót các trường hợp biên (edge case).

---

## 3. Yêu cầu phi chức năng (NFR)

### Điều thấy khó / phức tạp

NFR dễ bị viết chung chung kiểu *"hệ thống phải nhanh, phải bảo mật"* mà không có con số cụ thể. Phần khó là phải **lượng hóa từng yêu cầu** và giải thích tại sao chọn ngưỡng đó.

**Ví dụ 1 – Dynamic Pricing latency:**  
Ban đầu chỉ ghi "tính giá phải nhanh". Sau khi phân tích luồng người dùng (nhấn xem giá → chờ → chọn mua), nếu tính giá > 1 giây thì người dùng sẽ thoát ra. Từ đó xác định ngưỡng **< 500ms/request** là hợp lý và yêu cầu kỹ thuật: Redis Cache + Active Eviction (không dùng TTL thụ động).

**Ví dụ 2 – Bảo mật dữ liệu:**  
Không chỉ ghi "mã hóa dữ liệu" mà phải chỉ rõ: mã hóa **AES-256 tại tầng database**, chỉ Operator/Manager mới xem được, các vai trò khác thấy dạng `P****123`. Điều này xuất phát trực tiếp từ Nghị định 13/2023.

**Ví dụ 3 – Uptime 99,9%:**  
Phải hiểu con số này nghĩa là gì: tương đương ≤ 8,7 giờ downtime/năm. Từ đó mới xác định được cần Failover tự động < 30 giây, RTO ≤ 4 giờ, RPO ≤ 1 giờ.

### Ý tưởng mới nhận ra

NFR không phải viết một lần rồi thôi – trong quá trình thiết kế các Use Case, phát hiện thêm nhiều ràng buộc mới. Ví dụ, đến khi thiết kế BNPL mới thấy cần bổ sung NFR về **BNPL loan status UX** (progress bar từng bước) và **dynamic price transparency** (banner giải thích khi giá thay đổi).

---

## 4. Tích hợp và Dịch chuyển Dữ liệu

### Điều thấy khó / phức tạp

**4.1. Tích hợp cổng thanh toán (VNPay/MoMo)**

Phần khó không phải là gọi API, mà là xử lý **IPN Webhook**:
- Sau khi khách thanh toán, cổng thanh toán gọi ngược lại về hệ thống qua IPN
- Phải **xác minh chữ ký HMAC-SHA256** trước khi cập nhật trạng thái – nếu không, kẻ xấu có thể giả mạo webhook để lấy tour mà không trả tiền
- Phải xử lý trường hợp IPN đến **nhiều lần** (idempotent): không được cộng tiền hay trừ slot 2 lần

**4.2. Tích hợp BNPL + eKYC**

Phát hiện một ràng buộc pháp lý khó: khi khách hủy đơn BNPL, **không được hoàn tiền mặt trực tiếp cho khách** mà phải hoàn qua cancellation API của provider – vì khoản tiền đó thực chất là khoản vay (CBR-21 – quy định NHNN). Điều này làm thay đổi toàn bộ luồng hoàn tiền so với thanh toán thông thường.

**4.3. Data Migration từ Excel sang hệ thống**

Bài toán tưởng đơn giản nhưng thực tế có nhiều edge case:
- Dữ liệu Excel của mỗi doanh nghiệp có format khác nhau → phải cung cấp template chuẩn
- Không thể import tất cả rồi báo lỗi sau → phải **validate từng dòng trước khi ghi vào DB**
- Import lại cùng file không được tạo bản ghi trùng → cần **idempotent import**

### Ý tưởng mới nhận ra

Khi thiết kế tích hợp Dynamic Pricing, nhận ra phải giải quyết **3 yêu cầu mâu thuẫn cùng lúc**: giá thay đổi liên tục theo thị trường (< 500ms) + minh bạch với khách (khóa giá 15 phút khi checkout) + không vô lý (biên độ 50%–150% theo Luật Giá 2023). Giải pháp là dùng Redis Cache với Active Eviction thay vì TTL thụ động.

---

## Tổng kết

| Nhiệm vụ | Điều thấy khó nhất | Bài học rút ra |
|---|---|---|
| Lý do chọn đề tài | Chứng minh khoảng trống thị trường có thực | Cần so sánh cụ thể với giải pháp hiện có |
| Tìm văn bản pháp lý | Dịch điều khoản luật thành Business Rules | Đọc luật từ góc độ logic lập trình |
| Yêu cầu phi chức năng | Lượng hóa từng yêu cầu bằng con số cụ thể | NFR phải cập nhật liên tục khi thiết kế UC |
| Tích hợp thanh toán | Xử lý IPN Webhook idempotent + xác minh chữ ký | Bảo mật luồng callback quan trọng hơn luồng chính |
| Tích hợp BNPL | Ràng buộc hoàn tiền qua provider (CBR-21) | Pháp lý ảnh hưởng trực tiếp đến thiết kế kỹ thuật |
| Data Migration | Validate trước import, idempotent | Không bao giờ để dữ liệu bẩn vào database |
