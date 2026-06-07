# Ma trận vai trò và quyền - TrustBite

| Chức năng | Khách chưa đăng nhập | Người dùng | Chủ quán | FOODGOD | Quản trị viên | Siêu quản trị |
|---|---|---|---|---|---|---|
| Xem danh sách quán | Có | Có | Có | Có | Có | Có |
| Xem đánh giá công khai | Có | Có | Có | Có | Có | Có |
| Viết đánh giá | Không | Có | Không cho quán của mình | Có | Không | Không |
| Tải hóa đơn | Không | Có | Không | Có | Không | Không |
| Bình chọn hữu ích | Không | Có | Không | Có | Có | Có |
| Lưu quán yêu thích | Không | Có | Có | Có | Có | Có |
| Báo cáo đánh giá | Không | Có | Có | Có | Có | Có |
| Gửi yêu cầu claim quán | Không | Không | Có | Không | Có | Có |
| Cập nhật thông tin quán | Không | Không | Chỉ quán có claim đã duyệt | Không | Có | Có |
| Phản hồi đánh giá | Không | Không | Chỉ quán của mình | Không | Có | Có |
| Duyệt quán | Không | Không | Không | Không | Có | Có |
| Duyệt hóa đơn/đánh giá nghi vấn | Không | Không | Không | Không | Có | Có |
| Xử lý báo cáo kiểm duyệt | Không | Không | Không | Không | Có | Có |
| Khóa tài khoản người dùng | Không | Không | Không | Không | Có | Có |
| Quản lý quản trị viên | Không | Không | Không | Không | Không | Có |
| Xem audit log | Không | Không | Không | Không | Có giới hạn | Có |
| Override quyết định | Không | Không | Không | Không | Không | Có |

## Ghi chú

- Chủ quán không được đánh giá quán do mình sở hữu hoặc đang có claim đã duyệt.
- Quản trị viên không tham gia đánh giá công khai để tránh xung đột lợi ích.
- FOODGOD thuộc game hóa nâng cao/tương lai; MVP chỉ cần hỗ trợ mã cấp hạng nếu có.
