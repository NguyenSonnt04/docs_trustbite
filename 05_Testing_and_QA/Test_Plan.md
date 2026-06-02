# SYSTEM TEST PLAN & UAT SPECIFICATION
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | System Test Plan & UAT Specification |
| **Phiên bản** | v1.0.0 (Enterprise Standard) |
| **Tác giả** | QA Lead / QC Manager |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. PHẠM VI KIỂM THỬ (TESTING SCOPE)

Tài liệu này xác định kế hoạch kiểm thử toàn diện hệ thống **TrustBite** trước khi bàn giao cho người dùng cuối (UAT). Phạm vi kiểm thử tập trung vào các cấu phần cốt lõi:

*   **Kiểm thử chức năng (Functional Testing):** Đăng nhập OTP, Tìm kiếm quán ăn, Viết review, Hệ thống thăng cấp bậc (EXP) và mở khóa Huy hiệu.
*   **Kiểm thử tích hợp (Integration Testing):** Quá trình truyền dữ liệu từ cổng API đến dịch vụ AWS Textract (OCR) và đối chiếu khoảng cách GPS.
*   **Kiểm thử bảo mật & Chống gian lận (Anti-Fraud Validation):** Đảm bảo hệ thống phát hiện và chặn đứng mọi hành vi gian lận hóa đơn giả hoặc seeding bài viết ảo.

---

## 2. KỊCH BẢN KIỂM THỬ NGHIỆM THU (UAT TEST CASES)

### Kịch bản 1: Xác thực Hóa đơn hợp lệ bằng OCR (P0 - Must Pass)
*   **Mục tiêu:** Đảm bảo hệ thống trích xuất chính xác thông tin hóa đơn thật và gắn nhãn "Verified Review" cho bài viết.
*   **Các bước thực hiện:**
    1.  Đăng nhập tài khoản User cấp độ 1 (Newbie).
    2.  Chọn quán ăn "Phở Thật 100" trên bản đồ.
    3.  Nhập đánh giá 4 sao và bình luận >50 ký tự.
    4.  Tải lên ảnh hóa đơn thanh toán hợp lệ của quán "Phở Thật 100" in trong vòng 24 giờ qua.
    5.  Bật định vị GPS tại vị trí cách quán 50m.
    6.  Bấm gửi đánh giá.
*   **Kết quả kỳ vọng:**
    *   Hệ thống chạy thanh trạng thái quét OCR thành công.
    *   Bài review được tự động duyệt hiển thị công khai ở trạng thái **Verified Review**.
    *   Tài khoản User được cộng **+50 EXP** ngay lập tức.
    *   Mã băm (SHA-256) của hóa đơn được lưu vào CSDL để quản lý trùng lặp.

---

### Kịch bản 2: Phát hiện Hóa đơn trùng lặp (Anti-Fraud - P0)
*   **Mục tiêu:** Ngăn chặn hành vi một hóa đơn được dùng bởi nhiều tài khoản khác nhau để seeding điểm uy tín.
*   **Các bước thực hiện:**
    1.  Tài khoản User A thực hiện Kịch bản 1 thành công (Hóa đơn X được ghi nhận).
    2.  Đăng nhập tài khoản User B trên một thiết bị khác.
    3.  User B viết review cho cùng quán ăn đó và tải lên **chính xác ảnh hóa đơn X** mà User A đã sử dụng.
    4.  Bấm gửi đánh giá.
*   **Kết quả kỳ vọng:**
    *   Hệ thống so khớp mã băm và phát hiện trùng lặp mã hash của ảnh hóa đơn X trong CSDL.
    *   Hệ thống từ chối cấp nhãn Verified cho User B, chuyển bài viết thành **Review Tham Khảo**.
    *   Hệ thống ghi nhận log cảnh báo tài khoản User B có hành vi "Nghi ngờ lạm dụng hóa đơn".

---

### Kịch bản 3: Kiểm tra định vị GPS ngoài bán kính cho phép (P1)
*   **Mục tiêu:** Xác minh hệ thống từ chối duyệt hóa đơn nếu người dùng viết review khi đang ở quá xa quán ăn.
*   **Các bước thực hiện:**
    1.  Đăng nhập tài khoản User bất kỳ.
    2.  Chọn quán ăn "Cà phê View Đẹp" ở Quận 1.
    3.  Bật định vị GPS khi thiết bị đang ở cách quán ăn **1.5 km** (ngoài bán kính 200m cho phép).
    4.  Nhập review, tải lên hóa đơn hợp lệ.
    5.  Bấm gửi đánh giá.
*   **Kết quả kỳ vọng:**
    *   Hệ thống tính khoảng cách Haversine và xác định khoảng cách >200m.
    *   Hệ thống hiển thị cảnh báo: *"Vị trí định vị của bạn không khớp với quán ăn. Bài đánh giá sẽ được ghi nhận ở trạng thái tham khảo thông thường."*
    *   Review được lưu ở trạng thái **Reference (Tham khảo)**, chỉ được cộng **+10 EXP**.

---

### Kịch bản 4: Thăng cấp bậc & Mở khóa Huy hiệu (Gamification - P0)
*   **Mục tiêu:** Đảm bảo hệ thống tự động thăng cấp và trao tặng huy hiệu khi người dùng đạt đủ điều kiện.
*   **Các bước thực hiện:**
    1.  Tài khoản User hiện tại đang có **90 EXP** và đã viết 4 review thường.
    2.  User thực hiện viết 1 bài review có hóa đơn hợp lệ (+50 EXP).
*   **Kết quả kỳ vọng:**
    *   Tổng EXP của User tăng lên **140 EXP** (>100 EXP).
    *   Hệ thống tự động chạy hoạt ảnh chúc mừng và cập nhật cấp bậc của User từ **NEWBIE** lên **APPRENTICE (Thực Thần Tập Sự)**.
    *   Quyền hạn của User được mở rộng (Có thể bấm nút "Hữu ích" cho bài viết của người khác).

