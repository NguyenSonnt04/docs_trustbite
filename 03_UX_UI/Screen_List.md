# Danh sách màn hình - TrustBite

Tài liệu này liệt kê màn hình theo hướng mobile-first. Màu sắc, layout chi tiết và visual style chưa chốt ở giai đoạn này.

| ID | Màn hình | Tác nhân | MVP | Ghi chú |
|---|---|---|---|---|
| MOB-001 | Trang chủ / bản đồ quán mobile | Khách chưa đăng nhập, người dùng | Có | Hiển thị danh sách và bản đồ, bottom sheet/mobile interaction. |
| MOB-002 | Kết quả tìm kiếm | Khách chưa đăng nhập, người dùng | Có | Tìm theo từ khóa, bộ lọc và sắp xếp. |
| MOB-003 | Chi tiết quán | Khách chưa đăng nhập, người dùng | Có | Hiển thị điểm tin cậy, đánh giá đã xác minh và đánh giá tham khảo. |
| MOB-004 | Đăng nhập OTP | Khách chưa đăng nhập | Có | Yêu cầu OTP và xác thực OTP. |
| MOB-005 | Gửi đánh giá | Người dùng | Có | Nhập 4 tiêu chí chấm điểm, bình luận và media tùy chọn. |
| MOB-006 | Tải hóa đơn | Người dùng | Có | Chọn/chụp file, thông báo quyền riêng tư và GPS tùy chọn. |
| MOB-007 | Kết quả xác minh | Người dùng | Có | Giải thích trạng thái đã xác minh, chờ duyệt, tham khảo hoặc bị từ chối. |
| MOB-008 | Hồ sơ người dùng | Người dùng | Có | Hiển thị EXP, cấp hạng và lịch sử đánh giá. |
| MOB-009 | Báo cáo đánh giá | Người dùng, chủ quán | Có | Chọn lý do và mô tả báo cáo. |
| ADM-001 | Bảng điều khiển quản trị web | Quản trị viên | Có | Tóm tắt các hàng đợi cần xử lý. |
| ADM-002 | Hàng đợi hóa đơn của quản trị viên | Quản trị viên | Có | Hiển thị bằng chứng, điểm rủi ro và quyết định xử lý. |
| ADM-003 | Hàng đợi kiểm duyệt | Quản trị viên | Có | Xử lý báo cáo và hành động kiểm duyệt. |
| ADM-004 | Xác minh quán | Quản trị viên | Có | Duyệt/từ chối quán hoặc yêu cầu claim. |
| MER-001 | Yêu cầu claim quán | Chủ quán | P1 | Form gửi yêu cầu xác nhận quyền quản lý. |
| MER-002 | Cổng chủ quán | Chủ quán | P1 | Quản lý hồ sơ quán, phản hồi đánh giá và báo cáo. |
| MOB-010 | Danh sách thông báo | Người dùng, chủ quán, quản trị viên | P1 | Danh sách thông báo trong ứng dụng. |

## Nguyên tắc UX cho MVP

- Trạng thái đã xác minh và tham khảo phải dễ hiểu trong 3 giây.
- Màn hình tải hóa đơn phải có thông báo quyền riêng tư rõ ràng: ảnh gốc lưu riêng tư, thông tin nhạy cảm được che nếu hiển thị.
- GPS là tùy chọn; nội dung giao diện không được tạo cảm giác ép buộc người dùng.
- Hàng đợi quản trị phải ưu tiên bằng chứng, điểm rủi ro và lý do để xử lý nhanh.
