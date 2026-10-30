---
name: vib-uat-testcase
description: Sinh kịch bản kiểm thử UAT (test case, test scenario, dữ liệu test) từ BRD, user story hoặc acceptance criteria cho hệ thống ngân hàng; xuất bảng Markdown hoặc file Excel/CSV. Dùng khi người dùng nói "viết test case", "UAT", "kịch bản kiểm thử", "test scenario", "test data", "nghiệm thu", "RTM", "ma trận truy vết".
---

# Sinh test case UAT cho nghiệp vụ ngân hàng

## Quy trình
1. Đọc yêu cầu đầu vào, liệt kê các **requirement ID** (BR/FR/US) cần phủ.
2. Với mỗi yêu cầu, áp dụng kỹ thuật thiết kế test:
   - **Phân vùng tương đương & giá trị biên** — hạn mức, số tiền, tuổi khách hàng, kỳ hạn.
   - **Bảng quyết định** — quy tắc phí, lãi suất, điều kiện phê duyệt nhiều biến.
   - **Chuyển trạng thái** — vòng đời hồ sơ/thẻ/giao dịch.
   - **Luồng ngoại lệ** — timeout, mất kết nối, đối tác lỗi, dữ liệu sai định dạng.
   - **Phân quyền** — đúng vai trò được phép, sai vai trò bị chặn, maker không tự duyệt.
3. Sinh test case theo bảng bên dưới, ưu tiên (High/Medium/Low) theo rủi ro tài chính & tuân thủ.
4. Tạo **dữ liệu test giả lập** (không dùng dữ liệu khách hàng thật).
5. Kết thúc bằng **RTM** (Requirement → Test case) và thống kê độ phủ.

## Định dạng test case
| TC ID | Req ID | Tiêu đề | Tiền điều kiện | Dữ liệu test | Các bước | Kết quả mong đợi | Ưu tiên | Loại (Positive/Negative/Boundary) |
|---|---|---|---|---|---|---|---|---|

- "Các bước" đánh số `1. … 2. …`; "Kết quả mong đợi" phải kiểm chứng được: số dư thay đổi bao nhiêu,
  bút toán hạch toán gì, thông báo hiển thị chính xác nội dung nào, bản ghi audit log nào.

## Điểm kiểm tra bắt buộc với giao dịch tài chính
- Số dư trước/sau, phí, VAT trên phí, làm tròn; bút toán Nợ/Có đúng tài khoản GL.
- Không trừ tiền 2 lần khi retry/double-click; hoàn tiền khi giao dịch thất bại.
- Thông báo (SMS/OTT/push/email) đúng nội dung và che thông tin nhạy cảm.
- Hạn mức ngày được reset đúng thời điểm; giao dịch qua ngày/cut-off.
- Báo cáo/đối soát cuối ngày phản ánh đúng giao dịch.

## Dữ liệu test giả lập gợi ý
- Họ tên: `NGUYEN VAN TEST`, CCCD: `001099000001` (12 chữ số), SĐT: `0900000001`.
- Số tiền biên: hạn mức − 1, = hạn mức, hạn mức + 1; số tiền 0, âm, chứa ký tự, số thập phân với VND.

## Xuất Excel
Nếu người dùng yêu cầu file, dùng skill `xlsx` (nếu có) hoặc Python `openpyxl` để tạo file với các sheet:
`Test Cases`, `Test Data`, `RTM`, `Summary`; đóng băng hàng tiêu đề, bật filter, cột ưu tiên tô màu.
