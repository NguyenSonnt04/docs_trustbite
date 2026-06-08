# Đặc tả chức năng - TrustBite

| Thông tin tài liệu | Chi tiết |
|---|---|
| Phiên bản | v1.2.0 |
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

## GPS-001: Xác minh GPS tùy chọn

### Mục tiêu
Ghi nhận vị trí người dùng nếu được cho phép và dùng khoảng cách tới quán như tín hiệu hỗ trợ xác minh.

### Tác nhân
Người dùng đã đăng ký, hệ thống.

### Điều kiện tiên quyết
- Người dùng đã đăng nhập.
- Đánh giá hoặc hóa đơn đang trong luồng xác minh.
- Quán có tọa độ hợp lệ để tính khoảng cách.
- Người dùng chủ động cấp quyền GPS tại thời điểm gửi đánh giá hoặc tải hóa đơn.

### Dữ liệu vào
- ID đánh giá hoặc ID xác minh hóa đơn.
- Vĩ độ.
- Kinh độ.
- Độ chính xác GPS.
- Thời điểm ghi nhận vị trí.

### Luồng chính
1. Người dùng được hỏi quyền GPS theo ngữ cảnh khi gửi đánh giá hoặc tải hóa đơn.
2. Người dùng đồng ý chia sẻ vị trí.
3. Mobile app gửi tọa độ, độ chính xác và thời điểm ghi nhận.
4. Hệ thống kiểm tra tọa độ, độ chính xác và thời gian ghi nhận.
5. Hệ thống tính khoảng cách giữa vị trí người dùng và quán bằng Haversine.
6. Hệ thống ghi nhận `gpsDistanceMeters` và tín hiệu rủi ro GPS.
7. Hệ thống chuyển tín hiệu GPS vào điểm rủi ro tổng của xác minh hóa đơn.

### Quy tắc nghiệp vụ
- GPS là tùy chọn và không được dùng làm điều kiện bắt buộc để gửi đánh giá.
- Người dùng không cấp GPS không bị từ chối tự động.
- GPS trong 200m không tăng rủi ro.
- GPS 200-500m tăng rủi ro mức trung bình.
- GPS lớn hơn 500m tăng rủi ro mức cao.
- GPS accuracy lớn hơn 100m chỉ là tín hiệu rủi ro, không reject cứng.
- Mobile app không được tự quyết định kết quả xác minh dựa trên GPS.
- Dữ liệu GPS gốc chỉ dùng cho xác minh/audit và tuân thủ chính sách lưu giữ dữ liệu.

### Luồng thay thế
- Người dùng từ chối GPS → hệ thống tiếp tục OCR/hash và cộng rủi ro theo rule thiếu GPS.
- GPS permission bị hệ điều hành chặn → mobile hiển thị trạng thái không có GPS và cho phép tiếp tục.
- Tọa độ không hợp lệ → hệ thống bỏ qua tín hiệu GPS và ghi lý do.
- Quán thiếu tọa độ → hệ thống không tính khoảng cách và không reject tự động.
- Độ chính xác GPS thấp → hệ thống tăng rủi ro nhưng vẫn xử lý các tín hiệu khác.

### Tiêu chí nghiệm thu
- Bối cảnh người dùng cấp GPS trong 200m, khi tải hóa đơn hợp lệ, thì tín hiệu GPS không làm tăng điểm rủi ro.
- Bối cảnh người dùng từ chối GPS, khi tải hóa đơn hợp lệ, thì hệ thống vẫn xử lý OCR/hash và không tự động từ chối.
- Bối cảnh GPS xa hơn 500m, khi tính rủi ro, thì hệ thống tăng điểm rủi ro và có thể chuyển duyệt thủ công hoặc đánh dấu tham khảo theo tổng điểm.

---

## TRUST-001: Tính điểm tin cậy quán

### Mục tiêu
Tính và hiển thị điểm tin cậy của quán dựa trên đánh giá có trọng số, ưu tiên đánh giá đã xác minh.

### Tác nhân
Hệ thống, quản trị viên.

