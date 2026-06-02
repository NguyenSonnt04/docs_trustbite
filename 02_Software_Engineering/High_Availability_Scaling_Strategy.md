# HIGH AVAILABILITY & SCALING STRATEGY (20,000+ CONCURRENT USERS)
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | High Availability & Scaling Strategy |
| **Phiên bản** | v1.0.0 (Enterprise Standard) |
| **Tác giả** | Lead DevOps / System Architect |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. THỬ THÁCH KHI HỆ THỐNG ĐẠT 20,000 USER ĐỒNG THỜI
Khi hệ thống có 20,000 người dùng kết nối và gửi yêu cầu cùng một thời điểm, các điểm nghẽn (bottlenecks) nguy hiểm nhất gây chết server bao gồm:
*   **Database Exhaustion:** Quá nhiều kết nối truy vấn đọc/ghi trực tiếp vào CSDL PostgreSQL làm cạn kiệt bộ nhớ RAM và CPU của máy chủ DB, gây sập database.
*   **Compute Block:** Máy chủ Backend chính (Node.js) bị nghẽn luồng xử lý do phải chạy các tác vụ nặng (quét OCR, xử lý ảnh, tính toán khoảng cách địa lý).
*   **Network Congestion:** Băng thông mạng bị quá tải do phải truyền tải dữ liệu ảnh hóa đơn dung lượng lớn liên tục.

Để giải quyết triệt để, chúng tôi áp dụng **Kiến trúc phân tải 4 tầng** trên hạ tầng AWS:

---

## 2. CHIẾN LƯỢC PHÂN TẢI & CO GIÃN TỰ ĐỘNG (COMPUTE SCALING)

### 2.1. AWS Application Load Balancer (ALB)
*   Đặt một bộ cân bằng tải **ALB** ở tuyến đầu của dịch vụ Backend. ALB làm nhiệm vụ tiếp nhận toàn bộ các request HTTPS từ API Gateway và phân phối đều (Round-Robin) cho các container phía sau.
*   ALB tự động kiểm tra trạng thái sức khỏe (Health Check) của các container, nếu phát hiện container nào bị lỗi/sập, nó lập tức ngắt kết nối và chuyển hướng traffic sang các container lành lặn khác.

### 2.2. ECS Fargate Auto-Scaling (Co giãn tự động không độ trễ)
*   Ứng dụng Node.js/NestJS được đóng gói chạy trên cụm **Amazon ECS Fargate** đa vùng (Multi-AZ).
*   **Cấu hình Target Tracking Scaling Policy:**
    *   *Chỉ số kích hoạt:* Scale out (tự động tăng thêm container) nếu mức sử dụng **CPU trung bình vượt quá 70%** hoặc **RAM vượt quá 75%** trong vòng 2 phút liên tiếp.
    *   *Giới hạn cấu hình:* Chạy tối thiểu **3 containers** (chia đều cho 3 Availability Zones để phòng ngừa thảm họa sập 1 vùng) và cho phép tự động tăng tối đa lên **30 containers** khi có bão truy cập.
    *   Khi lượng truy cập giảm xuống, hệ thống tự động tắt bớt container (Scale in) để tiết kiệm chi phí.

---

## 3. CHIẾN LƯỢC TỐI ƯU CƠ SỞ DỮ LIỆU (DATABASE SCALING & CONNECTION POOLING)

*CSDL luôn là điểm nghẽn dễ sập nhất. Chúng tôi áp dụng hai giải pháp sống còn:*

