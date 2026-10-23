# Worksheet — phần flaky test và AI triage

Module phần việc của Vỹ cho **Pipeline Postmortem**. Người tham gia đọc log API thật, lập cây giả thuyết trước, rồi hỏi AI trên cùng dữ liệu và đối chiếu. Bộ này không triển khai thêm pipeline GitHub Actions/Jenkins hoặc thay phần CI chung của nhóm.

| File / folder | Dùng cho ai |
| --- | --- |
| [api-postmortem-participant.zip](api-postmortem-participant.zip) | **Chỉ phát ZIP này** cho người tham gia; giải nén ra một workspace riêng |
| [participant/Activity_Worksheet.md](participant/Activity_Worksheet.md) | Phiếu câu hỏi, ô trả lời, lịch làm bài và prompt AI |
| [participant/data.md](participant/data.md) | Bối cảnh, metadata và console gốc của ba run được chọn |
| [participant/workflow-excerpt.yml](participant/workflow-excerpt.yml) | Trích đoạn YAML chỉ để đọc; không phải workflow hoàn chỉnh để thực thi |
| `participant/.agents/skills/ci-log-triage/` | Skill log-only, nguồn và giấy phép; dùng cho Codex hoặc cung cấp SKILL.md cho trợ lý đọc file |
| [facilitator/Runbook.md](facilitator/Runbook.md) | Chuẩn bị, lịch điều phối, gợi ý theo mức và bàn giao cho nhóm |
| [facilitator/Answer_Key.md](facilitator/Answer_Key.md) | Đáp án, số liệu và nguyên nhân; chỉ mở ở phần công bố kết quả |
| [facilitator/source.json](facilitator/source.json) | Nguồn, quy tắc trích chọn, revision và thao tác tạo packet |

## Phạm vi và cách ghép với phần chung

Dữ liệu chọn `run-02`, `run-04`, `run-06` từ batch order history v2: đủ trường hợp retry phục hồi, hết retry vẫn fail và pass lần đầu. Đây là trích đoạn phục vụ hoạt động, không phải ba run mới hoặc mẫu ngẫu nhiên để ước lượng tỷ lệ lỗi. Log được giữ nguyên giá trị và số dòng; đáp án không nằm trong packet phát.

Module dùng **khung 25 phút của đề T07**: 3 phút nhận dữ liệu, 10 phút phân tích thủ công, 5 phút hỏi AI, 4 phút công bố/thảo luận, 3 phút nêu bài học. Khi ghép phần CI của nhóm, giữ tổng 25 phút; không cộng thêm một phiên 25 phút cho mỗi công cụ. Trích đoạn YAML lấy từ workflow đo thật nhưng lược input, lời gọi test và bước organizer; đây là ranh giới dữ liệu của phần triage, không phải lỗi YAML để người học sửa.

Nhóm ghép liên kết module này vào `Activity_Worksheet.md` chung và phần dẫn nhập CI khi hoàn thiện sản phẩm nộp. Các bước dựng hoặc thay đổi CI/Jenkins do phần pipeline của nhóm phụ trách. Không đưa report, User Guide, repository EShop hoặc thư mục facilitator cho AI trước khi nhóm tham gia lưu phân tích.

## Kiểm chứng và nghiệm thu

Đã kiểm tra nội dung packet với dữ liệu nguồn, các số liệu đáp án, trích đoạn YAML, liên kết và phạm vi file trong ZIP. Đây là kiểm tra biên soạn; không được tính thành một lượt AI triage mù hoặc một nhóm khác thử làm.

- [x] Có câu hỏi, cây giả thuyết ba tầng, prompt AI, phần đối chiếu và bài học.
- [x] Dữ liệu và đáp án tách riêng; người/AI nhận cùng `data.md` và YAML.
- [x] Có lịch 25 phút, gợi ý và hướng dẫn bàn giao cho phần CI chung.
- [ ] Nhóm ghép và review worksheet chung.
- [ ] Nhóm khác thử làm trong ≤25 phút không cần hỗ trợ; ghi lại kết quả trong Runbook.

Yêu cầu gốc: brief T07 mục 7–9. Đây là module đã soạn cho phần flaky/AI, chưa tự xác nhận nghiệm thu toàn bộ G-04 của nhóm.
