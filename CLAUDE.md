# CLAUDE.md — Hướng dẫn cho Claude Code

Repo này gồm 2 phần:

1. `index.html` — trang portfolio cá nhân (GitHub Pages).
2. `.claude/skills/` — bộ skill Claude Code hỗ trợ công việc **Business Analyst ngân hàng (VIB)**.

## Bối cảnh người dùng

- Vai trò: Business Analyst, 4+ năm kinh nghiệm, làm việc trong ngân hàng thương mại tại Việt Nam (VIB).
- Ngôn ngữ mặc định: **tiếng Việt**; thuật ngữ chuyên ngành giữ song ngữ khi cần (VD: "Hạn mức (Limit)").
- Đầu ra ưu tiên: Markdown có cấu trúc rõ ràng, bảng, sơ đồ Mermaid; xuất `.docx` / `.xlsx` khi được yêu cầu.

## Quy tắc bảo mật BẮT BUỘC (áp dụng cho mọi skill)

- **Không bao giờ** commit vào repo này dữ liệu thật của khách hàng hoặc tài liệu nội bộ VIB
  (repo là public). Chỉ dùng dữ liệu giả lập (mock data).
- Khi người dùng dán dữ liệu có thông tin cá nhân (họ tên, CCCD, số tài khoản, số thẻ, SĐT, email, địa chỉ),
  hãy **che (mask)** trong mọi đầu ra: số thẻ `9704 **** **** 1234`, CCCD `0791******45`, STK `****6789`.
- Không gợi ý gửi dữ liệu nội bộ ra dịch vụ bên ngoài.
- Khi trích dẫn văn bản pháp luật / quy định NHNN, luôn ghi số hiệu và nhắc người dùng **kiểm tra hiệu lực
  hiện hành** — không tự bịa số điều khoản.

## Danh sách skill

| Skill | Dùng khi |
|---|---|
| `vib-brd-srs` | Viết BRD / SRS / FRS cho dự án ngân hàng |
| `vib-user-story` | Viết user story, acceptance criteria (Gherkin), business rules |
| `vib-compliance-check` | Rà soát yêu cầu theo quy định NHNN, AML, bảo vệ dữ liệu cá nhân |
| `vib-process-flow` | Vẽ quy trình As-Is / To-Be (Mermaid, swimlane, maker-checker) |
| `vib-uat-testcase` | Sinh kịch bản kiểm thử UAT từ yêu cầu |
| `vib-data-mapping` | Data mapping, đặc tả API, quy tắc validate trường dữ liệu |
| `vib-sql-analysis` | Viết / giải thích SQL phân tích dữ liệu ngân hàng |
| `vib-meeting-minutes` | Biên bản họp, workshop thu thập yêu cầu, action items |
| `vib-change-impact` | Phân tích tác động Change Request |
| `vib-banking-glossary` | Tra cứu / giải thích thuật ngữ nghiệp vụ ngân hàng |

## Danh sách agent (`.claude/agents/`)

| Agent | Vai trò |
|---|---|
| `vib-brd-reviewer` | Review độc lập BRD/SRS/user story (chỉ đọc, không sửa file) |
| `vib-test-designer` | Thiết kế test case UAT, dữ liệu test, RTM từ tài liệu yêu cầu |
| `vib-regulation-researcher` | Tra cứu quy định pháp luật/NHNN mới và tác động tới yêu cầu |
