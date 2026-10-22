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

**Tích hợp liên tục (Continuous Integration – CI)**

CI là thực hành tích hợp các thay đổi mã nguồn vào nhánh chung thường xuyên, kết hợp với build và kiểm thử tự động để phát hiện lỗi sớm. Thay vì phát triển riêng trong thời gian dài rồi mới ghép mã, các thành viên đưa vào từng thay đổi nhỏ và kiểm tra khả năng hoạt động của chúng khi kết hợp với phần mã hiện có. Cách làm này giúp giảm khối lượng thay đổi phải xem xét khi xảy ra lỗi và hạn chế việc dồn các vấn đề tích hợp đến cuối giai đoạn phát triển.

Để thực hiện CI, dự án cần có kho mã nguồn chung, quy trình kiểm tra tự động và cơ chế phản hồi kết quả. Khi có thay đổi, hệ thống CI lấy mã, chuẩn bị môi trường, chạy các lệnh kiểm tra và thông báo trạng thái. Nhóm phát triển sử dụng phản hồi đó để review, sửa lỗi hoặc tiếp tục tích hợp. Vì vậy, CI bao gồm cả cơ chế tự động hóa và cách làm việc của nhóm: tích hợp thường xuyên, duy trì bộ kiểm thử và xử lý sớm các lần kiểm tra thất bại.

**Phân phối liên tục và triển khai liên tục (CD)**

CD có thể được dùng để chỉ **Continuous Delivery** hoặc **Continuous Deployment**. Cả hai đều mở rộng quy trình sau bước tích hợp và kiểm tra, nhưng khác nhau ở cách đưa thay đổi lên môi trường production, tức môi trường phục vụ người dùng thực tế.

Continuous Delivery hướng tới việc duy trì phần mềm ở trạng thái có thể phát hành khi cần. Quy trình chuẩn bị bản phát hành và kiểm tra được tự động hóa để việc triển khai có thể thực hiện lặp lại, theo yêu cầu. Tuy nhiên, thời điểm phát hành lên production vẫn có thể phụ thuộc vào quyết định của con người.

Continuous Deployment tự động đưa những thay đổi đã vượt qua các điều kiện kiểm tra lên production. Như vậy, khác biệt chính là mức độ tự động hóa ở bước phát hành: Continuous Delivery bảo đảm khả năng phát hành, còn Continuous Deployment tự động thực hiện việc triển khai đối với những thay đổi đạt yêu cầu. Cả hai đều cần dựa trên quy trình kiểm tra đáng tin cậy.

**Pipeline và cơ chế hoạt động**

Pipeline là quy trình tự động hóa gồm các công việc và quan hệ thực thi giữa chúng. Một pipeline CI/CD có thể bắt đầu từ thay đổi mã nguồn và kết thúc ở kiểm thử, tạo bản phát hành hoặc triển khai, tùy mục tiêu của dự án. Các công việc phụ thuộc nhau được thực hiện theo thứ tự; những công việc độc lập có thể được tổ chức chạy song song.

Một pipeline kiểm tra mã nguồn thường gồm các bước sau:

1. **Checkout:** lấy phiên bản mã nguồn cần kiểm tra từ kho quản lý phiên bản.
2. **Cài dependency:** chuẩn bị các thư viện và công cụ cần thiết cho dự án.
3. **Kiểm tra tĩnh:** kiểm tra mã theo những quy tắc đã cấu hình, chẳng hạn bằng linter.
4. **Build:** biên dịch, chuyển đổi hoặc đóng gói mã nếu công nghệ của dự án yêu cầu.
5. **Kiểm thử:** chạy bộ test tự động và ghi nhận kết quả.
6. **Lưu kết quả và phản hồi:** lưu log, báo cáo, sản phẩm cần giữ lại và công bố trạng thái cho nhóm phát triển.

![Luồng thực thi và phản hồi trong CI](assets/theory/ci-pipeline.png)

*Hình 1. Luồng thực thi và phản hồi của một pipeline CI điển hình.*

Sơ đồ minh họa một luồng phổ biến, không phải cấu trúc bắt buộc cho mọi dự án. Khi một bước kiểm tra thất bại, các công việc phụ thuộc phía sau thường được bỏ qua; các bước thu thập log, báo cáo và dọn dẹp cần được cấu hình để xử lý cả trường hợp có lỗi. Nhờ vậy, lập trình viên có thể xác định bước thất bại và sử dụng bằng chứng đã lưu để điều tra. Nếu triển khai CD, pipeline được mở rộng thêm các bước chuẩn bị bản phát hành, kiểm tra trên môi trường phù hợp và triển khai theo chính sách đã định.

**Pipeline as code**

Pipeline as code là cách định nghĩa quy trình tự động hóa bằng mã hoặc file cấu hình được quản lý phiên bản cùng dự án. Thay vì chỉ thiết lập qua giao diện, nhóm mô tả các công việc, điều kiện kích hoạt và quan hệ thực thi trong file. GitHub Actions sử dụng các file YAML trong thư mục `.github/workflows`; Jenkins hỗ trợ định nghĩa pipeline bằng `Jenkinsfile`.

Cách tổ chức này giúp cấu hình pipeline có lịch sử thay đổi, có thể được review cùng mã nguồn và khôi phục khi cần. Các thành viên cũng có một định nghĩa chung về quy trình kiểm tra, giảm sự phụ thuộc vào thao tác thủ công trên từng máy. Để dựng lại toàn bộ môi trường, dự án vẫn cần quản lý các yếu tố đi kèm như phiên bản runtime, công cụ và dịch vụ phụ thuộc.

Đối với kiểm thử phần mềm, CI/CD tạo điều kiện đưa bộ test vào quy trình phát triển thường xuyên và sử dụng kết quả làm phản hồi cho mỗi thay đổi. Hiệu quả của quy trình phụ thuộc vào chất lượng bộ test và môi trường thực thi; tự động hóa không thay thế việc thiết kế kiểm thử. Trong seminar này, nhóm tập trung triển khai phần CI: kiểm tra mã nguồn, chạy bộ test backend EShop và lưu báo cáo. Phần triển khai ứng dụng lên production nằm ngoài phạm vi thực hiện.

### 2.2. Test harness và fixture

**Khái niệm và vai trò của test harness**

Test harness là tập hợp công cụ, cấu hình và mã hỗ trợ dùng để chuẩn bị điều kiện, thực thi kiểm thử và thu thập kết quả. Một bộ test muốn chạy tự động cần có cách khởi tạo dữ liệu, gọi hệ thống được kiểm tra, xác định kết quả đúng hay sai và dọn dẹp sau khi chạy. Test harness kết nối những công việc này thành một quy trình có thể thực hiện lặp lại, thay vì phụ thuộc vào việc chuẩn bị thủ công của người kiểm thử.

Cần phân biệt test harness với test case và test runner. **Test case** mô tả một tình huống cụ thể, gồm điều kiện, dữ liệu đầu vào, thao tác và kết quả mong đợi. **Test runner**, chẳng hạn Jest, tìm các test cần chạy, điều phối thực thi và ghi nhận kết quả. **Test harness** bao quát runner cùng các thành phần hỗ trợ để test chạy trong điều kiện phù hợp. Test runner ở đây là công cụ chạy kiểm thử, khác với runner hoặc agent của hệ thống CI sẽ được trình bày ở mục 2.3.

