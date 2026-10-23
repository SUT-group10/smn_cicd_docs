# User Guide — CI/CD and Test-Harness Engineering

## Mục lục

1. [GitHub Actions — pipeline chính, quality gate và artifact](github-actions.md)
2. [Jenkins — nền tảng đối chiếu](jenkins.md)
3. [Flaky test và AI log triage — thực hành v2 từng bước](flaky-ai.md)

Phần flaky/AI kèm ZIP đầu vào, skill và ảnh giao diện thật. Người học có thể thực hành triage bằng gói mẫu trước, rồi thực hiện phần thu CI/xác minh khi có môi trường và quyền phù hợp. Output mẫu và phần giải thích nguyên nhân dành cho bước đối chiếu sau khi đã lưu phân tích.

Hình ảnh dùng trong guide đặt tại `assets/`.

## Xuất PDF

Mở terminal tại thư mục `user_guide`. Đọc Markdown trực tiếp không cần công cụ xuất PDF. Nếu cần PDF, cài Pandoc và XeLaTeX, rồi chạy bằng PowerShell:

```powershell
pandoc README.md github-actions.md jenkins.md flaky-ai.md --resource-path='.' --toc --pdf-engine=xelatex -V mainfont=Arial -o user_guide.pdf
```

Lệnh xuất dùng đủ ba phần guide. Kiểm tra bảng, ảnh, font tiếng Việt và liên kết trên PDF trước khi đưa vào bộ nộp.
