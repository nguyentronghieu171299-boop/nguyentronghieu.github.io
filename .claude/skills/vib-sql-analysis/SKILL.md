---
name: vib-sql-analysis
description: Viết, giải thích, tối ưu câu SQL phân tích dữ liệu ngân hàng (giao dịch, số dư, dư nợ, nợ quá hạn, khách hàng, thẻ, funnel onboarding MyVIB) và diễn giải kết quả thành insight cho báo cáo. Dùng khi người dùng nói "viết SQL", "query", "truy vấn", "phân tích dữ liệu", "báo cáo", "giải thích câu SQL", "tối ưu query", "Oracle", "SQL Server", "dashboard số liệu".
---

# SQL & phân tích dữ liệu ngân hàng

## Trước khi viết SQL
1. Hỏi (nếu chưa rõ): hệ quản trị (Oracle / SQL Server / PostgreSQL / Hive-Spark), tên bảng & cột thực tế,
   kỳ dữ liệu, định nghĩa chỉ tiêu (VD: "khách hàng active" = có giao dịch tài chính trong 30 ngày?).
2. Nếu người dùng chưa cung cấp schema, dùng tên bảng **giả định rõ ràng** và ghi chú để họ thay.

## Nguyên tắc viết
- Dùng CTE (`WITH`) đặt tên có nghĩa, chú thích tiếng Việt cho từng bước.
- Lọc theo partition/ngày dữ liệu sớm; tránh `SELECT *`; cẩn thận `JOIN` gây nhân bản dòng (fan-out).
- Số dư/dư nợ là **dữ liệu snapshot** → lấy theo ngày chốt, không cộng dồn qua nhiều ngày.
- Phân biệt ngày giao dịch (`txn_date`) vs ngày hạch toán (`posting_date`) vs ngày giá trị (`value_date`).
- Quy đổi ngoại tệ theo tỷ giá ngày chốt; ghi rõ nguồn tỷ giá.
- **Che dữ liệu cá nhân** trong kết quả mẫu: chỉ trả CIF/ID đã mã hóa hoặc 4 số cuối.
- Chỉ viết câu `SELECT` phân tích; không viết `UPDATE/DELETE/DDL` trên môi trường thật trừ khi người dùng yêu cầu rõ.

## Mẫu chỉ tiêu thường gặp
- Tăng trưởng huy động / dư nợ theo kỳ (MoM, YoY), theo sản phẩm, chi nhánh, phân khúc.
- Nhóm nợ & tỷ lệ nợ xấu (nhóm 3–5 / tổng dư nợ); roll-rate giữa các bucket DPD (0, 1–30, 31–60, 61–90, >90).
- Funnel onboarding eKYC: tải app → đăng ký → eKYC thành công → mở TK → giao dịch đầu tiên.
- Tỷ lệ giao dịch thất bại theo mã lỗi / kênh / khung giờ.
- Chi tiêu thẻ theo MCC, khách hàng active, doanh số trung bình.

## Ví dụ
```sql
-- Tỷ lệ chuyển đổi funnel eKYC theo tuần (bảng giả định: app_events)
WITH steps AS (
  SELECT user_id,
         TRUNC(event_time, 'IW')                                   AS week_start,
         MAX(CASE WHEN event_name = 'register_start' THEN 1 END)   AS s1_register,
         MAX(CASE WHEN event_name = 'ekyc_success'   THEN 1 END)   AS s2_ekyc,
         MAX(CASE WHEN event_name = 'account_opened' THEN 1 END)   AS s3_account
  FROM app_events
  WHERE event_time >= DATE '2026-07-01'
  GROUP BY user_id, TRUNC(event_time, 'IW')
)
SELECT week_start,
       COUNT(s1_register)                                         AS register,
       COUNT(s2_ekyc)                                             AS ekyc_ok,
       COUNT(s3_account)                                          AS opened,
       ROUND(COUNT(s2_ekyc)    / NULLIF(COUNT(s1_register),0)*100, 1) AS pct_ekyc,
       ROUND(COUNT(s3_account) / NULLIF(COUNT(s2_ekyc),0)*100, 1)     AS pct_open
FROM steps
GROUP BY week_start
ORDER BY week_start;
```

## Diễn giải kết quả
Sau khi người dùng dán kết quả: nêu 3–5 insight chính (con số cụ thể, so sánh), giả thuyết nguyên nhân,
câu hỏi cần kiểm chứng thêm, và gợi ý biểu đồ phù hợp.