**Các thành phần chính**

Các thành phần của harness phối hợp để đưa test từ trạng thái đã chuẩn bị đến kết quả có thể kiểm tra được. Hình 2 minh họa mối quan hệ giữa đầu vào, runner, vòng đời test và báo cáo; SUT là đối tượng mà test gọi để quan sát hành vi.

![Các thành phần và vòng đời của test harness](assets/theory/test-harness.png)

*Hình 2. Các thành phần của test harness và các giai đoạn thực hiện một lượt kiểm thử.*

Vai trò của những thành phần chính được tóm tắt trong Bảng 2.

*Bảng 2. Các thành phần chính của test harness*

| Thành phần | Vai trò |
| --- | --- |
| Test runner | Phát hiện, thực thi test, gọi các hook và ghi nhận kết quả |
| Fixture | Cung cấp dữ liệu, đối tượng và trạng thái cần thiết cho test |
| Setup/teardown | Chuẩn bị và thu hồi dữ liệu, kết nối, tài nguyên trong vòng đời kiểm thử |
| Assertion | Đối chiếu kết quả thực tế với kết quả mong đợi |
| Helper và test double | Hỗ trợ thao tác lặp lại hoặc thay thế phụ thuộc khi cần cô lập |
| Reporter | Tổng hợp kết quả thành log và các định dạng báo cáo |

Assertion cần dựa trên yêu cầu hoặc đặc tả của chức năng. Nếu chỉ dùng hành vi hiện tại của SUT làm chuẩn, test có thể xác nhận lại một lỗi đang tồn tại. Chẳng hạn, kiểm tra checkout không nên chỉ xác nhận request trả về thành công mà còn cần đối chiếu những dữ liệu hoặc thay đổi trạng thái mà yêu cầu quy định.

Helper giúp tái sử dụng những thao tác như tạo tài khoản mẫu hoặc gửi request đã xác thực. Test double phục vụ việc thay thế một phụ thuộc để kiểm soát điều kiện kiểm tra. Trong đó, stub cung cấp câu trả lời đã định trước; fake có phần cài đặt hoạt động nhưng được đơn giản hóa; mock gắn với kỳ vọng về tương tác cần xác minh. Việc thay thế phải phù hợp mục tiêu test: một unit test có thể cô lập dịch vụ ngoài, còn test tích hợp cần giữ các tương tác thật thuộc phạm vi đang kiểm tra.

**Fixture và vòng đời kiểm thử**

Fixture là những dữ liệu, đối tượng và trạng thái mà test cần để bắt đầu từ điều kiện đã biết. Ví dụ, test giỏ hàng có thể cần một người dùng, một sản phẩm đang bán và giỏ ban đầu rỗng. Fixture không chỉ là file dữ liệu mẫu; nó có thể bao gồm đối tượng trong bộ nhớ, tài nguyên tạm và trạng thái database. Kết quả mong đợi là phần dùng để đánh giá hành vi sau thao tác, cần được phân biệt với điều kiện ban đầu do fixture cung cấp.

Một lượt kiểm thử thường được tổ chức thành bốn giai đoạn:

1. **Setup:** khởi tạo tài nguyên, tạo dữ liệu và thiết lập điều kiện cần thiết.
2. **Thực thi:** gọi chức năng hoặc gửi yêu cầu đến SUT.
3. **Assertion:** kiểm tra kết quả trả về và những thay đổi trạng thái liên quan.
4. **Teardown:** dọn dữ liệu, đóng kết nối và giải phóng tài nguyên đã sử dụng.

Ví dụ minh họa, test thêm sản phẩm vào giỏ bắt đầu bằng việc tạo người dùng, sản phẩm và giỏ rỗng. Sau đó, test gọi API thêm sản phẩm, kiểm tra sản phẩm cùng số lượng trong giỏ, rồi dọn dữ liệu đã tạo. Cách tổ chức này giúp xác định rõ điều kiện xuất phát, hành động được kiểm tra và trạng thái cần khôi phục cho lần chạy tiếp theo.

Trong Jest, `beforeEach` và `afterEach` hỗ trợ công việc trước và sau từng test; `beforeAll` và `afterAll` hỗ trợ công việc một lần trong phạm vi file hoặc nhóm test tương ứng. Những tài nguyên có thể tái sử dụng được khởi tạo chung để giảm chi phí, nhưng dữ liệu mà test thay đổi vẫn cần được kiểm soát. Cleanup nên được đặt trong cơ chế teardown của framework để vẫn được thực hiện khi assertion thất bại, thay vì chỉ đặt sau assertion trong thân test.

Các thao tác bất đồng bộ cũng phải được chờ đúng cách. Nếu setup chưa tạo xong dữ liệu mà test đã gọi API, lỗi có thể xuất hiện tùy thời điểm thực thi. Tương tự, nếu test kết thúc trước khi assertion bất đồng bộ hoàn tất, kết quả ghi nhận có thể không phản ánh đầy đủ việc kiểm tra. Trả về Promise hoặc sử dụng `async`/`await` đúng cách giúp runner biết khi nào thao tác thực sự hoàn thành.

**Tính độc lập và khả năng tái lập**

Test độc lập không cần một test khác chạy trước để tạo dữ liệu hoặc thiết lập điều kiện cho nó. Chẳng hạn, test giỏ hàng cần tự chuẩn bị tài khoản sử dụng, thay vì mặc định test đăng nhập đã tạo tài khoản đó. Khi các test phụ thuộc lẫn nhau, thay đổi thứ tự, chạy riêng hoặc chạy song song có thể làm kết quả khác đi dù chức năng được kiểm tra không đổi.

Khả năng tái lập là khả năng thực hiện lại kiểm thử với những điều kiện đã xác định và thu được kết quả nhất quán. Để đạt mục tiêu này, harness cần kiểm soát những trạng thái có thể ảnh hưởng như dữ liệu database, biến toàn cục, mock, file tạm và tài nguyên dùng chung. Đối với thời gian, dữ liệu ngẫu nhiên hoặc dịch vụ bên ngoài, cần cố định hoặc kiểm soát chúng khi phù hợp với mục tiêu kiểm thử. Chạy test tuần tự có thể giảm xung đột tài nguyên nhưng không tự loại bỏ phụ thuộc trạng thái.

Ngoài dọn dữ liệu, harness cần đóng các kết nối, timer và tài nguyên đã mở để tránh ảnh hưởng đến test sau hoặc khiến tiến trình không kết thúc. Kết quả của runner cần được chuyển thành báo cáo dễ đọc và giữ nguyên trạng thái pass/fail khi đưa vào CI. Một harness được tổ chức tốt giúp nhóm phân biệt lỗi của SUT với lỗi do điều kiện kiểm thử, đồng thời cung cấp nền tảng cho việc điều tra flaky test ở các mục tiếp theo.

### 2.3. Runner / agent

**Vai trò trong quá trình thực thi pipeline**

