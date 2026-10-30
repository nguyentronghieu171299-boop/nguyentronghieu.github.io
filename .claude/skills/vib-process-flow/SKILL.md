---
name: vib-process-flow
description: Vẽ và phân tích quy trình nghiệp vụ ngân hàng As-Is / To-Be bằng Mermaid (flowchart swimlane, sequence diagram, state diagram) — mở tài khoản, giải ngân, phát hành thẻ, chuyển tiền, tra soát, phê duyệt maker-checker. Dùng khi người dùng nói "vẽ quy trình", "process flow", "BPMN", "swimlane", "sequence diagram", "luồng nghiệp vụ", "As-Is", "To-Be", "state machine trạng thái".
---

# Vẽ quy trình nghiệp vụ ngân hàng

## Chọn loại sơ đồ
| Mục đích | Loại Mermaid |
|---|---|
| Luồng công việc giữa các phòng ban / vai trò | `flowchart LR` với `subgraph` làm swimlane |
| Tương tác giữa hệ thống (App → API Gateway → Core → Napas) | `sequenceDiagram` |
| Vòng đời trạng thái (hồ sơ vay, thẻ, giao dịch) | `stateDiagram-v2` |
| Cấu trúc dữ liệu | `erDiagram` |
| Lộ trình dự án | `gantt` |

## Quy trình
1. Liệt kê: actor/vai trò, hệ thống, sự kiện bắt đầu, sự kiện kết thúc, điểm quyết định, SLA.
2. Vẽ **As-Is** trước (nếu có), đánh dấu điểm đau bằng `:::pain` (nút màu đỏ) và ghi chú lý do.
3. Vẽ **To-Be**, chỉ rõ bước được tự động hóa / loại bỏ.
4. Bổ sung bảng mô tả bước: `Bước | Vai trò | Hệ thống | Mô tả | Đầu vào | Đầu ra | SLA | Kiểm soát`.
5. Tóm tắt khác biệt As-Is vs To-Be và lợi ích định lượng (giảm số bước, giảm thời gian xử lý…).

## Nguyên tắc
- Mỗi điểm quyết định phải có **đủ nhánh** (Đạt / Không đạt / Cần bổ sung hồ sơ).
- Luôn thể hiện: maker–checker, nhánh từ chối, nhánh timeout/lỗi hệ thống, điểm sàng lọc AML nếu có.
- Nhãn tiếng Việt ngắn gọn, đặt trong dấu ngoặc kép để tránh lỗi ký tự: `A["Kiểm tra CIC"]`.
- Không đưa tên người thật, chỉ dùng vai trò.

## Mẫu swimlane
```mermaid
flowchart LR
  classDef pain fill:#fde2e1,stroke:#c0392b
  subgraph KH["Khách hàng"]
    A(["Bắt đầu: Nộp hồ sơ vay trên MyVIB"])
  end
  subgraph RM["Chuyên viên QHKH"]
    B["Kiểm tra đủ hồ sơ"]
  end
  subgraph TD["Thẩm định"]
    C{"Đạt điều kiện?"}
    D["Lập tờ trình"]
  end
  subgraph PD["Cấp phê duyệt"]
    E{"Phê duyệt?"}
  end
  A --> B --> C
  C -- "Đạt" --> D --> E
  C -- "Thiếu hồ sơ" --> B
  C -- "Không đạt" --> X(["Từ chối & thông báo KH"])
  E -- "Duyệt" --> F(["Giải ngân"])
  E -- "Không duyệt" --> X
```

## Mẫu sequence (chuyển tiền nhanh)
```mermaid
sequenceDiagram
  autonumber
  actor KH as Khách hàng
  participant App as MyVIB
  participant GW as API Gateway
  participant Core as Core Banking
  participant NP as Napas
  KH->>App: Nhập thông tin chuyển tiền
  App->>GW: Kiểm tra tên người nhận
  GW->>NP: Truy vấn tên
  NP-->>App: Tên chủ tài khoản
  KH->>App: Xác nhận + sinh trắc học/OTP
  App->>Core: Hạch toán (idempotency key)
  Core->>NP: Lệnh chuyển
  alt Thành công
    NP-->>Core: OK
    Core-->>App: Thành công
  else Timeout
    Core-->>App: Đang xử lý → tra soát tự động
  end
```
