# SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | Software Requirements Specification (SRS) |
| **Phiên bản** | v1.4.0 (Enterprise Standard) |
| **Tác giả** | Engineering Team (SA & Lead Backend) |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. YÊU CẦU GIAO DIỆN HỆ THỐNG (EXTERNAL INTERFACE REQUIREMENTS)

### 1.1. Giao diện Người dùng (User Interface Layout)
*   **Bố cục Trang chủ mặc định (Split-Screen Layout):**
    *   Đối với màn hình máy tính (Độ phân giải lớn hơn hoặc bằng **1024px**): Trang chủ hiển thị cố định dạng chia đôi màn hình tỉ lệ 50-50.
        *   **Cột bên trái:** Chứa thanh tìm kiếm cố định, bộ lọc nhanh Trust Score và danh sách cuộn (scrollable list) hiển thị các thẻ thông tin quán ăn lân cận.
        *   **Cột bên phải:** Bản đồ tương tác chiếm toàn bộ chiều cao màn hình hiển thị các ghim vị trí.
    *   Đối với màn hình di động (Độ phân giải nhỏ hơn **1024px**): Bản đồ hiển thị toàn màn hình, danh sách quán ăn lân cận được thu gọn thành một bảng trượt từ dưới lên (BottomSheet/Drawer) hoặc chuyển đổi qua lại bằng một nút bấm nổi (Floating Toggle Button).

### 1.2. API & Kết nối Ngoại vi (External API Integrations)
*   **SMS Gateway API:** Sử dụng để gửi mã OTP xác thực số điện thoại của người dùng khi đăng ký tài khoản mới.
*   **Map API (Google Maps / Mapbox API):** Dùng để lấy tọa độ địa lý (Latitude, Longitude) của các quán ăn, hiển thị bản đồ tìm kiếm và hỗ trợ tính năng định vị GPS để xác thực vị trí người dùng.
*   **OCR Engine API (Cloud Vision / OCR Service / AWS Textract):** Dùng để gửi ảnh hóa đơn thanh toán lên hệ thống, trích xuất văn bản tự động (các thông tin tên quán, thời gian, tổng tiền).
*   **AI Bedrock Service (AWS Bedrock API):** Sử dụng mô hình ngôn ngữ lớn (ví dụ: Claude 3.5 Sonnet) để đọc hiểu và tổng hợp các bài review verified của quán ăn.

---

## 2. ĐẶC TẢ CHI TIẾT TÍNH NĂNG KỸ THUẬT (FUNCTIONAL SPECIFICATIONS)

### 2.1. Phân hệ Đăng ký & Xác thực (Authentication Module)
*   **SRS-AUTH-001:** Người dùng đăng ký phải cung cấp Số điện thoại hợp lệ tại Việt Nam (định dạng `+84` hoặc `0...`).
*   **SRS-AUTH-002:** Mã OTP gồm 6 chữ số ngẫu nhiên được gửi qua SMS và có hiệu lực trong vòng 120 giây. Hệ thống giới hạn tối đa 3 lần yêu cầu gửi OTP trong 10 phút trên cùng 1 số điện thoại để chống spam tấn công (DDoS).

### 2.2. Động cơ Chống Gian lận (Anti-Fraud Engine Specs)

#### SRS-FRAUD-001: Xác thực Hóa đơn bằng OCR
Khi người dùng tải ảnh hóa đơn (JPEG, PNG, định lượng tối đa 5MB) lên hệ thống:
1.  **Trích xuất văn bản (Text Extraction):** Hệ thống gửi ảnh tới bộ quét OCR để trích xuất các từ khóa.
2.  **Đố chiếu Tên quán (Merchant Name Matching):** Sử dụng thuật toán so khớp chuỗi (Levenshtein Distance) để kiểm tra tên quán trên hóa đơn với tên quán trên hệ thống. Độ tương đồng phải đạt tối thiểu **80%**.
3.  **Kiểm tra Thời gian (Timestamp Validation):** Thời gian in trên hóa đơn phải nằm trong khoảng **48 giờ** so với thời điểm viết review.
4.  **Kiểm tra Trùng lặp (Duplicate Detection):** Hệ thống tính toán mã băm SHA-256 của tệp ảnh hóa đơn và lưu trữ vào cơ sở dữ liệu. Nếu mã băm này đã tồn tại, hệ thống từ chối xác thực và đánh dấu review này là "Nghi ngờ gian lận".

#### SRS-FRAUD-002: Đối chiếu Vị trí (GPS Validation)
Khi người dùng thực hiện review, nếu họ chọn xác thực vị trí bằng GPS:
1.  Hệ thống lấy tọa độ thực tế của người dùng qua trình duyệt/điện thoại (Latitude, Longitude).
2.  Tính khoảng cách địa lý giữa tọa độ người dùng và tọa độ quán ăn trên hệ thống bằng công thức **Haversine**.
3.  **Điều kiện hợp lệ:** Khoảng cách phải nhỏ hơn hoặc bằng **200 mét**. Nếu vượt quá, review sẽ không được duyệt nhãn "Verified Review".