File pipeline mô tả công việc cần làm, nhưng các lệnh như cài dependency, build và test vẫn cần một môi trường để thực thi. Runner hoặc agent là thành phần nhận công việc từ hệ thống CI và thực hiện các lệnh trên môi trường đó. Môi trường có thể được bố trí trên máy vật lý, máy ảo hoặc trong container, với hệ điều hành, runtime và công cụ phù hợp với dự án.

Việc tách điều phối khỏi thực thi giúp hệ thống phân công công việc đến nơi có khả năng xử lý, theo dõi trạng thái và thu thập phản hồi. Chẳng hạn, hệ thống CI lựa chọn nơi chạy một job kiểm thử Node.js; runner hoặc agent tại đó lấy mã nguồn, gọi các lệnh npm rồi gửi lại log và trạng thái. Test runner như Jest được gọi trong bước test và chịu trách nhiệm chạy bộ kiểm thử. Hai lớp runner này có nhiệm vụ khác nhau: một lớp thực thi công việc CI, một lớp thực thi các test bên trong công việc đó.

**Runner trong GitHub Actions**

Trong GitHub Actions, mỗi job được giao cho một runner phù hợp với cấu hình `runs-on`. Runner chuẩn bị workspace để làm việc với mã nguồn và thực hiện các step của job. Các step có thể cài công cụ, chạy script hoặc gọi action. Những job riêng biệt không nên mặc định chia sẻ cùng workspace; nếu cần sử dụng đầu ra của nhau, workflow phải có cơ chế truyền dữ liệu phù hợp.

GitHub Actions hỗ trợ hai mô hình chính:

- **GitHub-hosted runner:** do GitHub cấp phát và bảo trì. Với mô hình máy ảo thông thường, mỗi job chạy trên một VM mới có sẵn một số công cụ. Nhóm giảm được công sức quản lý máy chạy, nhưng vẫn cần khai báo những phiên bản công cụ mà dự án yêu cầu.
- **Self-hosted runner:** do người sử dụng triển khai và quản lý. Mô hình này cho phép chủ động phần cứng, phần mềm và kết nối tới dịch vụ nội bộ. Đổi lại, bên vận hành chịu trách nhiệm duy trì môi trường, cập nhật công cụ và xử lý dữ liệu còn lại giữa các lần chạy.

Sự khác nhau này ảnh hưởng cách chuẩn bị môi trường. Với máy được cấp phát mới, những dữ liệu cần tái sử dụng phải được lưu và khôi phục có chủ đích. Với máy tồn tại lâu dài, các file hoặc tiến trình từ job trước có thể còn lại nếu không được dọn. Vì vậy, khả năng tái lập phụ thuộc cả mô hình runner và cách workflow quản lý trạng thái.

**Mô hình controller–agent trong Jenkins**

Jenkins sử dụng controller để quản lý cấu hình, job, hàng đợi và điều phối công việc. Agent là tiến trình kết nối với controller để thực hiện tác vụ trên node tương ứng. Node biểu diễn môi trường thực thi mà Jenkins quản lý; các công cụ phục vụ build và test phải có sẵn hoặc được chuẩn bị tại môi trường đó.

Một node có thể được gắn các **label** mô tả khả năng, chẳng hạn hỗ trợ Linux hoặc có môi trường Node.js. Pipeline sử dụng label để yêu cầu nơi chạy phù hợp. **Executor** là một vị trí thực thi tác vụ trên node; số executor ảnh hưởng lượng công việc có thể chạy đồng thời. Executor không phải một CPU được cấp riêng, nên tăng số executor không đồng nghĩa tăng tài nguyên xử lý và có thể làm các công việc cạnh tranh tài nguyên.

Jenkins có thể chạy tác vụ trên built-in node trong controller. Tuy nhiên, bố trí agent riêng giúp tách tải build/test khỏi hoạt động điều phối và hạn chế việc script build tác động trực tiếp đến môi trường controller. Có thể đặt số executor của built-in node bằng 0 để chỉ thực thi build trên các agent đã cấu hình. Việc tách agent không bắt buộc phải sử dụng một máy vật lý khác; điều quan trọng là cách bố trí và quản lý môi trường thực thi.

Agent có thể được cấu hình cố định để dùng qua nhiều build hoặc được cấp phát tạm thời theo nhu cầu. Khai báo `agent` trong Jenkinsfile xác định nơi thực thi toàn bộ pipeline hoặc từng stage, tùy vị trí khai báo. Như vậy, việc môi trường tồn tại lâu dài hay được tạo mới là lựa chọn triển khai, không phải sự khác biệt bắt buộc giữa Jenkins và GitHub Actions.

![Vai trò điều phối và thực thi trong GitHub Actions và Jenkins](assets/theory/runner-agent.png)

*Hình 3. Phân biệt thành phần điều phối với môi trường thực thi công việc trên GitHub Actions và Jenkins.*

**Môi trường thực thi và độ tin cậy của kết quả**

Kết quả kiểm thử phụ thuộc cả mã nguồn lẫn điều kiện chạy. Một test pass trên máy cá nhân nhưng fail trên CI có thể liên quan đến khác biệt runtime, dependency, dữ liệu hoặc dịch vụ phụ thuộc. Các điều kiện như múi giờ, tốc độ máy và mức độ chạy song song cũng có thể làm lộ hành vi không ổn định. Do đó, lựa chọn runner hoặc agent cần đi kèm việc xác định rõ môi trường và ghi nhận những thông tin cần thiết để chạy lại khi có lỗi.

Các thông tin quan trọng gồm phiên bản mã nguồn, hệ điều hành, runtime, trình quản lý package, bộ lệnh và cấu hình kiểm thử. Workspace cần được chuẩn bị để tránh dùng nhầm dữ liệu hoặc báo cáo cũ. Đồng thời, cần phân biệt việc làm sạch workspace với việc giữ cache có chủ đích, vì cache có thể được lưu ngoài workspace. Container hỗ trợ đóng gói môi trường, nhưng dữ liệu, cấu hình và dịch vụ bên ngoài vẫn cần được quản lý.

Môi trường thực thi cũng ảnh hưởng thời gian chạy. Hai pipeline sử dụng cùng bộ test có thể cho thời gian khác nhau do cấu hình máy, mạng, cache hoặc các công việc khác đang chia sẻ tài nguyên. Khi đối chiếu GitHub Actions với Jenkins, cần ghi nhận các điều kiện này và phân biệt thời gian chờ, thời gian chuẩn bị môi trường, thời gian test với tổng thời gian pipeline. Đây là cơ sở để diễn giải phép đo phù hợp với cấu hình thực tế đã sử dụng.

### 2.4. Cache và artifact

**Khái niệm và mục đích sử dụng**

Trong quá trình chạy pipeline, nhiều dữ liệu được tải về hoặc tạo ra trong workspace. Nếu môi trường thực thi được thu hồi sau khi job kết thúc, những dữ liệu này không còn sẵn cho lần chạy tiếp theo hoặc cho người cần xem lại kết quả. Cache và artifact cung cấp hai cách giữ dữ liệu với mục đích khác nhau: cache phục vụ tái sử dụng để giảm công việc lặp lại, còn artifact lưu đầu ra của một lần chạy để sử dụng hoặc kiểm tra sau đó.

