# Quy tắc nghiệp vụ - TrustBite

## 1. Quy tắc xác thực

| ID | Quy tắc | Mức ưu tiên |
|---|---|---|
| BR-AUTH-001 | OTP gồm 6 chữ số và hết hạn sau 120 giây. | P0 |
| BR-AUTH-002 | Tối đa 3 lần gửi OTP trong 10 phút cho cùng số điện thoại. | P0 |
| BR-AUTH-003 | Tối đa 5 lần nhập sai OTP trước khi khóa tạm thời. | P0 |

## 2. Quy tắc quán

| ID | Quy tắc | Mức ưu tiên |
|---|---|---|
| BR-REST-001 | Chỉ restaurant ACTIVE được hiển thị công khai. | P0 |
| BR-REST-002 | Chủ quán chỉ chỉnh được quán có claim APPROVED. | P1 |
| BR-REST-003 | Quán bị SUSPENDED không nhận đánh giá mới. | P0 |

## 3. Quy tắc đánh giá

| ID | Quy tắc | Mức ưu tiên |
|---|---|---|
| BR-REV-001 | Rating mỗi tiêu chí từ 1 đến 5. | P0 |
| BR-REV-002 | Comment tối thiểu 50 ký tự. | P0 |
| BR-REV-003 | Người dùng bị restricted không được viết review. | P0 |
| BR-REV-004 | Chủ quán không được đánh giá quán của mình. | P0 |
| BR-REV-005 | Đánh giá HIDDEN/DELETED không hiển thị công khai và không tính trust score. | P0 |

## 4. Quy tắc hóa đơn

| ID | Quy tắc | Mức ưu tiên |
|---|---|---|
| BR-OCR-001 | Hóa đơn file chỉ nhận JPG, PNG, HEIC. | P0 |
| BR-OCR-002 | File tối đa 10MB. | P0 |
| BR-OCR-003 | Hash trùng bị reject/fraud flag. | P0 |
| BR-OCR-004 | OCR/GPS dùng risk scoring, không dùng rule cứng ngoại trừ duplicate hash. | P0 |
| BR-OCR-005 | Hóa đơn nên trong 48 giờ; quá hạn tăng risk. | P0 |

## 5. Quy tắc gian lận

| ID | Quy tắc | Mức ưu tiên |
|---|---|---|
| BR-FRAUD-001 | Risk score 0-30 auto verified. | P0 |
| BR-FRAUD-002 | Điểm rủi ro 31-60 chuyển quản trị viên xử lý. | P0 |
| BR-FRAUD-003 | Risk score 61-99 reference only. | P0 |
| BR-FRAUD-004 | Risk score >=100 rejected/fraud flag. | P0 |

## 6. Quy tắc quản trị viên

| ID | Quy tắc | Mức ưu tiên |
|---|---|---|
| BR-ADM-001 | Mọi quyết định của quản trị viên bắt buộc có lý do. | P0 |
| BR-ADM-002 | Mọi quyết định của quản trị viên phải ghi audit log. | P0 |
| BR-ADM-003 | Trường hợp closed không được xử lý lại trừ khi Siêu quản trị override. | P1 |

## 7. Quy tắc quyền riêng tư

| ID | Quy tắc | Mức ưu tiên |
|---|---|---|
| BR-PRIV-001 | GPS là optional. | P0 |
| BR-PRIV-002 | Hóa đơn ảnh gốc lưu private, không public trực tiếp. | P0 |
| BR-PRIV-003 | Dữ liệu nhạy cảm trên hóa đơn phải được che nếu hiển thị. | P0 |
| BR-PRIV-004 | Retention tuân thủ Data Retention Policy. | P0 |
