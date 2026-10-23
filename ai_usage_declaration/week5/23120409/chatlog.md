# NHẬT KÝ TRÒ CHUYỆN VỚI AI (AI CHAT LOG)

- **Môn học:** Kiểm thử phần mềm (Software Testing)
- **Tuần báo cáo:** Tuần thực hành 5 (Week 5)
- **Sinh viên:** Nguyễn Khắc Vượng
- **MSSV:** 23120409
- **Nhóm / Đề tài:** Nhóm 10 — T07: CI/CD & Test-Harness Engineering
- **Công cụ:** Codex; chưa xác nhận model cụ thể của phiên.
- **Ngày tổng hợp:** 23/10/2026, Asia/Bangkok (UTC+07:00)
- **Bản khai và Human Oversight:** [AI_Usage_Declaration.md](AI_Usage_Declaration.md).

> Đây là nhật ký chọn lọc, được tổng hợp lại từ hội thoại phục vụ tuần 5; không phải bản export nguyên vẹn toàn bộ phiên. Các khối **Prompt** bên dưới là trích đoạn nguyên văn của sinh viên, giữ cách viết gốc. **Result** và **Adjustment / Human Oversight** là phần tóm tắt, không giả là trích nguyên văn phản hồi AI. Không tự gán ngày/giờ cho từng entry khi hội thoại được cung cấp không có timestamp đầy đủ.

## Entry Log 1 — Cơ sở lý thuyết và yêu cầu hình minh họa

**Prompt:**

```text
về phần cơ sở lí thuyết CICD thì cần viết những phần nào
```

```text
Viết cho tôi. Lưu ý với các sơ đồ không dùng mermaid tạo ảnh để mô tả cho dễ đọc và chuyển sang pdf không bị lỗi
```

**Result (tóm tắt):** AI hỗ trợ tổ chức và viết cơ sở lý thuyết về CI/CD, harness, fixture, runner/agent, cache/artifact, coverage và quality gate. Các sơ đồ được lưu thành PNG phục vụ Markdown và PDF.

**Adjustment / Human Oversight:** Sinh viên yêu cầu soạn từng phần, đúng lý thuyết và đủ chi tiết. Trước đó đã yêu cầu viết lại nội dung chưa phù hợp, bỏ cách gắn nguồn sau từng câu. Phần flaky test lý thuyết 2.6 được dành cho thành viên phụ trách, không tự nhận là đã viết hoàn chỉnh trong lượt hỗ trợ này.

## Entry Log 2 — Viết section 4 về hệ thống kiểm thử

**Prompt:**

```text
viết tiếp section 4 cho tôi đi
```

**Result (tóm tắt):** AI đọc backend EShop, test, helper, cấu hình Jest và baseline B-04; viết phần SUT, bộ kiểm thử, fixture/isolation và kết quả baseline. Nội dung ghi 39 test, 29 pass, 10 fail; phân biệt smoke qua API với GUI/mobile E2E. AI bổ sung sơ đồ PNG về các đường thực thi test và vòng đời fixture.

**Adjustment / Human Oversight:** Sinh viên yêu cầu tiếp tục viết theo bộ khung. Kiểm tra nguồn và số liệu bằng công cụ là phần AI thực hiện; không khai báo một lần chạy bộ test mới chỉ từ thao tác đọc các log đã lưu.

## Entry Log 3 — Viết section 5 và kiểm tra repo vừa cập nhật

**Prompt:**

```text
viết tiếp section 5 đi
```

```text
bạn check repo mới pull về có không nếu không có hỏi lại tôi
```

```text
kiểm thử workflow xem thử có chưa
```

**Result (tóm tắt):** Ban đầu AI tìm thấy cấu hình workflow nhưng ít minh chứng Task-C. Sau yêu cầu kiểm tra trực tiếp, AI đọc các run và cấu hình repository trên GitHub, tải artifact và chạy lại run `39818068371` thành attempt 2. Kết quả: 39 test, 29 pass/10 fail, exact cache hit, upload success và job failure. AI kiểm tra JSON, XML, thời điểm của từng attempt và đối chiếu từng ID fail.

**Adjustment / Human Oversight:** Sinh viên không chấp nhận kết luận chỉ dựa trên các file cũ, yêu cầu kiểm tra bản vừa pull và workflow thực tế. AI ghi riêng run xanh của PR #20 vì diff sửa `server.js`; không coi nó là baseline SUT chưa thay đổi. Snapshot branch protection được đọc có review bắt buộc nhưng danh sách required CI check rỗng; không tự khai đã hoàn tất gate.

## Entry Log 4 — Viết section 6 về Jenkins

**Prompt:**

```text
tiếp tục đến section 6 đi
```

**Result (tóm tắt):** AI đọc Jenkinsfile, Compose/Dockerfile, tài liệu hướng dẫn và dữ liệu Jenkins build #4. Soạn kiến trúc, môi trường, sáu stage, JUnit/Coverage, archive và npm cache. Phần chẩn đoán sử dụng `A-CHECKOUT-02`: expected 60000000, received 1, đối chiếu route ở cùng commit.

