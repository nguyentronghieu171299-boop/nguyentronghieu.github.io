---
name: vib-user-story
description: Viết user story, acceptance criteria dạng Gherkin (Given/When/Then), business rules và chia nhỏ epic cho backlog Jira trong dự án ngân hàng (VIB). Dùng khi người dùng nói "viết user story", "acceptance criteria", "AC", "Gherkin", "chia epic", "backlog", "refine story", hoặc cần chuyển BRD thành story cho đội Agile/Scrum.
---

# User Story & Acceptance Criteria cho ngân hàng

## Quy trình

1. Xác định **persona** cụ thể (không dùng "người dùng" chung chung):
   KHCN trên MyVIB, Giao dịch viên (GDV), Kiểm soát viên (KSV), Chuyên viên thẩm định tín dụng,
   Chuyên viên Tuân thủ/AML, Admin hệ thống, Đối tác qua API…
2. Chia epic → story theo nguyên tắc **INVEST**; ưu tiên cắt theo luồng giá trị (vertical slice),
   không cắt theo tầng kỹ thuật (FE/BE).
3. Mỗi story viết theo định dạng dưới đây. Luôn bao gồm **happy path + ít nhất 2 luồng lỗi/ngoại lệ**.
4. Tách **Business Rules** ra bảng riêng để tái sử dụng (đánh mã BR-xx) và tham chiếu trong AC.
5. Đánh dấu `[CẦN XÁC NHẬN]` cho mọi giá trị chưa chắc (hạn mức, phí, thời gian timeout…).

## Định dạng story

```markdown
### <MÃ-STORY>: <Tiêu đề ngắn>
**Là** <persona>, **tôi muốn** <hành động>, **để** <giá trị nghiệp vụ>.

**Ngữ cảnh / Ghi chú:** …
**Business rules áp dụng:** BR-01, BR-03
**Phụ thuộc:** <hệ thống / story khác>
**Ước lượng gợi ý:** <S/M/L> — lý do

#### Acceptance Criteria
Scenario 1: <happy path>
  Given …
  And …
  When …
  Then …

Scenario 2: <vượt hạn mức / lỗi validate>
Scenario 3: <timeout / hệ thống đối tác lỗi>

#### Definition of Done bổ sung
- [ ] Audit log ghi nhận đủ người thực hiện, thời gian, giá trị trước/sau
- [ ] Dữ liệu nhạy cảm được che trên UI/log
- [ ] Thông báo lỗi có cả tiếng Việt & tiếng Anh
- [ ] Đã cập nhật RTM / test case UAT
```

## Các luồng ngoại lệ hay bị bỏ sót trong ngân hàng
- Vượt hạn mức lần / ngày; số dư không đủ (tính cả phí); tài khoản bị phong tỏa / đóng / ngủ đông.
- OTP sai quá số lần cho phép, hết hạn; sinh trắc học không khớp; thiết bị chưa đăng ký.
- Timeout từ core / Napas → trạng thái "đang xử lý", cần tra soát, **không trừ tiền 2 lần** (idempotency).
- Giao dịch sau cut-off / ngày nghỉ; thay đổi tỷ giá giữa lúc báo giá và lúc xác nhận.
- Người dùng thoát app giữa chừng; mất mạng; double-click nút xác nhận.
- Trùng khớp danh sách cấm vận/AML → treo giao dịch chờ duyệt.

## Đầu ra cho Jira
Khi người dùng yêu cầu, xuất thêm bảng CSV: `Summary,Description,Acceptance Criteria,Priority,Labels,Epic Link,Story Points`
(Description/AC đặt trong dấu ngoặc kép, xuống dòng dùng `\n`).
