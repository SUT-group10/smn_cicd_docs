# Đáp án — chỉ công bố sau phần hỏi AI

Nguồn và cách chọn dữ liệu ở [source.json](source.json). Các con số bên dưới thuộc **ba run được chọn**, không phải kết quả toàn bộ batch 10 run.

## 1. Kết quả cần đọc được

| Run | Chuỗi attempt | Retry dùng | Kết quả cuối |
| --- | --- | ---: | --- |
| run-02 | FAIL → PASS | 1 | PASS |
| run-04 | FAIL → FAIL → FAIL | 2 | FAIL |
| run-06 | PASS | 0 | PASS |

Tổng: **3 run, 6 attempt, 3 retry**; lần đầu 1 PASS/2 FAIL; kết quả cuối 2 PASS/1 FAIL; toàn bộ attempt có 2 PASS/4 FAIL. Retry tối đa hai không có nghĩa run nào cũng chạy ba lần: pass thì dừng.

Mọi attempt fail ở `HISTORY_ORDER_COUNT`, expected 1/actual 0. Các request submit/history/confirmation đều HTTP 200; không có dấu vết HTTP bị từ chối trong phần cấp. Vì vậy đây là failure assertion cần giải thích bằng snapshot và thứ tự, không thể coi tất cả request 200 là test đã pass.

YAML chỉ cho thấy chuẩn bị Node, cài dependency và quy tắc upload. `if: !cancelled()` cho phép thử upload khi bước trước fail mà run không bị cancel; nó không đổi exit code của test hoặc chứng minh artifact đã tồn tại. Không suy kết quả test hoặc cấu hình chạy bị lược từ đoạn YAML; dùng metadata/log đã cấp.

## 2. Dẫn chứng quyết định

- **run-02/attempt 1:** L0005 gửi submit ở 172.552 ms; L0007 gửi history 292.894 ms; L0010 nhận history 0 đơn tại 297.566 ms; L0012 mới nhận submit 200 tại 329.849 ms. L0017 confirmation thấy 1 đơn tại 333.382 ms nhưng L0024 assertion history vẫn fail.
- **run-02/attempt 2:** L0034 nhận submit tại 490.677 ms; L0035 gửi history tại 522.363 ms; L0038 nhận 1 đơn và L0050 assertion pass. Đây là fixture mới, không phải đơn của attempt trước vừa xuất hiện.
- **run-06/attempt 1:** L0008 nhận submit tại 209.347 ms, trước L0009 gửi history tại 304.250 ms. L0012 history có 1 đơn và L0024 pass.
- **run-04:** các assertion tại L0024/L0050/L0076 đều expected 1/actual 0; cả ba attempt có history nhận rỗng trước submit response. Retry không thay quan hệ đồng bộ của test.

Điểm dễ nhầm: assertion fail có thể được ghi **sau** submit/confirmation. Test giữ snapshot history đã lấy trước đó; assertion chạy muộn hơn không làm snapshot cũ biến thành dữ liệu mới. Cần xét thời điểm request đọc và response của nó, không chỉ thời điểm dòng assertion.

Tất cả mốc so ở cùng run/attempt. Chưa có mốc commit DB trong phần cấp; response hoàn tất không tự là transaction commit. Một phần dữ liệu được chọn vì có các kết quả khác nhau không dùng để suy tỷ lệ lỗi dài hạn.

## 3. Một cây ba tầng hợp lệ

```text
Tầng 1: Cùng revision có attempt kiểm tra số đơn PASS/FAIL
├─ Tầng 2: Điều phối giữa thao tác ghi và đọc
│  └─ Tầng 3: History có thể đọc khi submit chưa hoàn tất
└─ Tầng 2: Thời điểm dữ liệu nhìn thấy qua đường đọc
   └─ Tầng 3: Bản ghi có thể chưa khả kiến ngay trên đường truy vấn history
```

Nhánh đầu được timing hỗ trợ mạnh hơn; nhánh sau là hướng cần phân biệt, chưa xác nhận lỗi read-after-write của backend. Chấp nhận cây khác nếu đủ ba tầng, có dẫn chứng và biết thu hẹp/bác bỏ. Không bắt tạo ba nguyên nhân ngang nhau hoặc suy chính xác hàm timer từ log.

Ba next-checks hợp lý:

1. Rà source điều phối hai request, kiểm tra request/Promise nào thực sự được đợi. Nếu history không phụ thuộc submit completion, nhánh điều phối được hỗ trợ; nếu có chờ đúng, cần kiểm tra thêm mốc log và đường đọc.
2. Chạy biến thể chỉ đọc sau POST 200, giữ fixture/oracle và tắt retry. Nếu không còn đọc sớm và snapshot đúng, hỗ trợ cơ chế; nếu vẫn rỗng, giả thuyết chưa đủ.
3. Thu mốc commit/query gắn request ID và fixture. Đọc trước commit hỗ trợ snapshot sớm; đọc sau commit vẫn thiếu đơn đòi điều tra ghi/đọc/identity.

Các bước này là đề xuất ở thời điểm triage, không được ghi thành đã chạy trong hoạt động.

## 4. Nguyên nhân thật — chỉ dành cho phần công bố

Trong thí nghiệm v2, test khởi phát POST qua gateway tạo latency sampled 20–220 ms, rồi chế độ `timed` chỉ chờ cố định 120 ms trước GET history. GET chưa phụ thuộc POST hoàn tất. Sau đó test mới chờ POST, đọc confirmation và assertion trên **snapshot history ban đầu**. Như vậy biến thiên latency ảnh hưởng việc lần đọc đó có thấy đơn hay chưa; verdict do HTTP/assertion thật quyết định.

Bản sửa `awaited` đợi POST thành công rồi gửi history. [Batch CI 39911447981](../../../report/assets/flaky/v2/README.md#hai-batch-ci) đã có 10/10 PASS không retry, mọi POST hoàn tất trước GET. Đây là bằng chứng riêng sau triage từ hồ sơ thí nghiệm, không phải nhóm tham gia hoặc AI vừa tự thực hiện khi đọc packet.

## 5. Đánh giá AI và bài học

Đối chiếu output thực tế của nhóm theo các mục quan sát được, không theo việc giống hệt câu chữ đáp án:

- Đếm đúng ba run/sáu attempt/ba retry và giữ verdict đầu/cuối.
- Viện dẫn đúng run/dòng, không đếm metadata và console thành lượt mới.
- Không gán HTTP 200 thành toàn test pass, không coi retry là bản sửa.
- Giữ nhãn giả thuyết khi chưa có source/commit; có đúng ba bước kiểm tra phân biệt được.
- Nêu một giá trị AI bổ sung hoặc một điểm không có ích/sai, có dẫn chứng. Đồng thuận với người vẫn là kết quả hợp lệ.

Bài học có thể là: giữ failure trước retry; chờ điều kiện hoàn tất thay vì đoán bằng thời gian; phân biệt snapshot với trạng thái mới; kiểm tra căn cứ của AI thay vì tin lời giải thích có vẻ hợp lý. Không bắt mỗi nhóm tìm một lỗi AI nếu output của lượt đó không có lỗi được xác nhận.
