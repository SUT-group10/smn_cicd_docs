# Hồ sơ người–AI v2 — order history

Cùng đầu vào [CI 39860438568](https://github.com/SUT-group10/eshop-sut-group10/actions/runs/39860438568), revision đo `52e400689109b5423ad04eb2bef41280a0f149ff`. Đây là bằng chứng cho chương 9–10; khi tổ chức lượt triage mới, **chỉ cấp ZIP analysis và skill**, chưa cấp các bản phân tích hoặc organizer.

| Cần đọc | Bản lưu và thông tin chính |
| --- | --- |
| Input chung | [ZIP analysis](ai-sol/api-analysis-input-39860438568-1.zip): 43 file, 10 run/19 attempt |
| Người phân tích | [Bản Vượng](human/phan-tich-order-history.md), tự thực hiện **20 phút**, hoàn thiện trước khi xem AI, theo xác nhận bàn giao |
| AI phân tích | **GPT-6.1 Sol**; [output nguyên bản](ai-sol/response.md), [prompt được cung cấp lại](ai-sol/prompt.md), [metadata](ai-sol/metadata.json) |
| Thời lượng AI | **39 giây** đến mốc chốt bằng chứng, 05:10:33–05:11:12 ngày 23/10/2026 UTC+7; chưa gồm soạn trả lời. Tổng thời gian hoàn thiện chưa xác định; không tính hệ số nhanh hơn so với 20 phút của Vượng |
| Phương pháp | Bản dùng chung trong [User Guide: SKILL.md](../../../../user_guide/assets/flaky-ai/ci-log-triage/SKILL.md), [nguồn](../../../../user_guide/assets/flaky-ai/ci-log-triage/SOURCES.md), [giấy phép](../../../../user_guide/assets/flaky-ai/ci-log-triage/LICENSE.txt); nội dung khớp bản E-06 |
| So sánh và đánh giá | [E-08](AI_vs_Human_Triage.md), [E-10](E-10.md): dẫn chứng, giá trị/noise và ba failure modes; không ghi thành ba lần AI chẩn đoán sai |
| Xác minh sau triage | [Hai batch CI](../../flaky/v2/README.md#hai-batch-ci); E-09 `awaited` 10/10 PASS không retry |
| Nguồn và hồ sơ chi tiết | [ZIP phụ trợ](../../flaky/v2/supporting-evidence.zip): source-index, README bàn giao người/AI, metadata CI và organizer; trạng thái review được lưu tại thời điểm bàn giao, không tự suy ra approval |
| Hồ sơ cart v1 | [ZIP lưu trữ v1](../../flaky/cart-v1-archive.zip); thời lượng Sol 90 giây thuộc v1, không áp dụng cho v2 |

Input, output AI, prompt, bản phân tích Vượng và JSON metadata giữ nguyên byte. Các đường dẫn tương đối trong metadata gốc mang ngữ cảnh EShop; dùng các liên kết ở bảng trên để mở bản docs. Hồ sơ có một output được bàn giao, chưa có toàn bộ tool trace phiên gốc để kiểm toán mọi tài nguyên model đã đọc.
