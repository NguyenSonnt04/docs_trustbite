# 🗄️ DETAILED POSTGRESQL SCHEMA SPECIFICATION
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | Database Schema Specification |
| **Phiên bản** | v1.0.0 (Enterprise Standard) |
| **Tác giả** | Database Administrator (DBA) / System Architect |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. THIẾT KẾ CƠ SỞ DỮ LIỆU LOGIC (LOGICAL DATABASE DESIGN)

Hệ thống sử dụng cơ sở dữ liệu quan hệ **PostgreSQL v15+** để đảm bảo tính toàn vẹn dữ liệu chặt chẽ và khả năng truy vấn tối ưu. Dưới đây là kiến trúc các thực thể và mối quan hệ giữa chúng (ERD logic):

```text
[users] (1) <------------------- (N) [reviews] (N) -------------------> (1) [restaurants]
   |                                    |                                      |
   | (1)                                | (1)                                  | (1)
   |                                    |                                      |
   v (N)                                v (N)                                  v (N)
[user_badges]                     [price_history]                        [price_history]
```

---

## 2. QUY HOẠCH CHỈ MỤC & TỐI ƯU HÓA TRUY VẤN (INDEXING STRATEGY)

Để đảm bảo website phản hồi nhanh dưới 1.0 giây khi lượng dữ liệu lớn, chúng tôi áp dụng các chỉ mục (Indexes) chuyên biệt sau:

### 2.1. Chỉ mục B-Tree thông thường (B-Tree Indexes)
*   `idx_users_phone_number` trên bảng `users(phone_number)`: Tối ưu hóa tốc độ đăng nhập và xác thực OTP.
*   `idx_reviews_user_restaurant` trên bảng `reviews(user_id, restaurant_id)`: Tăng tốc độ truy xuất các review của một quán ăn cụ thể bởi một người dùng cụ thể.

### 2.2. Chỉ mục Không gian (Spatial Indexing - PostGIS)
*   `idx_restaurants_geo` trên tọa độ địa lý của quán ăn: Sử dụng loại chỉ mục **GiST (Generalized Search Tree)** để tìm kiếm nhanh các quán ăn trong bán kính 1km - 5km xung quanh vị trí của người dùng mà không cần duyệt qua toàn bộ dữ liệu (Full Table Scan).

---

## 3. THIẾT KẾ TRÌNH KÍCH HOẠT TỰ ĐỘNG (TRIGGERS & FUNCTIONS)

Để giảm tải cho ứng dụng Backend và đảm bảo tính đồng bộ dữ liệu, cơ sở dữ liệu PostgreSQL được tích hợp các Trigger tự động:

### 3.1. Tự động tính Điểm Chân Thật của Quán ăn
Mỗi khi có một review mới được đăng, hoặc một review bị xóa/cập nhật:
*   Một hàm **PL/pgSQL** sẽ tự động được kích hoạt để tính toán lại điểm trung bình có trọng số (**RestTrustScore**) của quán ăn đó (theo công thức trọng số cấp bậc người dùng tại tệp *Anti_Fraud_Specification.md*).
*   Kết quả sẽ được ghi trực tiếp vào cột `trust_score` của bảng `restaurants`.

### 3.2. Tự động cập nhật Thời gian thay đổi (Updated At)
*   Tự động cập nhật cột `updated_at` của bất kỳ hàng nào khi có sự thay đổi dữ liệu mà backend không cần truyền giá trị thời gian lên.