**Cache** thường được dùng cho dữ liệu có thể tạo lại nhưng tốn thời gian, chẳng hạn package đã tải hoặc kết quả trung gian của quá trình build. **Artifact** có thể là gói ứng dụng, log, báo cáo kiểm thử, coverage hoặc ảnh chụp lỗi. Một pipeline có thể sử dụng cả hai: khôi phục cache để cài dependency nhanh hơn, sau đó lưu báo cáo của lượt kiểm thử hiện tại làm artifact.

**Cơ chế hoạt động của cache**

Cache hoạt động bằng cách lưu dữ liệu có thể tái sử dụng và khôi phục chúng khi có nhu cầu phù hợp. Ví dụ, trong lần chạy đầu, trình quản lý package tải các thư viện từ registry. Nếu dữ liệu package đã tải được lưu lại, lần chạy sau có thể dùng chúng để giảm lượng dữ liệu phải tải qua mạng. Tuy nhiên, pipeline vẫn phải có khả năng tải hoặc tạo lại dữ liệu khi cache không tồn tại; cache là cơ chế tối ưu, không phải điều kiện bắt buộc để kết quả kiểm tra đúng.

**Khóa cache** dùng để nhận diện tập dữ liệu cần khôi phục. Khóa có thể kết hợp hệ điều hành, thông tin công cụ và hash của lockfile, tùy loại dữ liệu được lưu. Khi lockfile thay đổi, hash thay đổi để phân biệt tập dependency tương ứng. Cách thiết kế khóa cần phản ánh những yếu tố làm dữ liệu cũ không còn phù hợp, thay vì dùng chung một khóa cho mọi trạng thái của dự án.

Kết quả tra cứu thường được mô tả bằng **cache hit** và **cache miss**. Với `actions/cache`, hit có nghĩa tìm được khóa khớp chính xác. Khi không có khóa khớp chính xác, công cụ có thể thử các khóa dự phòng nếu được cấu hình. Vì vậy, việc khôi phục được một cache theo khóa dự phòng chưa đồng nghĩa cache đó khớp hoàn toàn với khóa hiện tại. Cơ chế lưu cache mới cũng có điều kiện riêng; chẳng hạn, cách lưu mặc định của `actions/cache` phụ thuộc việc job kết thúc thành công.

Đối với npm, cần phân biệt cache package với thư mục `node_modules`. Cơ chế cache npm của `actions/setup-node` giữ dữ liệu cache của trình quản lý package, không lưu `node_modules`. Sau khi khôi phục cache, `npm ci` vẫn cài dependency theo lockfile. Lệnh này xóa `node_modules` có sẵn, không tự cập nhật lockfile và báo lỗi nếu dependency khai báo trong `package.json` không phù hợp với lockfile. Nhờ đó, tái sử dụng package đã tải và tạo lại cây dependency là hai công việc có thể kết hợp trong cùng một lượt chạy.

Cache hit không bảo đảm tổng pipeline luôn nhanh hơn. Pipeline còn có thể mất thời gian khôi phục cache, giải nén, cài đặt hoặc thực hiện những công việc khác. Mức cải thiện cần được quan sát ở đúng bước chịu tác động, đồng thời ghi nhận điều kiện của các lượt đo. Với môi trường tồn tại lâu dài, dữ liệu được giữ trên máy cũng cần được quản lý có chủ đích; chỉ có file còn lại từ lần chạy trước chưa đủ chứng minh cơ chế cache đã được cấu hình và hoạt động đúng.

**Artifact và khả năng truy vết kết quả**

Artifact giữ sản phẩm hoặc bằng chứng được tạo ra trong một run/build cụ thể. Trong pipeline kiểm thử, log cho biết quá trình thực thi, báo cáo JUnit XML mô tả kết quả từng test, còn báo cáo coverage cung cấp dữ liệu về phần mã được thực thi. Những file này giúp người review xem lại kết quả sau khi job kết thúc và hỗ trợ điều tra mà không phải chạy lại ngay từ đầu.

Việc tạo file và việc lưu file thành artifact là hai bước riêng. File nằm trong workspace chưa chắc có thể truy cập sau khi môi trường chạy bị thu hồi. Pipeline cần chỉ rõ dữ liệu cần upload hoặc archive, đặt tên dễ nhận diện và áp dụng thời hạn lưu phù hợp. GitHub Actions có cơ chế upload/download artifact; Jenkins có thể dùng `archiveArtifacts` để lưu file cùng build. Artifact cũng có thể được dùng để chuyển đầu ra giữa các job hoặc các giai đoạn của quy trình.

![Phân biệt luồng dữ liệu của cache và artifact](assets/theory/cache-artifact.png)

*Hình 4. Cache phục vụ tái sử dụng dữ liệu, còn artifact giữ đầu ra gắn với một lần chạy cụ thể.*

Các lần kiểm thử thất bại đặc biệt cần log và báo cáo để xác định nguyên nhân. Vì vậy, bước thu thập bằng chứng cần được cấu hình để vẫn thực hiện khi bước test lỗi, đồng thời giữ nguyên trạng thái thất bại của bước kiểm tra. Nếu lỗi xảy ra trước khi test tạo báo cáo, pipeline chỉ có thể lưu những dữ liệu đã có, chẳng hạn log cài dependency. Thu thập bằng chứng không có nghĩa mọi trường hợp thất bại đều sinh đầy đủ báo cáo test và coverage.

**Phân biệt và sử dụng trong pipeline kiểm thử**

*Bảng 3. Phân biệt cache và artifact*

| Tiêu chí | Cache | Artifact |
| --- | --- | --- |
| Mục đích chính | Giảm công việc phải thực hiện lại | Lưu sản phẩm và bằng chứng của lần chạy |
| Dữ liệu thường dùng | Package đã tải, dữ liệu build có thể tái sử dụng | Log, báo cáo test, coverage, gói ứng dụng |
| Cách nhận diện | Khóa, phiên bản và phạm vi cache | Run/build và tên artifact |
| Khi dữ liệu không có | Có thể tải hoặc tạo lại theo quy trình | Có thể thiếu bằng chứng để xem lại lần chạy |
| Quan hệ với lần chạy sau | Có thể được khôi phục nếu phù hợp | Được dùng khi cần đúng đầu ra đã lưu |

Cache và artifact không nên thay thế vai trò cho nhau. Đưa báo cáo cũ vào cache rồi khôi phục như kết quả kiểm thử mới sẽ làm sai ý nghĩa của bằng chứng. Ngược lại, lưu toàn bộ dữ liệu dependency thành artifact chưa tạo ra cơ chế lựa chọn và khôi phục cache phù hợp cho những lần chạy sau.

Để kết quả có thể truy vết, mỗi bộ artifact cần gắn với phiên bản mã nguồn, run/build và điều kiện thực thi tương ứng. Thư mục báo cáo nên được chuẩn bị để tránh lưu nhầm file còn sót từ lượt trước. Việc giữ đúng dữ liệu đầu vào có thể tái sử dụng và đúng đầu ra cần kiểm tra giúp pipeline vừa giảm thời gian xử lý, vừa cung cấp bằng chứng đáng tin cậy cho quá trình đánh giá.

### 2.5. Coverage và quality gate

**Khái niệm code coverage**

