# GAMIFICATION DESIGN SPECIFICATIONS
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | Gamification Design Specifications |
| **Phiên bản** | v1.0.0 (Enterprise Standard) |
| **Tác giả** | Game Designer / Product PM |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. THIẾT KẾ CẤP BẬC VÀ ĐIỂM KINH NGHIỆM (RANKS & EXP PROGRESSION)

Hệ thống danh vọng xã hội của TrustBite chia người dùng làm 4 cấp bậc chính. Để đạt được cấp bậc cao hơn, người dùng không chỉ cần tích lũy đủ điểm kinh nghiệm (**EXP**) mà còn phải đạt các **chỉ số xác thực hóa đơn thực tế** bắt buộc. Điều này ngăn chặn việc spam bài viết nhảm để thăng cấp.

### 1.1. Công thức tích lũy EXP (EXP Acquisition Rules)

| Hành động | Số EXP nhận được | Điều kiện / Giới hạn |
| :--- | :---: | :--- |
| **Viết Review thông thường** | **+10 EXP** | Tối đa 2 bài/ngày |
| **Viết Review được xác minh hóa đơn (Verified)**| **+50 EXP** | Không giới hạn, bắt buộc OCR và GPS thành công |
| **Nhận lượt "Hữu ích" (Helpful Vote)** | **+5 EXP / lượt** | Nhận từ những người dùng cấp 2 trở lên |
| **Trở thành người đầu tiên review quán mới** | **+20 EXP (Bonus)** | Được cộng thêm vào điểm review đó |

---

### 1.2. Bảng Cấp Bậc Danh Vọng (Social Ranks Table)

| Cấp bậc | EXP Yêu cầu | Điều kiện đi kèm (Bắt buộc) | Quyền hạn đặc biệt trong hệ thống |
| :--- | :---: | :--- | :--- |
| **Cấp 1: Người Mới (NEWBIE)** | 0 | Không có | *   Viết review cơ bản.<br>*   Đọc review của người khác. |
| **Cấp 2: Thực Thần Tập Sự (APPRENTICE)** | 100 | *   Viết tối thiểu **5 bài review**.<br>*   Tối thiểu **2 hóa đơn** đã xác minh thành công. | *   Mở khóa tính năng **Bình chọn review có ích** của người khác.<br>*   Được quyền gửi **Góp ý kín** cho chủ quán. |
| **Cấp 3: Người Sành Ăn (GOURMET)** | 500 | *   Viết tối thiểu **20 bài review**.<br>*   Tối thiểu **10 hóa đơn** đã xác minh thành công. | *   Được ưu tiên hiển thị bình luận lên đầu trang.<br>*   Ý kiến bình chọn review có ích được tính trọng số cao hơn. |
| **Cấp 4: Thần Ăn Đã Chứng (FOODGOD)** | 2,000 | *   Viết tối thiểu **40 bài review**.<br>*   Tối thiểu **25 hóa đơn** đã xác minh thành công. | *   Trở thành **Thẩm định viên tập sự** (Có quyền duyệt nhanh hoặc gắn cờ báo cáo spam).<br>*   Đánh giá có trọng số điểm cao nhất (x1.5). |

---

## 2. HỆ THỐNG HUY HIỆU ĐỘC ĐỘC (BADGES SPECIFICATION)

Huy hiệu là những danh hiệu đặc biệt được gắn vào hồ sơ cá nhân của người dùng nhằm ghi nhận những thành tựu hoặc thói quen ăn uống độc đáo của họ.

```text
+-------------------------------------------------------------------+
|                           MẪU HIỂN THỊ PROFILE USER                |
|                                                                   |
|   [ Avatar ]  Nguyễn Văn A                                        |
|               Rank: NGƯỜI SÀNH ĂN  (Level 3)                      |
|                                                                   |
|   Huy hiệu đạt được:                                              |
|   [Receipt Master] [Cú Đêm] [Người Khai Phá]              |
+-------------------------------------------------------------------+
```

### Chi tiết các Huy hiệu chính:

#### 1. Huy hiệu: Receipt Master (Chuyên Gia Xác Minh)
*   **Mô tả:** Dành cho những người dùng luôn luôn có ý thức chứng minh trải nghiệm bằng hóa đơn thật.
*   **Điều kiện mở khóa:** Đạt **10 bài review liên tiếp** đều được xác minh hóa đơn (Verified Review) thành công.
*   **Hiệu ứng hiển thị:** Huy hiệu màu vàng hổ phách viền phát sáng, hiển thị ngay cạnh tên hiển thị.

#### 2. Huy hiệu: Người Khai Phá (The Explorer)
*   **Mô tả:** Tôn vinh những người đi đầu trong việc tìm kiếm các địa điểm ăn uống mới chưa ai biết tới.
*   **Điều kiện mở khóa:** Viết **review đầu tiên** (được phê duyệt thành công) cho **5 quán ăn mới** vừa xuất hiện trên hệ thống.
*   **Hiệu ứng hiển thị:** Huy hiệu màu cam đỏ gradient hình tên lửa đang cất cánh.

#### 3. Huy hiệu: Cú Đêm (The Night Owl)
*   **Mô tả:** Dành cho những chuyên gia ẩm thực chuyên đi ăn đêm.
*   **Điều kiện mở khóa:** Có tối thiểu **10 bài review** được đăng trong khung giờ từ **22:00 tối đến 04:00 sáng**.
*   **Hiệu ứng hiển thị:** Huy hiệu màu tím huyền bí hình chú chim cú dưới trăng khuyết.

