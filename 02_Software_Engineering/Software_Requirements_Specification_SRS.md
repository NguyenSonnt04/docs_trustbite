# 📝 SOFTWARE REQUIREMENTS SPECIFICATION (SRS)
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | Software Requirements Specification (SRS) |
| **Phiên bản** | v1.0.0 (Enterprise Standard) |
| **Tác giả** | Engineering Team (SA & Lead Backend) |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. YÊU CẦU GIAO DIỆN HỆ THỐNG (EXTERNAL INTERFACE REQUIREMENTS)

### 1.1. Giao diện Người dùng (User Interface)
*   **Thiết kế:** Áp dụng phong cách tối giản hiện đại (Modern Premium Dark Mode), tối ưu hóa hiển thị trên cả trình duyệt máy tính (Desktop) và điện thoại di động (Responsive).
*   **Nguyên tắc hiển thị:** Toàn bộ thông tin quan trọng như **Trust Score** và các review **Verified** phải được đặt ở vị trí trung tâm, trực quan.

### 1.2. API & Kết nối Ngoại vi (External API Integrations)
*   **SMS Gateway API:** Sử dụng để gửi mã OTP xác thực số điện thoại của người dùng khi đăng ký tài khoản mới.
*   **Map API (Google Maps / Mapbox API):** Dùng để lấy tọa độ địa lý (Latitude, Longitude) của các quán ăn, hiển thị bản đồ tìm kiếm và hỗ trợ tính năng định vị GPS để xác thực vị trí người dùng.
*   **OCR Engine API (Cloud Vision / OCR Service):** Dùng để gửi ảnh hóa đơn thanh toán lên hệ thống, trích xuất văn bản tự động (các thông tin tên quán, thời gian, tổng tiền).

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

---

## 3. YÊU CẦU PHI CHỨC NĂNG (NON-FUNCTIONAL REQUIREMENTS)

### 3.1. Bảo mật & Quyền riêng tư (Security & Privacy)
*   **Mã hóa dữ liệu:** Toàn bộ mật khẩu của người dùng phải được mã hóa bằng thuật toán **bcrypt** (với số vòng băm `rounds = 10`) trước khi lưu vào cơ sở dữ liệu.
*   **Che giấu thông tin hóa đơn (Data Masking):** Trước khi hiển thị ảnh hóa đơn của khách hàng lên trang công khai, hệ thống phải chạy thuật toán định vị khu vực chữ số và tự động che (blur/blackout) các thông tin nhạy cảm: Số tài khoản ngân hàng, Họ tên khách hàng, Số thẻ tín dụng.

### 3.2. Hiệu năng & Khả năng Mở rộng (Performance & Scalability)
*   **Tốc độ phản hồi (Response Time):** Thời gian tải trang chủ và thực hiện các câu truy vấn tìm kiếm quán ăn phải dưới **1.0 giây** trong điều kiện mạng ổn định.
*   **Khả năng chịu tải (Concurrent Users):** Hệ thống được thiết kế để chịu tải tối thiểu **1,000 kết nối đồng thời** trên mỗi giây (1,000 TPS) không gây suy giảm hiệu năng.