**Adjustment / Human Oversight:** Phạm vi là tiếp tục report. AI dùng build #4 đã lưu, không chạy mới Jenkins; không gọi volume cache là cache hit khi chưa đo. Nội dung giữ riêng branch coverage Jest 39,09% và Coverage plugin 41,74%.

## Entry Log 5 — Viết ngắn gọn phần so sánh

**Prompt:**

```text
với section 7 tôi cần viết thêm cái gì
```

```text
bạn có thể viết phần này ngắn gọn được không
```

**Result (tóm tắt):** AI đề xuất bốn mục điều kiện đo, kết quả, cấu hình/vận hành và nhận xét; sau đó viết section 7 với một bảng so sánh. Thời gian GitHub Actions 65 giây và Jenkins 17,600 giây được ghi cho các lượt đã chọn; có giới hạn về runtime, cache và số lần thực thi test.

**Adjustment / Human Oversight:** Sinh viên yêu cầu rút gọn. AI không suy từ một cặp lượt chạy rằng một nền tảng nhanh hơn hoặc tốt hơn trong mọi bối cảnh.

## Entry Log 6 — Rút gọn tài liệu tham khảo

**Prompt:**

```text
phần phụ lục ấy xóa hết đi trỏ đến các documentation của các công cụ là được rồi
```

```text
phần tài liệu tham khảo ấy. bỏ hết trỏ đến mấy documentation chính của csac công cụ thôi
```

**Result (tóm tắt):** AI bỏ phần phụ lục và rút danh sách 42 nguồn chi tiết xuống 9 liên kết documentation chính: GitHub Actions, Jenkins, Jest, Supertest, npm, Docker, GitLab CI/CD, ChatGPT và Datadog. Nội dung các section được giữ khi rút danh sách tham khảo.

**Adjustment / Human Oversight:** Sinh viên làm rõ rằng yêu cầu là mục tài liệu tham khảo. Việc xóa quá phạm vi các bản sao minh chứng bởi AI được ghi riêng ở entry tiếp theo.

## Entry Log 7 — Phát hiện việc xóa nhầm log

**Prompt:**

```text
ủa nãy bạn xóa các file log cũ đi à
```

**Result (tóm tắt):** AI thừa nhận đã hiểu quá rộng yêu cầu và xóa các bản sao log/minh chứng trong `report/assets/github-actions/evidence/` và `report/assets/jenkins/evidence/`. Khi kiểm tra lại, hai thư mục đã tồn tại, đủ 42 file, khớp bản Git; số file cần khôi phục lúc kiểm tra bằng công cụ là 0. Log gốc trong repository mã nguồn không bị thao tác xóa này ảnh hưởng.

**Adjustment / Human Oversight:** Sinh viên trực tiếp phát hiện và hỏi lại phạm vi thay đổi. Không ghi rằng chính AI đã thực hiện khôi phục trong lần kiểm tra cuối, vì các file đã hiện diện trước thao tác kiểm tra đó. Đây là một trường hợp yêu cầu review thay đổi do AI tạo, không phải kết quả được che khỏi bản khai.

## Entry Log 8 — Kịch bản quay demo

**Prompt:**

```text
oke bạn iu của tôi ơi. Với cái này quay clip demo thì kịch bản là gì
```

```text
nếu chia ra 3 clip được không một cái về github actions 1 cái jenkins 1 cái về flaky triage test
```

**Result (tóm tắt):** AI đọc yêu cầu và các kết quả đã có, đề xuất video khoảng 7 phút; sau đó chia thành ba đoạn GitHub Actions, Jenkins và flaky test/AI triage. Kịch bản đi từ workflow tới log/artifact, chẩn đoán checkout, lịch sử flake, phân tích và kiểm chứng sau sửa.

**Adjustment / Human Oversight:** Sinh viên chủ động đề xuất chia ba clip. AI nhắc phân biệt kết quả đã lưu với thao tác quay mới, kết quả sau sửa local với CI và công cụ AI thực tế trong minh chứng. Đây là tư vấn kịch bản, không phải video đã được quay/xuất.

## Entry Log 9 — Mục lục slide và learning objectives

**Prompt:**

```text
slide bao gồm những gì vậy
```

```text
không ý tôi là ở trong slide kia bộ table of content là gì chưa phải là của tôi
```

```text
mở rộng với slide của tôi nên bao gồm các content nào nhỉ
```

```text
tiếng anh
```

**Result (tóm tắt):** AI đọc PDF `EN_CICD_TestHarness.pdf` do sinh viên cung cấp. Sau khi hiểu nhầm phạm vi câu hỏi, AI phân biệt mục lục của tài liệu mẫu với đề xuất mục lục slide của nhóm. Tiếp tục hỗ trợ diễn đạt learning objectives tiếng Anh cho CI/CD, harness, hai nền tảng CI và AI triage.

