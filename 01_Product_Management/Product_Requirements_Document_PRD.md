# PRODUCT REQUIREMENTS DOCUMENT (PRD)
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | Product Requirements Document (PRD) |
| **Phiên bản** | v1.1.0 (Enterprise Standard) |
| **Tác giả** | Product Management Team |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. CHÂN DUNG NGƯỜI DÙNG MỤC TIÊU (USER PERSONAS)

### Persona A: "Khách ăn thông thái" (The Smart Diner)
*   **Mô tả:** Người trẻ từ 18 - 35 tuổi, thích đi ăn uống ngoài tiệm, thường xuyên tìm kiếm các quán ăn/nước uống mới.
*   **Nỗi đau (Pain Points):**
    *   Bị lừa bởi các clip review ảo trên TikTok/Facebook, đến quán ăn thực tế chất lượng rất tệ và giá cả đắt đỏ.
    *   Khó phân biệt được review nào là seeding của quán, review nào là thật.
*   **Mong muốn:** Có một website chỉ cho biết địa điểm ngon thật, giá cả chính xác và review của những người thực sự đã đi ăn ở đó.

### Persona B: "Chủ quán chân chính" (The Honest Merchant)
*   **Mô tả:** Chủ nhà hàng, quán cafe tự tin về chất lượng món ăn và dịch vụ của mình, kinh doanh tử tế.
*   **Nỗi đau (Pain Points):**
    *   Không có nhiều ngân sách để thuê KOL quảng cáo giá đắt đỏ.
    *   Bị đối thủ chơi xấu thuê click-farm vào đánh giá 1 sao hàng loạt.
*   **Mong muốn:** Một nền tảng minh bạch để đăng ký thông tin quán, chứng minh chất lượng thực tế và được bảo vệ trước các đánh giá ác ý không có bằng chứng hóa đơn.

---

## 2. DANH SÁCH YÊU CẦU TÍNH NĂNG (PRODUCT FEATURE REQUIREMENTS)

Chúng tôi phân loại tính năng theo mức độ ưu tiên:
*   **P0 (Must-Have):** Bắt buộc phải có để hệ thống có thể hoạt động (Core MVP).
*   **P1 (Should-Have):** Rất cần thiết để tăng trải nghiệm người dùng và chống gian lận nâng cao.
*   **P2 (Could-Have):** Các tính năng mở rộng có thể bổ sung sau khi hệ thống ổn định.

### 2.1. Quản lý Tài khoản & Định danh
| ID | Tính năng | Mô tả | Ưu tiên |
| :--- | :--- | :--- | :---: |
| **F-01** | Đăng ký qua SĐT | Người dùng bắt buộc nhập SĐT và xác thực OTP để tránh tài khoản clone ảo. | **P0** |
| **F-02** | Liên kết Mạng xã hội | Cho phép liên kết Facebook/Google để lấy thông tin avatar và họ tên thật. | **P1** |

### 2.2. Tìm kiếm, Khám phá & Lọc thông tin
| ID | Tính năng | Mô tả | Ưu tiên |
| :--- | :--- | :--- | :---: |
| **F-03** | Tìm kiếm thông minh | Tìm theo tên món ăn, tên quán hoặc địa lý (quận/huyện). | **P0** |
| **F-04** | Bộ lọc Trust Score | Lọc các quán có điểm chân thật từ cao xuống thấp (dựa trên Verified Reviews). | **P0** |
| **F-05** | Bản đồ Ẩm thực Tương tác | Bản đồ trực quan hiển thị vị trí hiện tại của người dùng (GPS), các quán ăn lân cận xung quanh. Hỗ trợ hệ thống **Ghim màu thông minh (Smart Pins)**: Màu xanh lá cho quán **đã từng ăn** (có review verified), màu đỏ trái tim cho quán **yêu thích**, màu xám cho quán **chưa ăn**. Tích hợp bộ lọc nhanh trực tiếp trên bản đồ. | **P0** |
| **F-06** | Biểu đồ biến động giá | Hiển thị lịch sử giá của các món ăn chính được người dùng cập nhật qua hóa đơn. | **P1** |

### 2.3. Hệ thống Viết Đánh giá & Chống gian lận
| ID | Tính năng | Mô tả | Ưu tiên |
| :--- | :--- | :--- | :---: |
| **F-07** | Đánh giá đa chiều | Cho phép xếp hạng 1-5 sao theo 4 tiêu chí: Đồ ăn, Giá cả, Phục vụ, Không gian. | **P0** |
| **F-08** | Quét Hóa đơn (OCR) | Tải ảnh hóa đơn thanh toán lên hệ thống, tự động quét kiểm tra ngày giờ và tên quán để cấp nhãn "Verified Review". | **P0** |
| **F-09** | Định vị GPS | Đối chiếu tọa độ thực tế của người dùng với tọa độ quán khi thực hiện review. | **P1** |
| **F-10** | Chống trùng hóa đơn | Mỗi hóa đơn sau khi quét sẽ bị lưu mã hash, không cho phép sử dụng lại hóa đơn này cho bài viết khác. | **P0** |

### 2.4. Phân hệ Game hóa (Gamification)
| ID | Tính năng | Mô tả | Ưu tiên |
| :--- | :--- | :--- | :---: |
| **F-11** | Tích lũy EXP | Nhận điểm kinh nghiệm (EXP) khi viết review, đặc biệt cộng điểm lớn khi quét hóa đơn thành công. | **P0** |
| **F-12** | Cấp bậc người dùng | Thăng cấp từ Người Mới lên Thực Thần Tập Sự -> Người Sành Ăn -> Thần Ăn Đã Chứng. | **P0** |
| **F-13** | Bộ sưu tập Huy hiệu | Đạt các huy hiệu đặc trưng như: Receipt Master, Cú Đêm, Khai Phá. | **P1** |

---

## 3. CHỈ SỐ ĐO LƯỜNG THÀNH CÔNG (SUCCESS METRICS - KPIS)

*   **Tỷ lệ Giữ chân Người dùng (Retention Rate):** Đạt tối thiểu 35% user quay lại website trong vòng 30 ngày (D30 Retention).
*   **Chỉ số Hài lòng của Khách ăn (User CSAT):** Điểm đánh giá mức độ tin cậy của website đạt từ 4.5/5.0 trở lên qua các khảo sát định kỳ.
*   **Tốc độ Phê duyệt của Admin:** Thời gian duyệt hồ sơ quán ăn đăng ký mới trung bình dưới 12 giờ làm việc.
