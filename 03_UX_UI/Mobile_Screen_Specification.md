# Đặc tả màn hình mobile - TrustBite

| Thông tin tài liệu | Chi tiết |
|---|---|
| Loại tài liệu | Mobile screen specification |
| Phiên bản | v1.0.0 |
| Trạng thái | Bản nháp |
| Chủ sở hữu | UX Lead / Product Manager |
| Ngày cập nhật | 2026-06-07 |

---

## 1. Phạm vi

Tài liệu này mô tả yêu cầu chức năng, trạng thái và nội dung cần có cho các màn hình mobile MVP. Tài liệu **không chốt màu sắc, bố cục pixel-level, typography, icon set hoặc visual style cuối cùng**.

---

## 2. Trạng thái bắt buộc cho mọi màn hình dữ liệu

Mỗi màn hình có dữ liệu từ API phải có:

- Loading state.
- Error state.
- Empty state nếu dữ liệu rỗng.
- Retry action khi phù hợp.
- Auth-required state nếu người dùng cần đăng nhập.
- Offline/network weak messaging nếu request thất bại vì kết nối.

---

## 3. Màn hình MVP

### MOB-001: Home Map/List

| Nhóm | Yêu cầu |
|---|---|
| Mục tiêu | Người dùng tìm và duyệt quán gần mình hoặc theo từ khóa. |
| Thành phần | Search input, map/list toggle hoặc bottom sheet, filter chips, restaurant cards, trust signal, verified review count. |
| State | Loading restaurants, no result, map permission denied, API error. |
| Analytics | `restaurant_search_performed`, `restaurant_detail_viewed`. |

### MOB-002: Restaurant Detail

| Nhóm | Yêu cầu |
|---|---|
| Mục tiêu | Người dùng hiểu độ tin cậy của quán và xem đánh giá. |
| Thành phần | Tên quán, địa chỉ, điểm tin cậy, số đánh giá verified/reference, CTA viết đánh giá, tab review, merchant reply nếu có. |
| State | Restaurant not found, suspended/closed, reviews empty, reviews loading. |
| Trust UX | Phải giải thích được khác biệt giữa Verified và Reference trong ngôn ngữ ngắn, dễ hiểu. |

### MOB-003: OTP Request / Verify

| Nhóm | Yêu cầu |
|---|---|
| Mục tiêu | Người dùng đăng nhập bằng số điện thoại và OTP. |
| Thành phần | Phone input, request OTP, OTP input, resend countdown, rate limit message. |
| State | OTP sent, OTP expired, wrong OTP, rate limited, network error. |
| Security | Không hiển thị OTP trong log/debug UI. |

### MOB-004: Write Review

| Nhóm | Yêu cầu |
|---|---|
| Mục tiêu | Người dùng gửi đánh giá 4 tiêu chí. |
| Thành phần | 4 rating inputs, comment field, character counter, visitedAt, media optional, submit CTA. |
| State | Draft, validation error, submitting, submit failed, submitted. |
| Validation | Rating 1-5, comment tối thiểu 50 ký tự, restaurant ACTIVE. |

### MOB-005: Receipt Upload

| Nhóm | Yêu cầu |
|---|---|
| Mục tiêu | Người dùng tải/chụp hóa đơn để xác minh đánh giá. |
| Thành phần | File picker/camera action, preview, file constraints, privacy notice, GPS optional prompt, upload progress. |
| State | Permission denied, file too large, unsupported type, uploading, upload failed, uploaded. |
| Privacy | Nói rõ ảnh gốc lưu riêng tư và GPS là tùy chọn. |

### MOB-006: Verification Result

| Nhóm | Yêu cầu |
|---|---|
| Mục tiêu | Người dùng hiểu trạng thái xác minh sau upload. |
| Thành phần | Status badge, explanation, next action, estimated/admin review note nếu pending. |
| State | Processing, verified, pending admin review, reference only, rejected. |
| Copy | Không dùng ngôn ngữ đổ lỗi; giải thích vì sao có thể cần admin rà soát. |

### MOB-007: Profile Overview

| Nhóm | Yêu cầu |
|---|---|
| Mục tiêu | Người dùng xem hồ sơ, EXP, rank và lịch sử đóng góp. |
| Thành phần | Display name, avatar, rank, EXP, review history, settings link. |
| State | Not logged in, loading, API error, no reviews. |

### MOB-008: Report Review

| Nhóm | Yêu cầu |
|---|---|
| Mục tiêu | Người dùng báo cáo đánh giá vi phạm. |
| Thành phần | Reason selector, description, evidence optional P1, submit CTA. |
| State | Submitted, duplicate report, validation error, API error. |

### MOB-009: Account Settings

| Nhóm | Yêu cầu |
|---|---|
| Mục tiêu | Người dùng quản lý tài khoản và quyền riêng tư cơ bản. |
| Thành phần | Edit profile, logout, delete account request, privacy links, permission explanation. |
| State | Save failed, logout failed, delete request submitted. |

---

## 4. Copy và trust language

- Dùng từ ngắn, rõ, tránh thuật ngữ kỹ thuật khi không cần.
- `Verified`: đánh giá có bằng chứng đủ tin cậy.
- `Reference`: đánh giá có thể hữu ích nhưng chưa đủ bằng chứng để tính trọng số cao.
- `Pending admin review`: đang được kiểm tra thủ công khi tín hiệu tự động chưa đủ rõ.
- `Rejected`: không đủ điều kiện xác minh hoặc vi phạm rule.

---

## 5. Accessibility mobile

- Touch target tối thiểu theo chuẩn platform.
- Badge màu phải có nhãn chữ.
- Form error nằm gần field liên quan.
- VoiceOver/TalkBack label cho CTA, rating input và trạng thái xác minh.
- Không phụ thuộc vào màu để truyền đạt trạng thái.
- Hỗ trợ dynamic text ở mức hợp lý cho màn hình P0.
