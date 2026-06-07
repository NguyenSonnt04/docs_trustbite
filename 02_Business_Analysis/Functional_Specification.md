# Đặc tả chức năng - TrustBite

| Thông tin tài liệu | Chi tiết |
|---|---|
| Phiên bản | v1.1.0 |
| Trạng thái | Bản nháp |
| Chủ sở hữu | Chuyên viên phân tích nghiệp vụ |

## 1. Tổng quan chức năng

Đặc tả chức năng là tài liệu trung tâm chuyển PRD thành yêu cầu nghiệp vụ triển khai. Mọi API, DB, UX và QA phải truy vết được về các module chức năng dưới đây.

## 2. Tác nhân

- Khách chưa đăng nhập
- Người dùng đã đăng ký
- Người đánh giá đã xác minh
- Chủ quán
- Quản trị viên
- Siêu quản trị
- FOODGOD / tương lai
- Hệ thống

## 3. Module chức năng

| Module | Mã | Thuộc MVP |
|---|---|---|
| Xác thực | AUTH | Có |
| Khám phá quán | REST | Có |
| Gửi đánh giá | REV | Có |
| Xác minh hóa đơn | OCR | Có |
| Xác minh GPS | GPS | Có, nhưng là tùy chọn |
| Điểm tin cậy | TRUST | Có |
| Cổng chủ quán | MERCH | Một phần/P1 |
| Cổng quản trị | ADM | Có |
| Kiểm duyệt | MOD | Có |
| Thông báo | NOTIF | P1 |
| Game hóa | GAME | Cơ bản |

---

## AUTH-001: Đăng ký/đăng nhập bằng OTP

### Mục tiêu
Cho phép người dùng xác thực bằng số điện thoại.

### Tác nhân
Khách chưa đăng nhập, người dùng đã đăng ký.

### Điều kiện tiên quyết
- Số điện thoại hợp lệ.
- Người dùng không bị khóa bởi lạm dụng hoặc vượt giới hạn tần suất.

### Dữ liệu vào
- Số điện thoại.
- Mã OTP.

### Luồng chính
1. Người dùng nhập số điện thoại.
2. Hệ thống kiểm tra định dạng.
3. Hệ thống tạo OTP và gửi SMS.
4. Người dùng nhập OTP.
5. Hệ thống xác thực OTP.
6. Hệ thống tạo tài khoản nếu chưa tồn tại hoặc đăng nhập nếu tài khoản đã tồn tại.

### Quy tắc nghiệp vụ
- OTP gồm 6 chữ số.
- OTP hết hạn sau 120 giây.
- Tối đa 3 lần gửi trong 10 phút cho cùng số điện thoại.
- Tối đa 5 lần nhập sai trước khi khóa tạm thời.

### Luồng thay thế
- OTP sai → tăng số lần nhập sai.
- OTP hết hạn → yêu cầu gửi OTP mới.
- Vượt giới hạn tần suất → từ chối tạm thời.

### Tiêu chí nghiệm thu
- Bối cảnh OTP đúng còn hạn, khi người dùng xác thực, thì đăng nhập thành công.
- Bối cảnh OTP hết hạn, khi người dùng xác thực, thì hệ thống trả lỗi hết hạn.

---

## REST-001: Tìm kiếm quán

### Mục tiêu
Cho phép khách/người dùng tìm quán theo tên, vị trí và bộ lọc.

### Tác nhân
Khách chưa đăng nhập, người dùng đã đăng ký.

### Điều kiện tiên quyết
- Quán tồn tại và ở trạng thái ACTIVE để hiển thị công khai.

### Dữ liệu vào
- Từ khóa.
- Vị trí hoặc khung bản đồ.
- Bộ lọc và sắp xếp.

### Luồng chính
1. Người dùng mở trang tìm kiếm hoặc bản đồ.
2. Người dùng nhập từ khóa hoặc di chuyển bản đồ.
3. Hệ thống truy vấn các quán ACTIVE.
4. Hệ thống trả danh sách gồm điểm tin cậy và số lượng đánh giá.

### Quy tắc nghiệp vụ
- Không hiển thị công khai quán DRAFT/PENDING/SUSPENDED/CLOSED.
- Sắp xếp mặc định theo mức liên quan, khoảng cách và điểm tin cậy.

### Tiêu chí nghiệm thu
- Bối cảnh có quán ACTIVE khớp từ khóa, khi tìm kiếm, thì kết quả hiển thị đúng.

---

## REV-001: Gửi đánh giá quán

### Mục tiêu
Cho phép người dùng gửi đánh giá 4 tiêu chí cho một quán.

### Tác nhân
Người dùng đã đăng ký.

### Điều kiện tiên quyết
- Người dùng đã đăng nhập.
- Quán ở trạng thái ACTIVE.
- Người dùng không bị khóa quyền đánh giá.
- Chủ quán không được đánh giá quán của mình.

### Dữ liệu vào
- ID quán.
- Điểm món ăn.
- Điểm giá.
- Điểm phục vụ.
- Điểm không gian.
- Bình luận.
- Thời điểm ghé quán.
- Media tùy chọn.

