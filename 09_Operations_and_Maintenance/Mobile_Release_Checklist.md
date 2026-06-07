# Checklist release mobile - TrustBite

| Thông tin tài liệu | Chi tiết |
|---|---|
| Loại tài liệu | Mobile release checklist |
| Phiên bản | v1.0.0 |
| Trạng thái | Bản nháp |
| Chủ sở hữu | Mobile Lead / DevOps / QA Lead |
| Ngày cập nhật | 2026-06-07 |

---

## 1. Phạm vi

Checklist dùng cho TestFlight, Google Play Internal Testing, beta và production release của mobile app TrustBite.

---

## 2. Trước khi build

- Version name và build number đã tăng.
- Environment config đúng: staging, beta hoặc production.
- API base URL đúng.
- Không có secret nhạy cảm trong app bundle.
- Feature flag đúng với phạm vi release.
- Privacy copy cho hóa đơn/GPS đã được QA kiểm tra.
- Analytics/crash reporting bật đúng environment.
- Source map/dSYM mapping được upload nếu dùng Sentry/Crashlytics.

---

## 3. Trước khi gửi beta

- 100% mobile P0 test case đạt.
- Upload hóa đơn kiểm thử trên iOS và Android.
- HEIC iOS kiểm thử nếu app hỗ trợ HEIC.
- GPS denied không chặn flow.
- Logout xóa token local.
- App mở lại được trạng thái OCR pending.
- Không log OTP/token/GPS gốc/OCR text trong crash/analytics.
- Backend staging/beta có seed data và OCR mock/provider ổn định.

---

## 4. Store metadata tối thiểu

| Nhóm | Yêu cầu |
|---|---|
| App name | TrustBite hoặc tên được phê duyệt |
| Description | Nêu rõ đánh giá có xác minh bằng hóa đơn, không hứa quá mức |
| Privacy | Khai báo số điện thoại, ảnh hóa đơn, GPS tùy chọn, analytics/crash nếu có |
| Screenshots | Chưa cần chốt visual trong tài liệu này, nhưng phải đúng chức năng release |
| Support contact | Email hoặc form support |
| Data deletion | Có hướng dẫn yêu cầu xóa tài khoản/dữ liệu |

---

## 5. Production readiness

- Backend production đã backup và monitoring.
- Rate limit OTP/upload/review bật.
- Object storage private.
- Admin portal hoạt động để xử lý case pending.
- Có người trực xử lý hàng đợi trong thời gian beta đầu.
- Rollback plan có sẵn.
- Crash dashboard và API error dashboard có người theo dõi.

---

## 6. Sau release

- Theo dõi crash-free users.
- Theo dõi OTP success rate.
- Theo dõi receipt upload success rate.
- Theo dõi verification result distribution.
- Theo dõi pending admin queue SLA.
- Triage lỗi blocker/critical hằng ngày trong beta.
