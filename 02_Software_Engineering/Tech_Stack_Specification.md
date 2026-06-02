# 💻 TECHNOLOGY STACK & ECOLOGICAL SPECIFICATION
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | Technology Stack & Ecological Specification |
| **Phiên bản** | v1.0.0 (Enterprise Standard) |
| **Tác giả** | Lead Architect / Principal Engineer |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. PHÂN HỆ FRONTEND (UI LAYER): NEXT.JS NATIVE

Hệ thống Frontend được phát triển trên nền tảng **Next.js v14+ (App Router)** chạy bằng **TypeScript** để tối ưu hóa SEO và cung cấp trải nghiệm giao diện người dùng mượt mà nhất.

### 1.1. Khung phát triển chính (Core Framework)
*   **Framework:** **Next.js v14+** (Sử dụng cơ chế Server Components để tối ưu hóa tốc độ tải trang và SSR cho SEO).
*   **Ngôn ngữ:** **TypeScript v5+** (Đảm bảo kiểm soát chặt chẽ kiểu dữ liệu từ compile-time).
*   **Styling (Giao diện):** **Tailwind CSS v3+** kết hợp **CSS Modules** để xây dựng các Component linh hoạt, tái sử dụng cao.
*   **Quản lý trạng thái (State Management):** **Zustand** (Nhẹ, hiệu năng cao hơn Redux cho các ứng dụng vừa và lớn) hoặc **React Context** cho các trạng thái UI đơn giản.

### 1.2. Các thư viện quan trọng (NPM Packages)
*   **Định tuyến & Gọi API:** **TanStack Query (React Query) v5** để tối ưu hóa việc cache dữ liệu gọi từ API Backend, tự động gọi lại (refetch) và đồng bộ trạng thái dữ liệu.
*   **Xử lý Form & Validation:** **React Hook Form** kết hợp với **Zod** để kiểm tra và xác thực dữ liệu nhập liệu từ các ô Input (ví dụ: kiểm tra SĐT hợp lệ, bình luận đủ 50 ký tự) ngay tại Frontend.
*   **Icons & Components:** **Lucide React** (Bộ icon hiện đại) và **Framer Motion** (Cho các hiệu ứng micro-animations mượt mà, drag-and-drop cho phần quét hóa đơn).

---

## 2. PHÂN HỆ BACKEND (API LAYER): NODE.JS & NESTJS

Backend được xây dựng theo kiến trúc **Modular Architecture** sử dụng framework **NestJS** chạy trên nền **Node.js LTS** để đảm bảo tính mở rộng và khả năng bảo trì lâu dài.

### 2.1. Khung phát triển chính (Core Framework)
*   **Runtime:** **Node.js LTS (v20+)** – Đảm bảo tính ổn định và bảo mật cao nhất.
*   **Framework:** **NestJS v10+** (Framework hướng đối tượng cho Node.js, tích hợp sẵn Dependency Injection, giúp chia nhỏ code thành các Module: AuthModule, ReviewModule, MerchantModule, GameModule rất khoa học).
*   **Ngôn ngữ:** **TypeScript v5+**.

### 2.2. Các thư viện quan trọng (NPM Packages)
*   **Kết nối CSDL (ORM):** **Prisma ORM** (Công cụ ORM hiện đại nhất cho Node.js, tự động sinh các kiểu dữ liệu TypeScript từ database schema, truy vấn PostgreSQL cực kỳ an toàn và tránh lỗi n+1 query).
*   **Bảo mật & Mã hóa:** 
    *   **bcrypt:** Dùng để băm mật khẩu người dùng với hệ số muối (salt rounds) bằng 10.
    *   **jsonwebtoken (JWT):** Tạo và xác thực các JSON Web Tokens phục vụ định danh phiên hoạt động bảo mật.
*   **Tích hợp AWS Cloud:** **@aws-sdk/client-s3**, **@aws-sdk/client-cognito-identity-provider**, **@aws-sdk/client-textract** để giao tiếp với các dịch vụ lưu trữ, định danh và quét hóa đơn AI của AWS.
*   **Hàng đợi & Xử lý bất đồng bộ:** **BullMQ** kết hợp với **Redis** để đẩy các tác vụ nặng (như gửi ảnh đi phân tích OCR) vào hàng đợi ngầm, tránh làm nghẽn luồng xử lý API chính của người dùng.

---

## 3. MÔI TRƯỜNG PHÁT TRIỂN & TIÊU CHUẨN ĐỒNG BỘ (DEVELOPMENT TOOLING)

Để đảm bảo toàn bộ đội ngũ lập trình viên viết code đồng quán và không gặp lỗi "chạy được trên máy tôi nhưng lỗi trên máy staging", hệ thống quy chuẩn các công cụ:

*   **Quản lý Package:** **pnpm** (Tốc độ cài đặt nhanh hơn npm/yarn và tiết kiệm dung lượng ổ đĩa tối đa nhờ cơ chế hard link).
*   **Quy chuẩn viết code:** **ESLint** (Quy tắc cấu trúc code) kết hợp **Prettier** (Tự động format căn lề code). Thiết kế sẵn file `.eslintrc.json` và `.prettierrc` trong thư mục dự án.
*   **Công cụ chạy cục bộ (Docker):** Sử dụng **Docker Compose** để khởi tạo nhanh môi trường PostgreSQL và Redis cục bộ trên máy của Developer chỉ bằng một dòng lệnh (`docker-compose up -d`), giúp tiết kiệm thời gian cấu hình môi trường.
