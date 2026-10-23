# BÁO CÁO TUYÊN BỐ SỬ DỤNG AI (AI USAGE DECLARATION)

- **Môn học:** Kiểm thử phần mềm (Software Testing)
- **Tuần báo cáo:** Tuần thực hành 5 (Week 5)
- **Họ và tên sinh viên:** Nguyễn Khắc Vượng
- **Mã số sinh viên (MSSV):** 23120409
- **Nhóm / Đề tài:** Nhóm 10 — T07: CI/CD & Test-Harness Engineering
- **Ngày tổng hợp:** 23/10/2026, Asia/Bangkok (UTC+07:00)
- **Phạm vi công việc:** Soạn và chỉnh sửa báo cáo seminar; đối chiếu kết quả CI; chuẩn bị nội dung slide, hình pipeline Jenkins và kịch bản demo; tổ chức tài liệu tham khảo và bản khai sử dụng AI.


## 1. Trạng thái sử dụng AI trong tuần

- [ ] **Tôi không sử dụng AI trong tuần này.**
- [x] **Tôi có sử dụng công cụ AI trong tuần này.**

Tôi sử dụng Codex để hỗ trợ đọc repository, giải thích nội dung kỹ thuật, soạn report và nội dung trình bày. Tôi xác định phần cần viết, yêu cầu chỉnh cách trình bày, đặt câu hỏi và điều chỉnh phạm vi qua từng lượt trao đổi. Các thay đổi file, lần kiểm tra bằng công cụ và lần chạy lại workflow được AI thực hiện theo yêu cầu được ghi trong chatlog.

## 2. Công cụ AI đã sử dụng

| STT | Tên công cụ | Phiên bản / Model | Mục đích sử dụng | Mức độ đóng góp ước tính |
| --- | --- | --- | --- | --- |
| 1 | Codex | ChatGPT 6.1 Sol | Đọc mã nguồn và minh chứng; viết, sửa Markdown; kiểm tra workflow và artifact; tư vấn slide/demo; tạo hình pipeline Jenkins | 80% |

Không suy model của phiên này từ mẫu của thành viên khác hoặc từ model được ghi cho lượt AI triage E-08. Bản tổng hợp hiện chưa có cơ sở định lượng tỷ lệ đóng góp giữa sinh viên và AI.

## 3. Tóm tắt quá trình sử dụng AI

### 3.1. Soạn cơ sở lý thuyết và khảo sát công cụ

- **Phần AI hỗ trợ:** Đề xuất nội dung CI/CD, test harness, fixture, runner/agent, cache/artifact, coverage và quality gate; soạn nội dung và tạo hình PNG minh họa. Hỗ trợ khảo sát và giải thích lựa chọn GitHub Actions, Jenkins cùng hướng AI.
- **Human Oversight:** Tôi yêu cầu viết theo từng section, đủ chi tiết nhưng đúng lý thuyết; yêu cầu không dùng Mermaid và không đính nguồn sau từng câu. Khi nội dung chưa phù hợp, tôi yêu cầu xóa hoặc viết lại phần đang soạn và rút gọn tài liệu tham khảo.
- **Kết quả:** Nội dung được biên soạn trong report; hình minh họa được lưu riêng. Không khai báo rằng toàn bộ các nhận định đã được tôi kiểm chứng độc lập chỉ từ việc đồng ý tiếp tục viết.

### 3.2. Viết hệ thống kiểm thử và các phần CI trong report

- **Phần AI hỗ trợ:** Đọc SUT, test, fixture, Jenkinsfile, workflow và kết quả đã lưu để viết section 4–6. Phân biệt smoke qua API với kiểm thử giao diện; giải thích baseline 39 test, 29 pass/10 fail, cơ chế lưu báo cáo sau khi test lỗi và khác biệt giữa các nguồn coverage.
- **Human Oversight:** Tôi yêu cầu tiếp tục từng section, yêu cầu kiểm tra bản repo vừa pull khi chưa thấy minh chứng và yêu cầu kiểm thử workflow trước khi chốt kết quả GitHub Actions.
- **Kết quả:** AI soạn nội dung dựa trên file và log. Số liệu Jenkins lấy từ build #4 đã lưu, không phải một build Jenkins mới trong phiên viết report. Bộ test/SUT baseline không bị AI sửa để tạo kết quả xanh.

### 3.3. Kiểm tra GitHub Actions và viết phần so sánh

- **Phần AI hỗ trợ:** Đọc GitHub API/CLI, metadata job, log, artifact và cấu hình bảo vệ nhánh; chạy lại run `39818068371` thành attempt 2. Đối chiếu số test, từng ID fail, coverage và trạng thái upload; viết section 7 theo yêu cầu ngắn gọn.
- **Human Oversight:** Tôi yêu cầu kiểm tra workflow thực tế thay vì chỉ đọc tài liệu. Khi được đề xuất nội dung section 7, tôi yêu cầu viết ngắn gọn.
- **Kết quả:** Lượt chạy lại ghi nhận 29 pass, 10 fail; exact cache hit và upload artifact thành công. AI phân biệt run xanh của PR #20 có sửa SUT với baseline. Snapshot được kiểm tra có yêu cầu review nhưng danh sách required CI checks rỗng; không khai báo đã hoàn tất CI gate chỉ vì nhánh có bảo vệ. Các thao tác kiểm tra và chạy lại này do AI thực hiện, không phải phép kiểm chứng thủ công độc lập của tôi.

### 3.4. Chỉnh tài liệu tham khảo và xử lý việc xóa nhầm minh chứng

