# Hướng dẫn OpenAPI và API contract - TrustBite

| Thông tin tài liệu | Chi tiết |
|---|---|
| Loại tài liệu | API contract guidelines |
| Phiên bản | v1.0.0 |
| Trạng thái | Bản nháp |
| Chủ sở hữu | Backend Lead / Mobile Lead |
| Ngày cập nhật | 2026-06-07 |

---

## 1. Mục tiêu

API của TrustBite phải đủ rõ để mobile, admin web, backend và QA làm việc song song. `API_Specification.md` là tài liệu đọc chính; OpenAPI 3.0/3.1 phải được tạo trước khi implementation production bắt đầu.

---

## 2. Quy ước chung

| Nhóm | Quy ước |
|---|---|
| Base URL | `/api/v1` |
| Format | JSON UTF-8, riêng upload dùng multipart hoặc signed URL flow |
| ID | UUID |
| Time | ISO-8601 kèm timezone |
| Auth | Bearer access token, refresh/session token theo auth strategy |
| Pagination | `page`, `pageSize`, `total` cho MVP; cursor có thể dùng khi dữ liệu lớn |
| Error | Một format duy nhất cho toàn API |
| Idempotency | Bắt buộc cho mutation có rủi ro tạo trùng nếu mobile retry |

---

## 3. Status code bắt buộc

| Code | Dùng khi |
|---:|---|
| 200 | GET/PATCH/POST xử lý thành công nhưng không tạo resource mới |
| 201 | Tạo resource mới thành công |
| 202 | Nhận request bất đồng bộ, ví dụ upload hóa đơn đã nhận và OCR đang xử lý |
| 400 | Request sai format hoặc thiếu field |
| 401 | Chưa đăng nhập hoặc token hết hạn/không hợp lệ |
| 403 | Đã đăng nhập nhưng thiếu quyền |
| 404 | Resource không tồn tại hoặc không được phép thấy |
| 409 | Xung đột nghiệp vụ, ví dụ report trùng, claim trùng, duplicate receipt hash |
| 413 | File quá dung lượng |
| 415 | File type không hỗ trợ |
| 422 | Validation nghiệp vụ thất bại |
| 429 | Rate limit |
| 500 | Lỗi hệ thống |
| 503 | Provider ngoài hoặc worker/service tạm không sẵn sàng |

---

## 4. Error response chuẩn

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Comment must be at least 50 characters.",
    "details": [
      {
        "field": "comment",
        "code": "MIN_LENGTH",
        "message": "Minimum length is 50."
      }
    ],
    "requestId": "req_abc123"
  }
}
```

Error code phải ổn định để mobile mapping sang copy/localization.

---

## 5. Error code MVP

| Code | Ý nghĩa |
|---|---|
| `VALIDATION_ERROR` | Request không đạt schema hoặc business validation |
| `AUTH_REQUIRED` | Cần đăng nhập |
| `TOKEN_EXPIRED` | Access token hết hạn |
| `FORBIDDEN` | Thiếu quyền |
| `NOT_FOUND` | Không tìm thấy resource |
| `RATE_LIMITED` | Vượt giới hạn tần suất |
| `OTP_INVALID` | OTP sai |
| `OTP_EXPIRED` | OTP hết hạn |
| `FILE_TOO_LARGE` | File vượt dung lượng |
| `UNSUPPORTED_FILE_TYPE` | File type không hỗ trợ |
| `DUPLICATE_RECEIPT_HASH` | Hóa đơn trùng hash |
| `REVIEW_NOT_EDITABLE` | Review không còn được sửa |
| `RESTAURANT_NOT_ACTIVE` | Quán không ở trạng thái ACTIVE |
| `CLAIM_CONFLICT` | Claim quán xung đột |
| `REPORT_DUPLICATE` | Báo cáo trùng |
| `PROVIDER_UNAVAILABLE` | OCR/SMS/provider ngoài lỗi tạm thời |

---

## 6. Upload hóa đơn

MVP có thể chọn một trong hai flow, nhưng phải chốt trước implementation.

### Phương án A: Multipart API

```text
POST /receipts
Content-Type: multipart/form-data
```

Ưu điểm: đơn giản cho MVP. Nhược điểm: API chịu tải upload trực tiếp.

### Phương án B: Signed upload

```text
POST /receipts/upload-intents
PUT signedUrl
POST /receipts/{id}/complete
```

Ưu điểm: scale tốt hơn. Nhược điểm: nhiều bước hơn, mobile implementation phức tạp hơn.

### Quy tắc chung

- Backend luôn validate lại file type và size.
- Mobile retry phải dùng idempotency key hoặc upload intent id.
- API trả `202` nếu OCR xử lý bất đồng bộ.
- Response phải có `receiptVerificationId`, `status`, `processingStatus` và polling endpoint.

---

## 7. Auth lifecycle cho mobile

- Access token thời gian ngắn.
- Refresh/session token lưu trong secure storage.
- Logout phải revoke session nếu backend hỗ trợ.
- Khi tài khoản bị khóa, refresh phải thất bại và app đưa người dùng về auth/session expired state.
- Không lưu token trong log, analytics hoặc crash breadcrumb.

---

## 8. OpenAPI Definition of Done

Một endpoint chỉ được xem là sẵn sàng dev khi có:

- request schema,
- response schema thành công,
- error response có status code và error code,
- auth requirement,
- permission requirement,
- rate limit nếu có,
- ví dụ request/response,
- test case QA liên quan,
- mapping tới user story/feature code.
