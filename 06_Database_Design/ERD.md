# Sơ đồ quan hệ thực thể - TrustBite

## ERD logic

```mermaid
erDiagram
    users ||--o{ reviews : viet_danh_gia
    users ||--o{ otp_verifications : xac_thuc_otp
    users ||--o{ review_votes : binh_chon
    users ||--o{ favorites : luu_yeu_thich
    users ||--o{ merchants : lien_ket_chu_quan
    users ||--o{ audit_logs : thuc_hien_hanh_dong

    restaurants ||--o{ reviews : nhan_danh_gia
    restaurants ||--o{ restaurant_branches : co_chi_nhanh
    restaurants ||--o{ menu_items : co_menu
    restaurants ||--o{ restaurant_claims : duoc_claim
    restaurants ||--o{ favorites : duoc_yeu_thich

    merchants ||--o{ restaurant_claims : gui_claim

    reviews ||--o{ receipt_verifications : duoc_xac_minh
    reviews ||--o{ review_media : co_media
    reviews ||--o{ review_votes : nhan_binh_chon

    moderation_reports ||--o{ moderation_actions : tao_hanh_dong

    users ||--o{ notifications : nhan_thong_bao
```

## Ghi chú thiết kế chính

- `receipt_verifications` tách khỏi `reviews` để quản lý OCR/GPS/hash/rủi ro/quyết định quản trị độc lập.
- `moderation_reports` dùng `entity_type/entity_id` để có thể báo cáo đánh giá, quán hoặc nội dung của chủ quán nếu cần.
- `audit_logs` dùng entity generic để ghi mọi quyết định vận hành quan trọng.
- `device_fingerprints` là tín hiệu bảo mật tùy chọn và phải tuân thủ chính sách quyền riêng tư.