**Adjustment / Human Oversight:** Sinh viên liên tục làm rõ câu hỏi và lựa chọn ngôn ngữ. Không ghi rằng AI đã hoàn thiện deck hoặc tạo lại toàn bộ slide chỉ từ các nội dung trao đổi này.

## Entry Log 10 — Review nội dung quality gate trên PR

**Prompt (trích đoạn):**

```text
slide thế này hợp lí không
```

Sinh viên cung cấp nội dung slide có title “No green check, no merge”, phần status check, branch protection, coverage badge và đề xuất ảnh nút merge bị khóa.

**Result (tóm tắt):** AI đối chiếu tài liệu GitHub và đọc cấu hình bảo vệ nhánh/ruleset. Đề xuất tách coverage badge khỏi cơ chế gate, thay phát biểu tuyệt đối “broken code can’t reach main” bằng phát biểu về failing required checks, và yêu cầu ảnh minh chứng thể hiện đúng lý do merge bị chặn.

**Adjustment / Human Oversight:** Sinh viên đưa nội dung cụ thể để review. AI chỉ đọc cấu hình và tư vấn cách viết, không tự thay đổi branch protection hoặc khai báo đã triển khai required CI check.

## Entry Log 11 — Nội dung Jenkins và slide so sánh

**Prompt:**

```text
Phân Jenkins Architecture nên nói những gì
```

```text
tôi đã xong phần lí thuyết của jenkins về coponent và architecture đến jenkins pipe line nói lí thuyết xong ví dụ eshop nhir
```

```text
trong slide so sánh Jenkins với Github Actions viets những gì
```

**Result (tóm tắt):** AI gợi ý trình bày controller điều phối, agent thực thi; giới thiệu Jenkinsfile, stages/steps và post actions trước khi minh họa EShop. Đề xuất bảng so sánh pipeline definition, execution environment, GitHub integration, reporting, maintenance và kết quả baseline.

**Adjustment / Human Oversight:** Sinh viên quyết định thứ tự lý thuyết rồi ví dụ và hỏi cách đưa code lên slide. AI tư vấn dùng đoạn Jenkinsfile rút gọn, không đưa toàn bộ file thành ảnh chữ nhỏ.

## Entry Log 12 — Tạo hình pipeline Jenkins

**Prompt:**

```text
tạo cho tôi 1 ảnh mô tả pipeline trong jenkins hiện tại đi bạn
```

**Result (tóm tắt):** AI đọc Jenkinsfile hiện tại, vẽ ảnh PNG 4800 × 2700, tỷ lệ 16:9 bằng mã. Ảnh thể hiện Checkout, Environment, Install, Lint, Build, Tests; phần Tests post publish JUnit/Coverage và pipeline post archive artifact. Baseline build #4 được ghi riêng: 39 test, 29 pass/10 fail, FAILURE.

**Sản phẩm tại thời điểm tạo:** `slides/assets/jenkins-pipeline.png`. File không còn ở đường dẫn này trong checkout khi tổng hợp nhật ký; không gắn liên kết tải tới file không tồn tại và không tự tạo lại ảnh thay cho bản cũ.

**Adjustment / Human Oversight:** Sinh viên yêu cầu một hình mô tả pipeline hiện có. Ảnh là sơ đồ do AI tạo từ cấu hình, không được ghi nhận như screenshot Jenkins, kết quả chạy mới hoặc sản phẩm ảnh sinh viên tự vẽ.

## Entry Log 13 — Bổ sung phạm vi User Guide

**Prompt:**

```text
ngoài userguide về jenkins và github action cần userguide về gì nữa
```

**Result (tóm tắt):** AI đề xuất bảy mục: chuẩn bị môi trường, chạy harness, GitHub Actions, Jenkins, thí nghiệm flaky test, AI-assisted triage và troubleshooting/limitations. Mỗi phần nên có điều kiện, thao tác, kết quả mong đợi và cách xử lý lỗi.

**Adjustment / Human Oversight:** Đây là đề xuất cấu trúc; không tự coi các hướng dẫn bổ sung đã được triển khai, kiểm thử bởi người ngoài nhóm hoặc xuất bản.

## Ghi chú phạm vi

- Nhật ký phản ánh các lượt trao đổi được chọn, không chứng minh thời gian phân tích thủ công E-04 hoặc toàn bộ lịch sử sử dụng AI của sinh viên.
- Những thao tác đọc file, viết report, vẽ sơ đồ, truy vấn GitHub và chạy lại workflow nêu trên là phần AI hỗ trợ theo yêu cầu.
- Model, mức reasoning và tỷ lệ đóng góp của phiên hỗ trợ này chưa có xác nhận; không sao chép thông tin đó từ bản khai của sinh viên khác.
- Các góp ý và nội dung còn chưa được sinh viên xác nhận không được coi là review độc lập đã hoàn tất.
