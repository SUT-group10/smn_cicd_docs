# Báo cáo chi tiết — CI/CD and Test-Harness Engineering

## 1. Giới thiệu

### 1.1. Bối cảnh

Trong phát triển phần mềm, mã nguồn thường xuyên được cập nhật bởi nhiều thành viên. Mỗi thay đổi có thể ảnh hưởng đến các chức năng đang hoạt động. Nếu việc kiểm tra chỉ được thực hiện thủ công trước khi bàn giao, lỗi có thể được phát hiện muộn và khó xác định thay đổi gây ra lỗi. Vì vậy, nhóm phát triển cần một quy trình chạy kiểm thử thường xuyên và phản hồi sớm khi có thay đổi mã nguồn.

Tích hợp liên tục (Continuous Integration – CI) hỗ trợ quy trình này thông qua pipeline tự động, được cấu hình để kiểm tra mã nguồn khi có sự kiện như push hoặc pull request. Pipeline thực hiện các bước kiểm tra, ghi nhận kết quả và lưu bằng chứng phục vụ chẩn đoán lỗi. Để kết quả có thể tin cậy, bộ kiểm thử cần đi kèm một test harness bảo đảm việc chuẩn bị môi trường, tạo và dọn dẹp dữ liệu, thực thi test và xuất báo cáo được thực hiện nhất quán.

Một thách thức của kiểm thử tự động là flaky test – test có thể cho kết quả pass hoặc fail qua các lần chạy dù mã nguồn không đổi. Hiện tượng này làm nhiễu kết quả pipeline và tăng công sức điều tra. AI có thể được sử dụng để hỗ trợ đọc log và đề xuất giả thuyết nguyên nhân; tuy nhiên, các đề xuất đó cần được kiểm chứng bằng bằng chứng thực nghiệm.

Trong đề tài T07 – *CI/CD & Test-Harness Engineering*, nhóm thực hành trên backend của hệ thống EShop: xây dựng test harness và pipeline kiểm thử, triển khai trên GitHub Actions và Jenkins, đồng thời so sánh phân tích flaky test thủ công với phân tích có hỗ trợ của ChatGPT.

### 1.2. Mục tiêu

Báo cáo trình bày quá trình thực hiện và đánh giá các mục tiêu sau:

1. Xây dựng test harness và bộ kiểm thử backend EShop, với fixture, setup và cleanup hỗ trợ tính độc lập và khả năng tái lập của test.
2. Thiết lập workflow GitHub Actions để tự động kiểm tra mã nguồn, chạy kiểm thử và lưu log, báo cáo kiểm thử, coverage cùng các artifact liên quan.
3. Triển khai pipeline tương ứng trên Jenkins, chạy cùng bộ kiểm thử để đối chiếu kết quả, thời gian thực thi, cơ chế cache và công sức cấu hình, vận hành.
4. Tạo một flaky test do cơ chế chờ theo thời gian, quan sát kết quả qua 10 lần chạy, sau đó sửa nguyên nhân gây flake và chạy lại để xác nhận.
5. So sánh phân tích thủ công với phân tích có hỗ trợ của ChatGPT trên cùng log và lịch sử chạy, dựa trên độ đúng của giả thuyết, bằng chứng, bước xác minh, thời gian và các suy diễn thiếu căn cứ.
6. Rút ra bài học về độ tin cậy của pipeline, giá trị và giới hạn của AI trong phân tích flaky test, cùng cách áp dụng quy trình cho dự án khác.

### 1.3. Phạm vi

Đối tượng kiểm thử (System Under Test – SUT) là phần **backend của EShop**, được nhóm fork từ repo gốc `ttbhanh/eshop-sut` sang `SUT-group10/eshop-sut-group10`. Báo cáo tập trung vào:

- **Test harness và bộ kiểm thử backend:** unit test, API test, smoke test, các test kiểm tra harness, cùng cơ chế fixture, setup và cleanup.
- **GitHub Actions** là hệ thống CI chính: workflow chạy trên push và pull request, cache npm, lưu coverage XML và artifact, status check và branch protection cho nhánh `main`.
- **Jenkins** là nền tảng đối chiếu: chạy cùng bộ lệnh và cùng bộ kiểm thử để so sánh cấu hình, thời gian chạy, cache và công sức vận hành.
- **Thí nghiệm flaky test và phân tích log:** tạo một trường hợp flake có nguyên nhân đã biết; so sánh phân tích thủ công với ChatGPT theo cùng đầu vào và tiêu chí đánh giá. Kết luận được đối chiếu với nguyên nhân đã tạo và kết quả chạy sau khi sửa flake.