### 3.1. Phân luồng Đọc/Ghi (RDS Read Replicas)
Đặc thù của website review là **90% người dùng vào đọc thông tin** (xem menu, đọc review, tìm trên bản đồ) và chỉ có **10% người dùng thực hiện ghi dữ liệu** (viết review, đăng ký quán).
*   **RDS Primary Instance (Cơ sở dữ liệu Ghi):** Chỉ tiếp nhận các truy vấn dạng `INSERT`, `UPDATE`, `DELETE`.
*   **RDS Read Replicas (Bản sao chỉ đọc):** Thiết lập **2 bản sao chỉ đọc** chạy đồng bộ dữ liệu với Primary. Toàn bộ các API tìm kiếm, xem review của 20,000 user sẽ được ALB định tuyến chuyển hướng sang 2 Read Replicas này để xử lý.
*   👉 *Lợi ích:* Giải phóng 90% tải cho máy chủ database chính, đảm bảo DB không bao giờ bị quá tải CPU.

### 3.2. Quản lý kết nối thông qua AWS RDS Proxy
*   Node.js có tốc độ xử lý nhanh nhưng mỗi tiến trình kết nối trực tiếp vào PostgreSQL sẽ chiếm một lượng RAM cố định của DB. 20,000 kết nối đồng thời sẽ lập tức làm tràn RAM của PostgreSQL.
*   **Giải pháp:** Đặt **AWS RDS Proxy** đứng giữa Node.js và PostgreSQL. RDS Proxy làm nhiệm vụ gom nhóm và chia sẻ tài nguyên kết nối (Connection Pooling). Thay vì mở 20,000 kết nối thật, RDS Proxy chỉ mở khoảng 200 kết nối cố định vào DB và chia sẻ luân phiên cực nhanh cho 20,000 request của người dùng.
*   👉 *Lợi ích:* Tránh hoàn toàn lỗi "Too many connections" gây chết database.

---

## 4. CHIẾN LƯỢC BỘ NHỚ ĐỆM TẦNG SÂU (DEEP CACHING)

### 4.1. Edge Caching (Amazon CloudFront)
*   Cấu hình CloudFront CDN để lưu trữ bộ nhớ đệm (cache) cho các phản hồi API tĩnh hoặc bán tĩnh (Ví dụ: danh sách món ăn, thông tin địa chỉ quán ăn).
*   Khi có 10,000 user cùng xem thông tin của một quán trà sữa hot, CloudFront sẽ trả về dữ liệu cache ngay tại các máy chủ gần nhất ở Việt Nam mà **hoàn toàn không cần gọi request về server Node.js hay Database**.

### 4.2. In-Memory Caching (Amazon ElastiCache for Redis)
*   Các thông tin có tính cập nhật cao hơn (như điểm số Trust Score, top review nổi bật) sẽ được cache trong bộ nhớ RAM cực nhanh của **Redis**.
*   Redis có khả năng xử lý lên tới **150,000 request/giây** với độ trễ < 1 mili-giây.

---

## 5. XỬ LÝ BẤT ĐỒNG BỘ CÁC TÁC VỤ NẶNG (ASYNCHRONOUS BACKGROUND JOBS)

*   Tuyệt đối không chạy các tác vụ nặng như quét ảnh hóa đơn OCR (Textract) hay gọi AI tổng hợp review (Bedrock) trực tiếp trong luồng request của người dùng (Synchronous).
*   **Luồng xử lý bất đồng bộ (Async Queue):**
    1.  User gửi ảnh hóa đơn lên $\rightarrow$ Node.js lập tức đẩy tác vụ vào hàng đợi **BullMQ (Redis)**.
    2.  Server trả ngay về mã HTTP `202 Accepted` kèm ID hàng đợi cho khách hàng trong vòng 0.05 giây (Giao diện hiển thị trạng thái "Đang quét hóa đơn...").
    3.  Một cụm container chuyên dụng riêng biệt ở Backend (Worker Service) sẽ âm thầm rút tác vụ từ BullMQ ra để gửi đi quét OCR và cập nhật kết quả vào DB sau.
*   👉 *Lợi ích:* Đảm bảo server chính luôn rảnh rang 100% để tiếp nhận các kết nối mới từ 20,000 user mà không bao giờ bị nghẽn luồng hay treo máy.