### Điều kiện tiên quyết
- Quán tồn tại.
- Có ít nhất một đánh giá hợp lệ hoặc hệ thống có rule hiển thị trạng thái chưa đủ dữ liệu.
- Trạng thái đánh giá đã được đồng bộ theo state machine.

### Dữ liệu vào
- ID quán.
- Điểm trung bình của từng đánh giá.
- Trạng thái đánh giá.
- Trạng thái xác minh.
- Cấp hạng người dùng tại thời điểm tính điểm.

### Luồng chính
1. Đánh giá mới được tạo, xác minh, ẩn, từ chối hoặc xóa mềm.
2. Hệ thống phát sinh job tính lại điểm cho quán liên quan.
3. Hệ thống lấy các đánh giá đủ điều kiện tính điểm.
4. Hệ thống gán trọng số theo trạng thái xác minh và cấp hạng người dùng.
5. Hệ thống loại trừ đánh giá HIDDEN, REJECTED và DELETED.
6. Hệ thống tính điểm tin cậy và phân rã số lượng đánh giá VERIFIED/REFERENCE_ONLY.
7. Hệ thống cập nhật điểm hiển thị trên danh sách và chi tiết quán.

### Quy tắc nghiệp vụ
- Đánh giá VERIFIED có trọng số cao hơn đánh giá REFERENCE_ONLY.
- Đánh giá REFERENCE_ONLY vẫn có thể được hiển thị nhưng chỉ tính trọng số thấp.
- Đánh giá HIDDEN, REJECTED và DELETED không được tính điểm tin cậy.
- Cấp hạng người dùng chỉ ảnh hưởng trọng số nếu đánh giá là VERIFIED.
- Cấp hạng không được dùng để bỏ qua chống gian lận.
- Nếu quán chưa có đủ dữ liệu, hệ thống hiển thị trạng thái chưa đủ đánh giá thay vì điểm gây hiểu nhầm.
- Công thức và trọng số chi tiết theo `05_Security_Algorithms/Anti_Fraud_Specification.md`.

### Luồng thay thế
- Job tính điểm lỗi → hệ thống retry và giữ điểm gần nhất kèm log vận hành.
- Đánh giá bị ẩn sau khi đã tính điểm → hệ thống phải tính lại và loại trừ đánh giá đó.
- Người dùng bị hạ cấp hoặc EXP bị thu hồi → điểm có thể được tính lại theo policy nếu cấp hạng là input trọng số hiện hành.
- Không có đánh giá hợp lệ → điểm tin cậy để trống hoặc hiển thị trạng thái chưa đủ dữ liệu.

### Tiêu chí nghiệm thu
- Bối cảnh đánh giá VERIFIED mới được tạo, khi job tính điểm chạy, thì điểm tin cậy của quán được cập nhật.
- Bối cảnh đánh giá HIDDEN, khi job tính điểm chạy lại, thì đánh giá đó không còn ảnh hưởng tới điểm tin cậy.
- Bối cảnh quán chỉ có đánh giá REFERENCE_ONLY, khi hiển thị, thì hệ thống phân biệt rõ điểm/tín hiệu tham khảo với đánh giá đã xác minh.

---

## MERCH-001: Chủ quán xác nhận quyền quản lý quán

### Mục tiêu
Cho phép chủ quán gửi yêu cầu xác nhận quyền quản lý quán để cập nhật thông tin và phản hồi đánh giá trong phạm vi được duyệt.

### Tác nhân
Chủ quán, quản trị viên, siêu quản trị.

### Điều kiện tiên quyết
- Chủ quán đã đăng nhập.
- Quán tồn tại trong hệ thống.
- Chủ quán cung cấp thông tin liên hệ và bằng chứng quyền sở hữu/quản lý.
- Không có claim đang mở trùng cho cùng chủ quán và quán.

### Dữ liệu vào
- ID quán.
- Tên pháp lý hoặc tên kinh doanh.
- Thông tin liên hệ.
- Bằng chứng quyền sở hữu/quản lý.
- Ghi chú bổ sung nếu có.

