# 📖 TRUSTBITE - CHỮA LÀNH LÒNG TIN ẨM THỰC

Chào mừng bạn đến với Repository tài liệu chính thức của dự án **TrustBite** (Đại diện cho: *"Sự tin cậy trong từng miếng ăn"*). Đây là hệ thống tài liệu đặc tả toàn diện chuẩn Enterprise, được thiết kế và quy hoạch khoa học để phục vụ quá trình phát triển, kiểm thử và vận hành nền tảng.

---

## 📂 CẤU TRÚC HỆ THỐNG TÀI LIỆU (DOCUMENTATION TREE)

Kho tài liệu được chia thành các thư mục chuyên biệt tương ứng với các phòng ban và lĩnh vực chuyên môn:

### 📁 01. Quản lý Sản phẩm (Product Management)
Định hình tầm nhìn kinh doanh và các yêu cầu nghiệp vụ cốt lõi:
*   [Project_Charter.md](./01_Product_Management/Project_Charter.md): Hiến chương dự án (Mục tiêu, Lộ trình mốc thời gian, Vai trò nhân sự và OKRs).
*   [Product_Requirements_Document_PRD.md](./01_Product_Management/Product_Requirements_Document_PRD.md): Chân dung người dùng, danh sách yêu cầu tính năng phân cấp độ ưu tiên (P0, P1, P2).

### 📁 02. Kỹ thuật & Hạ tầng (Software Engineering)
Bản vẽ thiết kế hệ thống, hạ tầng đám mây và quy trình tự động hóa:
*   [Software_Requirements_Specification_SRS.md](./02_Software_Engineering/Software_Requirements_Specification_SRS.md): Đặc tả yêu cầu kỹ thuật chi tiết của các tính năng.
*   [System_Architecture_Design.md](./02_Software_Engineering/System_Architecture_Design.md): Sơ đồ thiết kế luồng dữ liệu kiến trúc hệ thống tổng quan.
*   [AWS_Cloud_Infrastructure.md](./02_Software_Engineering/AWS_Cloud_Infrastructure.md): Thiết kế hạ tầng Cloud Native 100% AWS (Lambda, ECS Fargate, WAF, Cognito, Textract).
*   [CI_CD_Pipeline.md](./02_Software_Engineering/CI_CD_Pipeline.md): Quy trình tự động hóa Git, Docker Build, và triển khai Blue/Green Zero-Downtime.
*   [Tech_Stack_Specification.md](./02_Software_Engineering/Tech_Stack_Specification.md): Đặc tả chi tiết các thư viện NPM, Next.js v14 và Node.js (NestJS).

### 📁 03. Giải thuật & Game hóa (Security & Algorithms)
Trái tim công nghệ của TrustBite:
*   [Anti_Fraud_Specification.md](./03_Security_Algorithms/Anti_Fraud_Specification.md): Thuật toán toán học Haversine tính bán kính GPS, giải thuật so khớp Levenshtein (OCR hóa đơn), và công thức tính điểm Trust Score trọng số.
*   [Gamification_Design.md](./03_Security_Algorithms/Gamification_Design.md): Hệ thống thăng cấp bậc người dùng (EXP) và bộ sưu tập huy hiệu danh vọng.

### 📁 04. Thiết kế Cơ sở dữ liệu (Database Design)
*   [PostgreSQL_Database_Schema.md](./04_Database_Design/PostgreSQL_Database_Schema.md): Thiết kế các bảng dữ liệu, khóa ngoại, chỉ mục (B-Tree, GiST PostGIS) và trigger tự động tính điểm.

### 📁 05. Kiểm thử & Đảm bảo chất lượng (Testing & QA)
*   [Test_Plan.md](./05_Testing_and_QA/Test_Plan.md): Quy hoạch kịch bản kiểm thử nghiệm thu người dùng (UAT) cho các luồng chống gian lận.

### 📁 06. Pháp lý & Quyền riêng tư (Compliance & Privacy)
*   [Privacy_Policy.md](./06_Compliance_and_Privacy/Privacy_Policy.md): Chính sách bảo vệ dữ liệu cá nhân (mã hóa mật khẩu, tự động làm mờ ảnh hóa đơn nhạy cảm, nguyên tắc định vị GPS).
*   [Content_Moderation_Policy.md](./06_Compliance_and_Privacy/Content_Moderation_Policy.md): Quy chế kiểm duyệt nội dung 3 lớp và các hình thức xử phạt gian lận.

### 📁 07. Vận hành & Phục hồi sự cố (Operations & Maintenance)
*   [Backup_and_Disaster_Recovery.md](./07_Operations_and_Maintenance/Backup_and_Disaster_Recovery.md): Chiến lược sao lưu tự động AWS RDS, RTO/RPO và kịch bản ứng phó khẩn cấp.

---

## 🛠️ ĐỀ XUẤT CÔNG NGHỆ CHÍNH THỨC
*   **Frontend:** Next.js v14+ (TypeScript, Tailwind CSS, Zustand, React Query)
*   **Backend:** Node.js v20+ (NestJS Framework, TypeScript, Prisma ORM, BullMQ)
*   **CSDL & Cache:** PostgreSQL (Amazon RDS) & Redis (Amazon ElastiCache)
*   **Cloud Platform:** Amazon Web Services (AWS)
