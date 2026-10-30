---
name: vib-data-mapping
description: Lập tài liệu data mapping giữa các hệ thống ngân hàng, đặc tả API (request/response, mã lỗi, OpenAPI), từ điển dữ liệu và quy tắc validate trường dữ liệu (CCCD, số tài khoản, số thẻ, SWIFT/BIC, số tiền VND, ngày tháng). Dùng khi người dùng nói "data mapping", "mapping field", "đặc tả API", "API spec", "interface spec", "data dictionary", "từ điển dữ liệu", "validate", "tích hợp hệ thống", "migration dữ liệu".
---

# Data mapping & đặc tả tích hợp

## 1. Bảng data mapping
| # | Trường nguồn (Hệ thống / Bảng.Cột) | Kiểu & độ dài nguồn | Trường đích | Kiểu & độ dài đích | Quy tắc chuyển đổi | Bắt buộc | Giá trị mặc định | Xử lý khi null / lỗi | Nhạy cảm (PII) | Ghi chú |
|---|---|---|---|---|---|---|---|---|---|---|

Luôn kiểm tra: không tương thích độ dài (cắt chuỗi?), mã hóa tiếng Việt (UTF-8, có dấu/không dấu),
định dạng ngày (`dd/MM/yyyy` vs ISO 8601), múi giờ (GMT+7), đơn vị tiền & số chữ số thập phân,
bảng mã danh mục (code mapping: giới tính, loại giấy tờ, tỉnh/thành sau sáp nhập đơn vị hành chính).

## 2. Đặc tả API
Với mỗi API ghi rõ: mục đích, method + endpoint, xác thực (OAuth2/mTLS/API key), header bắt buộc
(`X-Request-ID`, idempotency key, timestamp, chữ ký), request/response schema, mã lỗi, timeout & retry,
giới hạn tần suất, log & che dữ liệu, môi trường (SIT/UAT/PROD).

Bảng trường:
| Trường | Kiểu | Bắt buộc | Độ dài | Mô tả | Ví dụ | Validate |
|---|---|---|---|---|---|---|

Bảng mã lỗi:
| HTTP | Mã lỗi | Thông điệp (VI) | Message (EN) | Hành động phía client |
|---|---|---|---|---|

Khi được yêu cầu, sinh thêm **OpenAPI 3.0 YAML**. Nếu người dùng muốn đưa vào Postman, gợi ý dùng skill Postman.

## 3. Quy tắc validate thường gặp (kiểm tra lại với quy chuẩn nội bộ)
| Trường | Quy tắc gợi ý |
|---|---|
| CCCD | 12 chữ số `^\d{12}$`; 3 số đầu là mã tỉnh nơi đăng ký khai sinh |
| CMND cũ | 9 chữ số (chỉ còn giá trị lịch sử) |
| Số điện thoại di động VN | `^(0|\+84)(3|5|7|8|9)\d{8}$` |
| Số thẻ | 16–19 chữ số, kiểm tra Luhn; BIN nội địa Napas bắt đầu `9704` |
| SWIFT/BIC | `^[A-Z]{6}[A-Z0-9]{2}([A-Z0-9]{3})?$` |
| Số tiền VND | Số nguyên, > 0, không phần thập phân; hiển thị phân cách hàng nghìn |
| Mã số thuế DN | 10 chữ số, hoặc 13 ký tự dạng `XXXXXXXXXX-XXX` cho chi nhánh |
| Email | RFC 5322 rút gọn; chuyển về chữ thường |
| Họ tên | Chữ cái tiếng Việt có/không dấu, khoảng trắng; chuẩn hóa in hoa không dấu khi gửi Napas/tổ chức thẻ |

## 4. Migration dữ liệu
Bổ sung: quy tắc làm sạch, khử trùng lặp khách hàng (CIF), đối soát số lượng & tổng số dư trước/sau,
kế hoạch rollback, mẫu báo cáo ngoại lệ.