### Luồng chính
1. Chủ quán mở luồng claim quán.
2. Chủ quán chọn quán và nhập thông tin xác minh.
3. Hệ thống kiểm tra dữ liệu bắt buộc và claim trùng.
4. Hệ thống tạo claim ở trạng thái SUBMITTED hoặc PENDING_ADMIN_REVIEW.
5. Quản trị viên mở hàng đợi claim.
6. Quản trị viên xem bằng chứng và đưa ra quyết định kèm lý do.
7. Nếu được duyệt, hệ thống gán quyền quản lý quán cho chủ quán.
8. Hệ thống ghi audit log cho quyết định.

### Quy tắc nghiệp vụ
- Claim phải được quản trị viên duyệt trước khi chủ quán được chỉnh thông tin quán.
- Chủ quán chỉ chỉnh được quán có claim APPROVED.
- Chủ quán không được xóa đánh giá của người dùng.
- Chủ quán không được đánh giá quán do mình sở hữu hoặc đang có claim đã duyệt.
- Claim trùng hoặc tranh chấp ownership phải chuyển quản trị viên xử lý.
- Merchant portal chi tiết thuộc P1/V1.1; MVP có thể chỉ cần luồng claim tối thiểu nếu cần kiểm chứng giả thuyết chủ quán.

### Luồng thay thế
- Thiếu giấy tờ hoặc thông tin liên hệ → hệ thống trả lỗi validate.
- Claim trùng đang mở → hệ thống từ chối hoặc gộp vào case tranh chấp theo policy.
- Quản trị viên từ chối claim → claim chuyển REJECTED và ghi lý do.
- Chủ quán cố chỉnh quán chưa được duyệt → hệ thống trả lỗi thiếu quyền.

### Tiêu chí nghiệm thu
- Bối cảnh chủ quán gửi claim đủ thông tin, khi hệ thống lưu thành công, thì claim xuất hiện trong hàng đợi quản trị.
- Bối cảnh quản trị viên duyệt claim kèm lý do, khi quyết định được lưu, thì chủ quán có quyền quản lý quán đó và audit log được tạo.
- Bối cảnh chủ quán chưa có claim APPROVED, khi cập nhật thông tin quán, thì hệ thống từ chối do thiếu quyền.

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

---

## NOTIF-001: Thông báo cơ bản

### Mục tiêu
Thông báo cho người dùng, chủ quán hoặc quản trị viên khi có kết quả xử lý quan trọng liên quan đến xác minh, kiểm duyệt hoặc claim.

### Tác nhân
Người dùng đã đăng ký, chủ quán, quản trị viên, hệ thống.

### Điều kiện tiên quyết
- Người nhận tồn tại và có quyền xem sự kiện liên quan.
- Sự kiện nghiệp vụ đã được lưu thành công.
- Payload thông báo không chứa dữ liệu nhạy cảm vượt quá quyền của người nhận.

### Dữ liệu vào
- Loại sự kiện.
- ID người nhận.
- ID entity liên quan.
- Tiêu đề và nội dung ngắn.
- Payload điều hướng trong app/web nếu có.

### Luồng chính
1. Hệ thống nhận sự kiện nghiệp vụ sau khi admin hoặc worker xử lý xong.
2. Hệ thống xác định người nhận hợp lệ.
3. Hệ thống tạo thông báo trong ứng dụng.
4. Người nhận mở danh sách thông báo.
5. Hệ thống hiển thị thông báo theo thứ tự mới nhất.
6. Người nhận mở thông báo để đi tới màn hình liên quan.
7. Người nhận có thể đánh dấu thông báo đã đọc.

### Quy tắc nghiệp vụ
- MVP ưu tiên thông báo trong ứng dụng; push notification và email/SMS có thể để V1.1 nếu chưa cần.
- Không tạo thông báo nếu quyết định nghiệp vụ chưa lưu thành công.
- Không đưa OTP, token, ảnh hóa đơn gốc, GPS gốc hoặc OCR text đầy đủ vào nội dung thông báo.
- Người dùng chỉ nhận thông báo về entity mà họ có quyền xem.
- Thông báo trạng thái xác minh phải dùng wording rõ giữa VERIFIED, REFERENCE_ONLY, PENDING_ADMIN_REVIEW và REJECTED.

