# ⚖️ CONTENT MODERATION & USER CONDUCT POLICY
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | Content Moderation & User Conduct Policy |
| **Phiên bản** | v1.0.0 (Enterprise Standard) |
| **Tác giả** | Community Operations Director / Lead Moderator |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. ĐỊNH NGHĨA NỘI DUNG VI PHẠM (PROHIBITED CONTENT DEFINITION)

Để xây dựng một cộng đồng review văn minh, trung thực, hệ thống **TrustBite** quy định nghiêm ngặt các loại nội dung bị cấm hiển thị công khai. Bất kỳ bài đánh giá nào vi phạm các điều khoản sau sẽ bị tự động gắn cờ hoặc xóa bỏ ngay lập tức:

### 1.1. Ngôn từ và Thái độ không phù hợp
*   Sử dụng từ ngữ tục tĩu, thô tục, chửi thề hoặc có tính chất phân biệt vùng miền, giới tính, tôn giáo.
*   Công kích cá nhân trực tiếp (nhân viên phục vụ, chủ quán) thay vì nhận xét khách quan về chất lượng món ăn và chất lượng dịch vụ.

### 1.2. Bão đánh giá ác ý (Brigading / Review Bombing)
*   Nghiêm cấm các nhóm người dùng tập trung đánh giá 1 sao hàng loạt cho một quán ăn do các sự cố truyền thông hoặc bê bối trên mạng xã hội không liên quan đến chất lượng ẩm thực thực tế của quán.
*   *Quy chế xử lý:* Hệ thống tự động khóa tính năng đánh giá của quán ăn đó nếu phát hiện lượng review 1 sao tăng đột biến gấp 500% so với ngày thường mà không đi kèm hóa đơn kiểm chứng.

### 1.3. Cáo buộc nghiêm trọng không có bằng chứng
*   Các cáo buộc liên quan đến **ngộ độc thực phẩm**, **có dị vật nguy hiểm** (mảnh thủy tinh, kim tiêm, côn trùng gây hại...) trong đồ ăn bắt buộc phải đi kèm ảnh chụp hóa đơn rõ ràng, thời gian khớp và ảnh chụp bằng chứng thực tế.
*   Trường hợp cáo buộc ngộ độc thực phẩm quy mô lớn, hệ thống yêu cầu cung cấp thêm biên bản kiểm tra y tế hoặc hóa đơn bệnh viện có mốc thời gian trùng khớp. Nếu không có bằng chứng, bài viết sẽ bị ẩn để tránh hủy hoại việc kinh doanh của quán ăn vô căn cứ.

---

## 2. QUY TRÌNH KIỂM DUYỆT 3 LỚP (3-TIER MODERATION PROCESS)

Chúng tôi áp dụng quy trình kiểm duyệt kết hợp giữa công nghệ AI tự động và nhân sự vận hành:

```text
[ Review mới đăng ]
       │
       ▼
[ LỚP 1: AI Quét Nội Dung ] ───(Phát hiện từ cấm, spam)───> [ Gắn cờ ẩn chờ duyệt ]
       │
       ▼ (Hợp lệ)
[ LỚP 2: Cộng Đồng Báo Cáo ] ──(Chủ quán/User bấm Report)──> [ Đẩy lên hàng đợi Admin ]
       │
       ▼ (Không bị báo cáo)
[ Hiển thị Công khai ] <────────(Phê duyệt giữ lại)──────── [ LỚP 3: Admin Phán Quyết ]
```

### 2.1. Lớp 1: Hệ thống AI tự động (Automated Text Filter)
*   Sử dụng NLP để lọc các từ ngữ tục tĩu tiếng Việt trong danh sách đen (Blacklist).
*   Quét phát hiện các ký tự đặc biệt lặp lại liên tục, hoặc bài viết quá ngắn (<50 ký tự) sẽ bị chặn ngay tại giao diện viết review.

### 2.2. Lớp 2: Cộng đồng báo cáo (Community Flagging)
*   Cả người dùng khác và chủ quán đều có nút **"Báo cáo vi phạm"**.
*   Khi bài review nhận đủ 3 lượt báo cáo từ các tài khoản có cấp bậc cấp 2 (Apprentice) trở lên, bài viết sẽ tạm thời bị ẩn khỏi trang chính của quán và đẩy vào hàng đợi của Admin để xử lý.

---

## 3. HÌNH THỨC PHẠT VÀ XỬ LÝ VI PHẠM (PENALTIES SYSTEM)

### 3.1. Đối với Khách ăn (User)
*   **Vi phạm lần 1:** Cảnh cáo bằng thông báo trong app, trừ **-100 EXP**.
*   **Vi phạm lần 2 (Cố tình photoshop hóa đơn giả hoặc seeding bài viết ảo):** Hạ cấp bậc User về **NEWBIE**, khóa quyền viết review trong 14 ngày.
*   **Vi phạm lần 3 (Nghiêm trọng):** Khóa tài khoản và số điện thoại vĩnh viễn trên toàn hệ thống TrustBite.

### 3.2. Đối với Quán ăn (Merchant)
*   Nếu phát hiện chủ quán thuê dịch vụ seeding để nâng điểm Trust Score:
    *   *Phạt lần 1:* Hủy bỏ toàn bộ các review seeding ảo. Gửi thư cảnh cáo.
    *   *Phạt lần 2:* Treo huy hiệu màu đỏ **"Cảnh báo: Có dấu hiệu thao túng đánh giá"** nổi bật trên trang quán ăn trong **30 ngày**.
    *   *Phạt lần 3:* Khóa trang thông tin quán vĩnh viễn, ẩn quán khỏi bản đồ tìm kiếm của TrustBite.
