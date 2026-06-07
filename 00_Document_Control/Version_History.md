# Lịch sử phiên bản tài liệu - TrustBite

| Phiên bản | Ngày | Tác giả | Tóm tắt | Trạng thái |
|---|---|---|---|---|
| v1.0.0 | 2026-06-01 | Initial Team | Bộ tài liệu ý tưởng/định hướng kỹ thuật ban đầu | Đã thay thế |
| v2.0.0 | 2026-06-07 | Rà soát tài liệu | Chuẩn hóa cấu trúc, tách MVP/V1/Tương lai, bổ sung BA/API/DB/QA docs | Đang rà soát |
| v2.1.0 | 2026-06-07 | Gộp cập nhật repo | Kéo cập nhật mới nhất từ repo, thêm sơ đồ kiến trúc và đồng bộ tài liệu hạ tầng AWS theo chuẩn MVP | Đang rà soát |
| v2.2.0 | 2026-06-07 | Rà soát mobile-first | Cập nhật định hướng mobile-first, bổ sung mobile architecture, UX mobile, analytics, OpenAPI, threat model, mobile QA và release checklist | Đang rà soát |

## Quy tắc kiểm soát thay đổi

- Mọi thay đổi P0 phải cập nhật PRD, Functional Spec, SRS, API/DB nếu bị ảnh hưởng.
- Mọi thay đổi trạng thái nghiệp vụ phải cập nhật State Machines và Test Cases.
- Mọi thay đổi API phải cập nhật API Specification và QA regression checklist.
- Tài liệu chỉ chuyển `Đã phê duyệt` sau khi PO, BA, Engineering và QA sign-off.
