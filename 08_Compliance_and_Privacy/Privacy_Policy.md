# Chính sách quyền riêng tư và bảo vệ dữ liệu người dùng - TrustBite

| Thông tin tài liệu | Chi tiết |
|---|---|
| Loại tài liệu | Chính sách quyền riêng tư |
| Phiên bản | v2.1.0 |
| Trạng thái | Đang rà soát |
| Chủ sở hữu | Pháp lý / Bảo mật |
| Ngày cập nhật | 2026-06-07 |

---

## 1. Phạm vi

TrustBite xử lý dữ liệu cá nhân để xác thực đánh giá và chống gian lận. Dữ liệu nhạy cảm nhất gồm số điện thoại, ảnh hóa đơn, GPS tùy chọn, log bảo mật và audit log.

---

## 2. Dữ liệu được thu thập

| Dữ liệu | Mục đích | Quy tắc MVP |
|---|---|---|
| Số điện thoại | Đăng nhập OTP, định danh tài khoản | Bắt buộc với người dùng đã đăng ký. |
| Ảnh hóa đơn | Xác minh đánh giá | Lưu riêng tư theo mặc định. |
| Văn bản OCR | Đối chiếu tên quán/thời gian | Dùng cho xác minh. |
| Tọa độ GPS | Tín hiệu xác minh tùy chọn | Cần sự đồng ý của người dùng; không theo dõi liên tục. |
| Hash IP/user-agent | Giới hạn tần suất và bảo mật | Chỉ lưu ngắn hạn. |
| Audit log | Trách nhiệm giải trình vận hành | Chỉ người có quyền mới được truy cập. |

---

## 3. Quyền riêng tư về số điện thoại / xác thực

- MVP dùng đăng nhập OTP, không yêu cầu mật khẩu.
- OTP phải được hash trước khi lưu.
- Không ghi OTP, token hoặc đầy đủ số điện thoại vào log.
- Số điện thoại nên được che trong giao diện quản trị khi không cần xem đầy đủ.

---

## 4. Quyền riêng tư về hóa đơn

- Ảnh hóa đơn gốc được lưu riêng tư, không hiển thị công khai trực tiếp.
- Nếu cần hiển thị bằng chứng công khai, chỉ hiển thị bản đã che dữ liệu nhạy cảm.
- Dữ liệu cần che gồm: tên khách, số tài khoản, số thẻ, mã giao dịch nhạy cảm, số điện thoại nếu có.
- Thời gian lưu giữ tuân thủ `Data_Retention_Policy.md`.

---

## 5. Quyền riêng tư về GPS và quyền mobile

- GPS là tùy chọn.
- TrustBite không theo dõi vị trí liên tục.
- GPS chỉ được lấy tại thời điểm người dùng chủ động gửi đánh giá/hóa đơn và cấp quyền.
- GPS dùng để tính điểm rủi ro, không phải điều kiện từ chối tuyệt đối.
- Dữ liệu GPS gốc phải có thời gian lưu ngắn và bị giới hạn quyền truy cập.
- Quyền camera/photo library chỉ được hỏi khi người dùng chủ động chọn/chụp hóa đơn.
- Quyền notification nếu triển khai phải được hỏi sau khi người dùng hiểu giá trị nhận thông báo, không hỏi ngay khi mở app nếu không cần.

---

## 6. Chia sẻ dữ liệu

TrustBite không bán hoặc chia sẻ dữ liệu hóa đơn, số điện thoại, GPS cho chủ quán hoặc bên quảng cáo. Chủ quán chỉ được xem thông tin cần thiết để phản hồi/quản lý quán, không được xem ảnh hóa đơn gốc của người dùng.

---

## 7. Quyền của người dùng

Người dùng có quyền:

- yêu cầu xem dữ liệu cá nhân,
- yêu cầu xóa tài khoản,
- yêu cầu xóa hoặc ẩn đánh giá,
- từ chối GPS,
- khiếu nại quyết định kiểm duyệt/xác minh nếu có quy trình hỗ trợ.

Một số bản ghi audit hoặc bảo mật có thể được giữ lại theo chính sách lưu giữ dữ liệu để chống gian lận hoặc đáp ứng nghĩa vụ pháp lý.
