# Chính sách lưu giữ dữ liệu - TrustBite

| Loại dữ liệu | Thời gian lưu giữ | Chính sách xử lý |
|---|---:|---|
| Bản ghi OTP | 30 ngày | Lưu để điều tra lạm dụng, sau đó xóa hoặc ẩn danh hóa. |
| Ảnh hóa đơn gốc | Mặc định 180 ngày | Lưu riêng tư; xóa hoặc ẩn danh hóa sau thời hạn lưu giữ, trừ khi có tranh chấp hoặc yêu cầu pháp lý. |
| Bản hóa đơn đã che dữ liệu | 365 ngày | Dùng làm bằng chứng xác minh nếu cần hiển thị công khai. |
| Tọa độ GPS | 30-90 ngày | Chỉ dùng cho xác minh/audit; sau đó tổng hợp hoặc ẩn danh hóa. |
| Nội dung đánh giá | Đến khi người dùng yêu cầu xóa hoặc bị kiểm duyệt xóa | Xóa mềm để phục vụ audit nếu pháp lý cho phép. |
| Cờ gian lận | 2 năm | Dùng cho phân tích bảo mật/chống lạm dụng; giới hạn quyền truy cập. |
| Audit log | 3 năm | Hồ sơ vận hành/pháp lý. |
| Hash thiết bị/IP | 30-90 ngày | Chỉ dùng cho giới hạn tần suất và bảo mật; không dùng để profiling dài hạn trong MVP. |
| Thông báo | 1 năm | Xóa thông báo đã đọc và quá hạn. |

## Nguyên tắc

- Chỉ thu thập lượng dữ liệu tối thiểu cần cho xác minh.
- GPS là tùy chọn.
- Ảnh hóa đơn được lưu riêng tư theo mặc định.
- Bằng chứng hiển thị công khai phải được che dữ liệu nhạy cảm.
- Yêu cầu xóa tài khoản phải xóa hoặc ẩn danh hóa dữ liệu cá nhân, trừ khi cần giữ lại vì nghĩa vụ pháp lý/audit.
- Mọi hình thức fingerprint thiết bị phải được rà soát quyền riêng tư trước khi triển khai.
