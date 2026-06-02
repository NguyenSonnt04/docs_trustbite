# TRUSTBITE - RESTORING TRUST IN FOOD REVIEWS

Welcome to the official documentation repository for the **TrustBite** project ("Trust in every bite"). This repository contains the comprehensive, enterprise-standard specification documents designed and structured to guide the development, testing, and operations of the platform.

---

## SYSTEM DOCUMENTATION TREE

The documentation is organized into specialized directories corresponding to different operational domains and technical departments:

### 01. Product Management
Defines the business vision and core product requirements:
*   **Project Charter**: High-level project guidelines, roadmap, milestones, resources, and OKRs.
*   **Product Requirements Document (PRD)**: User personas, functional requirements, and prioritized feature roadmap (P0, P1, P2).

### 02. Software Engineering
System architecture designs, AWS cloud infrastructure blueprints, and DevOps automation workflows:
*   **Software Requirements Specification (SRS)**: Detailed functional specs and technical constraints for core features.
*   **System Architecture & Database Design**: High-level architectural patterns and database ERD overview.
*   **AWS Cloud Infrastructure**: 100% cloud-native AWS design (Lambda, ECS Fargate, WAF, Cognito, Textract).
*   **CI/CD Pipeline & Deployment Playbook**: Automation pipelines (Git branch model, Docker, Blue/Green deployment, and IaC).
*   **Technology Stack Specification**: Detailed NPM package ecosystem, Next.js v14, and Node.js (NestJS) specifications.

### 03. Security & Algorithms
The core mathematical and analytical engine of TrustBite:
*   **Anti-Fraud Specification**: GPS validation using the Haversine formula, receipt text extraction using OCR Levenshtein distance, and weighted RestTrustScore algorithms.
*   **Gamification Design**: User reputation tiers (EXP progression), core badges, and social rank permissions.

### 04. Database Design
*   **PostgreSQL Database Schema**: Schema specifications, relational designs, key indexes (B-Tree, GiST PostGIS), and automated rating-sync triggers.

### 05. Testing & QA
*   **System Test Plan**: User Acceptance Testing (UAT) scenarios designed specifically to validate the anti-fraud and gamification mechanics.

### 06. Compliance & Privacy
*   **Privacy Policy**: Data protection guidelines, receipt image masking filters, and GPS location tracking rules complying with privacy laws.
*   **Content Moderation Policy**: Three-tier content moderation pipeline, spam detection rules, and penalty guidelines for users and merchants.

### 07. Operations & Maintenance
*   **Backup & Disaster Recovery**: Automated RDS backup strategies, PITR recovery procedures, cross-region replication, and RTO/RPO targets.

---

## TECHNICAL STACK SUMMARY
*   **Frontend:** Next.js v14+ (TypeScript, Tailwind CSS, Zustand, React Query)
*   **Backend:** Node.js v20+ (NestJS Framework, TypeScript, Prisma ORM, BullMQ)
*   **Database & Cache:** PostgreSQL (Amazon RDS) & Redis (Amazon ElastiCache)
*   **Cloud Infrastructure:** Amazon Web Services (AWS)
