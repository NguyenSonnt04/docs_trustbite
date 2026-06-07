# Đặc tả API - TrustBite

| Thông tin tài liệu | Chi tiết |
|---|---|
| Loại tài liệu | Đặc tả API |
| Phiên bản | v1.1.0 |
| Trạng thái | Bản nháp |
| Chủ sở hữu | Trưởng nhóm Backend / Mobile Lead |

---

## 1. Quy ước API

- Base URL: `/api/v1`.
- Định dạng: JSON, UTF-8; upload hóa đơn dùng multipart hoặc signed upload flow theo `OpenAPI_Guidelines.md`.
- Auth: Bearer access token + refresh/session token, trừ public endpoints.
- IDs: UUID.
- Thời gian: ISO-8601 kèm timezone.
- API phải được chuẩn hóa thành OpenAPI 3.0/3.1 trước khi implementation production.
- Error code phải ổn định để mobile app mapping sang copy/localization.
- Status code tối thiểu theo `OpenAPI_Guidelines.md`: 200, 201, 202, 400, 401, 403, 404, 409, 413, 415, 422, 429, 500, 503.
- Phản hồi lỗi:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Comment must be at least 50 characters.",
    "details": [],
    "requestId": "req_abc123"
  }
}
```

---

## 2. Xác thực

### POST /auth/otp/request

Yêu cầu:

```json
{
  "phoneNumber": "+84901234567"
}
```

Phản hồi:

```json
{
  "requestId": "uuid",
  "expiresInSeconds": 120,
  "nextAllowedRequestAt": "2026-06-07T12:10:00+07:00"
}
```

### POST /auth/otp/verify

Yêu cầu:

```json
{
  "phoneNumber": "+84901234567",
  "otpCode": "123456"
}
```

Phản hồi:

```json
{
  "accessToken": "jwt-or-session-token",
  "user": {
    "id": "uuid",
    "displayName": "Nguyen Van A",
    "role": "USER",
    "rankCode": "NEWBIE"
  }
}
```

### POST /auth/refresh

Yêu cầu phụ thuộc auth strategy. Mobile app dùng endpoint này hoặc session refresh tương đương khi access token hết hạn.

Phản hồi:

```json
{
  "accessToken": "new-access-token",
  "expiresInSeconds": 900
}
```

### POST /auth/logout

Phản hồi:

```json
{ "success": true }
```

Ghi chú mobile: logout phải xóa token local và revoke refresh/session token nếu backend hỗ trợ.

---

## 3. Người dùng

### GET /users/me

Phản hồi:

```json
{
  "id": "uuid",
  "phoneNumber": "+84901234567",
  "displayName": "Nguyen Van A",
  "status": "ACTIVE",
  "expPoints": 120,
  "rankCode": "APPRENTICE"
}
```

### PATCH /users/me

Yêu cầu:

```json
{
  "displayName": "Tên hiển thị",
  "avatarUrl": "https://..."
}
```

---

## 4. Quán

### GET /restaurants

Tham số truy vấn:

```text
keyword, lat, lng, radiusMeters, minTrustScore, sort, page, pageSize
```

Phản hồi:

```json
{
  "items": [
    {
      "id": "uuid",
      "name": "Phở Thật 100",
      "address": "Quận 1, TP.HCM",
      "latitude": 10.776,
      "longitude": 106.700,
      "trustScore": 4.4,
      "verifiedReviewCount": 35,
      "referenceReviewCount": 12,
      "status": "ACTIVE"
    }
  ],
  "page": 1,
  "pageSize": 20,
  "total": 1
}
```

### GET /restaurants/nearby

Tham số truy vấn:

```text
northEastLat, northEastLng, southWestLat, southWestLng
```

### GET /restaurants/{restaurantId}

Response gồm hồ sơ quán, phân rã điểm đánh giá và trạng thái chủ quán.

### GET /restaurants/{restaurantId}/reviews

Tham số truy vấn:

```text
status=VERIFIED|REFERENCE_ONLY|ALL&page=1&pageSize=20
```

Phản hồi phải phân biệt rõ review `VERIFIED` và `REFERENCE_ONLY` để mobile hiển thị trust badge.

---

## 5. Đánh giá

### POST /reviews

Yêu cầu:

```json
{
  "restaurantId": "uuid",
  "foodRating": 5,
  "priceRating": 4,
  "serviceRating": 4,
  "ambienceRating": 5,
  "comment": "Đồ ăn ngon, phục vụ tốt, giá hợp lý. Không gian sạch sẽ và dễ chịu.",
  "visitedAt": "2026-06-07T12:30:00+07:00"
}
```

Phản hồi:

```json
{
  "reviewId": "uuid",
  "status": "SUBMITTED",
  "nextStep": "UPLOAD_RECEIPT"
}
```

### GET /reviews/{reviewId}

Response gồm điểm đánh giá, bình luận, trạng thái, trạng thái xác minh và media.

### PATCH /reviews/{reviewId}

Chỉ cho phép khi đánh giá ở DRAFT/SUBMITTED và chưa chốt xác minh, tùy chính sách.

### DELETE /reviews/{reviewId}

Xóa mềm theo chính sách của chủ sở hữu hoặc quản trị viên.

### POST /reviews/{reviewId}/votes

Yêu cầu:

```json
{ "voteType": "HELPFUL" }
```

### POST /reviews/{reviewId}/gps

Endpoint tùy chọn nếu GPS không gửi kèm `POST /receipts`.

Yêu cầu:

```json
{
  "latitude": 10.776,
  "longitude": 106.700,
  "accuracyMeters": 45,
  "capturedAt": "2026-06-07T12:30:00+07:00"
}
```

Phản hồi:

```json
{
  "reviewId": "uuid",
  "gpsSignalAccepted": true
}
```

---

## 6. Hóa đơn

### POST /receipts

Content-Type: `multipart/form-data`

Fields:

```text
reviewId: uuid
restaurantId: uuid
receiptImage: file
latitude: optional number
longitude: optional number
gpsAccuracyMeters: optional number
capturedAt: optional datetime
```

Phản hồi: `202 Accepted`

```json
{
  "receiptVerificationId": "uuid",
  "status": "UPLOADED",
  "processingStatus": "HASH_CHECKING"
}
```

Ghi chú: nếu mobile retry upload, backend nên hỗ trợ idempotency key hoặc upload intent để tránh tạo bản ghi trùng.

### GET /receipts/{receiptVerificationId}

Phản hồi:

```json
{
  "id": "uuid",
  "reviewId": "uuid",
  "status": "VERIFIED",
  "ocrSimilarity": 92.5,
  "gpsDistanceMeters": 80,
  "fraudRiskScore": 10,
  "decision": "AUTO_VERIFIED"
}
```

Giá trị `status` hợp lệ: `UPLOADED`, `HASH_CHECKING`, `OCR_PROCESSING`, `OCR_SUCCESS`, `OCR_FAILED`, `DUPLICATE_DETECTED`, `VERIFIED`, `PENDING_ADMIN_REVIEW`, `REFERENCE_ONLY`, `REJECTED`.

---

## 7. Kiểm duyệt

### POST /moderation/reports

Yêu cầu:

```json
{
  "entityType": "REVIEW",
  "entityId": "uuid",
  "reasonCode": "SPAM_OR_FAKE",
  "description": "Review có dấu hiệu seeding."
}
```

Phản hồi:

```json
{
  "reportId": "uuid",
  "status": "SUBMITTED"
}
```

---

## 8. Chủ quán

### POST /merchant/claims

Yêu cầu:

```json
{
  "restaurantId": "uuid",
  "businessName": "Công ty ABC",
  "evidenceUrl": "https://..."
}
```

Phản hồi:

```json
{
  "claimId": "uuid",
  "status": "SUBMITTED"
}
```

### PATCH /merchant/restaurants/{restaurantId}

Chỉ cho phép với claim đã được duyệt.

### POST /merchant/reviews/{reviewId}/reply

Yêu cầu:

```json
{
  "message": "Cảm ơn bạn đã góp ý, quán sẽ cải thiện."
}
```

---

## 9. Quản trị viên

### GET /admin/queues/receipts

Tham số truy vấn:

```text
status, minRiskScore, maxRiskScore, page, pageSize
```

### POST /admin/receipt-verifications/{id}/decision

Yêu cầu:

```json
{
  "decision": "APPROVE_VERIFIED",
  "reason": "Receipt name and timestamp match restaurant records."
}
```

Phản hồi:

```json
{
  "receiptVerificationId": "uuid",
  "newStatus": "VERIFIED",
  "reviewStatus": "VERIFIED",
  "auditLogId": "uuid"
}
```

### GET /admin/moderation/reports

### POST /admin/moderation/reports/{id}/decision

Yêu cầu:

```json
{
  "action": "HIDE_REVIEW",
  "reason": "Contains abusive language."
}
```

### POST /admin/restaurant-claims/{id}/decision

Yêu cầu:

```json
{
  "decision": "APPROVE",
  "reason": "Business ownership documents verified."
}
```

---

## 10. Thông báo

### GET /notifications

Phản hồi:

```json
{
  "items": [
    {
      "id": "uuid",
      "type": "RECEIPT_VERIFIED",
      "title": "Review của bạn đã được xác minh",
      "readAt": null,
      "createdAt": "2026-06-07T12:30:00+07:00"
    }
  ]
}
```

### PATCH /notifications/{id}/read

Phản hồi:

```json
{ "success": true }
```

---

## 11. Game hóa

### GET /gamification/me

Phản hồi:

```json
{
  "expPoints": 120,
  "rankCode": "APPRENTICE",
  "badges": []
}
```