Code coverage là phép đo mức độ mã nguồn được thực thi khi chạy một bộ kiểm thử. Công cụ coverage ghi nhận những phần mã đã được chạy và tổng hợp thành các tỷ lệ trên phạm vi mã được đưa vào thống kê. Thông tin này giúp nhận diện những phần chưa được test chạm tới, từ đó định hướng việc bổ sung kiểm thử.

Công thức tổng quát là:

`Coverage (%) = số phần tử mã đã được thực thi / tổng số phần tử mã thuộc phạm vi đo × 100`

“Phần tử mã” có thể là câu lệnh, dòng mã, hàm hoặc nhánh, tùy tiêu chí đang xét. Ví dụ minh họa, nếu có 80 trong tổng số 100 câu lệnh thuộc phạm vi đo được thực thi, statement coverage là 80%. Đây là tỷ lệ thực thi câu lệnh, không có nghĩa 80% yêu cầu nghiệp vụ đã được kiểm tra hoặc phần mềm có xác suất đúng 80%.

Coverage cũng khác với tỷ lệ test pass. Một bộ test có thể đều pass nhưng chỉ chạm tới một phần nhỏ mã nguồn; ngược lại, một lượt chạy có test fail vẫn có thể tạo báo cáo coverage. Khi diễn giải kết quả, cần xem riêng test nào đạt yêu cầu và phần mã nào đã được thực thi.

**Các mức coverage thường dùng**

*Bảng 4. Các tiêu chí code coverage thường dùng*

| Tiêu chí | Nội dung đo | Ý nghĩa khi đọc báo cáo |
| --- | --- | --- |
| Statement coverage | Tỷ lệ câu lệnh được thực thi | Chỉ ra những câu lệnh chưa được chạy |
| Line coverage | Tỷ lệ dòng mã thực thi được đã được chạy | Giúp xác định vị trí chưa được test chạm tới trong mã nguồn |
| Function coverage | Tỷ lệ hàm được gọi ít nhất một lần | Chỉ ra những hàm chưa được sử dụng trong lần kiểm thử |
| Branch coverage | Tỷ lệ nhánh được đi qua theo cách công cụ ghi nhận | Cho biết những nhánh xử lý chưa được thực thi |

Các tiêu chí cung cấp những góc nhìn khác nhau. Một dòng có thể chứa nhiều câu lệnh nên line coverage và statement coverage không nhất thiết bằng nhau. Một hàm đã được gọi có thể vẫn còn các nhánh chưa chạy. Branch coverage giúp quan sát các nhánh rẽ, nhưng không đồng nghĩa đã kiểm tra mọi đường đi hoặc mọi tổ hợp điều kiện trong chương trình.

Ví dụ, một hàm có điều kiện `quantity > 0`: nếu đúng thì chấp nhận, nếu sai thì từ chối. Test với `quantity = 1` đã gọi hàm nhưng chỉ đi qua nhánh chấp nhận. Thêm test với `quantity = 0` giúp đi qua cả nhánh từ chối. Tuy nhiên, nếu đặc tả yêu cầu số lượng phải là số nguyên dương, hai test này vẫn có thể bỏ sót việc hàm chấp nhận `quantity = 1.5`. Đi qua cả hai nhánh chưa chứng minh việc kiểm tra dữ liệu đầu vào đã đầy đủ.

**Ý nghĩa và giới hạn của coverage**

Coverage phản ánh việc thực thi mã, không trực tiếp đánh giá độ mạnh của assertion. Chẳng hạn, một test checkout có thể chạy qua logic tính tổng tiền nhưng chỉ kiểm tra mã HTTP trả về. Nếu tổng tiền sai mà response vẫn thành công, test có thể pass dù phần mã tính tiền đã được coverage ghi nhận. Vì vậy, cần kết hợp coverage với việc xem xét đặc tả, dữ liệu đầu vào, điều kiện biên và các assertion thực sự được sử dụng.

Giá trị của coverage là giúp tìm khoảng trống kiểm thử và ưu tiên những phần cần kiểm tra thêm, đặc biệt ở nhánh lỗi hoặc chức năng có ảnh hưởng lớn. Không có một ngưỡng coverage duy nhất phù hợp cho mọi dự án. Mức yêu cầu cần dựa trên rủi ro, độ quan trọng của chức năng và mục tiêu kiểm thử; việc tăng tỷ lệ bằng các test ít giá trị không thay thế được kiểm tra hành vi nghiệp vụ.

Phạm vi thống kê cũng ảnh hưởng kết quả. Nếu chỉ tính những file được test import, các file hoàn toàn chưa có test có thể không xuất hiện trong mẫu số. Trong Jest, cấu hình `collectCoverageFrom` cho phép xác định tập file cần đưa vào phép đo, kể cả những file chưa được test thực thi. Khi so sánh các lượt chạy, cần giữ rõ phạm vi file, cấu hình công cụ và loại coverage được sử dụng.

Dữ liệu coverage có thể được xuất thành bảng trên console, báo cáo HTML, JSON hoặc XML như Cobertura. Các định dạng phục vụ cách đọc và tích hợp khác nhau; khi so sánh tỷ lệ cần xác định đúng nguồn và tiêu chí, tránh lấy line coverage của nguồn này so với branch coverage của nguồn khác. Báo cáo nên cho phép truy từ tỷ lệ tổng hợp đến file hoặc vị trí mã còn thiếu kiểm thử.

**Quality gate, status check và branch protection**

Quality gate là tập điều kiện dùng để quyết định một thay đổi có được chuyển sang bước tiếp theo hay không. Trong pipeline kiểm thử, điều kiện có thể gồm lint và build thành công, test không có lỗi, hoặc coverage đạt ngưỡng đã thống nhất. Ví dụ minh họa, một dự án có thể yêu cầu toàn bộ test pass, line coverage ít nhất 80% và branch coverage ít nhất 70%. Những tỷ lệ này chỉ minh họa cách đặt điều kiện, không phải cấu hình thực nghiệm của nhóm hay tiêu chuẩn chung cho mọi dự án.

Thu thập dữ liệu và thực thi điều kiện là hai công việc riêng. Chỉ tạo báo cáo hoặc badge coverage chưa làm pipeline thất bại khi tỷ lệ thấp. Muốn dùng coverage làm quality gate, pipeline cần có bước đánh giá ngưỡng và phản ánh việc không đạt vào kết quả kiểm tra. Với Jest, `coverageThreshold` có thể được dùng để cấu hình yêu cầu coverage; nếu không đạt ngưỡng đã đặt, lần chạy sẽ thất bại.

Status check công bố trạng thái của một hoạt động kiểm tra trên commit hoặc pull request. Trên GitHub, thông tin có thể được cung cấp qua checks hoặc commit statuses; GitHub Actions tạo checks khi workflow chạy. Kết quả này giúp người review biết công việc đang chờ, đang chạy hay đã hoàn thành. Tuy nhiên, một check thất bại chỉ thực sự ngăn merge khi nó được yêu cầu bởi quy tắc áp dụng cho nhánh đích.

