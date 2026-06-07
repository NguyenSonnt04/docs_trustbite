# Tiêu chí nghiệm thu - MVP TrustBite

## Nghiệm thu phát hành MVP

| Nhóm | Tiêu chí |
|---|---|
| Mobile app | Người dùng hoàn tất luồng P0 trên iOS/Android theo device matrix tối thiểu. |
| Xác thực | Người dùng có thể yêu cầu và xác thực OTP; OTP sai, hết hạn hoặc vượt giới hạn đều được xử lý đúng. |
| Quán | Khách/người dùng có thể tìm kiếm và xem quán ACTIVE. |
| Đánh giá | Người dùng đã đăng nhập có thể gửi đánh giá hợp lệ theo 4 tiêu chí; dữ liệu không hợp lệ bị từ chối. |
| Hóa đơn | Người dùng có thể tải hóa đơn hợp lệ; file không hợp lệ bị từ chối; hash trùng bị phát hiện. |
| Xác minh | Điểm rủi ro ánh xạ đúng sang VERIFIED, PENDING_ADMIN_REVIEW, REFERENCE_ONLY hoặc REJECTED. |
| GPS | GPS là tùy chọn; thiếu GPS không làm hệ thống tự động từ chối. |
| Quản trị viên | Quản trị viên có thể duyệt/từ chối hóa đơn, báo cáo hoặc claim đang chờ xử lý kèm lý do. |
| Audit | Quyết định của quản trị viên tạo audit log. |
| Kiểm duyệt | Người dùng/chủ quán có thể báo cáo đánh giá; quản trị viên có thể xử lý báo cáo. |
| Quyền riêng tư | Ảnh hóa đơn gốc là riêng tư; dữ liệu nhạy cảm trên hóa đơn không hiển thị công khai theo mặc định. |
| QA | 100% test case P0 và mobile P0 đạt; không còn lỗi blocker hoặc critical. |

## Ví dụ tiêu chí nghiệm thu tính năng

### REV-001

- Bối cảnh người dùng đã đăng nhập và quán ACTIVE, khi điểm đánh giá từ 1-5 và bình luận tối thiểu 50 ký tự, thì đánh giá được tạo ở trạng thái SUBMITTED.
- Bối cảnh bình luận dưới 50 ký tự, khi gửi, thì hệ thống trả lỗi validate.

### OCR-001

- Bối cảnh file hóa đơn là JPG/PNG/HEIC và tối đa 10MB, khi tải lên, thì bản ghi xác minh hóa đơn được tạo.
- Bối cảnh hash file đã tồn tại, khi tải lên, thì hóa đơn bị từ chối và cờ gian lận được tạo.
- Bối cảnh điểm rủi ro 0-30, khi OCR hoàn tất, thì đánh giá chuyển VERIFIED.

### ADM-001

- Bối cảnh case đang chờ xử lý, khi quản trị viên gửi quyết định kèm lý do, thì trạng thái đối tượng thay đổi và audit log được tạo.
- Bối cảnh quản trị viên không nhập lý do, khi gửi quyết định, thì API trả lỗi validate.
