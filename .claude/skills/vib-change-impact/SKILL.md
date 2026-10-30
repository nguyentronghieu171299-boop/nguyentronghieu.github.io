---
name: vib-change-impact
description: Phân tích tác động của Change Request (CR) hoặc thay đổi quy định/sản phẩm lên quy trình, hệ thống, dữ liệu, báo cáo, tài liệu, đào tạo và tuân thủ trong ngân hàng; ước lượng mức độ và đề xuất phương án. Dùng khi người dùng nói "change request", "CR", "phân tích tác động", "impact analysis", "thay đổi yêu cầu", "ảnh hưởng gì", "quy định mới thì phải sửa gì".
---

# Phân tích tác động Change Request

## Quy trình
1. **Tóm tắt thay đổi**: hiện tại là gì → đề xuất đổi thành gì → lý do (nghiệp vụ / quy định / sự cố).
2. **Phân loại**: Quy định bắt buộc | Cải tiến nghiệp vụ | Sửa lỗi | Kỹ thuật. CR do quy định ⇒ ghi rõ hạn hiệu lực.
3. **Quét tác động** theo các chiều dưới đây; với mỗi chiều chấm mức Cao / Trung bình / Thấp / Không.
4. **Đề xuất phương án** (tối thiểu 2 nếu có thể) với ưu/nhược điểm, chi phí tương đối, rủi ro.
5. **Khuyến nghị** & bước tiếp theo (ai phê duyệt, cần cập nhật tài liệu nào).

## Các chiều tác động
| Chiều | Câu hỏi kiểm tra |
|---|---|
| Khách hàng & kênh | Trải nghiệm thay đổi thế nào? Kênh nào (MyVIB, quầy, call center, đối tác)? Cần thông báo khách hàng? |
| Quy trình & vận hành | Bước nào thêm/bớt? SLA? Khối lượng công việc của GDV/back-office? |
| Hệ thống | Core, thẻ, LOS, CRM, AML, ESB/API, app; cấu hình hay phải phát triển? |
| Dữ liệu | Trường mới, migration, dữ liệu lịch sử, data warehouse, báo cáo NHNN/nội bộ? |
| Tích hợp đối tác | Napas, tổ chức thẻ, CIC, bảo hiểm, fintech — có cần đối tác thay đổi? |
| Tài chính & kế toán | Phí, lãi, bút toán GL, đối soát? |
| Rủi ro & tuân thủ | Kiểm soát nào bị ảnh hưởng? Cần ý kiến Pháp chế/Tuân thủ/ATTT? |
| Tài liệu | BRD/SRS, user story, test case, quy trình nội bộ, biểu phí, T&C, FAQ |
| Đào tạo & truyền thông | Đối tượng cần đào tạo, tài liệu hướng dẫn |
| Kiểm thử | Phạm vi regression, UAT, môi trường, dữ liệu test |
| Tiến độ | Ảnh hưởng release hiện tại? Phụ thuộc? |

## Mẫu đầu ra
```markdown
# Phân tích tác động — CR-<số>: <Tiêu đề>
## 1. Mô tả thay đổi (Hiện tại → Đề xuất → Lý do)
## 2. Phân loại & mức độ khẩn cấp
## 3. Ma trận tác động
| Chiều | Mức độ | Chi tiết | Bộ phận liên quan |
## 4. Phương án
| PA | Mô tả | Ưu điểm | Nhược điểm | Nỗ lực (S/M/L) | Rủi ro |
## 5. Khuyến nghị
## 6. Danh sách tài liệu / artefact cần cập nhật
## 7. Câu hỏi mở
```