### Luồng thay thế
- Người nhận không còn ACTIVE → hệ thống không gửi push, nhưng có thể lưu thông báo nội bộ theo policy.
- Payload lỗi hoặc thiếu entity → không tạo thông báo và ghi log vận hành.
- Người nhận không có quyền xem entity → hệ thống không tạo thông báo cho người đó.
- Push provider lỗi nếu có triển khai → không làm rollback quyết định nghiệp vụ đã lưu.

### Tiêu chí nghiệm thu
- Bối cảnh quản trị viên xử lý báo cáo, khi quyết định được lưu, thì người tạo báo cáo nhận thông báo trong ứng dụng.
- Bối cảnh hóa đơn được xác minh, khi worker cập nhật trạng thái VERIFIED, thì chủ đánh giá nhận thông báo kết quả.
- Bối cảnh thông báo đã đọc, khi người dùng đánh dấu đã đọc, thì `readAt` được cập nhật và thông báo không còn nằm trong nhóm chưa đọc.

---

## GAME-001: EXP và cấp hạng cơ bản

### Mục tiêu
Ghi nhận đóng góp của người dùng bằng EXP và cấp hạng mà không làm tăng động cơ spam hoặc bỏ qua chống gian lận.

### Tác nhân
Người dùng đã đăng ký, người đánh giá đã xác minh, hệ thống, quản trị viên.

### Điều kiện tiên quyết
- Người dùng tồn tại và không bị xóa.
- Đánh giá đã được tạo thành công.
- Kết quả xác minh hoặc kiểm duyệt đã được xác định nếu hành động phụ thuộc trạng thái review.

### Dữ liệu vào
- ID người dùng.
- ID đánh giá.
- Trạng thái đánh giá.
- Trạng thái xác minh.
- Sự kiện nghiệp vụ tạo EXP hoặc thu hồi EXP.

### Luồng chính
1. Người dùng gửi đánh giá hợp lệ hoặc đánh giá được xác minh.
2. Hệ thống nhận sự kiện review/verification/moderation.
3. Hệ thống xác định hành động có đủ điều kiện cộng EXP hay không.
4. Hệ thống cộng EXP theo bảng game hóa V1.
5. Hệ thống tính lại cấp hạng nếu người dùng đạt ngưỡng mới.
6. Hồ sơ người dùng hiển thị EXP và cấp hạng mới.

### Quy tắc nghiệp vụ
- Đánh giá tham khảo hợp lệ cộng ít EXP hơn đánh giá VERIFIED.
- Đánh giá VERIFIED được cộng EXP cao hơn vì có bằng chứng xác minh.
- EXP không được cộng cho hóa đơn trùng hash hoặc đánh giá bị REJECTED.
- EXP có thể bị thu hồi nếu đánh giá bị HIDDEN hoặc REJECTED sau kiểm duyệt.
- Cấp hạng không được dùng để bỏ qua chống gian lận.
- EXP từ đánh giá tham khảo cần có giới hạn theo ngày để tránh spam.
- Quy tắc EXP và ngưỡng cấp hạng chi tiết theo `05_Security_Algorithms/Gamification_Design.md`.

### Luồng thay thế
- Đánh giá đang PENDING_ADMIN_REVIEW → chưa cộng EXP verified cho tới khi có quyết định.
- Đánh giá chuyển từ VERIFIED sang HIDDEN → hệ thống thu hồi hoặc vô hiệu hóa EXP theo quyết định kiểm duyệt.
- Job cập nhật EXP lỗi → hệ thống retry và không được cộng trùng cho cùng sự kiện.
- Người dùng bị hạn chế đánh giá → không được nhận EXP từ hành động vi phạm trong thời gian bị hạn chế.

### Tiêu chí nghiệm thu
- Bối cảnh đánh giá được VERIFIED, khi job game hóa chạy, thì người dùng được cộng EXP verified đúng rule.
- Bối cảnh đánh giá bị REJECTED do hash trùng, khi job game hóa chạy, thì người dùng không được cộng EXP.
- Bối cảnh EXP đạt ngưỡng cấp hạng mới và điều kiện bổ sung đạt, khi hệ thống tính lại, thì `rankCode` của người dùng được cập nhật.