Các giới hạn của phạm vi thực hiện gồm:

- **Không sửa lỗi nghiệp vụ trong mã nguồn SUT.** Các lỗi được phát hiện sẽ được ghi nhận trong kết quả kiểm thử; nhóm giữ các assertion theo đặc tả, không thay đổi kết quả mong đợi hoặc bỏ test để làm pipeline thành công. Việc sửa flaky test thuộc phạm vi mã kiểm thử.
- Không kiểm thử frontend web, trang quản trị và ứng dụng mobile.
- Không triển khai ứng dụng (deploy). Phần thực nghiệm tập trung vào CI và dừng ở bước kiểm thử, xuất báo cáo và lưu artifact.

### 1.4. Thành viên và phân công

Nhóm 10 gồm ba thành viên. Bảng 1 tóm tắt phân công chính; thông tin đóng góp chi tiết được dẫn tại [Seminar contribution](../contribution/contribution.md).

*Bảng 1. Phân công công việc*

| MSSV | Họ và tên | Phụ trách chính |
| --- | --- | --- |
| 23120242 | Nguyễn Văn Bình Dương | Quản lý phạm vi và tiến độ; pipeline GitHub Actions; cơ sở lý thuyết; điều phối tài liệu nộp |
| 23120409 | Nguyễn Khắc Vượng | Pipeline Jenkins và so sánh hai nền tảng; khảo sát công cụ; phân tích flaky test thủ công |
| 23120410 | Trần Quốc Vỹ | Test harness và bộ kiểm thử; thí nghiệm flaky test; AI triage; failure modes và hướng áp dụng cho dự án khác |

## 2. Cơ sở lý thuyết

### 2.1. CI/CD và pipeline as code

### 2.2. Test harness và fixture

### 2.3. Runner / agent

### 2.4. Cache và artifact

### 2.5. Coverage và quality gate

### 2.6. Flaky test

## 3. Khảo sát và lựa chọn công cụ

### 3.1. Công cụ CI truyền thống

### 3.2. Hướng AI

### 3.3. Lựa chọn của nhóm và lý do

## 4. Hệ thống kiểm thử

### 4.1. SUT: backend EShop

### 4.2. Bộ kiểm thử

### 4.3. Fixture và tính độc lập

### 4.4. Baseline

## 5. Triển khai pipeline trên GitHub Actions

### 5.1. Kiến trúc và thành phần

### 5.2. Workflow và trigger

### 5.3. Các bước của pipeline

### 5.4. Cache và artifact

### 5.5. Status check và branch protection

### 5.6. Coverage badge

### 5.7. Kết quả

## 6. Triển khai pipeline trên Jenkins

### 6.1. Kiến trúc và thành phần

### 6.2. Môi trường triển khai

### 6.3. Thiết kế pipeline

### 6.4. Báo cáo, artifact và cache

### 6.5. Kết quả

### 6.6. Minh họa chẩn đoán lỗi

## 7. So sánh GitHub Actions và Jenkins

### 7.1. Điều kiện đo

### 7.2. Kết quả đo

### 7.3. So sánh cấu hình và vận hành

### 7.4. Nhận xét

## 8. Thí nghiệm flaky test

### 8.1. Thiết kế flaky test

### 8.2. Protocol thí nghiệm

### 8.3. Kết quả 10 lần chạy

### 8.4. Sửa flake và xác nhận

## 9. AI triage và so sánh với phân tích thủ công

### 9.1. Phân tích thủ công

### 9.2. Chuẩn bị dữ liệu và prompt

### 9.3. Kết quả AI triage

### 9.4. So sánh theo rubric

## 10. Thảo luận

### 10.1. AI hữu ích và gây nhiễu ở đâu

### 10.2. Failure modes của AI

### 10.3. Hạn chế

## 11. Áp dụng cho dự án khác

## 12. Kết luận và bài học

## 13. Tài liệu tham khảo

## Phụ lục A. Truy vết số liệu
