# Backlog câu chuyện người dùng - TrustBite

| ID | Mức ưu tiên | Câu chuyện người dùng | Tóm tắt nghiệm thu |
|---|---|---|---|
| AUTH-US-001 | P0 | Là khách chưa đăng nhập, tôi muốn đăng nhập bằng OTP để có thể viết đánh giá. | OTP đúng thì đăng nhập thành công; OTP hết hạn hoặc vượt giới hạn thì bị từ chối. |
| REST-US-001 | P0 | Là khách chưa đăng nhập, tôi muốn tìm kiếm quán để chọn nơi ăn uống. | Chỉ quán trạng thái ACTIVE được trả về. |
| REST-US-002 | P0 | Là người dùng, tôi muốn xem chi tiết quán để so sánh đánh giá đã xác minh và đánh giá tham khảo. | Trang chi tiết hiển thị điểm tin cậy và danh sách đánh giá. |
| REV-US-001 | P0 | Là người dùng, tôi muốn gửi đánh giá theo 4 tiêu chí để chia sẻ trải nghiệm. | Đánh giá hợp lệ được tạo ở trạng thái SUBMITTED. |
| OCR-US-001 | P0 | Là người dùng, tôi muốn tải hóa đơn để đánh giá của tôi có thể được xác minh. | Hóa đơn hợp lệ được xác minh hoặc chuyển trạng thái chờ duyệt/tham khảo theo điểm rủi ro. |
| GPS-US-001 | P0 | Là người dùng, tôi muốn GPS là tùy chọn để bảo vệ quyền riêng tư. | Thiếu GPS không làm hệ thống tự động từ chối đánh giá. |
| ADM-US-001 | P0 | Là quản trị viên, tôi muốn xử lý hóa đơn chờ duyệt để các trường hợp nghi vấn được quyết định rõ ràng. | Quyết định cập nhật trạng thái và ghi audit log. |
| MOD-US-001 | P0 | Là người dùng, tôi muốn báo cáo đánh giá không phù hợp để nội dung được kiểm duyệt. | Báo cáo đi vào hàng đợi quản trị. |
| MERCH-US-001 | P1 | Là chủ quán, tôi muốn xác nhận quyền quản lý quán để quản lý hồ sơ quán. | Yêu cầu claim chuyển sang trạng thái chờ duyệt rồi được duyệt/từ chối bởi quản trị viên. |
| NOTIF-US-001 | P1 | Là người dùng, tôi muốn nhận thông báo để biết kết quả xử lý của quản trị viên. | Thông báo được tạo sau quyết định của quản trị viên. |
| GAME-US-001 | P0 | Là người đánh giá, tôi muốn nhận EXP cho đánh giá đã xác minh để được ghi nhận đóng góp. | EXP được cập nhật sau khi đánh giá được xác minh. |
