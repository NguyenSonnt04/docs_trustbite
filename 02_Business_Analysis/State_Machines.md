# Máy trạng thái - TrustBite

| Thông tin tài liệu | Chi tiết |
|---|---|
| Loại tài liệu | Máy trạng thái nghiệp vụ |
| Phiên bản | v2.2.0 |
| Trạng thái | Đang rà soát |
| Chủ sở hữu | BA / Kỹ thuật |
| Ngày cập nhật | 2026-06-07 |

---

## 1. Nguyên tắc chuẩn hóa

- Trạng thái cần admin xử lý dùng chung tên `PENDING_ADMIN_REVIEW` trên review, receipt verification, claim và moderation khi phù hợp.
- State machine được mô tả theo nhánh thực tế, không hiểu là chuỗi tuyến tính bắt buộc.
- Mọi chuyển trạng thái do quản trị viên thực hiện phải ghi audit log.
- Backend là nguồn sự thật về trạng thái. Mobile app chỉ hiển thị trạng thái từ API.

---

## 2. Trạng thái đánh giá

```text
DRAFT
→ SUBMITTED
  → VERIFIED
  → REFERENCE_ONLY
  → PENDING_ADMIN_REVIEW
      → VERIFIED
      → REFERENCE_ONLY
      → REJECTED
  → REJECTED

VERIFIED / REFERENCE_ONLY / PENDING_ADMIN_REVIEW / REJECTED
  → HIDDEN
  → DELETED
```

| Trạng thái | Ý nghĩa |
|---|---|
| DRAFT | Đánh giá đang soạn hoặc chưa gửi đầy đủ. |
| SUBMITTED | Đánh giá đã gửi, chờ xác minh hoặc lựa chọn tải hóa đơn. |
| VERIFIED | Đánh giá đủ tin cậy để tính trọng số cao. |
| REFERENCE_ONLY | Đánh giá tham khảo, trọng số thấp. |
| PENDING_ADMIN_REVIEW | Cần quản trị viên xử lý. |
| REJECTED | Không được chấp nhận để hiển thị/tính điểm theo rule. |
| HIDDEN | Bị ẩn bởi kiểm duyệt/quản trị viên. |
| DELETED | Xóa mềm theo yêu cầu của người dùng, quản trị viên hoặc quyền riêng tư. |

---

## 3. Trạng thái xác minh hóa đơn

```text
UPLOADED
→ HASH_CHECKING
  → DUPLICATE_DETECTED
      → REJECTED
  → OCR_PROCESSING
      → OCR_FAILED
          → PENDING_ADMIN_REVIEW
          → REFERENCE_ONLY
      → OCR_SUCCESS
          → VERIFIED
          → PENDING_ADMIN_REVIEW
          → REFERENCE_ONLY
          → REJECTED
```

| Trạng thái | Ý nghĩa |
|---|---|
| UPLOADED | File đã được lưu riêng tư. |
| HASH_CHECKING | Đang tính/kiểm tra hash. |
| OCR_PROCESSING | Đang OCR. |
| OCR_SUCCESS | OCR hoàn tất. |
| OCR_FAILED | OCR lỗi kỹ thuật hoặc không đọc được. |
| DUPLICATE_DETECTED | Hash trùng. |
| VERIFIED | Hóa đơn đã xác minh. |
| PENDING_ADMIN_REVIEW | Cần quản trị viên rà soát. |
| REFERENCE_ONLY | Không đủ xác minh, nhưng review có thể giữ dạng tham khảo theo rule. |
| REJECTED | Hóa đơn bị từ chối. |

---

## 4. Trạng thái quán

```text
DRAFT
→ PENDING_VERIFICATION
  → ACTIVE
  → REJECTED

ACTIVE
  → SUSPENDED
  → CLOSED

SUSPENDED
  → ACTIVE
  → CLOSED
```

| Trạng thái | Ý nghĩa |
|---|---|
| DRAFT | Quán mới tạo hoặc chưa đủ dữ liệu. |
| PENDING_VERIFICATION | Chờ xác minh dữ liệu/quyền quản lý. |
| ACTIVE | Được hiển thị công khai. |
| SUSPENDED | Tạm ngưng hiển thị hoặc hạn chế do vận hành/vi phạm. |
| CLOSED | Quán đóng cửa. |
| REJECTED | Không được chấp nhận. |

---

## 5. Trạng thái xác nhận quyền quản lý quán

```text
SUBMITTED
→ PENDING_ADMIN_REVIEW
  → APPROVED
  → REJECTED
  → CANCELLED
```

---

## 6. Trạng thái báo cáo kiểm duyệt

```text
SUBMITTED
→ TRIAGED
→ PENDING_ADMIN_REVIEW
  → ACTION_TAKEN
  → REJECTED
  → CLOSED
```

| Trạng thái | Ý nghĩa |
|---|---|
| SUBMITTED | Báo cáo mới được gửi. |
| TRIAGED | Đã phân loại sơ bộ. |
| PENDING_ADMIN_REVIEW | Chờ quản trị viên xử lý. |
| ACTION_TAKEN | Đã áp dụng hành động kiểm duyệt. |
| REJECTED | Báo cáo bị từ chối. |
| CLOSED | Đã đóng sau xử lý hoặc không cần hành động thêm. |

---

## 7. Trạng thái người dùng

```text
ACTIVE
→ REVIEW_RESTRICTED
→ SUSPENDED
→ DELETED

REVIEW_RESTRICTED
→ ACTIVE
```

---

## 8. Quy tắc chuyển trạng thái

- Đánh giá `VERIFIED` có thể chuyển `HIDDEN` nếu vi phạm nội dung.
- `HIDDEN`, `REJECTED` và `DELETED` không tính điểm tin cậy.
- Hóa đơn `REJECTED` không được chuyển `VERIFIED` trừ khi siêu quản trị override.
- `DUPLICATE_DETECTED` phải tạo fraud flag loại `DUPLICATE_RECEIPT_HASH`.
- Mọi chuyển trạng thái do quản trị viên thực hiện phải ghi audit log.
- Mobile app phải refetch trạng thái từ API khi app quay lại foreground hoặc người dùng mở lại màn hình kết quả xác minh.
