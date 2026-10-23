# CI/CD & Test-Harness Engineering — Seminar Documents

Bộ tài liệu seminar của **Nhóm 10**, môn **Kiểm thử phần mềm (Software Testing)**, đề tài **T07 — CI/CD & Test-Harness Engineering**.

Đề tài sử dụng backend EShop để xây dựng test harness, triển khai CI với **GitHub Actions**, đối chiếu với **Jenkins** và nghiên cứu **flaky test cùng AI-assisted log triage**. Repository này tập hợp tài liệu báo cáo, hướng dẫn, hoạt động seminar và hồ sơ đóng góp của nhóm.

## Truy cập nhanh

- [Tool Survey Proposal](proposal/Tool_Survey_Proposal.md)
- [Báo cáo chi tiết](report/report.md)
- [User Guide](user_guide/README.md)
- [Hoạt động Pipeline Postmortem — phần flaky test và AI triage](activity_worksheet/flaky-ai/README.md)
- [Thông tin video demo](demo/demo_videos.md)

## Cấu trúc thư mục

| Thư mục | Nội dung |
| --- | --- |
| [proposal/](proposal/) | Khảo sát công cụ, phương án lựa chọn và phạm vi đề tài. |
| [report/](report/) | Báo cáo chi tiết về cơ sở lý thuyết, hệ thống kiểm thử, triển khai CI, thực nghiệm và đánh giá; hình ảnh đi kèm nằm trong `assets/`. |
| [user_guide/](user_guide/) | Hướng dẫn thao tác với GitHub Actions, Jenkins, flaky test và AI log triage; kèm tài nguyên thực hành, hình minh họa. |
| [slides/](slides/) | Thư mục dành cho bài trình bày và tài nguyên minh họa dùng trên slide. |
| [demo/](demo/) | Danh mục liên kết video demo, thời lượng và nội dung từng clip. |
| [activity_worksheet/](activity_worksheet/) | Tài liệu hoạt động Pipeline Postmortem: dữ liệu cho người tham gia, câu hỏi, hướng dẫn điều phối và đáp án. |
| [source/](source/) | Thông tin và liên kết tới repository mã nguồn EShop. |
| [contribution/](contribution/) | Thông tin phân công và theo dõi đóng góp của các thành viên. |
| [git_log/](git_log/) | Nhật ký commit phục vụ theo dõi quá trình thực hiện. |
| [ai_usage_declaration/](ai_usage_declaration/) | Bản khai sử dụng AI và chatlog, tổ chức theo tuần và MSSV. |

## Hướng dẫn đọc

1. Đọc **Proposal** để nắm mục tiêu, phạm vi và lý do lựa chọn công cụ.
2. Đọc **Report** để xem phương pháp triển khai, kết quả thực nghiệm và nhận xét.
3. Làm theo **User Guide** để thực hành:
   - [GitHub Actions](user_guide/github-actions.md)
   - [Jenkins](user_guide/jenkins.md)
   - [Flaky test và AI log triage](user_guide/flaky-ai.md)
4. Sử dụng **Activity Worksheet** cho hoạt động seminar. Tài liệu người tham gia và tài liệu điều phối được tách riêng; đáp án dành cho bước đối chiếu sau khi phân tích.
5. Xem **Contribution**, **Git Log** và **AI Usage Declaration** để theo dõi quá trình thực hiện và việc sử dụng AI.

## Repository liên quan

- **Mã nguồn và cấu hình CI:** [SUT-group10/eshop-sut-group10](https://github.com/SUT-group10/eshop-sut-group10)
- **Bộ tài liệu seminar:** [SUT-group10/smn_cicd_docs](https://github.com/SUT-group10/smn_cicd_docs)

Các lệnh chạy ứng dụng, test và pipeline được mô tả trong User Guide và repository mã nguồn. Các file Markdown trong bộ tài liệu có thể đọc trực tiếp trên GitHub; hình ảnh được liên kết theo đường dẫn tương đối để đi cùng nội dung khi chuyển thư mục hoặc xuất bản.
