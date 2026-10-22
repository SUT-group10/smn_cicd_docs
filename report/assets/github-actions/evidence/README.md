# Minh chứng GitHub Actions cho section 5

Đợt kiểm chứng ngày **23/10/2026, UTC+07:00**. Metadata, log và artifact được đọc trực tiếp từ repository `SUT-group10/eshop-sut-group10` bằng GitHub CLI/API. Job chạy lại bắt đầu lúc 00:30:02 và kết thúc lúc 00:31:07 theo UTC+07:00, theo metadata API. Đây là snapshot tại thời điểm kiểm tra; cấu hình GitHub và trạng thái PR có thể thay đổi sau đó.

## Các lượt chạy

| Run / attempt | Nguồn | Kết quả được xác minh | File lưu tại đây |
| --- | --- | --- | --- |
| #8 / 1 | [39799889101](https://github.com/SUT-group10/eshop-sut-group10/actions/runs/39799889101/attempts/1), push `main`, head `b32104dec9c93ed77f475b545643f590d166aa8c` | Partial restore khóa cũ, save khóa mới; job failure | `run-8-attempt-1.json`, `run-8-attempt-1-jobs.json`, `run-8-attempt-1.log` |
| #11 / 1 | [39818068371, attempt 1](https://github.com/SUT-group10/eshop-sut-group10/actions/runs/39818068371/attempts/1), PR #19 | 39 test: 29 pass, 10 fail, 0 skip; exact cache hit; upload success | `run-11-attempt-1.*`, `run-11-artifact/` |
| #11 / 2 | [39818068371, attempt 2](https://github.com/SUT-group10/eshop-sut-group10/actions/runs/39818068371/attempts/2), chạy lại trong đợt kiểm chứng | Cùng 10 ID fail với attempt 1; exact cache hit; upload success | `run-11-attempt-2.*`, `run-11-attempt-2-artifact/` |
| #12 / 1 | [39820430997](https://github.com/SUT-group10/eshop-sut-group10/actions/runs/39820430997/attempts/1), PR #20 | 39 pass, 0 fail, 0 skip trên SUT thay đổi; exact cache hit; upload success | `run-12-attempt-1.*`, `run-12-artifact/` |

PR #19: head SHA `cbdcb885cc3974bea7f3b1749f28aa2f9a9bfd91`, merge SHA trong log `d450424190c213b85c314603b6830d3a7ac0b604`.

PR #20: head SHA `7f76d8404be9458843ef781f1d80aa5e814bb4d7`, merge SHA trong log `3c152f8e17522d3627726e163091a8be37dad23d`.

Thời gian job được tính từ `completed_at - started_at` của job trong file `*-jobs.json`: lần lượt 103 giây (#8/1), 90 giây (#11/1), 65 giây (#11/2), 66 giây (#12/1). Thời gian bước Install được tính từ timestamp của chính step: 78, 72, 46 và 47 giây. API có độ phân giải giây; không xem đây là phép đo thời gian độ chính xác cao.

## Nguồn cấu hình

- `backend-ci.yml`: bản sao `.github/workflows/ci.yml` từ checkout local ở commit `fe41113025e58eb835540cdc16d6fd8dc7be697b`. Commit gần nhất sửa workflow là `5297dd82b0512709ab598fa9a2cbb42bd8a0dd51`.
- `main-protection.json`: các trường liên quan được lấy từ `GET /repos/SUT-group10/eshop-sut-group10/branches/main/protection`. API trả cấu hình review, enforce admins; danh sách required check rỗng. Không sửa cấu hình bảo vệ nhánh trong đợt kiểm chứng.
- `main-rules.json`: phản hồi `GET /repos/SUT-group10/eshop-sut-group10/rules/branches/main`, là danh sách rỗng tại thời điểm đọc.
- `pr-20-files.json`: phản hồi API danh sách file/diff của PR #20. PR thay đổi `backend/server.js`, không thay file test; không coi run xanh này là kết quả của SUT baseline. PR vẫn mở tại thời điểm đọc.
- README trên `main` được kiểm tra qua Contents API, blob SHA `22e12108044dad1dfe0a1072f3a272a81fbe1cdc`: chưa có coverage badge. Workflow được kiểm tra cũng chưa có bước công bố badge.

## Cách thu và kiểm tra dữ liệu

Metadata run được lấy qua Actions REST API. Metadata job được lấy qua endpoint theo attempt để tránh nhầm giữa hai lượt của run #11:

```sh
gh api repos/SUT-group10/eshop-sut-group10/actions/runs/39818068371/attempts/2
gh api repos/SUT-group10/eshop-sut-group10/actions/runs/39818068371/attempts/2/jobs
gh run view 39818068371 --attempt 2 --repo SUT-group10/eshop-sut-group10 --log
```

Lệnh được dùng để kiểm chứng lại workflow:

```sh
gh run rerun 39818068371 --repo SUT-group10/eshop-sut-group10
```

Artifact được tải bằng `gh run download`, theo tên `backend-ci-reports-<run_id>`, vào thư mục riêng cho mỗi lượt thu. Các file trong các thư mục `*-artifact/` được giữ nguyên nội dung tải về. Đã đọc JSON kết quả và coverage, parse XML, đối chiếu ID fail, kiểm tra ca smoke pass trong full suite, đối chiếu thời điểm bắt đầu Jest với đúng attempt. File `test-results/jest-report.json` của attempt 2 bắt đầu lúc `2026-10-22T17:30:58.257Z`, khác attempt 1 lúc `2026-10-22T16:56:22.314Z`.

Log `*.log` là bản text do `gh run view --log` xuất ra, có tiền tố job/step/timestamp của CLI và có thể chứa mã màu ANSI. File metadata JSON được xuất dưới dạng UTF-8 từ CLI; `main-protection.json` chỉ giữ các trường liên quan bằng bộ lọc `--jq`. Không coi các bản xuất text này là byte gốc của gói log ZIP.

Danh sách SHA-256 trong `sha256sums.txt` dùng để kiểm tra các file minh chứng đã lưu. Việc có hash chỉ xác nhận nội dung bản lưu, không chứng minh các cấu hình hiện tại trên GitHub vẫn giống snapshot.
