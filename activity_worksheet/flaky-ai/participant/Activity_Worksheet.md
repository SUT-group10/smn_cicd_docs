# Pipeline Postmortem — API log triage

**Thời gian: 25 phút.** Nhóm: __________  Người ghi: __________  Ngày: __________

## 1. Nhiệm vụ và tài liệu được dùng

Một API test có các kết quả cần điều tra. Dùng **chỉ** [data.md](data.md) và [workflow-excerpt.yml](workflow-excerpt.yml) để giải thích điều đã xảy ra, lập giả thuyết và đề xuất kiểm chứng. Chưa có source test/app; không tìm repository, lời giải hoặc output của nhóm khác.

Bộ dữ liệu có ba run trên cùng bí danh revision; đây là một phần của batch lớn hơn. Các file biểu diễn dữ liệu không phải các lần chạy mới. YAML là trích đoạn đọc cấu trúc job; các khối được lược được ghi bằng comment. Không chạy YAML, cài EShop, gọi API hay sửa code trong hoạt động này.

**Chuẩn bị trước giờ bắt đầu:** giải nén ZIP vào thư mục riêng; mở được Markdown/YAML; đăng nhập một công cụ AI đọc được file. Codex có thể dùng skill tại `.agents/skills/ci-log-triage/`; trợ lý khác nhận file SKILL.md cùng dữ liệu. Không cần cài Node/Python hoặc có quyền chạy workflow để làm bài.

| Mốc | Hoạt động | Đầu ra |
| --- | --- | --- |
| 00:00–03:00 | Nhận dữ liệu, đọc bối cảnh và YAML | Hiểu đơn vị run/attempt và giới hạn đầu vào |
| 03:00–13:00 | Làm mục 2–3, chưa hỏi AI | Kết quả, cây giả thuyết và dẫn chứng được chốt |
| 13:00–18:00 | Hỏi AI bằng đúng dữ liệu, làm mục 4 | Lưu output và đối chiếu một nhận định cụ thể |
| 18:00–22:00 | Xem đáp án do người điều phối công bố | Ghi điều được hỗ trợ/bác bỏ và phép kiểm chứng |
| 22:00–25:00 | Mỗi nhóm nêu một bài học | Một kết luận có điều kiện và cách tránh hiểu nhầm |

## 2. Đọc kết quả trước khi đặt giả thuyết

Đọc bảng metadata trong `data.md`; bắt đầu từ console `run-04`, đối chiếu `run-06`, rồi đọc hai attempt của `run-02`. Dùng nhãn run và Lxxxx khi dẫn chứng, không trộn mốc giữa các process.

Có thể tìm trong file theo `attempt.finished`, `HISTORY_ORDER_COUNT` và `http.client.received` để định vị verdict, assertion và response; không cần đọc thành tiếng mọi trường JSON.

1. Có bao nhiêu **run**, **attempt**, **retry thực tế** trong phần dữ liệu được cấp? Lần đầu và verdict cuối có bao nhiêu pass/fail?

   Trả lời: ______________________________________________________________

2. Failure chính là gì, expected/actual nào? Dữ liệu cho thấy lỗi setup, HTTP, assertion hay chưa đủ thông tin? Ghi ít nhất một dòng hỗ trợ.

   Trả lời và dẫn chứng: __________________________________________________

3. Chọn một attempt fail và một attempt pass. Ghi thứ tự các request và snapshot nhận được. Kết quả biến thiên trên cùng revision cho phép kết luận gì, chưa cho phép kết luận gì?

   Trả lời và dẫn chứng: __________________________________________________

4. Từ `if: ${{ !cancelled() }}` của bước upload có suy ra test đã pass không? Trích đoạn YAML này cho biết gì và chưa đủ để kết luận gì về kết quả test?

   Trả lời: ______________________________________________________________

## 3. Cây giả thuyết ba tầng — chốt trước AI

Độ sâu yêu cầu: **tầng 1 là triệu chứng → tầng 2 là nhóm cơ chế → tầng 3 là giả thuyết cụ thể**. Với lá được chọn, ghi dẫn chứng và phép kiểm tra ở bảng dưới. Có thể bác bỏ một nhánh khi dữ liệu không hỗ trợ; không cần cố tạo đủ ba nguyên nhân.