#### SRS-FRAUD-003: Cảnh báo Lệch giá (Price Deviation Detection)
*   Khi hệ thống OCR trích xuất thông tin hóa đơn, backend tiến hành bóc tách các cặp khóa-giá trị dạng `[ItemName, Price]`.
*   Hệ thống so khớp giá trị `Price` này với giá trị của món ăn tương ứng trong bảng dữ liệu thực đơn chính thức (`menu_items`) của quán ăn.
*   **Ngưỡng cảnh báo:** Nếu giá trên hóa đơn cao hơn giá menu đăng ký từ **5%** trở lên, hệ thống sẽ:
    1.  Tự động gắn cờ cảnh báo **`price_deviation = TRUE`** trên quán ăn.
    2.  Hiển thị thông báo đỏ trên giao diện quán: *"Cảnh báo: Phát hiện giá thực tế tại quán lệch so với menu đăng ký."*
    3.  Tạm khóa nhãn "Verified Menu" cho đến khi chủ quán cập nhật bảng giá khớp thực tế.

#### SRS-FRAUD-004: Phát hiện Ảnh sửa đổi (Image Forensic Checker)
Trước khi gửi ảnh hóa đơn vào động cơ OCR, hệ thống chạy quy trình pháp y hình ảnh ngầm để phát hiện dấu hiệu can thiệp bằng Photoshop, Canva hoặc công cụ AI:
1.  **Kiểm tra Siêu dữ liệu (EXIF Metadata Check):**
    *   Hệ thống phân tích phân đoạn `APP1` của tệp tin JPEG/PNG để đọc siêu dữ liệu EXIF.
    *   **Quy tắc từ chối:** Nếu trường dữ liệu phần mềm (`Software`, `Creator Tool`) chứa các từ khóa nằm trong danh sách đen: `Photoshop`, `Canva`, `GIMP`, `Pixlr`, `Lightroom`, `Illustrator`, hệ thống lập tức gắn cờ **`is_manipulated = TRUE`**.
    *   **Quy tắc kiểm tra ảnh camera:** Đối với ảnh được khai báo chụp trực tiếp từ camera điện thoại, hệ thống bắt buộc kiểm tra các thẻ EXIF phần cứng: `Make` (Hãng điện thoại), `Model` (Tên máy), `FNumber`, `ExposureTime`. Nếu thiếu toàn bộ các thông tin này, ảnh bị đánh dấu nghi vấn.
2.  **Phân tích Mức độ Nén (Error Level Analysis - ELA):**
    *   Hệ thống thực hiện lưu tạm ảnh hóa đơn ở một mức nén JPEG cố định (ví dụ: 95% quality).
    *   Tính toán sự chênh lệch màu sắc của từng pixel (pixel-wise difference) giữa ảnh gốc và ảnh nén tạm thời.
    *   **Nhận diện bất thường:** Nếu ảnh có sự chỉnh sửa cục bộ (ví dụ: ghép đè chữ số mới lên hóa đơn cũ), khu vực bị chỉnh sửa sẽ hiển thị độ tương phản nén cực kỳ cao (sáng rực lên) so với các khu vực nền giấy thông thường của hóa đơn. Nếu độ lệch trung bình vượt quá ngưỡng **`12.5%`**, ảnh bị kết luận đã bị sửa đổi.

---

### 2.3. Bản đồ Ẩm thực Tương tác (Interactive Food Map Module)

#### SRS-MAP-001: Lấy tọa độ thời gian thực
*   Hệ thống sử dụng HTML5 Geolocation API để yêu cầu quyền lấy tọa độ hiện tại của thiết bị người dùng (độ chính xác yêu cầu < 50m). Tọa độ này được gửi lên backend để truy vấn các quán ăn lân cận trong bán kính mặc định 5km.

#### SRS-MAP-002: Phân loại Ghim bản đồ (Smart Pins Logic)
*   Khi render bản đồ, Backend trả về thêm thông tin trạng thái mối quan hệ của User hiện tại với từng quán ăn để Frontend thay đổi màu sắc và icon của ghim (pins):
    *   **STATUS_NOT_VISITED (Màu xám):** Không có bản ghi review verified nào của User này tại quán.
    *   **STATUS_VISITED (Màu xanh lá có Checkmark):** Có ít nhất một bản ghi review verified của User này tại quán.
    *   **STATUS_FAVORITE (Màu đỏ có Trái Tim):** Quán nằm trong danh sách yêu thích của User.

#### SRS-MAP-003: Truy vấn không gian theo Bounding Box
*   Mỗi khi người dùng di chuyển hoặc phóng to/thu nhỏ bản đồ (drag/zoom event), Frontend sẽ lấy tọa độ 4 góc của khung hình (North-East, South-West coordinates) gửi lên API.
*   Backend sử dụng câu lệnh SQL PostGIS (`ST_MakeEnvelope`) kết hợp chỉ mục GiST để chỉ quét và trả về danh sách các quán ăn nằm bên trong khung nhìn hiển thị hiện tại, tối ưu tốc độ render bản đồ.

