# 🚀 CI/CD PIPELINE & DEPLOYMENT PLAYBOOK
## DỰ ÁN: PLATFORM ĐÁNH GIÁ ẨM THỰC TIN CẬY - TRUSTBITE

| Thông tin tài liệu | Chi tiết |
| :--- | :--- |
| **Loại tài liệu** | CI/CD Pipeline & DevOps Automation Playbook |
| **Phiên bản** | v1.0.0 (Enterprise Standard) |
| **Tác giả** | Lead DevOps / Cloud Infrastructure Engineer |
| **Trạng thái** | Đã phê duyệt (Approved) |

---

## 1. MÔ HÌNH PHÁT TRIỂN NHÁNH GIT (GIT BRANCHING MODEL)

Hệ thống áp dụng mô hình **Trunk-Based Development** kết hợp với các nhánh tính năng ngắn hạn (Short-lived Feature Branches) để đảm bảo tốc độ tích hợp nhanh và liên tục:

```text
[ Nhánh main ] <─────────────────────────────────────── [ Deploy to Production ]
      ▲
      │ (Pull Request approved + CI Green)
[ Nhánh staging ] <─────────────────────────── [ Deploy to Staging & QA testing ]
      ▲
      │ (Merge Feature)
[ Nhánh feature/F-01 ] ─── (Code & Local Test)
```

*   **`feature/*`:** Nhánh phát triển tính năng độc lập bởi developer.
*   **`staging`:** Nhánh tích hợp dữ liệu, tự động deploy lên môi trường Staging (QA kiểm thử).
*   **`main`:** Nhánh ổn định tuyệt đối, tự động deploy lên môi trường Production (cho người dùng thật).

---

## 2. QUY TRÌNH TỰ ĐỘNG HÓA TÍCH HỢP & TRIỂN KHAI (CI/CD PIPELINE)

Hệ thống sử dụng **AWS CodePipeline** kết hợp **AWS CodeBuild** để tự động hóa 100% quá trình từ lúc Dev đẩy code lên GitHub đến lúc hệ thống chạy trên môi trường thực tế:

```mermaid
graph LR
    Push[Dev push code to GitHub] --> CI[1. CONTINUOUS INTEGRATION]
    
    subgraph CI Pipeline (AWS CodeBuild)
        CI --> Lint[Linting & Code Style]
        Lint --> Test[Unit & Integration Tests]
        Test --> Build[Docker Build & Push to Amazon ECR]
    end
    
    Build --> CD[2. CONTINUOUS DEPLOYMENT]
    
    subgraph CD Pipeline (AWS CodeDeploy)
        CD --> DeployLambda[Deploy Serverless Lambda Functions]
        CD --> DeployECS[Deploy ECS Container on Fargate]
    end
    
    DeployECS --> Verified[3. Zero-Downtime Rollout & Health Check]
```

### 2.1. Giai đoạn Tích hợp Liên tục (Continuous Integration - CI)
Mỗi khi có Pull Request được tạo vào nhánh `staging` hoặc `main`:
1.  **Code Quality Check:** Tự động chạy ESLint/Prettier để kiểm tra chất lượng và phong cách viết code.
2.  **Automated Testing:** Tự động chạy toàn bộ Unit Tests và Integration Tests. Nếu có bất kỳ test case nào thất bại, pipeline sẽ bị chặn (Block) không cho phép merge.
3.  **Build & Package:** 
    *   Đối với Backend chính (ECS): Đóng gói ứng dụng vào **Docker Container**. Gắn thẻ tag (phiên bản) và đẩy ảnh (image) lên **Amazon ECR (Elastic Container Registry)**.
    *   Đối với Frontend tĩnh: Build mã nguồn React/NextJS thành các tệp tin HTML/CSS/JS tĩnh.

### 2.2. Giai đoạn Triển khai Liên tục (Continuous Deployment - CD)
Khi code được merge thành công vào nhánh chính:
*   **Frontend Deploy:** Tự động đẩy toàn bộ tệp tĩnh mới lên **Amazon S3 Bucket** và gửi lệnh **CloudFront Invalidation** để xóa bộ nhớ đệm cũ trên toàn thế giới, giúp người dùng nhận giao diện mới ngay lập tức.
*   **Backend Lambda Deploy:** Cập nhật mã nguồn hàm Lambda mới thông qua công cụ **AWS SAM (Serverless Application Model)**.
*   **Backend ECS Deploy (Fargate):** Thực hiện cơ chế **Blue/Green Deployment (Rolling Update)**:
    1.  Dựng cụm Container mới (Green) song song cụm Container cũ đang chạy (Blue).
    2.  Tiến hành **Health Check (Kiểm tra trạng thái)** cụm mới.
    3.  Nếu cụm mới phản hồi ổn định (HTTP 200 OK), AWS API Gateway sẽ tự động chuyển hướng dần 100% lưu lượng truy cập sang cụm mới (Green) và tắt cụm cũ (Blue).
    4.  👉 *Tác dụng:* **Zero-Downtime Deployment** - Người dùng hoàn toàn không bị gián đoạn hay mất kết nối khi hệ thống nâng cấp phiên bản mới.

---

## 3. HẠ TẦNG DƯỚI DẠNG MÃ NGUỒN (INFRASTRUCTURE AS CODE - IAC)

Để đảm bảo môi trường phát triển (Dev), kiểm thử (Staging) và chạy thật (Production) giống nhau 100%, toàn bộ hạ tầng AWS của TrustBite được định nghĩa bằng mã nguồn **Terraform** hoặc **AWS CloudFormation**. 

Mọi thay đổi về phần cứng (tăng cấu hình RDS, thêm RAM cho container) đều phải tạo Pull Request duyệt qua mã IaC, không thao tác thủ công bằng tay trên giao diện web của AWS.
