# Minh chứng flaky test v2 — order history

Hồ sơ cho chương 8 của report, lưu từ repository `SUT-group10/eshop-sut-group10` ngày 23/10/2026. Revision lưu hồ sơ là `713a56bfec82196298edfba60664073085b62de0`; **revision đo từng batch** ở bảng dưới.

## Hai batch CI

| Hồ sơ | Run / revision đo | Kết quả |
| --- | --- | --- |
| E-03 — trước sửa | [39860438568](https://github.com/SUT-group10/eshop-sut-group10/actions/runs/39860438568), `52e400689109b5423ad04eb2bef41280a0f149ff` | `timed`, 10 run, retry tối đa 2; lần đầu 5 PASS/5 FAIL; 3 phục hồi; cuối 8 PASS/2 FAIL; 19 attempt, 9 retry, 11 attempt fail; 0 evidence error |
| E-09 — xác nhận bản sửa | [39911447981](https://github.com/SUT-group10/eshop-sut-group10/actions/runs/39911447981), `13e50679ef6fa9261dfe933babf62a4a5722fb86` | `awaited`, 10 run, retry=0; 10/10 PASS ngay lần đầu; mọi POST hoàn tất trước GET history; 0 evidence error |

Runtime ghi nhận: Node 24.14.0, Jest 30.5.2, Python 3.10.12, Linux/runner Ubuntu 22.04. Hai batch có seed, revision và thời điểm riêng; không gộp thành một mẫu hoặc dùng để tính riêng tác động của retry/so tốc độ. Mốc monotonic chỉ so trong cùng run.

## Mở bằng chứng nào?

| Cần kiểm tra | File |
| --- | --- |
| Thiết kế, oracle, fixture và quy tắc retry | [Protocol](order-history-protocol.md) |
| Dữ liệu chung cho người/AI trước sửa | [ZIP analysis](../../triage/v2/ai-sol/api-analysis-input-39860438568-1.zip): 10 run/19 attempt, giữ nguyên gói gốc |
| Số liệu CI, log gốc và kiểm tra sau sửa | [supporting-evidence.zip](supporting-evidence.zip): `flaky/v2/e03-ci/` và `flaky/v2/e09-ci/`, gồm `batch.json`, README chi tiết, job logs và artifact organizer; E-09 có cả ZIP analysis sau sửa |
| Nguồn sao chép và trạng thái PR lúc bàn giao | Trong cùng ZIP: `flaky/v2/source-index.json`; đường dẫn trong index là cấu trúc trước khi gom hồ sơ |
| Hình và cách tái tạo | [Timeline](order-history-timeline.png); script `flaky/v2/draw_figures.py` trong ZIP, dùng matplotlib/Pillow và font Segoe UI trên Windows |
| Phân tích người–AI, thời lượng và failure modes | [Hồ sơ triage v2](../../triage/v2/README.md) |
| Thí nghiệm cart cũ | [cart-v1-archive.zip](../cart-v1-archive.zip): toàn bộ hồ sơ/hình/script v1; không cộng kết quả này vào v2 |

ZIP phụ trợ giữ nguyên byte của 13 file đã gom. Đường dẫn bên trong tính từ `report/assets/`; giải nén theo cây này vào **bản sao repo docs** nếu cần mở liên kết cũ hoặc tái tạo hình. Organizer/source và kết luận không cấp cho người/AI trước triage; chỉ phát ZIP analysis và skill.

Lưu ý thời gian E-03: **13.418 giây** là cửa sổ các process Jest, **13.441 giây** là collector, **104 giây** là job; khoảng workflow `run_started_at → updated_at` là 109 giây, không phải trường completed_at chuyên biệt. `batch.json` và organizer trong ZIP giữ mốc nguồn. Việc đóng gói hồ sơ không tạo run hoặc kết quả AI mới.