Branch protection cho phép đặt các yêu cầu như review và status check bắt buộc trước khi merge vào nhánh được bảo vệ. Quy trình cần chọn đúng check, bảo đảm check chạy những công việc dự kiến và kiểm tra quyền bypass. Đặc biệt, một job bị bỏ qua có thể vẫn có kết quả được GitHub chấp nhận cho yêu cầu merge; vì vậy, trạng thái thỏa điều kiện không tự chứng minh toàn bộ bộ test đã được thực hiện.

![Quan hệ giữa kết quả kiểm tra, quality gate và điều kiện merge](assets/theory/quality-gate.png)

*Hình 5. Kết quả kiểm tra được đối chiếu với các điều kiện đã cấu hình; hiệu lực chặn merge phụ thuộc quy tắc bảo vệ nhánh.*

Một quality gate có ý nghĩa cần đánh giá đúng phiên bản mã, giữ nguyên kết quả thất bại và được áp dụng trong quy trình tích hợp. Khi gate không đạt, log và báo cáo giúp xác định điều kiện nào chưa đáp ứng để nhóm xử lý và kiểm tra lại. Khi gate đạt, thay đổi đã thỏa những điều kiện được khai báo, nhưng kết quả đó vẫn cần được xem cùng các yêu cầu review và những giới hạn của bộ kiểm thử.

### 2.6. Flaky test

## 3. Khảo sát và lựa chọn công cụ

Việc lựa chọn công cụ xuất phát từ hai nhu cầu của đề tài: tự động thực thi bộ kiểm thử backend EShop và hỗ trợ phân tích các kết quả không ổn định. Nhóm xem xét khả năng chạy các lệnh kiểm tra hiện có, tích hợp với kho mã nguồn, lưu báo cáo, công sức cấu hình và yêu cầu vận hành. Đối với hướng AI, tiêu chí bổ sung là khả năng sử dụng log làm đầu vào, đưa ra giả thuyết có thể kiểm chứng và lưu lại quá trình phân tích để đối chiếu với cách làm thủ công.

Phần khảo sát tập trung vào khả năng và mức độ phù hợp của từng phương án với phạm vi seminar. Các nhận xét về công sức triển khai là đánh giá định tính theo nhu cầu của nhóm; số liệu về thời gian chạy và kết quả so sánh AI với người được trình bày ở các chương thực nghiệm.

### 3.1. Công cụ CI truyền thống

**GitHub Actions**

GitHub Actions là nền tảng tự động hóa gắn với repository trên GitHub. Workflow có thể được kích hoạt bởi các sự kiện như push hoặc pull request, sau đó giao job cho runner để thực hiện những bước đã khai báo. Đối với EShop, workflow có thể chuẩn bị Node.js, cài dependency, chạy các lệnh kiểm tra và lưu báo cáo; các thao tác này sử dụng lại bộ lệnh của dự án thay vì yêu cầu viết một bộ test riêng cho nền tảng CI.

Điểm phù hợp với nhóm là mã nguồn và phản hồi kiểm tra được quản lý trong cùng hệ thống. Người review có thể theo dõi kết quả ngay trên pull request và liên kết các check cần thiết với quy tắc bảo vệ nhánh. Việc sử dụng GitHub-hosted runner cũng giúp giảm phần hạ tầng phải tự quản lý khi bắt đầu triển khai.

Nhóm vẫn cần hiểu YAML, sự kiện kích hoạt, quyền truy cập và cách bố trí job. Tính đúng của workflow phụ thuộc vào việc cấu hình đúng bộ lệnh, thu thập báo cáo và giữ nguyên trạng thái lỗi. Về chi phí, standard GitHub-hosted runner được sử dụng miễn phí cho repository công khai; larger runner và việc lưu trữ có chính sách riêng. Vì vậy, lựa chọn công cụ cần xét loại runner và nhu cầu lưu bằng chứng, không chỉ nhìn chi phí chạy test.

**Jenkins**

Jenkins là máy chủ tự động hóa mã nguồn mở, hỗ trợ mô tả quy trình bằng Jenkinsfile và mở rộng chức năng thông qua plugin. Với agent được chuẩn bị phù hợp, Jenkins có thể thực hiện cùng các lệnh npm và bộ kiểm thử mà nhóm sử dụng trên GitHub Actions. Khả năng tự quản lý môi trường cho phép nhóm chủ động công cụ, dữ liệu được giữ giữa các build và cách công bố báo cáo.

Trong seminar, Jenkins cung cấp một mô hình triển khai khác để đối chiếu: nhóm phải thiết lập controller, agent, kết nối repository và những plugin cần cho pipeline. Qua đó, việc so sánh có thể xét cả định nghĩa pipeline, báo cáo kiểm thử lẫn công sức vận hành, thay vì chỉ so cú pháp YAML với Jenkinsfile.

Jenkins được phân phối theo giấy phép MIT, nhưng việc sử dụng vẫn cần tài nguyên máy chạy, dung lượng lưu trữ và công sức quản trị. Nhóm phải duy trì phiên bản công cụ, plugin và cấu hình kết nối. Đây là phần công việc cần tính đến khi đánh giá một hệ thống CI tự quản lý, dù không có chi phí giấy phép sử dụng Jenkins.

**GitLab CI/CD**

GitLab CI/CD hỗ trợ mô tả pipeline bằng file `.gitlab-ci.yml`, trong đó khai báo các job, stage, biến và điều kiện thực thi. Các job được thực hiện bởi runner. Về khả năng kiểm thử, nền tảng này cũng có thể chạy một dự án Node.js với các lệnh cài dependency và Jest nếu môi trường được chuẩn bị phù hợp.

GitLab CI/CD là phương án phù hợp để xem xét khi dự án sử dụng hệ sinh thái GitLab. Nền tảng cũng hỗ trợ tích hợp repository bên ngoài như GitHub; tuy nhiên, cần xem xét thêm cách đồng bộ, quyền truy cập và điều kiện của gói sử dụng. Với đề tài đã tổ chức mã nguồn và pull request trên GitHub, triển khai thêm GitLab sẽ tăng đầu mối cấu hình. Nhóm vì vậy khảo sát GitLab như một phương án thay thế, đồng thời giới hạn thực nghiệm ở hai nền tảng GitHub Actions và Jenkins để tập trung vào yêu cầu đối chiếu của đề tài.

**Đối chiếu theo nhu cầu của seminar**

*Bảng 5. Đối chiếu định tính các nền tảng CI được khảo sát*

| Tiêu chí | GitHub Actions | Jenkins | GitLab CI/CD |
| --- | --- | --- | --- |
| Định nghĩa pipeline | YAML trong `.github/workflows` | Jenkinsfile | `.gitlab-ci.yml` |
| Thành phần thực thi | GitHub-hosted hoặc self-hosted runner | Agent trên node được cấu hình | GitLab-hosted hoặc runner tự quản lý |
| Công việc cần học và thiết lập | Workflow, sự kiện, job, quyền truy cập | Pipeline, controller, agent, plugin | Job/stage, biến, điều kiện chạy, runner |
| Phù hợp với EShop | Tích hợp trực tiếp với repo và PR trên GitHub | Chạy cùng bộ lệnh; cần cấu hình kết nối GitHub | Chạy cùng bộ lệnh; cần thêm tổ chức dự án hoặc tích hợp repo ngoài |
| Chi phí và vận hành | Xét runner, hạn mức và lưu trữ; giảm phần máy chủ nếu dùng hosted runner | Giấy phép MIT; tự quản lý hạ tầng và công cụ | Phụ thuộc gói sử dụng và mô hình runner |
| Nguồn học tập và mở rộng | GitHub Docs, action và workflow mẫu | Jenkins Docs và hệ sinh thái plugin | GitLab Docs và cấu hình CI/CD mẫu |

