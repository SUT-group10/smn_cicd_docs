# Runbook — phần API log triage

Chỉ người điều phối đọc. Phát [api-postmortem-participant.zip](../api-postmortem-participant.zip), không phát folder này hoặc toàn repo docs. Mục tiêu là người tham gia tự nối được bằng chứng, giả thuyết và phép kiểm chứng; không chấm theo việc nhớ đúng tên cơ chế trong source.

## 1. Chuẩn bị trước phiên

- Người tham gia mở được Markdown/YAML; tài khoản AI đã đăng nhập và đọc được file. Kiểm tra quyền đọc file trước giờ, không dùng năm phút hỏi AI để cài ứng dụng.
- Giải nén packet trong folder riêng. Đảm bảo nhóm chưa đọc report, User Guide có lời giải, bản Sol cũ hoặc source EShop. Nếu đã biết lời giải, ghi rõ lượt đó là diễn tập có biết trước.
- Nhóm hỏi AI dùng chính `data.md` và YAML mà con người đã nhận. Không thêm source hoặc gợi ý nguyên nhân cho một bên.
- Mở sẵn đồng hồ và [Answer_Key.md](Answer_Key.md), nhưng giữ đáp án ngoài màn hình chia sẻ tới phút 18.
- Bộ dữ liệu có sáu attempt được chọn từ ba run của batch thật; không có run mới được sinh khi biên soạn worksheet.

## 2. Điều phối theo khung đề T07

| Mốc | Người điều phối làm gì | Bằng chứng cần giữ |
| --- | --- | --- |
| 00:00–03:00 | Phát ZIP, nêu tên hai file dữ liệu, nhắc chỉ dùng thông tin được cấp | Nhóm/người ghi, giờ bắt đầu |
| 03:00–13:00 | Để nhóm đọc và lập cây ba tầng; chưa hỏi AI | Phiếu thủ công đã chốt/chụp trước AI |
| 13:00–18:00 | Cho hỏi AI bằng prompt ở phiếu; không tiết lộ đáp án | Công cụ/model, output đầy đủ, giờ và số lần thử |
| 18:00–22:00 | Công bố bảng đáp án và cơ chế; hỏi nhóm câu nào có/thiếu bằng chứng | Một so sánh cụ thể người–AI và một next-check |
| 22:00–25:00 | Mỗi nhóm phát biểu một bài học; chốt thời lượng và vướng mắc | Kết quả thử thật, không chỉ ô đánh dấu của người soạn |

Đây là khung 25 phút cho hoạt động chung của đề. Nếu nhóm trình bày thêm một tình huống CI/Jenkins, cần điều chỉnh nội dung bên trong khung, không kéo dài thành hai phiên 25 phút.

## 3. Gợi ý theo mức — dùng khi điều phối, không nhét vào prompt ban đầu

1. **Mức 1:** yêu cầu xác định đơn vị run/attempt và tìm failure assertion. Không chỉ sẵn hai dòng timing quyết định.
2. **Mức 2:** yêu cầu đối chiếu cùng `request_id` và cùng fixture; xem response của hai lần đọc có giống nhau không.
3. **Mức 3:** đề nghị sắp các sự kiện theo `elapsed_ms`, phân biệt thời điểm lấy snapshot với thời điểm assertion chạy. Ghi nhận nhóm đã nhận gợi ý.

Lượt nghiệm thu “nhóm khác tự làm ≤25 phút” cần thực hiện không có các gợi ý hỗ trợ này. Nếu đã can thiệp hoặc phải giải thích trực tiếp mới làm được, ghi đúng điều kiện và sửa hướng dẫn trước khi thử lại; không tính như đạt tiêu chí không hỗ trợ.

## 4. Công bố và debrief

Dùng [đáp án](Answer_Key.md), ưu tiên bốn câu hỏi:

1. Vì sao HTTP 200 không làm assertion đếm đơn tự pass?
2. Vì sao confirmation thấy đơn nhưng snapshot history vẫn fail?
3. Vì sao retry pass chưa chứng minh nguyên nhân đã được sửa?
4. AI bổ sung một dẫn chứng/phép kiểm tra nào, hay chỉ nói lại điều nhóm đã thấy?

Phần công bố có thể dẫn tới [E-09 xác nhận trên CI](../../../report/assets/flaky/v2/README.md#hai-batch-ci) và [E-10](../../../report/assets/triage/v2/E-10.md) sau khi đã lưu bài. Không coi các kết quả xác minh này là dữ liệu nhóm có trước hoặc AI đã tự chạy.

Nếu AI không dùng được, ghi là bước AI chưa hoàn thành. Có thể giới thiệu [output Sol của batch đầy đủ](../../../report/assets/triage/v2/ai-sol/response.md) để giảng giải sau giờ làm bài, nhưng phải ghi rõ đó là output trên 10 run, không phải output mới cho trích đoạn ba run và không phải kết quả của nhóm tham gia.

## 5. Bàn giao và nghiệm thu

Phần này đã có log, YAML trích đoạn, phiếu người học, prompt, đáp án và khung điều phối cho flaky/AI. Phần pipeline chung, mục lục và file `Activity_Worksheet.md` tổng hợp do nhóm ghép theo phân công; module không tự sửa cấu hình CI/Jenkins của thành viên khác.

Sau mỗi lượt thử, điền bảng này hoặc lưu phiếu đã làm. Dòng trống không phải bằng chứng đã thử.

| Nhóm/người thử | Ngày | Phút thực tế | AI/model | Có can thiệp/gợi ý? | Bước vướng | Kết quả và sửa hướng dẫn |
| --- | --- | --- | --- | --- | --- | --- |
| ______ | ______ | ______ | ______ | ______ | ______ | ______ |

Chỉ chốt nghiệm thu sau khi một nhóm khác tự lấy dữ liệu, lưu cây giả thuyết ba tầng trước AI, đọc/đối chiếu output và nêu bài học trong ≤25 phút, không cần người soạn hỗ trợ. Việc User Guide đã nghiệm thu không tự thay bằng lượt thử worksheet này.
