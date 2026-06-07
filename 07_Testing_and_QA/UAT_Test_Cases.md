# Kịch bản kiểm thử UAT - MVP TrustBite

## UAT-001: Người dùng tạo đánh giá đã xác minh

1. Đăng nhập bằng OTP.
2. Tìm quán đang ở trạng thái ACTIVE.
3. Gửi đánh giá với 4 tiêu chí và bình luận hợp lệ.
4. Tải hóa đơn hợp lệ.
5. Cho phép GPS trong phạm vi 200m nếu muốn tăng tín hiệu xác minh.

Kết quả mong đợi:

- Đánh giá chuyển VERIFIED.
- Trạng thái xác minh hóa đơn là VERIFIED.
- Người dùng nhận EXP.

## UAT-002: Hóa đơn trùng bị chặn

1. Người dùng A tải hóa đơn X thành công.
2. Người dùng B tải lại cùng hóa đơn X.

Kết quả mong đợi:

- Hóa đơn của người dùng B bị từ chối.
- Cờ gian lận được tạo.
- Đánh giá không được xác minh.

## UAT-003: OCR không chắc chắn chuyển quản trị viên xử lý

1. Người dùng tải hóa đơn có độ tương đồng OCR 60-79%.
2. Hệ thống tính mức rủi ro trung bình.

Kết quả mong đợi:

- Trường hợp xuất hiện trong hàng đợi quản trị.
- Quản trị viên có thể duyệt, từ chối hoặc đánh dấu tham khảo kèm lý do.
- Audit log được tạo.

## UAT-004: Từ chối GPS nhưng vẫn được xử lý

1. Người dùng gửi đánh giá hợp lệ.
2. Người dùng tải hóa đơn hợp lệ nhưng từ chối GPS.

Kết quả mong đợi:

- Hệ thống xử lý OCR và hash.
- Đánh giá không bị tự động từ chối chỉ vì thiếu GPS.

## UAT-005: Báo cáo đánh giá và quản trị viên xử lý

1. Người dùng báo cáo một đánh giá.
2. Quản trị viên mở hàng đợi kiểm duyệt.
3. Quản trị viên ẩn đánh giá kèm lý do.

Kết quả mong đợi:

- Báo cáo chuyển ACTION_TAKEN/CLOSED.
- Đánh giá chuyển HIDDEN.
- Audit log tồn tại.

## UAT-006: Chủ quán xác nhận quyền quản lý P1

1. Chủ quán gửi yêu cầu xác nhận quyền quản lý quán.
2. Quản trị viên duyệt yêu cầu.

Kết quả mong đợi:

- Claim chuyển APPROVED.
- Chủ quán có thể chỉnh hồ sơ quán đã được duyệt.
