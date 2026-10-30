---
name: vib-compliance-check
description: Rà soát yêu cầu, BRD, user story hoặc thiết kế sản phẩm ngân hàng theo quy định pháp luật Việt Nam và NHNN — an toàn thông tin, dịch vụ ngân hàng trực tuyến, xác thực sinh trắc học, mở tài khoản/eKYC, thẻ, phòng chống rửa tiền (AML/CFT), bảo vệ dữ liệu cá nhân. Dùng khi người dùng nói "check tuân thủ", "compliance", "quy định NHNN", "thông tư", "AML", "dữ liệu cá nhân", "PDPD", "rủi ro pháp lý", hoặc trước khi chốt BRD.
---

# Rà soát tuân thủ quy định cho yêu cầu ngân hàng

> ⚠️ Đây là công cụ hỗ trợ BA **phát hiện sớm** điểm cần lưu ý — không thay thế ý kiến chính thức của
> Khối Pháp chế & Tuân thủ. Văn bản pháp luật thay đổi thường xuyên: luôn nhắc người dùng **kiểm tra hiệu lực
> và nội dung bản gốc** (thuvienphapluat.vn, sbv.gov.vn, văn bản nội bộ VIB). Không bịa số điều/khoản;
> nếu không chắc điều khoản cụ thể, chỉ nêu tên văn bản và chủ đề cần kiểm tra.

## Quy trình

1. Đọc tài liệu/yêu cầu người dùng cung cấp; xác định **loại nghiệp vụ** và **dữ liệu** liên quan.
2. Đối chiếu với `references/regulations.md` để chọn nhóm văn bản liên quan.
3. Đi qua checklist bên dưới; với mỗi điểm, đánh giá:
   - ✅ Đã đáp ứng — trích dẫn đoạn yêu cầu chứng minh
   - ⚠️ Chưa rõ / thiếu — đề xuất yêu cầu bổ sung (viết sẵn dạng BR/NFR có ID)
   - ❌ Có thể vi phạm — giải thích và đề xuất phương án
4. Trả kết quả theo **mẫu báo cáo** dưới đây, kèm danh sách câu hỏi cần gửi Pháp chế/Tuân thủ/ATTT.

## Checklist

**A. Định danh & mở tài khoản / eKYC**
- Thu thập & xác minh thông tin định danh (CCCD gắn chip, đối chiếu CSDL quốc gia về dân cư / VNeID theo lộ trình NHNN).
- So khớp sinh trắc học khuôn mặt; chống giả mạo (liveness); hạn mức với tài khoản mở trực tuyến.
- Lưu trữ hồ sơ, video/hình ảnh eKYC; xử lý khi thông tin không khớp.

**B. Giao dịch trực tuyến & xác thực**
- Phân loại giao dịch & phương thức xác thực tương ứng (OTP, Smart OTP, sinh trắc học) theo quy định của NHNN.
- Xác thực sinh trắc học với giao dịch chuyển tiền vượt ngưỡng / lần đầu trên thiết bị mới.
- Quản lý phiên, khóa sau nhiều lần sai, đăng ký/huỷ thiết bị, cảnh báo giao dịch bất thường.
- Thông báo biến động số dư, hiển thị đầy đủ thông tin trước khi xác nhận.

**C. An toàn hệ thống thông tin**
- Phân loại cấp độ hệ thống; mã hóa dữ liệu khi truyền/lưu; quản lý khóa; log & lưu vết.
- Phân quyền tối thiểu, SoD, rà soát quyền định kỳ; bên thứ ba/thuê ngoài.

**D. Bảo vệ dữ liệu cá nhân**
- Xác định dữ liệu cơ bản vs nhạy cảm (tài chính, sinh trắc học, vị trí…).
- Mục đích xử lý, cơ sở pháp lý, **sự đồng ý** rõ ràng, có thể rút lại; thông báo xử lý dữ liệu.
- Quyền của chủ thể dữ liệu (truy cập, chỉnh sửa, xóa); thời hạn lưu trữ; chuyển dữ liệu ra nước ngoài / cho bên thứ ba.
- Đánh giá tác động xử lý dữ liệu cá nhân (DPIA) khi cần.

**E. AML/CFT**
- Nhận biết khách hàng (KYC/CDD/EDD), khách hàng có rủi ro cao, PEPs.
- Sàng lọc danh sách đen/cấm vận; báo cáo giao dịch giá trị lớn, giao dịch đáng ngờ, chuyển tiền điện tử.
- Lưu trữ hồ sơ theo thời hạn luật định.

**F. Sản phẩm cụ thể**
- Thẻ: phát hành, hạn mức, phí, khóa/mở thẻ, tra soát, giao dịch không xuất trình thẻ (CNP), 3-D Secure.
- Cho vay: điều kiện vay, mục đích vốn, giải ngân, thông tin minh bạch lãi suất/phí, phương thức trả nợ.
- Open API / đối tác: chuẩn kết nối, xác thực đối tác, sự đồng ý của khách hàng khi chia sẻ dữ liệu.

**G. Bảo vệ người tiêu dùng & minh bạch**
- Điều khoản điều kiện, biểu phí công khai, ngôn ngữ dễ hiểu; kênh khiếu nại & thời hạn xử lý tra soát.

## Mẫu báo cáo

```markdown
# Báo cáo rà soát tuân thủ — <Tên tài liệu/tính năng>
**Phạm vi rà soát:** … | **Ngày:** … | **Người rà soát:** <BA> (hỗ trợ bởi Claude)

## Tóm tắt
- Số điểm ✅: x | ⚠️: y | ❌: z
- Rủi ro chính: …

## Chi tiết
| # | Nhóm | Nội dung kiểm tra | Đánh giá | Bằng chứng / Vấn đề | Văn bản tham chiếu (cần xác minh) | Đề xuất |
|---|---|---|---|---|---|---|

## Yêu cầu bổ sung đề xuất
| ID | Yêu cầu | Loại (BR/NFR) | Ưu tiên |

## Câu hỏi gửi Pháp chế / Tuân thủ / ATTT
```
