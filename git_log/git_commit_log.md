# Git commit log — Nhóm 10

## 1. Phạm vi thống kê

Bản tổng hợp ngày **23/10/2026**, chốt theo lịch sử nhánh **`main`** của hai repository tại các SHA bên dưới. Danh sách bao gồm toàn bộ commit có thể truy ngược từ từng mốc, kể cả commit trên nhánh đã được merge vào `main`; không cộng lại một SHA nhiều lần trong cùng repository. Commit chưa được tích hợp vào các mốc này và thay đổi chưa commit không thuộc thống kê.

| Repository | SHA chốt trên `main` | Tổng commit |
| --- | --- | ---: |
| [Mã nguồn EShop](https://github.com/SUT-group10/eshop-sut-group10) | [`89c667692ad1b39d9409a9998a1774803213c8e7`](https://github.com/SUT-group10/eshop-sut-group10/commit/89c667692ad1b39d9409a9998a1774803213c8e7) | 61 |
| [Tài liệu seminar](https://github.com/SUT-group10/smn_cicd_docs) | [`f863ae8856d7db05192d9a39ba8a417568be6f47`](https://github.com/SUT-group10/smn_cicd_docs/commit/f863ae8856d7db05192d9a39ba8a417568be6f47) | 22 |

- Thời gian trong bảng là **thời gian tác giả tạo commit (author date)**, quy đổi về **UTC+07:00**, sắp xếp mới đến cũ trong từng repository.
- Quy thuộc commit theo **Git author**, sau khi gộp các bí danh của cùng một thành viên. Người merge và người viết nội dung có thể khác nhau.
- **Thường**: commit có tối đa một parent, gồm cả commit khởi tạo. **Merge**: commit có từ hai parent trở lên; xác định bằng cấu trúc Git, không suy đoán từ tiêu đề commit.
- Hai commit khởi tạo SUT của giảng viên được giữ trong danh sách để phản ánh đầy đủ lịch sử nhưng tách khỏi đóng góp của nhóm.
- Số commit dùng để truy vết quá trình làm việc; không quy đổi thành phần trăm đóng góp hoặc mức độ hoàn thành nhiệm vụ.

## 2. Thành viên và bí danh Git

| MSSV | Thành viên | Tài khoản GitHub | Tên tác giả xuất hiện trong Git |
| --- | --- | --- | --- |
| 23120242 | Nguyễn Văn Bình Dương | `BDCALM` | `HarryNguyen` |
| 23120409 | Nguyễn Khắc Vượng | `vinny2707` | `vinny2707`, `Nguyễn Khắc Vượng` |
| 23120410 | Trần Quốc Vỹ | `tranquocvy` | `tranquocvy`, `Trần Quốc Vỹ` |

Tác giả ngoài nhóm trong lịch sử SUT: `Hanh Tran`, `ttbhanh-hcmus`.

## 3. Thống kê commit theo thành viên

| Thành viên | Mã nguồn: thường | Mã nguồn: merge | Docs: thường | Docs: merge | Tổng |
| --- | ---: | ---: | ---: | ---: | ---: |
| Nguyễn Văn Bình Dương | 7 | 4 | 5 | 0 | 16 |
| Nguyễn Khắc Vượng | 9 | 4 | 12 | 2 | 27 |
| Trần Quốc Vỹ | 32 | 3 | 3 | 0 | 38 |
| **Tổng nhóm** | **48** | **11** | **20** | **2** | **81** |
| SUT gốc — ngoài nhóm | 2 | 0 | 0 | 0 | 2 |

**Tổng hai repository: 83 commit**, gồm **81 commit của nhóm** và **2 commit SUT gốc**.

## 4. Tổng kết công việc từng thành viên

Các nội dung dưới đây tổng hợp từ lịch sử thay đổi và hồ sơ trong repository. Commit minh họa giúp truy vết từng nhóm công việc; kết quả chạy và mức độ đáp ứng yêu cầu cần đối chiếu tài liệu, log và artifact tương ứng.

### 4.1. Nguyễn Văn Bình Dương — 23120242

- **GitHub Actions và Task A/C:** xây dựng workflow backend quality gates; xử lý cài đặt `sqlite3` trên runner; lưu minh chứng Task A và C. Các mốc tiêu biểu: [`cf00005`](https://github.com/SUT-group10/eshop-sut-group10/commit/cf00005f853633d52954c97f97649c2d4b95f9d7), [`647d7e2`](https://github.com/SUT-group10/eshop-sut-group10/commit/647d7e2fe9ec6b5265abf59067505bc504eaf6e6), [`585d648`](https://github.com/SUT-group10/eshop-sut-group10/commit/585d6489221443a57e502369b500b803141c9ea4).
- **Cache và coverage:** điều chỉnh cơ chế npm cache, tách bước restore/save để lưu cache khi test thất bại; cập nhật cấu hình chấp thuận dependency; bổ sung bước công bố coverage badge và hiển thị trong README. Các mốc: [`89b4982`](https://github.com/SUT-group10/eshop-sut-group10/commit/89b498210649ca65dbe65d7e7c6077c264b3b58e), [`5297dd8`](https://github.com/SUT-group10/eshop-sut-group10/commit/5297dd82b0512709ab598fa9a2cbb42bd8a0dd51), [`b32104d`](https://github.com/SUT-group10/eshop-sut-group10/commit/b32104dec9c93ed77f475b545643f590d166aa8c), [`569d7a3`](https://github.com/SUT-group10/eshop-sut-group10/commit/569d7a3d193eb44a6420e514b3c17b0c1d786db7).
- **Tài liệu:** xây dựng và hoàn thiện user guide GitHub Actions; cập nhật khai báo sử dụng AI, chatlog tuần 5 và bổ sung trường hợp AI hallucination. Các mốc: [`b185970`](https://github.com/SUT-group10/smn_cicd_docs/commit/b18597096ae09c856f79723312f8a56c6ca7fdd6), [`585cbd5`](https://github.com/SUT-group10/smn_cicd_docs/commit/585cbd5b271733f2a1cfc443b1eac63e49944217), [`5d528bf`](https://github.com/SUT-group10/smn_cicd_docs/commit/5d528bf9e9285c0dc36e12db39147fa57b83c0c2), [`2b8ebff`](https://github.com/SUT-group10/smn_cicd_docs/commit/2b8ebff5a00ea41997789b54be44d9d1a9eb7b6d).
- **Tích hợp:** thực hiện các merge liên quan test harness và GitHub Actions, gồm PR #9, nhánh `github-ci` và PR #22 về coverage badge.

### 4.2. Nguyễn Khắc Vượng — 23120409

- **Chuẩn bị đề tài:** bổ sung Tool Survey Proposal, yêu cầu seminar, checklist công việc và mẫu báo cáo AI hằng tuần. Các mốc: [`2012896`](https://github.com/SUT-group10/eshop-sut-group10/commit/2012896887c8fb207c1d1787f5afe91a2182312d), [`39742cc`](https://github.com/SUT-group10/eshop-sut-group10/commit/39742cc6acb1ec15a18ebb68e76d905b5fa3e727), [`3c04789`](https://github.com/SUT-group10/eshop-sut-group10/commit/3c04789a93ec597b63b8227e496d491efda58a9e).
- **Jenkins, Task D:** cấu hình Jenkins bằng Docker, agent và npm cache; viết Jenkinsfile cho lint, build, test; bổ sung xuất test/coverage report, hướng dẫn và minh chứng thực thi. Các mốc: [`9c084c7`](https://github.com/SUT-group10/eshop-sut-group10/commit/9c084c7086fad706c3144b688da752a84f8666ce), [`6c4b568`](https://github.com/SUT-group10/eshop-sut-group10/commit/6c4b5684e9bf9237b14a376a926115f92a8f1b0d), [`bb185c2`](https://github.com/SUT-group10/eshop-sut-group10/commit/bb185c22662af74c9bb43b70943a7350e154ed49), [`a559c53`](https://github.com/SUT-group10/eshop-sut-group10/commit/a559c53acb9a3b03cc05fca8a855bcc59c8eaae8), [`3af7e8a`](https://github.com/SUT-group10/eshop-sut-group10/commit/3af7e8aa2b3fae2c16d9ca6f4bcf000ab93499f1), [`eaca833`](https://github.com/SUT-group10/eshop-sut-group10/commit/eaca833cad68854cbe1e9be71c1c2dff6c5ec6e1).
- **Tài liệu seminar:** khởi tạo cấu trúc bộ nộp; viết hướng dẫn Jenkins; phát triển báo cáo từ phần giới thiệu, cơ sở CI/CD, lựa chọn công cụ và hệ thống kiểm thử đến triển khai GitHub Actions/Jenkins, so sánh hai nền tảng; bổ sung khai báo AI tuần 5 và README ở root. Các mốc: [`880fd82`](https://github.com/SUT-group10/smn_cicd_docs/commit/880fd822fa6ae16ea338770f17053c86a0e0ff17), [`c54fd7f`](https://github.com/SUT-group10/smn_cicd_docs/commit/c54fd7f03b7eb50d12dd7cf84a803d539f4ec624), [`1c5897a`](https://github.com/SUT-group10/smn_cicd_docs/commit/1c5897af2421aeaf63d7e0aaf5f860932c9967eb), [`1259ae2`](https://github.com/SUT-group10/smn_cicd_docs/commit/1259ae2549f8afb20e233b8397a770bc8aee3876), [`85b626a`](https://github.com/SUT-group10/smn_cicd_docs/commit/85b626a11ca4d4f0aa3024a4fce48353b9307b95), [`f863ae8`](https://github.com/SUT-group10/smn_cicd_docs/commit/f863ae8856d7db05192d9a39ba8a417568be6f47).
- **Phân tích thủ công và tích hợp:** có tài liệu phân tích flaky test tại `docs/evidence/E-04/phan-tich-vuong.md`; thực hiện merge các PR #12, #13, #15 trong repo mã nguồn và PR #1, #2 trong repo tài liệu. Tài liệu E-04 được đưa vào Git qua [`50e12b5`](https://github.com/SUT-group10/eshop-sut-group10/commit/50e12b5bf0f19af4b07b0a3b5669c4ecb366bc24) do **Trần Quốc Vỹ** đứng tên tác giả commit; vì vậy commit này được tính cho Vỹ trong bảng số lượng, còn nội dung phân tích được ghi nhận theo hồ sơ của Vượng.

- **Phân tích thủ công v2:** thực hiện thêm bản phân tích order history tại `docs/evidence/E-04/order-history-v2-39860438568/phan-tich-order-history.md`, đối chiếu 10 run/19 attempt và ảnh hưởng của retry. Hồ sơ được đưa vào repo trong [`713a56b`](https://github.com/SUT-group10/eshop-sut-group10/commit/713a56bfec82196298edfba60664073085b62de0) do Vỹ đứng tên commit; nội dung phân tích được ghi nhận cho Vượng theo hồ sơ bàn giao.

### 4.3. Trần Quốc Vỹ — 23120410

- **Test harness, Task B:** kiểm tra setup và baseline; bổ sung unit/API/smoke test; thiết kế fixture, cleanup và kiểm chứng isolation; chuẩn hóa lệnh chất lượng, coverage, tài liệu kiến trúc và quality gates. Các mốc: [`77e1870`](https://github.com/SUT-group10/eshop-sut-group10/commit/77e1870672a282308f0db0e2919bc471bbbda4eb), [`ec6d7bf`](https://github.com/SUT-group10/eshop-sut-group10/commit/ec6d7bf940afb4607b91867ca39465a4b3ddfbf5), [`3336f93`](https://github.com/SUT-group10/eshop-sut-group10/commit/3336f93e029830f047f9d0ba8c191fa8bc2f486a), [`a7f0923`](https://github.com/SUT-group10/eshop-sut-group10/commit/a7f0923a168b5228920c27be04452c2dfcfe4de7), [`c9ac902`](https://github.com/SUT-group10/eshop-sut-group10/commit/c9ac9024a4ef783f8d5d5b6360dbb6322b358527).
- **Thực nghiệm flaky test, Task E:** tạo thí nghiệm timing flake cho cart, workflow chạy thủ công, protocol và bộ minh chứng; lưu dữ liệu thô của mười lần chạy, chuẩn hóa log, tổ chức đầu vào triage, đối chiếu phân tích người/AI và ghi nhận kết quả trước–sau khi sửa đồng bộ. Các mốc: [`d9b06ae`](https://github.com/SUT-group10/eshop-sut-group10/commit/d9b06aef4bcd6ea54d6c102b24eef68ee32f3557), [`20525b8`](https://github.com/SUT-group10/eshop-sut-group10/commit/20525b8e2f99fcdede7e786cda7b7c2889dabf96), [`cafcb52`](https://github.com/SUT-group10/eshop-sut-group10/commit/cafcb524281778cdf13835cbaef002ca90f3ce6a), [`c07b12f`](https://github.com/SUT-group10/eshop-sut-group10/commit/c07b12f267ef4d2736d4c1bec15472721a8b3173), [`dec4102`](https://github.com/SUT-group10/eshop-sut-group10/commit/dec41025dd17fbc3c6beb3b8b803b995bbbe5bab), [`3a6290d`](https://github.com/SUT-group10/eshop-sut-group10/commit/3a6290defb626f4bd41c53620c2a253d27e235c5), [`6654c36`](https://github.com/SUT-group10/eshop-sut-group10/commit/6654c36d7b51876dafa9842dccfefbd5613fea12).
- **Mở rộng thực nghiệm v2:** bổ sung order-history experiment có quan sát retry, collector, protocol, minh chứng local/CI và kết quả đối chiếu triage; tích hợp vào `main` qua PR #21. Mốc: [`713a56b`](https://github.com/SUT-group10/eshop-sut-group10/commit/713a56bfec82196298edfba60664073085b62de0), [`d7b4d01`](https://github.com/SUT-group10/eshop-sut-group10/commit/d7b4d010108762e219e7d104cf71949661783117).
- **Tài liệu và hoạt động seminar:** viết giải thích flaky test bằng minh chứng EShop; bổ sung user guide flaky test/AI triage, phần báo cáo thực nghiệm v2 và worksheet API Postmortem; lưu khai báo AI, chatlog liên quan. Các mốc: [`8d892ca`](https://github.com/SUT-group10/eshop-sut-group10/commit/8d892caf5ed8bd88fc748419a5e7019a793a9d06), [`f3c0a8d`](https://github.com/SUT-group10/smn_cicd_docs/commit/f3c0a8d17df2dfe8c47b90538f79f5707f5cd3d3), [`4af3ca1`](https://github.com/SUT-group10/smn_cicd_docs/commit/4af3ca19ab6eb99511de743710d7b653b2d36459), [`3ea12b6`](https://github.com/SUT-group10/smn_cicd_docs/commit/3ea12b639d3ca97f64d7aef7008e8479ac7408fa).

## 5. Danh sách commit — repository mã nguồn

| SHA | Ngày giờ (UTC+07) | Tác giả Git | Loại | Commit message |
| --- | --- | --- | --- | --- |
| [`89c6676`](https://github.com/SUT-group10/eshop-sut-group10/commit/89c667692ad1b39d9409a9998a1774803213c8e7) | 23/10/2026 17:00:50 | HarryNguyen | Merge | Merge pull request #22 from SUT-group10/coverageBadge |
| [`569d7a3`](https://github.com/SUT-group10/eshop-sut-group10/commit/569d7a3d193eb44a6420e514b3c17b0c1d786db7) | 23/10/2026 16:50:39 | HarryNguyen | Thường | ci: publish backend coverage badge |
| [`d7b4d01`](https://github.com/SUT-group10/eshop-sut-group10/commit/d7b4d010108762e219e7d104cf71949661783117) | 23/10/2026 15:13:58 | Trần Quốc Vỹ | Merge | Merge pull request #21 from SUT-group10/test/issue-5-flaky-retry-v2 |
| [`713a56b`](https://github.com/SUT-group10/eshop-sut-group10/commit/713a56bfec82196298edfba60664073085b62de0) | 23/10/2026 04:35:57 | tranquocvy | Thường | test(E-03): add retry-aware order history experiment v2 |
| [`fe41113`](https://github.com/SUT-group10/eshop-sut-group10/commit/fe41113025e58eb835540cdc16d6fd8dc7be697b) | 22/10/2026 23:30:51 | Trần Quốc Vỹ | Merge | Merge pull request #18 from SUT-group10/test/issue-5-flaky-experiment |
| [`115df94`](https://github.com/SUT-group10/eshop-sut-group10/commit/115df94bcc548a7a67ce5903ac7f26bbaaf63984) | 22/10/2026 23:01:48 | tranquocvy | Thường | docs(E-10): add approved E-09 and E-10 AI reports |
| [`6654c36`](https://github.com/SUT-group10/eshop-sut-group10/commit/6654c36d7b51876dafa9842dccfefbd5613fea12) | 22/10/2026 22:58:00 | tranquocvy | Thường | docs(E-10): close evaluation with observed outcome and limits |
| [`6607908`](https://github.com/SUT-group10/eshop-sut-group10/commit/6607908b872c88d817b8d75f18936e8d79822e1d) | 22/10/2026 22:57:27 | tranquocvy | Thường | docs(E-09): record before-and-after synchronization checks |
| [`3a6290d`](https://github.com/SUT-group10/eshop-sut-group10/commit/3a6290defb626f4bd41c53620c2a253d27e235c5) | 22/10/2026 22:57:26 | tranquocvy | Thường | test(E-09): await cart addition before reading results |
| [`f057ac4`](https://github.com/SUT-group10/eshop-sut-group10/commit/f057ac453c3221eb7aba4f238b96e40e2e3af078) | 22/10/2026 22:50:42 | tranquocvy | Thường | docs(E-08): add approved E-06 to E-08 AI reports |
| [`dec4102`](https://github.com/SUT-group10/eshop-sut-group10/commit/dec41025dd17fbc3c6beb3b8b803b995bbbe5bab) | 22/10/2026 22:47:09 | tranquocvy | Thường | docs(E-08): compare reviewed human and AI triage results |
| [`50e12b5`](https://github.com/SUT-group10/eshop-sut-group10/commit/50e12b5bf0f19af4b07b0a3b5669c4ecb366bc24) | 22/10/2026 22:39:16 | tranquocvy | Thường | docs(E-04): add Vuong manual analysis evidence |
| [`9fd2a99`](https://github.com/SUT-group10/eshop-sut-group10/commit/9fd2a99919d59b8014f374f31a963e75a435c2c3) | 22/10/2026 22:36:19 | tranquocvy | Thường | docs(E-06): select and archive log-only triage skill |
| [`b32104d`](https://github.com/SUT-group10/eshop-sut-group10/commit/b32104dec9c93ed77f475b545643f590d166aa8c) | 22/10/2026 21:54:40 | HarryNguyen | Thường | chore: adjust approven for required dependencies for Task C07 |
| [`8d892ca`](https://github.com/SUT-group10/eshop-sut-group10/commit/8d892caf5ed8bd88fc748419a5e7019a793a9d06) | 22/10/2026 21:29:35 | tranquocvy | Thường | docs(F-01): explain flaky tests with EShop evidence |
| [`75b024e`](https://github.com/SUT-group10/eshop-sut-group10/commit/75b024e73f8584178399086ad92de4a85f2be375) | 22/10/2026 19:47:58 | HarryNguyen | Merge | Merge branch 'github-ci' |
| [`5297dd8`](https://github.com/SUT-group10/eshop-sut-group10/commit/5297dd82b0512709ab598fa9a2cbb42bd8a0dd51) | 22/10/2026 12:34:37 | HarryNguyen | Thường | ci(C-03): decouple npm cache into restore and save steps to handle test failure |
| [`89b4982`](https://github.com/SUT-group10/eshop-sut-group10/commit/89b498210649ca65dbe65d7e7c6077c264b3b58e) | 22/10/2026 11:51:08 | HarryNguyen | Thường | ci(C-03): use actions/cache with save-always to persist npm cache on baseline test failure |
| [`bdcd7a6`](https://github.com/SUT-group10/eshop-sut-group10/commit/bdcd7a62240a6d097789c271634d8aecb8d634a1) | 21/10/2026 23:54:19 | tranquocvy | Thường | docs(E-05): add approved chatlog and AI usage declaration |
| [`c07b12f`](https://github.com/SUT-group10/eshop-sut-group10/commit/c07b12f267ef4d2736d4c1bec15472721a8b3173) | 21/10/2026 23:47:09 | tranquocvy | Thường | docs(E-05): prepare normalized logs and shared triage input |
| [`2ae7103`](https://github.com/SUT-group10/eshop-sut-group10/commit/2ae710332ecb80dc37f309953d9c38d5367d0a21) | 21/10/2026 23:27:48 | tranquocvy | Thường | docs(E-03): add approved chatlog and AI usage declaration |
| [`623cf5b`](https://github.com/SUT-group10/eshop-sut-group10/commit/623cf5b960447660a3bab313452e8587bf30bfbe) | 21/10/2026 23:16:32 | tranquocvy | Thường | docs(E-03): report experiment results and complete checklist |
| [`cafcb52`](https://github.com/SUT-group10/eshop-sut-group10/commit/cafcb524281778cdf13835cbaef002ca90f3ce6a) | 21/10/2026 23:16:25 | tranquocvy | Thường | docs(E-03): preserve raw evidence from ten experiment runs |
| [`72228c2`](https://github.com/SUT-group10/eshop-sut-group10/commit/72228c21341c005e21dd7d6980ef5b44572f8328) | 21/10/2026 22:40:49 | Nguyễn Khắc Vượng | Merge | Merge pull request #15 from SUT-group10/develop |
| [`b252db4`](https://github.com/SUT-group10/eshop-sut-group10/commit/b252db4aa9e236b78879dfbbf7b882cdadb8580c) | 21/10/2026 21:59:30 | tranquocvy | Thường | docs(E-02): add approved chatlog and AI usage declaration |
| [`b9b0a25`](https://github.com/SUT-group10/eshop-sut-group10/commit/b9b0a25a219e91eaee04b91923c805e933fcfefb) | 21/10/2026 21:35:41 | tranquocvy | Thường | docs(E-02): mark E-01 and E-02 complete |
| [`cd24ec0`](https://github.com/SUT-group10/eshop-sut-group10/commit/cd24ec0fe4b3d1257674220b69203eb259a017a4) | 21/10/2026 21:30:49 | tranquocvy | Thường | docs(E-02): keep workflow registration on main |
| [`8fc6bd2`](https://github.com/SUT-group10/eshop-sut-group10/commit/8fc6bd25318c7de462596cfc3a8dd74724236bbf) | 21/10/2026 21:30:08 | Trần Quốc Vỹ | Merge | ci(E-02): upgrade manual workflow actions to Node 24 (#16) |
| [`169aeff`](https://github.com/SUT-group10/eshop-sut-group10/commit/169aeff1e6100e8266d7fabcedb8f98c322d7098) | 21/10/2026 21:29:04 | tranquocvy | Thường | ci(E-02): upgrade manual workflow actions to Node 24 |
| [`e5d3919`](https://github.com/SUT-group10/eshop-sut-group10/commit/e5d39198670fb6ce1255622ee2f8ee4f639815ea) | 21/10/2026 21:18:04 | tranquocvy | Thường | docs(E-02): define experiment protocol and verified rehearsal |
| [`068d385`](https://github.com/SUT-group10/eshop-sut-group10/commit/068d38559704f1262101266589c0d1f29eee1193) | 21/10/2026 21:14:21 | tranquocvy | Thường | ci(E-02): upgrade flaky workflow actions to Node 24 |
| [`05ed3ed`](https://github.com/SUT-group10/eshop-sut-group10/commit/05ed3edb9bffd0804bbe673a7b8a7f2217f56eae) | 21/10/2026 20:39:59 | Nguyễn Khắc Vượng | Merge | Merge pull request #13 from SUT-group10/ci/issue-5-register-flaky-workflow |
| [`20525b8`](https://github.com/SUT-group10/eshop-sut-group10/commit/20525b8e2f99fcdede7e786cda7b7c2889dabf96) | 21/10/2026 20:35:29 | tranquocvy | Thường | ci(E-02): register manual flaky workflow on main |
| [`bb96998`](https://github.com/SUT-group10/eshop-sut-group10/commit/bb969986fc0c67868f28eaa06d6eaf5780256c96) | 19/10/2026 15:39:44 | tranquocvy | Thường | docs(E-01): add approved chatlog and AI usage declaration |
| [`308341e`](https://github.com/SUT-group10/eshop-sut-group10/commit/308341e3202e546819fc5b1d92d51e2c094b87c1) | 19/10/2026 15:39:44 | tranquocvy | Thường | docs(E-01): document timing flake and local verification evidence |
| [`d9b06ae`](https://github.com/SUT-group10/eshop-sut-group10/commit/d9b06aef4bcd6ea54d6c102b24eef68ee32f3557) | 19/10/2026 15:39:31 | tranquocvy | Thường | test(E-01): add isolated cart timing flake experiment |
| [`b9c76a6`](https://github.com/SUT-group10/eshop-sut-group10/commit/b9c76a6340bdb10324f1f382f067029e40b84888) | 16/10/2026 22:34:31 | Nguyễn Khắc Vượng | Merge | Merge pull request #12 from SUT-group10/cicd/jenkins |
| [`0de1444`](https://github.com/SUT-group10/eshop-sut-group10/commit/0de1444d6f441ecaa3f07139cfb41b3ca53fcd99) | 16/10/2026 22:30:36 | vinny2707 | Merge | Merge remote-tracking branch 'origin/develop' into cicd/jenkins |
| [`eaca833`](https://github.com/SUT-group10/eshop-sut-group10/commit/eaca833cad68854cbe1e9be71c1c2dff6c5ec6e1) | 16/10/2026 22:26:08 | vinny2707 | Thường | docs(D-03): add evidence for task D-03 |
| [`3af7e8a`](https://github.com/SUT-group10/eshop-sut-group10/commit/3af7e8aa2b3fae2c16d9ca6f4bcf000ab93499f1) | 16/10/2026 22:20:16 | vinny2707 | Thường | ci(D-02): add evidence for task D-02 |
| [`585d648`](https://github.com/SUT-group10/eshop-sut-group10/commit/585d6489221443a57e502369b500b803141c9ea4) | 16/10/2026 20:35:01 | HarryNguyen | Thường | docs: add evidences for Task A and C |
| [`a559c53`](https://github.com/SUT-group10/eshop-sut-group10/commit/a559c53acb9a3b03cc05fca8a855bcc59c8eaae8) | 16/10/2026 19:35:34 | vinny2707 | Thường | ci(D-03): publish Jenkins test and coverage reports |
| [`647d7e2`](https://github.com/SUT-group10/eshop-sut-group10/commit/647d7e2fe9ec6b5265abf59067505bc504eaf6e6) | 16/10/2026 15:21:54 | HarryNguyen | Thường | ci(C-02): build sqlite3 from source on runner |
| [`650bd1c`](https://github.com/SUT-group10/eshop-sut-group10/commit/650bd1c456d6f0280a2730eac47d36c69eaef09c) | 16/10/2026 13:19:54 | HarryNguyen | Merge | Merge remote-tracking branch 'origin/develop' into github-ci |
| [`bb185c2`](https://github.com/SUT-group10/eshop-sut-group10/commit/bb185c22662af74c9bb43b70943a7350e154ed49) | 16/10/2026 12:43:13 | vinny2707 | Thường | ci(D-02): add Jenkinsfile for backend lint, build and tests |
| [`cf00005`](https://github.com/SUT-group10/eshop-sut-group10/commit/cf00005f853633d52954c97f97649c2d4b95f9d7) | 16/10/2026 12:17:03 | HarryNguyen | Thường | ci: add backend quality gates |
| [`6c4b568`](https://github.com/SUT-group10/eshop-sut-group10/commit/6c4b5684e9bf9237b14a376a926115f92a8f1b0d) | 16/10/2026 11:49:14 | vinny2707 | Thường | ci(D-01): add Jenkins agent, npm cache and setup guide with evidence |
| [`9c084c7`](https://github.com/SUT-group10/eshop-sut-group10/commit/9c084c7086fad706c3144b688da752a84f8666ce) | 16/10/2026 00:39:59 | vinny2707 | Thường | feat(D-01): finish configuration and setup guide for install jenkins by docker |
| [`083b393`](https://github.com/SUT-group10/eshop-sut-group10/commit/083b393e08a80b66ab4f094ed466801da970083c) | 13/10/2026 20:48:12 | HarryNguyen | Merge | Merge pull request #9 from BDCALM/docs/b-05-harness-diagra |
| [`6135704`](https://github.com/SUT-group10/eshop-sut-group10/commit/613570496f5d2352d390b679bb6f57132aca988b) | 13/10/2026 19:59:38 | tranquocvy | Thường | docs(G-08): document week 1 AI usage for B-01 through B-05 |
| [`3c04789`](https://github.com/SUT-group10/eshop-sut-group10/commit/3c04789a93ec597b63b8227e496d491efda58a9e) | 13/10/2026 18:55:22 | vinny2707 | Thường | feat(docs): add AI template for weekly report |
| [`c9ac902`](https://github.com/SUT-group10/eshop-sut-group10/commit/c9ac9024a4ef783f8d5d5b6360dbb6322b358527) | 13/10/2026 17:52:23 | tranquocvy | Thường | docs(B-05): document harness architecture and quality gates |
| [`075bb50`](https://github.com/SUT-group10/eshop-sut-group10/commit/075bb503e55a03ca5363cfee7c5b1d001cd7d4eb) | 13/10/2026 17:36:23 | tranquocvy | Thường | docs(B-04): document quality commands and clean clone verification |
| [`a7f0923`](https://github.com/SUT-group10/eshop-sut-group10/commit/a7f0923a168b5228920c27be04452c2dfcfe4de7) | 13/10/2026 17:20:35 | tranquocvy | Thường | chore(B-04): standardize backend quality commands and coverage |
| [`3336f93`](https://github.com/SUT-group10/eshop-sut-group10/commit/3336f93e029830f047f9d0ba8c191fa8bc2f486a) | 13/10/2026 16:39:02 | tranquocvy | Thường | test(B-03): add fixtures cleanup and isolation verification |
| [`ec6d7bf`](https://github.com/SUT-group10/eshop-sut-group10/commit/ec6d7bf940afb4607b91867ca39465a4b3ddfbf5) | 13/10/2026 16:07:25 | tranquocvy | Thường | test(B-02): add representative unit API and smoke tests |
| [`77e1870`](https://github.com/SUT-group10/eshop-sut-group10/commit/77e1870672a282308f0db0e2919bc471bbbda4eb) | 13/10/2026 15:58:02 | tranquocvy | Thường | docs(B-01): document verified setup and baseline inventory |
| [`39742cc`](https://github.com/SUT-group10/eshop-sut-group10/commit/39742cc6acb1ec15a18ebb68e76d905b5fa3e727) | 10/10/2026 16:01:45 | vinny2707 | Thường | feat(docs): add task ID E06 to AI triage checklist |
| [`2012896`](https://github.com/SUT-group10/eshop-sut-group10/commit/2012896887c8fb207c1d1787f5afe91a2182312d) | 10/10/2026 11:26:58 | vinny2707 | Thường | feat(docs): add tool survey proposal, seminar requirements and task checklist |
| [`85af3ba`](https://github.com/SUT-group10/eshop-sut-group10/commit/85af3ba875c88283615e22cb108f13e2fccaf0e9) | 15/05/2026 08:30:35 | ttbhanh-hcmus | Thường · SUT gốc | first upload |
| [`2f9bf2b`](https://github.com/SUT-group10/eshop-sut-group10/commit/2f9bf2bab45855abf211377a5d5eba10031649e3) | 15/05/2026 08:27:44 | Hanh Tran | Thường · SUT gốc | Initial commit |

## 6. Danh sách commit — repository tài liệu

| SHA | Ngày giờ (UTC+07) | Tác giả Git | Loại | Commit message |
| --- | --- | --- | --- | --- |
| [`f863ae8`](https://github.com/SUT-group10/smn_cicd_docs/commit/f863ae8856d7db05192d9a39ba8a417568be6f47) | 23/10/2026 17:12:58 | vinny2707 | Thường | feat(G-03): add readme for submission |
| [`2b8ebff`](https://github.com/SUT-group10/smn_cicd_docs/commit/2b8ebff5a00ea41997789b54be44d9d1a9eb7b6d) | 23/10/2026 17:06:12 | HarryNguyen | Thường | docs: Adding an  AI's Hallucination case |
| [`afd6274`](https://github.com/SUT-group10/smn_cicd_docs/commit/afd62747b00b2055b806a193af965ca547c766b6) | 23/10/2026 15:10:54 | Nguyễn Khắc Vượng | Merge | Merge pull request #2 from tranquocvy/docs/flaky-ai-v2 |
| [`3ea12b6`](https://github.com/SUT-group10/smn_cicd_docs/commit/3ea12b639d3ca97f64d7aef7008e8479ac7408fa) | 23/10/2026 14:59:46 | tranquocvy | Thường | docs(G-04): add API postmortem worksheet |
| [`4af3ca1`](https://github.com/SUT-group10/smn_cicd_docs/commit/4af3ca19ab6eb99511de743710d7b653b2d36459) | 23/10/2026 14:58:08 | tranquocvy | Thường | docs(G-03): document flaky experiment and human AI triage v2 |
| [`f3c0a8d`](https://github.com/SUT-group10/smn_cicd_docs/commit/f3c0a8d17df2dfe8c47b90538f79f5707f5cd3d3) | 23/10/2026 14:57:49 | tranquocvy | Thường | docs(G-02): add flaky and AI triage user guide |
| [`85b626a`](https://github.com/SUT-group10/smn_cicd_docs/commit/85b626a11ca4d4f0aa3024a4fce48353b9307b95) | 23/10/2026 14:00:43 | vinny2707 | Thường | feat(G-03): add ai use declaration in week 5 |
| [`5d528bf`](https://github.com/SUT-group10/smn_cicd_docs/commit/5d528bf9e9285c0dc36e12db39147fa57b83c0c2) | 23/10/2026 12:04:22 | HarryNguyen | Thường | AI-usage-declaration: for Week 5 -  update AI_usage &  chatlog.md |
| [`52b711b`](https://github.com/SUT-group10/smn_cicd_docs/commit/52b711bea1d5e73348a885b22235db35fd2063ea) | 23/10/2026 11:57:53 | HarryNguyen | Thường | AI-usage-declaration: update chatlog.md |
| [`585cbd5`](https://github.com/SUT-group10/smn_cicd_docs/commit/585cbd5b271733f2a1cfc443b1eac63e49944217) | 23/10/2026 11:49:57 | HarryNguyen | Thường | docs: complete user_guide for github-actions |
| [`e475d92`](https://github.com/SUT-group10/smn_cicd_docs/commit/e475d92d994eac4c7fdc7ec1554a23ecf00c4ddf) | 23/10/2026 00:58:26 | Nguyễn Khắc Vượng | Merge | Merge pull request #1 from SUT-group10/vuong |
| [`1259ae2`](https://github.com/SUT-group10/smn_cicd_docs/commit/1259ae2549f8afb20e233b8397a770bc8aee3876) | 23/10/2026 00:57:27 | vinny2707 | Thường | feat(G-03): add comparison between github actions and jenkins |
| [`0ee4ed8`](https://github.com/SUT-group10/smn_cicd_docs/commit/0ee4ed8eee527ba0e6cd291559b74d8875812128) | 23/10/2026 00:46:17 | vinny2707 | Thường | feat(G-03): add section jenkins implement ci pipeline |
| [`2cc9a5f`](https://github.com/SUT-group10/smn_cicd_docs/commit/2cc9a5f8d3d31576cf36d29477ac2813c13ed8ac) | 23/10/2026 00:38:39 | vinny2707 | Thường | feat(G-03): add section github actions implement ci pipeline |
| [`b185970`](https://github.com/SUT-group10/smn_cicd_docs/commit/b18597096ae09c856f79723312f8a56c6ca7fdd6) | 23/10/2026 00:30:29 | HarryNguyen | Thường | Github-action-user-guide: Con phan 6, 7 , 8 ,9, 10 |
| [`d632f06`](https://github.com/SUT-group10/smn_cicd_docs/commit/d632f06926020fb73a831236bc743215464ea673) | 23/10/2026 00:24:11 | vinny2707 | Thường | feat(G-03): add section testing system in EShop |
| [`eae8121`](https://github.com/SUT-group10/smn_cicd_docs/commit/eae8121ddb85c3b7d227475e6f19bc8c9be398cb) | 23/10/2026 00:16:11 | vinny2707 | Thường | feat(G-03): add section Tool and Tool Selection |
| [`c2aa7c5`](https://github.com/SUT-group10/smn_cicd_docs/commit/c2aa7c501fdf529f46467c24f9218a53d320372f) | 23/10/2026 00:05:04 | vinny2707 | Thường | feat(G-03): add section CI/CD fundamental |
| [`1c5897a`](https://github.com/SUT-group10/smn_cicd_docs/commit/1c5897af2421aeaf63d7e0aaf5f860932c9967eb) | 22/10/2026 23:41:21 | vinny2707 | Thường | feat(G-03): add section introduction to report |
| [`c54fd7f`](https://github.com/SUT-group10/smn_cicd_docs/commit/c54fd7f03b7eb50d12dd7cf84a803d539f4ec624) | 21/10/2026 23:15:40 | vinny2707 | Thường | feat(docs): add jenkins user guide |
| [`880fd82`](https://github.com/SUT-group10/smn_cicd_docs/commit/880fd822fa6ae16ea338770f17053c86a0e0ff17) | 21/10/2026 22:04:47 | vinny2707 | Thường | feat(docs): add structure for submission |
| [`eb4a4a2`](https://github.com/SUT-group10/smn_cicd_docs/commit/eb4a4a2bcbf46be51ada65ef10c7ac69914e0807) | 21/10/2026 21:34:11 | Nguyễn Khắc Vượng | Thường | Initial commit |

## 7. Cách đối chiếu và cập nhật

Danh sách lấy trực tiếp từ Git object của hai mốc chốt. Có thể đối chiếu tại trang commit đã liên kết hoặc chạy các lệnh sau trong repository tương ứng:

```powershell
# Mã nguồn EShop
git -C eshop-sut-group10 log 89c667692ad1b39d9409a9998a1774803213c8e7 --format="%H | %aI | %an | %P | %s"
git -C eshop-sut-group10 rev-list --count 89c667692ad1b39d9409a9998a1774803213c8e7

# Tài liệu seminar
git -C seminar_cicd_docs log f863ae8856d7db05192d9a39ba8a417568be6f47 --format="%H | %aI | %an | %P | %s"
git -C seminar_cicd_docs rev-list --count f863ae8856d7db05192d9a39ba8a417568be6f47

```

Lệnh `%aI` hiển thị múi giờ gốc của tác giả; bảng ở trên đã quy đổi thống nhất sang UTC+07. Khi cập nhật, lấy SHA `main` mới nhất của đúng hai repository nhóm, thống kê lại theo Git author và số parent, rồi cập nhật cả mốc chốt, bảng tổng hợp và danh sách chi tiết. Bản thay đổi của chính file này chỉ xuất hiện trong lần thống kê sau khi được commit và tích hợp vào mốc mới.