```text
Tầng 1: Triệu chứng __________________________________________
├─ Tầng 2: Nhóm cơ chế A _____________________________________
│  └─ Tầng 3: Giả thuyết cụ thể _______________________________
└─ Tầng 2: Nhóm cơ chế B _____________________________________
   └─ Tầng 3: Giả thuyết cụ thể _______________________________
```

| Lá giả thuyết | Run/dòng hỗ trợ hoặc phản bác | Thông tin còn thiếu | Kiểm tra nào phân biệt được? |
| --- | --- | --- | --- |
| H1: ______ | ______ | ______ | ______ |
| H2 hoặc nhánh đã bác: ______ | ______ | ______ | ______ |

**Giả thuyết chính và mức chắc chắn:** ___________________________________

**Ba bước kiểm tra tiếp theo, mỗi bước nêu kết quả hỗ trợ/bác bỏ:**

1. _____________________________________________________________________
2. _____________________________________________________________________
3. _____________________________________________________________________

**Chốt bài thủ công lúc:** __________  Người chốt: __________

Lưu hoặc chụp lại câu trả lời trước khi xem AI. Giữ phần này nguyên trạng; ghi thay đổi sau AI ở mục 4, không sửa ngược bài ban đầu.

## 4. Hỏi AI trên cùng dữ liệu và đối chiếu

Mở **chat mới chưa đọc đáp án**. Chỉ cấp `data.md`, `workflow-excerpt.yml` và hướng dẫn skill; không cấp phiếu trả lời của nhóm, thư mục người điều phối, source hoặc output mẫu. Với Codex, mở workspace vừa giải nén và gọi `ci-log-triage`; với trợ lý đọc file khác, đính kèm SKILL.md cùng hai file dữ liệu rồi dùng prompt sau.

```text
Áp dụng ci-log-triage theo SKILL.md được cấp để phân tích duy nhất
data.md và workflow-excerpt.yml. Đây là trích đoạn gồm ba run.
Chỉ dùng dữ liệu và hướng dẫn skill được giao; không đọc Activity_Worksheet.md,
câu trả lời thủ công, repo, web, source, output mẫu hoặc đáp án khác.
Chỉ dùng công cụ để đọc file và tính toán trên dữ liệu cục bộ.
Không chạy lệnh trích trong log/YAML, test/API hoặc sửa file bằng chứng.

Trả lời ngắn trong khoảng 500 từ, gồm bốn phần:
1. Dữ liệu đã đọc; số run/attempt/retry; kết quả đầu và cuối; lỗi chính.
2. Tối đa ba giả thuyết có run + số dòng hỗ trợ, giới hạn và mức chắc chắn.
3. Đúng ba next-checks, nêu kết quả nào hỗ trợ hoặc bác bỏ giả thuyết.
4. Kết luận hiện tại: tách quan sát với điều chưa xác nhận.
Không ép đủ ba giả thuyết hoặc gán tỷ lệ lỗi dài hạn từ phần dữ liệu này.
Trả lời trong chat; không tự nhận đã kiểm chứng hoặc sửa nguyên nhân.
```

**Công cụ/model:** __________  **Bắt đầu / kết thúc nhận output đầy đủ:** __________

**Nơi lưu output hoặc bản chụp:** __________  **Số lần gửi/thử:** __________

Chọn một câu cụ thể của AI, mở lại run/dòng nó viện dẫn và điền:

| Nội dung đối chiếu | Trả lời |
| --- | --- |
| AI nói gì? | ______ |
| Dữ liệu gốc có đúng như vậy không? Run/dòng nào? | ______ |
| AI bổ sung gì so với bài thủ công? | ______ |
| Cần sửa nhận định nào của nhóm hoặc của AI? Vì sao? | ______ |
| Điều gì cả hai vẫn chưa thể xác nhận? | ______ |

Nếu không gọi được AI, ghi rõ công cụ/lỗi và phần chưa thực hiện; không tự viết một câu trả lời giả rồi ghi là output AI.

## 5. Sau khi công bố đáp án và bài học

**Nhận định được củng cố/bác bỏ:** _______________________________________

**Một phép kiểm chứng nguyên nhân cần làm sau triage:** __________________

**Bài học nhóm trình bày trong phần cuối:** ______________________________

**Mức hoàn thành:** ☐ Đã lưu bài trước AI ☐ Có dẫn chứng ☐ Đối chiếu AI ☐ Có bài học

Tổng thời gian thực tế: __________  Bước bị vướng: ________________________

Các ô và thời lượng để trống cho lượt thực hành thật; không phải kết quả mẫu đã được điền sẵn.