Cả ba nền tảng đều có thể phục vụ việc chạy kiểm thử tự động. Với phạm vi này, khác biệt có ý nghĩa nằm ở cách tích hợp vào quy trình hiện có, mức độ tự quản lý và khả năng cung cấp bằng chứng dễ kiểm tra. Bảng khảo sát là cơ sở lựa chọn, chưa phải kết luận nền tảng nào chạy nhanh hơn hay cho kết quả kiểm thử tốt hơn.

### 3.2. Hướng AI

**Trợ lý ngôn ngữ hỗ trợ phân tích log**

Một hướng áp dụng AI là cung cấp log và bối cảnh cho trợ lý ngôn ngữ để hỗ trợ tóm tắt lỗi, tổ chức giả thuyết và đề xuất bước kiểm tra tiếp theo. ChatGPT được nhóm chọn để khảo sát theo cách này. Đầu vào được chuẩn bị từ kết quả CI; phản hồi cần nêu rõ quan sát nào hỗ trợ giả thuyết và thông tin nào còn thiếu.

Trong phạm vi seminar, ChatGPT được sử dụng để hỗ trợ triage trên dữ liệu nhóm cung cấp, không được tích hợp thành thành phần tự quyết định trạng thái pipeline. Cách tiếp cận này cho phép nhóm chuẩn bị cùng một bộ log cho người phân tích và AI, đồng thời lưu prompt và phản hồi để xem xét lại. Nó cũng phù hợp với bài toán muốn quan sát cách lập luận từ bằng chứng, thay vì chỉ nhận một nhãn pass/fail.

Hiệu quả phụ thuộc vào bối cảnh được cung cấp và cách đặt yêu cầu. Prompt cần nêu mục tiêu phân tích, dữ liệu được phép sử dụng và đầu ra mong muốn. Chẳng hạn, nhóm có thể yêu cầu phân biệt quan sát trong log với suy đoán, viện dẫn bằng chứng cho từng giả thuyết và đưa ra bước xác minh. Phản hồi vẫn phải được review vì lời giải thích có vẻ hợp lý có thể thiếu căn cứ hoặc bỏ sót thông tin quan trọng.

**Công cụ theo dõi kiểm thử và hỗ trợ xử lý flake**

Một hướng khác là tích hợp công cụ quan sát kiểm thử vào pipeline để thu thập dữ liệu thực thi qua nhiều lượt chạy. Datadog Test Optimization là ví dụ được khảo sát: công cụ cung cấp thông tin về các lần chạy, thời gian, lỗi và tình trạng không ổn định của test. Flaky Tests Management hỗ trợ theo dõi, triage và quản lý các test có flake trong một giao diện tập trung.

Hướng này phù hợp khi cần quan sát bộ test thường xuyên trên nhiều nhánh hoặc nhiều pipeline. Tuy nhiên, cần cấu hình thu thập dữ liệu và bảo đảm framework kiểm thử nằm trong phạm vi được hỗ trợ. Các khả năng AI như phân loại nguyên nhân hoặc gợi ý sửa còn có điều kiện dữ liệu và tích hợp tương ứng. Vì vậy, việc có một test fail rồi pass chưa đồng nghĩa đã sử dụng đầy đủ chức năng phân tích nguyên nhân bằng AI của nền tảng.

Cần phân biệt **phát hiện kết quả không ổn định** với **phân tích nguyên nhân**. Phát hiện có thể dựa trên việc quan sát cả pass và fail trong lịch sử chạy; phân tích nguyên nhân cần thêm bối cảnh và phép xác minh. Tính năng thống kê hoặc retry không tự trở thành chức năng AI. Khi đánh giá công cụ, nhóm cần chỉ rõ chức năng nào đang được sử dụng và bằng chứng nào cho thấy nó hỗ trợ được bài toán.

*Bảng 6. Đối chiếu hai cách tiếp cận hỗ trợ phân tích flaky test*

| Tiêu chí | ChatGPT với log do nhóm chuẩn bị | Công cụ tích hợp như Datadog Test Optimization |
| --- | --- | --- |
| Đầu vào | Log, lịch sử chạy, bối cảnh và prompt | Dữ liệu test được thu thập qua tích hợp |
| Cách sử dụng | Yêu cầu phân tích một trường hợp cụ thể | Theo dõi và quản lý kết quả qua nhiều lượt chạy |
| Đầu ra quan tâm | Tóm tắt, giả thuyết, bằng chứng và bước xác minh | Lịch sử, chỉ số, trạng thái flake và chức năng phân tích được hỗ trợ |
| Chuẩn bị | Chọn dữ liệu, đặt yêu cầu và lưu phản hồi | Cấu hình dịch vụ, thư viện thu thập dữ liệu và quyền truy cập |
| Giới hạn cần kiểm tra | Suy đoán thiếu căn cứ, bối cảnh thiếu, phản hồi chưa được xác minh | Điều kiện tích hợp, dữ liệu cần thiết và phạm vi tính năng của gói sử dụng |
| Vai trò trong seminar | Công cụ AI được dùng cho thí nghiệm đối chiếu | Phương án khảo sát, không triển khai trong phạm vi báo cáo |

Để phù hợp với giới hạn thời gian và mục tiêu seminar, nhóm chọn cách chuẩn bị dữ liệu rồi dùng ChatGPT hỗ trợ phân tích. Lựa chọn này giúp tập trung việc đánh giá vào chất lượng triage trên cùng đầu vào. Nó chưa khẳng định ChatGPT chính xác hơn công cụ chuyên dụng hoặc phân tích thủ công; những nhận xét về hiệu quả phải dựa trên kết quả thí nghiệm.

### 3.3. Lựa chọn của nhóm và lý do

Nhóm lựa chọn GitHub Actions làm CI chính, Jenkins làm nền tảng đối chiếu và ChatGPT làm công cụ hỗ trợ phân tích log. Ba công cụ đảm nhiệm những vai trò bổ sung cho nhau trong quy trình thực hiện.

*Bảng 7. Công cụ được chọn và vai trò trong đề tài*

| Công cụ | Vai trò | Nội dung cần thể hiện |
| --- | --- | --- |
| GitHub Actions | CI chính gắn với repository và pull request | Tự động kiểm tra, chạy test, lưu bằng chứng và cung cấp check cho quy trình merge |
| Jenkins | Nền tảng CI đối chiếu | Chạy bộ lệnh tương ứng, công bố báo cáo và so sánh cấu hình, môi trường, thời gian, vận hành |
| ChatGPT | Hỗ trợ triage flaky test | Phân tích cùng dữ liệu với người, đưa ra giả thuyết và bước kiểm chứng để đối chiếu |

**Lý do chọn GitHub Actions làm CI chính**