- **Phần AI hỗ trợ:** Rút phần tham khảo từ các liên kết chi tiết thành 9 trang documentation chính. Trong quá trình hiểu yêu cầu bỏ phụ lục, AI đã xóa quá phạm vi các bản sao log/minh chứng trong `report/assets/.../evidence/`.
- **Human Oversight:** Tôi hỏi lại việc xóa log, làm rõ yêu cầu chỉ liên quan tài liệu tham khảo.
- **Kết quả:** AI thừa nhận hiểu quá rộng yêu cầu. Ở lần kiểm tra tiếp theo, 42 file minh chứng đã có đầy đủ và khớp bản Git đã lưu; công cụ kiểm tra không phải khôi phục thêm file nào tại thời điểm đó. Không tự nhận rằng chính AI đã khôi phục các file trong lần kiểm tra này. Log gốc trong repository mã nguồn EShop không bị xóa bởi thao tác trên.

### 3.5. Chuẩn bị nội dung slide và hình pipeline Jenkins

- **Phần AI hỗ trợ:** Đọc PDF slide tham khảo; gợi ý mục lục, learning objectives, nội dung quality gate, Jenkins architecture, Pipeline và bảng so sánh hai nền tảng. Đọc Jenkinsfile hiện tại và tạo PNG 16:9 mô tả sáu stage, publisher và archive artifact.
- **Human Oversight:** Tôi làm rõ khi AI hiểu nhầm câu hỏi về mục lục PDF với mục lục slide của nhóm; yêu cầu nội dung tiếng Anh và hỏi lại cách diễn đạt quality gate. Tôi yêu cầu một hình pipeline Jenkins cụ thể để sử dụng trên slide.
- **Kết quả:** Có nội dung đề xuất; AI đã tạo hình tại `slides/assets/jenkins-pipeline.png` trong phiên trao đổi. Tại thời điểm tổng hợp bản khai, file không còn ở đường dẫn này trong checkout, nên không coi đó là liên kết sản phẩm hiện có. Hình được AI vẽ bằng mã từ Jenkinsfile, không phải ảnh chụp giao diện Jenkins hoặc bằng chứng của một build mới. Việc tư vấn nội dung không đồng nghĩa AI đã hoàn thiện toàn bộ PowerPoint.

### 3.6. Lập kịch bản demo và đề xuất User Guide

- **Phần AI hỗ trợ:** Đề xuất video khoảng 5–8 phút và phương án chia thành ba đoạn GitHub Actions, Jenkins, flaky test/AI triage. Đề xuất các mục hướng dẫn môi trường, harness, thí nghiệm flake, AI triage và troubleshooting.
- **Human Oversight:** Tôi chủ động đề xuất chia ba clip và hỏi những phần User Guide còn cần bổ sung.
- **Kết quả:** Đây là kịch bản và khung hướng dẫn được đề xuất. Bản khai không coi các clip đã được quay, xuất video hoặc các hướng dẫn mới đã được viết đầy đủ chỉ vì đã có tư vấn.

## 4. Chatlog, sản phẩm và trách nhiệm của sinh viên

- **Chatlog chọn lọc:** [chatlog.md](chatlog.md). Prompt trích nguyên văn được phân biệt với phần tổng hợp phản hồi; không có thời gian chi tiết cho từng entry nên không tự điền giờ.
- **Report được AI hỗ trợ biên soạn:** [report/report.md](../../../report/report.md).
- **Hình pipeline Jenkins:** Đã được tạo trong phiên tại `slides/assets/jenkins-pipeline.png`; chưa có file ở đường dẫn này trong checkout hiện tại. Nội dung được ghi như lịch sử hỗ trợ, không gắn liên kết tới một file không tồn tại.
- **Tài liệu Jenkins được đọc để đối chiếu:** [user_guide/jenkins.md](../../../user_guide/jenkins.md). Không mặc định toàn bộ tài liệu này được tạo trong phiên tuần 5 đang tổng hợp.
- **Tài liệu GitHub Actions của nhóm:** [user_guide/github-actions.md](../../../user_guide/github-actions.md). Không nhận toàn bộ phần triển khai/hướng dẫn của thành viên phụ trách GitHub Actions là sản phẩm riêng của tôi.
- **Vị trí ghi thông tin video:** [demo/demo_videos.md](../../../demo/demo_videos.md). Tư vấn kịch bản không phải bằng chứng đã có video hoàn chỉnh.

Tôi chịu trách nhiệm đọc, kiểm tra và quyết định sử dụng nội dung trước khi nộp. Các kết quả 10 lượt flaky test, 12 lượt local sau sửa và output AI E-08 được tham khảo từ dữ liệu đã có của nhóm, không phải các lượt thí nghiệm mới do tôi tự thực hiện trong hội thoại này. Phần phân tích thủ công E-04 và thời điểm thực hiện không được AI dựng lại trong bản khai.

## 5. Cam kết tính trung thực học thuật

Tôi cam kết khai báo đúng phạm vi AI đã hỗ trợ; phân biệt dữ liệu đã kiểm chứng, đề xuất và nội dung chưa hoàn tất. Tôi không xem phản hồi AI là bằng chứng tự động cho việc công cụ đã cấu hình đúng, test đã chạy hoặc tài liệu đã được review độc lập.

**Người khai báo:** Nguyễn Khắc Vượng  
**Ngày lập bản tổng hợp:** 23/10/2026  
**Xác nhận của sinh viên:** Sinh viên đọc và xác nhận nội dung, model và mức đóng góp trước khi nộp; tên được điền trong bản tổng hợp không thay chữ ký xác nhận.
