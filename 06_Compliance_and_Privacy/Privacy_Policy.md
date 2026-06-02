# 🛡️ PRIVACY POLICY & USER DATA PROTECTION SPECIFICATION
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | Privacy Policy & User Data Protection |
| **Phiên bản** | v1.0.0 (Enterprise Standard) |
| **Tác giả** | Legal Counsel / Information Security Officer (ISO) |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. MỤC ĐÍCH & PHẠM VI ÁP DỤNG

Do đặc thù nền tảng **TrustBite** yêu cầu thu thập các dữ liệu cá nhân nhạy cảm của người dùng để phục vụ quá trình chống gian lận (bao gồm ảnh chụp hóa đơn thanh toán chứa thông tin giao dịch, số điện thoại đăng ký và tọa độ vị trí thực tế GPS), tài liệu này quy định rõ các chính sách pháp lý và tiêu chuẩn kỹ thuật bảo vệ thông tin nhằm đảm bảo tuân thủ nghiêm ngặt Luật An ninh mạng Việt Nam và Nghị định 13/2023/NĐ-CP về bảo vệ dữ liệu cá nhân.

---

## 2. DANH MỤC DỮ LIỆU THU THẬP & PHƯƠNG PHÁP XỬ LÝ BẢO MẬT

### 2.1. Số điện thoại & Định danh tài khoản
*   **Dữ liệu thu thập:** Số điện thoại của người dùng khi đăng ký tài khoản.
*   **Phương pháp bảo mật:**
    *   Sử dụng dịch vụ **Amazon Cognito** bảo mật để quản lý định danh.
    *   Mật khẩu được băm (hash) một chiều bằng thuật toán **bcrypt** trước khi lưu trữ, cam kết không lưu mật khẩu dạng văn bản thuần túy (plaintext) trong bất kỳ nhật ký hệ thống (logs) nào.

### 2.2. Ảnh chụp Hóa đơn Thanh toán (Receipt Images)
*   **Dữ liệu thu thập:** Ảnh chụp hóa đơn thanh toán do người dùng tải lên để xác thực review. Hóa đơn có thể chứa thông tin tên khách hàng, mã giao dịch, số tài khoản ngân hàng hoặc số thẻ tín dụng (4 số cuối).
*   **Phương pháp xử lý bảo mật (Data Masking & Blurring):**
    *   Hệ thống Backend của TrustBite tích hợp giải thuật tự động phát hiện văn bản nhạy cảm bằng AI.
    *   Toàn bộ ảnh hóa đơn sau khi quét xong thông tin tên quán/tổng tiền sẽ tự động chạy qua **bộ lọc làm mờ (automatic blurring filter)** để bôi đen/làm mờ các khu vực thông tin cá nhân (Họ tên khách, Số tài khoản, Chi tiết thẻ).
    *   Chỉ phiên bản ảnh hóa đơn **đã làm mờ** mới được hiển thị công khai trên bài review để cộng đồng kiểm chứng. Phiên bản gốc được lưu trữ mã hóa trong phân vùng bảo mật riêng tư trên Amazon S3 và tự động xóa bỏ sau 30 ngày.

### 2.3. Tọa độ Định vị địa lý (GPS Location Data)
*   **Dữ liệu thu thập:** Tọa độ Latitude và Longitude của thiết bị di động tại thời điểm người dùng bấm nút xác thực vị trí viết review.
*   **Phương pháp bảo mật:**
    *   **Không theo dõi liên tục:** TrustBite cam kết không theo dõi vị trí của người dùng trong thời gian thực. Hệ thống chỉ yêu cầu quyền truy cập vị trí một lần duy nhất tại đúng thời điểm người dùng chủ động gửi bài đánh giá.
    *   **Mã hóa tọa độ:** Tọa độ GPS của người dùng chỉ được dùng để tính toán khoảng cách Haversine với quán ăn trên server tạm thời (in-memory compute), sau khi cho ra kết quả Đúng/Sai, dữ liệu tọa độ định vị của người dùng sẽ bị hủy bỏ, hệ thống chỉ lưu trữ kết quả Boolean `gps_verified = TRUE/FALSE` vào cơ sở dữ liệu.

---

## 3. CHÍNH SÁCH CHIA SẺ & LƯU TRỮ DỮ LIỆU (DATA RETENTION & SHARING)

*   **Không mua bán dữ liệu:** TrustBite cam kết tuyệt đối không bán, cho thuê hoặc chia sẻ dữ liệu hóa đơn, số điện thoại hay vị trí của người dùng cho bất kỳ bên thứ ba nào (kể cả các quán ăn đối tác trên hệ thống) vì mục đích quảng cáo hay tiếp thị.
*   **Quyền được xóa dữ liệu (Right to be Forgotten):** Người dùng có quyền yêu cầu xóa vĩnh viễn tài khoản của mình trên hệ thống. Khi tài khoản bị xóa, toàn bộ số điện thoại, lịch sử review và các ảnh hóa đơn liên quan sẽ bị xóa sạch khỏi cơ sở dữ liệu và lưu trữ đám mây của chúng tôi trong vòng 72 giờ làm việc.