EShop của nhóm được quản lý trên GitHub, nên GitHub Actions phù hợp để gắn kiểm thử với các sự kiện push và pull request. Kết quả kiểm tra có thể được xem cùng thay đổi mã nguồn, thuận tiện cho review và áp dụng quy tắc bảo vệ nhánh. Nhóm cũng có thể sử dụng runner do GitHub cung cấp để giảm công việc chuẩn bị máy chủ CI ban đầu. Việc triển khai tập trung vào workflow và bộ lệnh kiểm thử mà dự án đã chuẩn bị.

**Lý do triển khai thêm Jenkins**

Jenkins cho phép đối chiếu với mô hình CI mà nhóm trực tiếp quản lý hạ tầng và công cụ. Việc sử dụng cùng bộ kiểm thử trên hai nền tảng giúp xem xét những khác biệt về định nghĩa pipeline, môi trường chạy, cách lưu và hiển thị báo cáo. Nhóm vì vậy sử dụng Jenkins như một phần thực nghiệm đối chiếu, thay vì chỉ mô tả nó như phương án dự phòng.

Các phép so sánh cần ghi nhận phiên bản mã nguồn, bộ lệnh và điều kiện thực thi. Nếu hai môi trường khác nhau về runtime, tài nguyên hoặc trạng thái cache, phần nhận xét phải giải thích những khác biệt đó. Mục tiêu là xác định đặc điểm phù hợp của từng cấu hình và những yêu cầu vận hành đi kèm, không suy ra ưu thế chung chỉ từ một lượt chạy.

**Lý do chọn ChatGPT cho hướng AI**

Bài toán AI của nhóm là phân tích log và lịch sử chạy của một trường hợp flake có nguyên nhân đã biết. ChatGPT phù hợp với cách tổ chức thí nghiệm này vì nhóm có thể cung cấp đầu vào cụ thể, yêu cầu định dạng câu trả lời và giữ lại phản hồi để đánh giá. Thời gian chuẩn bị được tập trung vào chất lượng dữ liệu, cách phân tích và phép xác minh, thay vì xây dựng thêm một hệ thống thu thập test telemetry.

Việc đánh giá cần tách bản phân tích thủ công khỏi phản hồi AI, sử dụng cùng dữ liệu và đối chiếu theo tiêu chí thống nhất. Những đề xuất của AI được kiểm tra bằng log và kết quả thực thi, không dùng trực tiếp để thay đổi expected result hoặc bỏ test cho pipeline thành công. Thiết kế thí nghiệm và kết quả cụ thể được trình bày ở các chương về flaky test và AI triage.

GitLab CI/CD và Datadog được giữ ở phạm vi khảo sát để làm rõ những phương án có thể áp dụng trong bối cảnh khác. Với phạm vi hiện tại, tổ hợp GitHub Actions, Jenkins và ChatGPT đáp ứng ba phần việc của đề tài: kiểm thử tự động, đối chiếu hai nền tảng CI và đánh giá hỗ trợ AI khi phân tích lỗi không ổn định.

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

1. Martin Fowler. [*Continuous Integration*](https://martinfowler.com/articles/continuousIntegration.html).
2. Martin Fowler. [*Continuous Delivery*](https://martinfowler.com/bliki/ContinuousDelivery.html).
3. GitHub Docs. [*Workflows*](https://docs.github.com/en/actions/concepts/workflows-and-actions/workflows).
4. Jenkins User Handbook. [*Pipeline*](https://www.jenkins.io/doc/book/pipeline/).
5. Jest documentation. [*Setup and Teardown*](https://jestjs.io/docs/setup-teardown).
6. Jest documentation. [*Testing Asynchronous Code*](https://jestjs.io/docs/asynchronous).
7. Martin Fowler. [*Test Double*](https://martinfowler.com/bliki/TestDouble.html).
8. pytest documentation. [*About fixtures*](https://docs.pytest.org/en/stable/explanation/fixtures.html).
9. Jest documentation. [*Configuring Jest*](https://jestjs.io/docs/configuration).
10. GitHub Docs. [*GitHub-hosted runners*](https://docs.github.com/en/actions/concepts/runners/github-hosted-runners).
11. GitHub Docs. [*Self-hosted runners*](https://docs.github.com/en/actions/concepts/runners/self-hosted-runners).
12. Jenkins User Handbook. [*Managing Nodes*](https://www.jenkins.io/doc/book/managing/nodes/).
13. Jenkins User Handbook. [*Pipeline Syntax — agent*](https://www.jenkins.io/doc/book/pipeline/syntax/#agent).
14. Jenkins User Handbook. [*Controller Isolation*](https://www.jenkins.io/doc/book/security/controller-isolation/).
15. GitHub Docs. [*Dependency caching*](https://docs.github.com/en/actions/concepts/workflows-and-actions/dependency-caching).
16. GitHub Docs. [*Dependency caching reference*](https://docs.github.com/en/actions/reference/workflows-and-actions/dependency-caching).
17. GitHub Actions. [*actions/setup-node — Caching global packages data*](https://github.com/actions/setup-node#caching-global-packages-data).
18. npm documentation. [*npm ci*](https://docs.npmjs.com/cli/v11/commands/npm-ci/).
19. GitHub Docs. [*Store and share data with workflow artifacts*](https://docs.github.com/en/actions/tutorials/store-and-share-data).
20. Jenkins Pipeline Steps Reference. [*archiveArtifacts*](https://www.jenkins.io/doc/pipeline/steps/core/#archiveartifacts-archive-the-artifacts).
21. Carlos Arguelles, Marko Ivanković và Adam Bender. [*Code Coverage Best Practices*](https://testing.googleblog.com/2020/08/code-coverage-best-practices.html). Google Testing Blog.
22. GitHub Docs. [*About protected branches*](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches).
23. GitHub Docs. [*Status checks*](https://docs.github.com/en/pull-requests/reference/status-checks).
24. Istanbul documentation. [*Using Alternative Reporters*](https://istanbul.js.org/docs/advanced/alternative-reporters/).
25. GitHub Docs. [*Understanding GitHub Actions*](https://docs.github.com/en/actions/get-started/understand-github-actions).
26. GitHub Docs. [*GitHub Actions billing*](https://docs.github.com/en/billing/concepts/product-billing/github-actions).
27. Jenkins. [*MIT License*](https://www.jenkins.io/license/).
28. GitLab Docs. [*Get started with GitLab CI/CD*](https://docs.gitlab.com/ci/).
29. GitLab Docs. [*Runners*](https://docs.gitlab.com/ci/runners/).
30. GitLab Docs. [*CI/CD for external repositories*](https://docs.gitlab.com/ci/ci_cd_for_external_repos/).
31. OpenAI Docs. [*Use ChatGPT*](https://learn.chatgpt.com/docs/use-chatgpt).
32. OpenAI Docs. [*Prompting*](https://learn.chatgpt.com/docs/prompting).
33. Datadog documentation. [*Getting Started with Test Optimization*](https://docs.datadoghq.com/getting_started/test_optimization/).
34. Datadog documentation. [*Flaky Tests Management*](https://docs.datadoghq.com/tests/flaky_management/).
35. Datadog. [*Test Optimization*](https://www.datadoghq.com/product/test-optimization/).

## Phụ lục A. Truy vết số liệu
