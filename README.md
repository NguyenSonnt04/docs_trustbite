# TRUSTBITE - RESTORING TRUST IN FOOD REVIEWS

Welcome to the official documentation repository for the **TrustBite** project ("Trust in every bite"). This repository contains the comprehensive, enterprise-standard specification documents designed and structured to guide the development, testing, and operations of the platform.

---

## SYSTEM DOCUMENTATION TREE

The documentation is organized into specialized directories corresponding to different operational domains and technical departments:

### 01. Product Management
Defines the business vision and core product requirements:
*   [Project_Charter.md](./01_Product_Management/Project_Charter.md): High-level project guidelines, roadmap, milestones, resources, and OKRs.
*   [Product_Requirements_Document_PRD.md](./01_Product_Management/Product_Requirements_Document_PRD.md): User personas, functional requirements, and prioritized feature roadmap (P0, P1, P2).

### 02. Software Engineering
System architecture designs, AWS cloud infrastructure blueprints, and DevOps automation workflows:
*   [Software_Requirements_Specification_SRS.md](./02_Software_Engineering/Software_Requirements_Specification_SRS.md): Detailed functional specs and technical constraints for core features.
*   [System_Architecture_Design.md](./02_Software_Engineering/System_Architecture_Design.md): High-level architectural patterns and database ERD overview.
*   [AWS_Cloud_Infrastructure.md](./02_Software_Engineering/AWS_Cloud_Infrastructure.md): 100% cloud-native AWS design (Lambda, ECS Fargate, WAF, Cognito, Textract).
*   [CI_CD_Pipeline.md](./02_Software_Engineering/CI_CD_Pipeline.md): Automation pipelines (Git branch model, Docker, Blue/Green zero-downtime deployment, and IaC).
*   [Tech_Stack_Specification.md](./02_Software_Engineering/Tech_Stack_Specification.md): Detailed NPM package ecosystem, Next.js v14, and Node.js (NestJS) specifications.

### 03. Security & Algorithms
The core mathematical and analytical engine of TrustBite:
*   [Anti_Fraud_Specification.md](./03_Security_Algorithms/Anti_Fraud_Specification.md): GPS validation using the Haversine formula, receipt text extraction using OCR Levenshtein distance, and weighted RestTrustScore algorithms.
*   [Gamification_Design.md](./03_Security_Algorithms/Gamification_Design.md): User reputation tiers (EXP progression), core badges, and social rank permissions.

### 04. Database Design
*   [PostgreSQL_Database_Schema.md](./04_Database_Design/PostgreSQL_Database_Schema.md): Schema specifications, relational designs, key indexes (B-Tree, GiST PostGIS), and automated rating-sync triggers.

### 05. Testing & QA
*   [Test_Plan.md](./05_Testing_and_QA/Test_Plan.md): User Acceptance Testing (UAT) scenarios designed specifically to validate the anti-fraud and gamification mechanics.

### 06. Compliance & Privacy
*   [Privacy_Policy.md](./06_Compliance_and_Privacy/Privacy_Policy.md): Data protection guidelines, receipt image masking filters, and GPS location tracking rules complying with privacy laws.
*   [Content_Moderation_Policy.md](./06_Compliance_and_Privacy/Content_Moderation_Policy.md): Three-tier content moderation pipeline, spam detection rules, and penalty guidelines for users and merchants.

### 07. Operations & Maintenance
*   [Backup_and_Disaster_Recovery.md](./07_Operations_and_Maintenance/Backup_and_Disaster_Recovery.md): Automated RDS backup strategies, PITR recovery procedures, cross-region replication, and RTO/RPO targets.

---

## TECHNICAL STACK SUMMARY
*   **Frontend:** Next.js v14+ (TypeScript, Tailwind CSS, Zustand, React Query)
*   **Backend:** Node.js v20+ (NestJS Framework, TypeScript, Prisma ORM, BullMQ)
*   **Database & Cache:** PostgreSQL (Amazon RDS) & Redis (Amazon ElastiCache)
*   **Cloud Infrastructure:** Amazon Web Services (AWS)