### Luồng chính
1. Người dùng mở trang chi tiết quán.
2. Người dùng chọn viết đánh giá.
3. Người dùng nhập điểm và bình luận.
4. Hệ thống kiểm tra dữ liệu.
5. Hệ thống tạo đánh giá ở trạng thái SUBMITTED.
6. Hệ thống đề xuất tải hóa đơn để xác minh.

### Quy tắc nghiệp vụ
- Bình luận tối thiểu 50 ký tự.
- Điểm mỗi tiêu chí từ 1 đến 5.
- Đánh giá không có hóa đơn chỉ là đánh giá tham khảo nếu được hiển thị.

### Luồng thay thế
- Thiếu điểm → lỗi validate.
- Quán không ACTIVE → không cho gửi đánh giá.
- Người dùng bị hạn chế → từ chối.

### Tiêu chí nghiệm thu
- Bối cảnh người dùng gửi đánh giá hợp lệ, khi lưu thành công, thì đánh giá ở trạng thái SUBMITTED.

---

## OCR-001: Tải và xác minh hóa đơn

### Mục tiêu
Xác minh người dùng đã thật sự ăn tại quán.

### Tác nhân
Người dùng đã đăng ký, hệ thống, quản trị viên.

### Điều kiện tiên quyết
- Đánh giá tồn tại.
- Đánh giá thuộc người dùng đang tải hóa đơn.

### Dữ liệu vào
- Ảnh hóa đơn.
- ID đánh giá.
- ID quán.
- GPS tùy chọn.

### Luồng chính
1. Người dùng tải hóa đơn.
2. Hệ thống kiểm tra định dạng và dung lượng file.
3. Hệ thống lưu ảnh gốc ở vùng riêng tư.
4. Hệ thống tính SHA-256 hash.
5. Hệ thống kiểm tra trùng hóa đơn.
6. Hệ thống OCR ảnh.
7. Hệ thống tính độ tương đồng, rủi ro thời gian và rủi ro GPS nếu có.
8. Hệ thống tính điểm rủi ro gian lận.
9. Hệ thống quyết định tự động xác minh, chuyển duyệt thủ công, đánh dấu tham khảo hoặc từ chối.

### Quy tắc nghiệp vụ
- Chỉ nhận JPG/PNG/HEIC.
- Dung lượng tối đa 10MB.
- Hash trùng bị từ chối.
- OCR/GPS dùng cơ chế chấm điểm rủi ro.

### Luồng thay thế
- OCR timeout → retry; nếu vẫn lỗi thì chuyển duyệt thủ công hoặc đánh dấu tham khảo tùy rule.
- Hash trùng → từ chối và tạo cờ gian lận.
- Điểm rủi ro 31-60 → chuyển quản trị viên xử lý.

### Tiêu chí nghiệm thu
- Bối cảnh hóa đơn hợp lệ và rủi ro thấp, khi OCR hoàn tất, thì đánh giá chuyển VERIFIED.
- Bối cảnh hash trùng, khi tải hóa đơn, thì hóa đơn chuyển REJECTED.

---

## ADM-001: Quản trị viên xử lý thủ công

### Mục tiêu
Cho phép quản trị viên xử lý các trường hợp nghi vấn.

### Tác nhân
Quản trị viên, siêu quản trị.

### Điều kiện tiên quyết
- Trường hợp đang ở trạng thái cần xử lý.
- Quản trị viên có quyền phù hợp.

### Dữ liệu vào
- ID trường hợp.
- Quyết định: duyệt, từ chối, đánh dấu tham khảo hoặc ẩn.
- Lý do.

### Luồng chính
1. Quản trị viên mở hàng đợi.
2. Quản trị viên lọc case theo loại, trạng thái và điểm rủi ro.
3. Quản trị viên xem bằng chứng.
4. Quản trị viên chọn quyết định và nhập lý do.
5. Hệ thống cập nhật trạng thái đối tượng.
6. Hệ thống ghi audit log.
7. Hệ thống gửi thông báo nếu cần.

### Quy tắc nghiệp vụ
- Quyết định bắt buộc có lý do.
- Không được xử lý lại case đã đóng nếu không có quyền override.

### Tiêu chí nghiệm thu
- Bối cảnh quản trị viên duyệt hóa đơn đang chờ xử lý, khi gửi quyết định, thì trạng thái được cập nhật và audit log được tạo.

---

## MOD-001: Báo cáo đánh giá

### Mục tiêu
Cho phép người dùng/chủ quán báo cáo đánh giá vi phạm.

### Tác nhân
Người dùng đã đăng ký, chủ quán, quản trị viên.

### Điều kiện tiên quyết
- Đánh giá tồn tại và chưa bị xóa.

### Dữ liệu vào
- ID đánh giá.
- Mã lý do.
- Mô tả.
- Bằng chứng tùy chọn.

### Luồng chính
1. Tác nhân chọn báo cáo đánh giá.
2. Tác nhân chọn lý do.
3. Hệ thống tạo báo cáo kiểm duyệt.
4. Quản trị viên xử lý báo cáo trong hàng đợi.

### Quy tắc nghiệp vụ
- Không tạo báo cáo mở trùng cùng người dùng/đánh giá/lý do.
- Báo cáo nghiêm trọng có thể tạm ẩn đánh giá.

### Tiêu chí nghiệm thu
- Bối cảnh báo cáo hợp lệ, khi gửi, thì báo cáo chuyển SUBMITTED.
