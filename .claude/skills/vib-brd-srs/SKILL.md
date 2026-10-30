---
name: vib-brd-srs
description: Viết tài liệu BRD (Business Requirements Document), SRS/FRS cho dự án ngân hàng tại VIB — sản phẩm thẻ, cho vay, tiền gửi, thanh toán, ngân hàng số (MyVIB), eKYC. Dùng khi người dùng nói "viết BRD", "viết SRS", "FRS", "tài liệu yêu cầu", "đặc tả yêu cầu", "business requirement", hoặc đưa ý tưởng/ghi chú cần chuyển thành tài liệu yêu cầu.
---

# Viết BRD / SRS cho dự án ngân hàng

## Mục tiêu
Chuyển ý tưởng, ghi chú họp, email hoặc yêu cầu thô thành tài liệu yêu cầu đầy đủ, có thể review với
Business Owner, IT, Rủi ro, Pháp chế, Tuân thủ và Vận hành.

## Quy trình làm việc

1. **Thu thập ngữ cảnh** — nếu thiếu, hỏi tối đa 5 câu ngắn gọn:
   - Sản phẩm / nghiệp vụ nào? (Thẻ, Cho vay, Huy động, Thanh toán, Digital, Treasury…)
   - Kênh nào? (MyVIB, Internet Banking, Quầy/BPM, Call center, API đối tác)
   - Đối tượng khách hàng? (KHCN, KHDN, SME, Priority)
   - Hệ thống bị ảnh hưởng? (Core banking, Card system, LOS, CRM, AML, ESB/API Gateway…)
   - Mốc thời gian / ràng buộc quy định?
2. **Không bịa thông tin** — chỗ chưa rõ đánh dấu `[CẦN XÁC NHẬN: …]` và đưa vào mục *Câu hỏi mở*.
3. **Viết tài liệu** theo template bên dưới (BRD hoặc SRS tùy yêu cầu).
4. **Tự review** trước khi trả: mỗi yêu cầu có ID, đo lường được, không mơ hồ ("nhanh", "thân thiện" → thay
   bằng số liệu cụ thể), có ưu tiên MoSCoW, có truy vết về mục tiêu nghiệp vụ.
5. Gợi ý chạy `vib-compliance-check` nếu yêu cầu chạm đến dữ liệu cá nhân, giao dịch tiền, mở tài khoản, cho vay.

## Template BRD

```markdown
# BRD — <Tên dự án>
| Mã tài liệu | Phiên bản | Ngày | Tác giả | Trạng thái |
|---|---|---|---|---|
| BRD-<MÃ>-001 | 0.1 | <dd/mm/yyyy> | <BA> | Draft |

## Lịch sử thay đổi
## 1. Tổng quan
### 1.1 Bối cảnh & vấn đề hiện tại
### 1.2 Mục tiêu nghiệp vụ (đo lường được — KPI, baseline, target)
### 1.3 Phạm vi (In-scope / Out-of-scope)
## 2. Các bên liên quan (Stakeholder & RACI)
## 3. Quy trình hiện tại (As-Is) & đề xuất (To-Be)
## 4. Yêu cầu nghiệp vụ
| ID | Yêu cầu | Mô tả chi tiết | Ưu tiên (MoSCoW) | Nguồn | Mục tiêu liên quan |
|---|---|---|---|---|---|
| BR-01 | … | … | Must | Khối Bán lẻ | OBJ-01 |
## 5. Quy tắc nghiệp vụ (Business Rules) — hạn mức, phí, lãi suất, điều kiện, phê duyệt
## 6. Yêu cầu dữ liệu & báo cáo
## 7. Yêu cầu phi chức năng (bảo mật, hiệu năng, sẵn sàng, lưu vết/audit, lưu trữ)
## 8. Yêu cầu tuân thủ & quản trị rủi ro
## 9. Giả định, ràng buộc, phụ thuộc
## 10. Rủi ro dự án
## 11. Tiêu chí nghiệm thu
## 12. Câu hỏi mở
## Phụ lục: Thuật ngữ
```

## Template SRS / FRS (bổ sung so với BRD)

- **Use case** cho từng chức năng: Actor, Tiền điều kiện, Luồng chính, Luồng thay thế, Luồng ngoại lệ, Hậu điều kiện.
- **Đặc tả màn hình**: bảng trường dữ liệu (Tên trường, Kiểu, Bắt buộc, Độ dài, Giá trị mặc định, Validate, Thông báo lỗi).
- **Đặc tả tích hợp**: hệ thống nguồn/đích, API/message, tần suất, xử lý lỗi & timeout, idempotency, đối soát.
- **Ma trận phân quyền**: vai trò × chức năng (Maker / Checker / Approver / Viewer).
- **Thông báo & mã lỗi**: mã, nội dung tiếng Việt + tiếng Anh, điều kiện hiển thị.
- **Ma trận truy vết (RTM)**: BR → FR → Test case.

## Checklist đặc thù ngân hàng (luôn rà soát)

- Hạn mức giao dịch (theo lần / ngày / tháng, theo hạng khách hàng), phí, tỷ giá, làm tròn số tiền VND.
- Xác thực giao dịch (OTP, Smart OTP, sinh trắc học theo quy định NHNN cho giao dịch trực tuyến).
- Maker–Checker, phân cấp phê duyệt, phân tách nhiệm vụ (SoD).
- Cut-off time, ngày nghỉ/lễ, xử lý cuối ngày (EOD), giao dịch treo / hoàn tiền / tra soát.
- Audit trail: ai, làm gì, lúc nào, giá trị trước/sau.
- AML/CFT: sàng lọc danh sách đen/cấm vận, giao dịch đáng ngờ, giao dịch giá trị lớn.
- Dữ liệu cá nhân: mục đích thu thập, sự đồng ý của khách hàng, che dữ liệu, thời gian lưu trữ.
- Đối soát (reconciliation) với hệ thống đối tác (Napas, tổ chức thẻ, ví điện tử…).
