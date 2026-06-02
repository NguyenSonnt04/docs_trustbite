# 💾 BACKUP & DISASTER RECOVERY SPECIFICATION
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | Backup & Disaster Recovery Specification |
| **Phiên bản** | v1.0.0 (Enterprise Standard) |
| **Tác giả** | Lead DevOps / Lead Site Reliability Engineer (SRE) |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. MỤC TIÊU PHỤC HỒI HỆ THỐNG (RTO & RPO TARGETS)

Để đảm bảo hệ thống **TrustBite** có khả năng chống chịu thảm họa và duy trì hoạt động ổn định cấp độ doanh nghiệp, chúng tôi cam kết đạt được các chỉ số phục hồi sau:

*   **RTO (Recovery Time Objective - Thời gian phục hồi tối đa):** **< 2 giờ**. Hệ thống phải hoạt động bình thường trở lại trong vòng 2 tiếng kể từ khi xảy ra sự cố sập toàn diện hoặc thảm họa cháy nổ trung tâm dữ liệu.
*   **RPO (Recovery Point Objective - Mất mát dữ liệu tối đa):** **< 15 phút**. Lượng dữ liệu đánh giá, điểm số hoặc đăng ký mới tối đa có thể bị mất trong trường hợp khẩn cấp không được vượt quá 15 phút hoạt động gần nhất.

---

## 2. CHIẾN LƯỢC SAO LƯU TRÊN HẠ TẦNG AWS (BACKUP STRATEGY)

Hệ thống tận dụng tối đa các dịch vụ sao lưu tự động của AWS để đảm bảo tính an toàn dữ liệu:

### 2.1. Sao lưu Cơ sở dữ liệu chính (AWS RDS PostgreSQL Backup)
*   **Sao lưu tự động hàng ngày (Automated Backups):** AWS RDS được thiết lập tự động chụp ảnh đĩa (Snapshot) vào lúc **02:00 sáng hàng ngày** (khung giờ có lượt truy cập thấp nhất). Thời gian lưu trữ bản sao lưu là **30 ngày**.
*   **Point-in-Time Recovery (PITR):** Bật tính năng ghi nhật ký giao dịch liên tục (Transaction Logs). Cho phép DBA khôi phục cơ sở dữ liệu về bất kỳ thời điểm chính xác nào trong quá khứ (chính xác đến từng giây) trong vòng 30 ngày gần nhất.
*   **Sao lưu đa vùng (Cross-Region Backup Replication):** Tự động đồng bộ các bản Snapshot của database sang một Region địa lý khác của AWS (Ví dụ: Từ Singapore sang Tokyo) để đề phòng trường hợp toàn bộ trung tâm dữ liệu tại Singapore gặp thảm họa tự nhiên.

### 2.2. Sao lưu ảnh hóa đơn và mã nguồn (S3 & Git Backup)
*   **S3 Bucket Versioning:** Bật tính năng lưu phiên bản trên **Amazon S3 Receipts Bucket**. Bất kỳ hành động sửa đổi hay xóa ảnh hóa đơn vô tình nào từ phía con người (kể cả do lỗi code backend) đều có thể dễ dàng khôi phục lại phiên bản cũ trước đó.
*   **S3 Cross-Region Replication:** Ảnh hóa đơn đã được làm mờ sẽ tự động nhân bản sang một S3 bucket dự phòng ở Region khác dưới dạng không đồng bộ.

---

## 3. KỊCH BẢN ỨNG PHÓ SỰ CỐ (DISASTER SCENARIOS & ACTIONS)

### 3.1. Sự cố 1: Lỗi hỏng ổ cứng hoặc sập vùng vật lý của AWS RDS (Single-AZ Failure)
*   **Hành động tự động:** Do cơ sở dữ liệu RDS được cấu hình **Multi-AZ**, AWS sẽ tự động phát hiện sự cố phần cứng tại Zone A, lập tức chuyển đổi kết nối DNS sang máy chủ dự phòng đang chạy đồng bộ tại Zone B.
*   **Thời gian gián đoạn:** **< 60 giây**. Không mất mát dữ liệu ($RPO = 0$).

### 3.2. Sự cố 2: Toàn bộ Cơ sở dữ liệu bị hỏng nặng do lỗi lập trình (Data Corruption)
*   **Quy trình xử lý của SRE:**
    1.  Tạm dừng API Gateway, đưa website về chế độ "Bảo trì khẩn cấp".
    2.  Vào AWS RDS Console, xác định thời điểm chính xác xảy ra lỗi ghi đè dữ liệu hỏng (Ví dụ: `2026-06-01 22:30:15`).
    3.  Thực hiện lệnh **Restore to Point-in-Time** về mốc thời gian an toàn trước đó 5 giây (`2026-06-01 22:30:10`).
    4.  Kiểm tra tính nhất quán dữ liệu trên máy chủ RDS mới được dựng lên.
    5.  Cập nhật DNS trỏ API về máy chủ RDS mới và mở lại hệ thống.
*   **Thời gian gián đoạn:** **< 30 phút** ($RTO = 30m$, $RPO = 5s$).