#### SRS-MAP-004: Đồng bộ liên kết hai chiều (Bidirectional Event Binding)
*   Để tăng tốc độ tương tác trên giao diện chia đôi màn hình:
    *   **Tác động từ Danh sách sang Bản đồ:** Khi người dùng di chuột (hover) hoặc bấm vào một thẻ quán ăn trong danh sách bên trái, Frontend sẽ gửi lệnh điều khiển Map API để tự động dịch chuyển tâm bản đồ (panTo) về tọa độ quán ăn tương ứng và mở cửa sổ thông tin nhanh (Info Window).
    *   **Tác động từ Bản đồ sang Danh sách:** Khi người dùng click vào một Ghim quán ăn trên bản đồ, Frontend sẽ tự động đánh dấu nổi bật (highlight) thẻ quán ăn tương ứng ở danh sách bên trái và thực hiện hàm cuộn trang tự động (`scrollIntoView`) để đưa thẻ đó vào tầm nhìn của người dùng.

---

### 2.4. Phân hệ Trí tuệ Nhân tạo & Cộng đồng nâng cao (AI & Advanced Community)

#### SRS-AI-001: Tóm tắt Đánh giá bằng AI (AI Review Synthesizer)
*   Hệ thống thiết lập một tiến trình nền (Background Job) chạy định kỳ hoặc khi có >=10 bài review verified mới của quán ăn.
*   **Xử lý dữ liệu:** Lấy mảng văn bản (`comments`) của 50 bài review verified gần nhất của quán ăn làm dữ liệu đầu vào.
*   **AWS Bedrock API Call:** Gửi Prompt và dữ liệu đầu vào đến mô hình Claude 3.5 Sonnet trên AWS Bedrock.
*   **Prompt System:** *"Bạn là trợ lý đánh giá ẩm thực trung thực. Hãy tổng hợp 50 review thật này thành 3 phần ngắn gọn, trung thực: 1. Ưu điểm (Pros), 2. Nhược điểm (Cons), 3. Lời khuyên từ thực khách (Tips). Yêu cầu trả về định dạng JSON nghiêm ngặt."*
*   **Caching:** Lưu kết quả JSON tóm tắt vào bộ nhớ đệm Redis với thời gian hết hạn (TTL) là **24 giờ** để giảm chi phí gọi API AI.

#### SRS-QUEST-001: Quy trình Kích hoạt Nhiệm vụ Thẩm định (Mystery Quest Trigger)
*   **Điều kiện tự động kích hoạt:** Khi hệ thống phát hiện độ lệch chuẩn (Standard Deviation) của điểm số review của 1 quán ăn trong vòng 7 ngày qua lớn hơn **1.5** (cho thấy có sự tranh chấp dữ liệu gay gắt), hệ thống sẽ:
    1.  Tự động kích hoạt trạng thái **`mystery_quest_active = TRUE`** trên quán ăn.
    2.  Gửi thông báo đẩy (Push Notification) thông qua AWS SNS/FCM đến 5 người dùng có cấp bậc **FOODGOD** đang ở trong bán kính 10km lân cận.
    3.  Người dùng đầu tiên nhận nhiệm vụ sẽ có 24 giờ để đến ăn thực tế, quét hóa đơn và đăng bài viết thẩm định chi tiết.
    4.  Sau khi Admin duyệt bài thẩm định, hệ thống tự động gọi API kích hoạt quy trình hoàn tiền 100% bữa ăn vào ví liên kết của User và trao tặng huy hiệu đặc biệt.

---

## 3. YÊU CẦU PHI CHỨC NĂNG (NON-FUNCTIONAL REQUIREMENTS)

### 3.1. Bảo mật & Quyền riêng tư (Security & Privacy)
*   **Mã hóa dữ liệu:** Toàn bộ mật khẩu của người dùng phải được mã hóa bằng thuật toán **bcrypt** (với số vòng băm `rounds = 10`) trước khi lưu vào cơ sở dữ liệu.
*   **Che giấu thông tin hóa đơn (Data Masking):** Trước khi hiển thị ảnh hóa đơn của khách hàng lên trang công khai, hệ thống phải chạy thuật toán định vị khu vực chữ số và tự động che (blur/blackout) các thông tin nhạy cảm: Số tài khoản ngân hàng, Họ tên khách hàng, Số thẻ tín dụng.

### 3.2. Hiệu năng & Khả năng Mở rộng (Performance & Scalability)
*   **Tốc độ phản hồi (Response Time):** Thời gian tải trang chủ và thực hiện các câu truy vấn tìm kiếm quán ăn phải dưới **1.0 giây** trong điều kiện mạng ổn định.
*   **Khả năng chịu tải (Concurrent Users):** Hệ thống được thiết kế để chịu tải tối thiểu **1,000 kết nối đồng thời** trên mỗi giây (1,000 TPS) không gây suy giảm hiệu năng.
