# Minh chứng Jenkins cho section 6

Chương 6 sử dụng kết quả **đã lưu của build #4**, job `eshop-ci`, nhánh `cicd/jenkins`, commit `a559c53acb9a3b03cc05fca8a855bcc59c8eaae8`, ngày 16/10/2026. Không chạy một build Jenkins mới trong quá trình viết chương này.

Các file được sao chép từ checkout repository mã nguồn `eshop-sut-group10` tại commit `fe41113025e58eb835540cdc16d6fd8dc7be697b`. Nội dung Jenkinsfile và các file hạ tầng được đối chiếu Git, không có khác biệt với commit build #4. Thông tin môi trường và plugin D-01 là snapshot riêng, được thu trước build baseline trong cùng ngày.

## File và nguồn

| Bản lưu tại đây | Nguồn tính từ gốc repository mã nguồn | Vai trò |
| --- | --- | --- |
| `console-build-4.txt` | `docs/evidence/D-02/console-build-4.txt` | Console đầy đủ của build #4, đã bỏ ConsoleNote nhị phân và định dạng ANSI theo tài liệu nguồn |
| `build-4/results.json` | `docs/evidence/D-02/build-4/results.json` | Kết quả Jest theo từng test và thông báo lỗi |
| `build-4/junit.xml` | `docs/evidence/D-02/build-4/junit.xml` | Báo cáo JUnit được publisher sử dụng |
| `build-4/cobertura-coverage.xml` | `docs/evidence/D-02/build-4/cobertura-coverage.xml` | Báo cáo đầu vào của Coverage plugin |
| `build-4/coverage-summary.json` | `docs/evidence/D-02/build-4/coverage-summary.json` | Các bộ đếm coverage do Jest xuất |
| `build-4/environment.log` | `docs/evidence/D-02/build-4/environment.log` | Môi trường của đúng build #4 |
| `build-4/metadata.json` | `docs/evidence/D-02/build-4/metadata.json` | Metadata chọn lọc: job, build, SHA, thời gian và số liệu tổng hợp |
| `d01-environment.log` | `docs/evidence/D-01/environment.log` | Phiên bản controller, Docker và agent tại lần dựng môi trường |
| `plugins.txt` | `docs/evidence/D-01/plugins.txt` | Danh sách plugin của snapshot D-01; Stage View được cài bổ sung sau snapshot này |
| `config/Jenkinsfile` | `Jenkinsfile` | Các stage, xử lý lỗi, publisher và archive |
| `config/compose.yaml`, `config/compose.agent.yaml`, `config/agent.Dockerfile` | Các file cùng tên trong `infra/jenkins/` | Cấu hình controller, agent, runtime và volume |

Ảnh ở thư mục cha được sao chép nguyên file từ `user_guide/assets/jenkins/` của repo tài liệu: sơ đồ kiến trúc dùng chung; ảnh Stage View có hàng build #4; ảnh Tests có breadcrumb xác nhận build #4 của nhánh `cicd/jenkins`. Không dùng ảnh chi tiết test ở job `develop` build #1 làm ảnh của baseline này.

## Kiểm tra đã thực hiện

- Đọc JSON và parse cả hai báo cáo XML.
- Đối chiếu Jest JSON, JUnit XML và metadata: 39 test, 29 pass, 10 fail, 0 skip; JUnit ghi `errors=0`.
- So sánh đầy đủ ID fail với baseline và với artifact run #11 của GitHub Actions đã lưu ở chương 5; các tập ID trùng nhau.
- Đối chiếu coverage summary với metadata và console: statement 57,98%, branch 39,09%, function 39,72%, line 61,48%.
- Giữ riêng số Coverage plugin đọc từ Cobertura: line 174/283, branch 48/115; không thay số branch của Jest 52/133 bằng số plugin.
- Đối chiếu test `A-CHECKOUT-02` và route checkout ở commit `a559c53`; log ghi expected 60000000, received 1.
- Kiểm tra diff giữa commit B-04 `a7f0923` và commit build #4: SUT, các file test thực thi/helper và cấu hình Jest không đổi.

Thời gian **17,600 giây** lấy từ `durationMs=17600` trong metadata đã lưu. Thời gian **1,960 giây** lấy từ dòng tổng kết Jest của console. JUnit có trường thời gian riêng `1.942`; không dùng nó thay cho tổng thời gian Jest hoặc thời gian build.

## Tính toàn vẹn của bản sao

Các file được sao chép nguyên byte từ checkout local, không chỉnh sửa kết quả. `sha256sums.txt` tại đây ghi SHA-256 của các bản sao hiện có, dùng để kiểm tra khi chuyển bộ tài liệu.

Checkout Windows đã chuyển nhiều file văn bản sang CRLF. Khi đối chiếu manifest D-02 cũ trong repo nguồn, sáu file console/report khớp hash sau khi chỉ chuẩn hóa CRLF thành LF; riêng `build-4/metadata.json` chưa khớp manifest cũ theo các dạng dòng đã kiểm tra. Chưa xác định nguyên nhân khác biệt hash của metadata. Vì vậy, không tuyên bố toàn bộ bản sao khớp byte với manifest cũ; số liệu test và coverage được đối chiếu thêm với JSON, XML và console. Manifest mới chỉ chứng nhận nội dung bản sao được lưu kèm báo cáo này.

Đường dẫn `localhost:8080` trong metadata chỉ có ý nghĩa trên máy chạy Jenkins. Bản lưu giúp kiểm tra dữ liệu khi không truy cập được Jenkins hoặc khi chính sách lưu giữ đã xóa build cũ; không xác nhận môi trường Jenkins hiện tại vẫn giữ nguyên snapshot.
