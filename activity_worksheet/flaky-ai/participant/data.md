# API test — dữ liệu điều tra

## Bối cảnh được cấp

Test `Submitted order appears in account history`, ID `ORDER-HISTORY-01`, kiểm tra đơn vừa gửi có trong lịch sử tài khoản: history cần có đúng một đơn và khớp ID, chủ sở hữu, trạng thái, tổng tiền. Bảng dưới trích metadata của **ba run được chọn từ batch 10 run đã hoàn tất**, cùng bí danh `revision-01`. Không có run mới sinh ra khi làm worksheet; đây không phải mẫu ngẫu nhiên để suy tỷ lệ lỗi dài hạn.

Môi trường nguồn: Node v24.14.0, Jest 30.5.2, Linux. Mỗi run là một process Jest mới; một test fail được retry tối đa hai lần trong process, dừng khi pass hoặc hết retry. **Attempt đầu không tính là retry.** Mỗi attempt tạo app mới và SQLite in-memory riêng, bắt đầu với không đơn hàng; các API là endpoint EShop thật, gọi qua HTTP cục bộ bằng Supertest.

## Metadata trích từ history

`status` là verdict cuối, `invocations` là số attempt thực tế, `test_retries_used` là số retry và `process_exit_code` là exit code process. Bảng và console biểu diễn cùng các run, không cộng chúng thành những lượt độc lập.

| run_id | revision | status | invocations | test_retries_used | retry_limit | process_exit_code |
| --- | --- | --- | ---: | ---: | ---: | ---: |
| run-02 | revision-01 | passed | 2 | 1 | 2 | 0 |
| run-04 | revision-01 | failed | 3 | 2 | 2 | 1 |
| run-06 | revision-01 | passed | 1 | 0 | 2 | 0 |

## Cách đọc console

- `Lxxxx` là số dòng trong console của **chính run đó**, giữ từ bản gốc. Dẫn chứng cần cả run và dòng, ví dụ `run-04/L0007`.
- `t+...ms` là thời gian từ đồng hồ đơn điệu của process Jest trong run, gồm setup và retry; không reset về 0 ở attempt tiếp theo. Chỉ so thứ tự trong cùng run. UTC dùng định hướng, không để suy thứ tự giữa process.
- `request_id` nối các sự kiện client/server của cùng request. `attempt` và `fixture_id` phân biệt lần thử và dữ liệu; fixture đánh số lại trong mỗi run.
- `history` và `confirmation` là hai request đọc riêng, cả hai quan sát được giữ. `http.client.received` chứa response; `assertion.completed` chứa expected/actual và verdict của assertion; `attempt.finished` chứa verdict attempt.
- Đây là toàn bộ console của ba run được chọn, không chỉ các dòng lỗi. Dữ liệu sự kiện, giá trị, thứ tự và số dòng được giữ nguyên. Không có source, stack/code frame, thông số điều phối hoặc lời giải trong bộ phát.

## run-02

```text
L0001 2026-10-22T21:51:55.688Z t+3.143ms attempt=1 attempt.started {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-1", "retry_of_attempt": null}
L0002 2026-10-22T21:51:55.856Z t+170.990ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_CART_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0003 2026-10-22T21:51:55.857Z t+171.762ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_ORDER_COUNT", "status": "passed", "expected": 0, "actual": 0}
L0004 2026-10-22T21:51:55.857Z t+171.831ms attempt=1 fixture.ready {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-1", "initial_order_count": 0}
L0005 2026-10-22T21:51:55.858Z t+172.552ms attempt=1 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.submit", "operation": "submit", "method": "POST", "path": "/api/checkout"}
L0006 2026-10-22T21:51:55.860Z t+174.399ms attempt=1 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.submit", "method": "POST", "path": "/api/checkout"}
L0007 2026-10-22T21:51:55.978Z t+292.894ms attempt=1 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders"}
L0008 2026-10-22T21:51:55.980Z t+294.763ms attempt=1 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.history", "method": "GET", "path": "/api/orders/my-orders"}
L0009 2026-10-22T21:51:55.982Z t+296.610ms attempt=1 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 1.82}
L0010 2026-10-22T21:51:55.983Z t+297.566ms attempt=1 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 4.554, "response": {"order_count": 0, "orders": []}}
L0011 2026-10-22T21:51:56.015Z t+329.099ms attempt=1 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 154.679}
L0012 2026-10-22T21:51:56.015Z t+329.849ms attempt=1 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.submit", "operation": "submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 157.28, "response": {"order_id": 1}}
L0013 2026-10-22T21:51:56.016Z t+330.100ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "SUBMIT_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0014 2026-10-22T21:51:56.016Z t+330.141ms attempt=1 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0015 2026-10-22T21:51:56.017Z t+331.567ms attempt=1 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0016 2026-10-22T21:51:56.018Z t+332.660ms attempt=1 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 1.081}
L0017 2026-10-22T21:51:56.019Z t+333.382ms attempt=1 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a1.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 3.184, "response": {"order_count": 1, "orders": [{"id": 1, "user_id": 2, "status": "pending", "total_amount": 30000000}]}}
L0018 2026-10-22T21:51:56.019Z t+333.569ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0019 2026-10-22T21:51:56.019Z t+333.701ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0020 2026-10-22T21:51:56.019Z t+333.960ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0021 2026-10-22T21:51:56.020Z t+334.148ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0022 2026-10-22T21:51:56.020Z t+334.304ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_COUNT", "status": "passed", "expected": 1, "actual": 1}
L0023 2026-10-22T21:51:56.020Z t+334.447ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_ID", "status": "passed", "expected": 1, "actual": 1}
L0024 2026-10-22T21:51:56.021Z t+335.685ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "ERROR", "assertion_id": "HISTORY_ORDER_COUNT", "status": "failed", "expected": 1, "actual": 0}
L0025 2026-10-22T21:51:56.022Z t+336.077ms attempt=1 fixture.closed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-1"}
L0026 2026-10-22T21:51:56.022Z t+336.149ms attempt=1 attempt.finished {"test_id": "ORDER-HISTORY-01", "level": "ERROR", "status": "failed", "failure": {"category": "assertion", "code": "HISTORY_ORDER_COUNT", "message": "HISTORY_ORDER_COUNT: expected 1, received 0", "expected": 1, "actual": 0}}
L0027 2026-10-22T21:51:56.024Z t+338.320ms attempt=2 attempt.started {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-2", "retry_of_attempt": 1}
L0028 2026-10-22T21:51:56.076Z t+390.946ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_CART_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0029 2026-10-22T21:51:56.086Z t+400.957ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_ORDER_COUNT", "status": "passed", "expected": 0, "actual": 0}
L0030 2026-10-22T21:51:56.087Z t+401.072ms attempt=2 fixture.ready {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-2", "initial_order_count": 0}
L0031 2026-10-22T21:51:56.087Z t+401.526ms attempt=2 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.submit", "operation": "submit", "method": "POST", "path": "/api/checkout"}
L0032 2026-10-22T21:51:56.089Z t+403.028ms attempt=2 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.submit", "method": "POST", "path": "/api/checkout"}
L0033 2026-10-22T21:51:56.176Z t+490.021ms attempt=2 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 86.974}
L0034 2026-10-22T21:51:56.176Z t+490.677ms attempt=2 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.submit", "operation": "submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 89.134, "response": {"order_id": 1}}
L0035 2026-10-22T21:51:56.208Z t+522.363ms attempt=2 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders"}
L0036 2026-10-22T21:51:56.209Z t+523.962ms attempt=2 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.history", "method": "GET", "path": "/api/orders/my-orders"}
L0037 2026-10-22T21:51:56.211Z t+525.159ms attempt=2 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 1.187}
L0038 2026-10-22T21:51:56.211Z t+525.954ms attempt=2 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 3.565, "response": {"order_count": 1, "orders": [{"id": 1, "user_id": 2, "status": "pending", "total_amount": 30000000}]}}
L0039 2026-10-22T21:51:56.212Z t+526.144ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "SUBMIT_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0040 2026-10-22T21:51:56.212Z t+526.181ms attempt=2 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0041 2026-10-22T21:51:56.213Z t+527.355ms attempt=2 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0042 2026-10-22T21:51:56.214Z t+528.248ms attempt=2 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 0.883}
L0043 2026-10-22T21:51:56.214Z t+528.744ms attempt=2 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-02.a2.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 2.545, "response": {"order_count": 1, "orders": [{"id": 1, "user_id": 2, "status": "pending", "total_amount": 30000000}]}}
L0044 2026-10-22T21:51:56.214Z t+528.864ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0045 2026-10-22T21:51:56.214Z t+528.932ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0046 2026-10-22T21:51:56.215Z t+529.050ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0047 2026-10-22T21:51:56.215Z t+529.185ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0048 2026-10-22T21:51:56.215Z t+529.297ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_COUNT", "status": "passed", "expected": 1, "actual": 1}
L0049 2026-10-22T21:51:56.215Z t+529.360ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_ID", "status": "passed", "expected": 1, "actual": 1}
L0050 2026-10-22T21:51:56.215Z t+529.412ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_ORDER_COUNT", "status": "passed", "expected": 1, "actual": 1}
L0051 2026-10-22T21:51:56.215Z t+529.461ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_ORDER_ID", "status": "passed", "expected": 1, "actual": 1}
L0052 2026-10-22T21:51:56.215Z t+529.508ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_ORDER_OWNER", "status": "passed", "expected": 2, "actual": 2}
L0053 2026-10-22T21:51:56.215Z t+529.557ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_ORDER_TOTAL", "status": "passed", "expected": 30000000, "actual": 30000000}
L0054 2026-10-22T21:51:56.215Z t+529.641ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_ORDER_STATUS", "status": "passed", "expected": "pending", "actual": "pending"}
L0055 2026-10-22T21:51:56.215Z t+529.865ms attempt=2 fixture.closed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-2"}
L0056 2026-10-22T21:51:56.215Z t+529.931ms attempt=2 attempt.finished {"test_id": "ORDER-HISTORY-01", "level": "INFO", "status": "passed", "failure": null}
```

## run-04

```text
L0001 2026-10-22T21:51:58.529Z t+3.201ms attempt=1 attempt.started {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-1", "retry_of_attempt": null}
L0002 2026-10-22T21:51:58.703Z t+177.184ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_CART_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0003 2026-10-22T21:51:58.707Z t+181.017ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_ORDER_COUNT", "status": "passed", "expected": 0, "actual": 0}
L0004 2026-10-22T21:51:58.707Z t+181.124ms attempt=1 fixture.ready {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-1", "initial_order_count": 0}
L0005 2026-10-22T21:51:58.708Z t+182.160ms attempt=1 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.submit", "operation": "submit", "method": "POST", "path": "/api/checkout"}
L0006 2026-10-22T21:51:58.711Z t+184.943ms attempt=1 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.submit", "method": "POST", "path": "/api/checkout"}
L0007 2026-10-22T21:51:58.829Z t+302.575ms attempt=1 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders"}
L0008 2026-10-22T21:51:58.831Z t+304.389ms attempt=1 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.history", "method": "GET", "path": "/api/orders/my-orders"}
L0009 2026-10-22T21:51:58.832Z t+306.292ms attempt=1 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 1.877}
L0010 2026-10-22T21:51:58.833Z t+307.198ms attempt=1 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 4.524, "response": {"order_count": 0, "orders": []}}
L0011 2026-10-22T21:51:58.922Z t+395.920ms attempt=1 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 210.952}
L0012 2026-10-22T21:51:58.923Z t+396.626ms attempt=1 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.submit", "operation": "submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 214.449, "response": {"order_id": 1}}
L0013 2026-10-22T21:51:58.923Z t+396.848ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "SUBMIT_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0014 2026-10-22T21:51:58.923Z t+396.884ms attempt=1 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0015 2026-10-22T21:51:58.924Z t+398.127ms attempt=1 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0016 2026-10-22T21:51:58.925Z t+399.153ms attempt=1 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 1.016}
L0017 2026-10-22T21:51:58.926Z t+399.840ms attempt=1 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a1.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 2.902, "response": {"order_count": 1, "orders": [{"id": 1, "user_id": 2, "status": "pending", "total_amount": 30000000}]}}
L0018 2026-10-22T21:51:58.926Z t+399.987ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0019 2026-10-22T21:51:58.926Z t+400.076ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0020 2026-10-22T21:51:58.926Z t+400.211ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0021 2026-10-22T21:51:58.926Z t+400.310ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0022 2026-10-22T21:51:58.927Z t+400.421ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_COUNT", "status": "passed", "expected": 1, "actual": 1}
L0023 2026-10-22T21:51:58.927Z t+400.515ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_ID", "status": "passed", "expected": 1, "actual": 1}
L0024 2026-10-22T21:51:58.928Z t+401.653ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "ERROR", "assertion_id": "HISTORY_ORDER_COUNT", "status": "failed", "expected": 1, "actual": 0}
L0025 2026-10-22T21:51:58.928Z t+402.072ms attempt=1 fixture.closed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-1"}
L0026 2026-10-22T21:51:58.928Z t+402.142ms attempt=1 attempt.finished {"test_id": "ORDER-HISTORY-01", "level": "ERROR", "status": "failed", "failure": {"category": "assertion", "code": "HISTORY_ORDER_COUNT", "message": "HISTORY_ORDER_COUNT: expected 1, received 0", "expected": 1, "actual": 0}}
L0027 2026-10-22T21:51:58.930Z t+404.142ms attempt=2 attempt.started {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-2", "retry_of_attempt": 1}
L0028 2026-10-22T21:51:58.961Z t+434.843ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_CART_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0029 2026-10-22T21:51:58.961Z t+435.291ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_ORDER_COUNT", "status": "passed", "expected": 0, "actual": 0}
L0030 2026-10-22T21:51:58.962Z t+435.338ms attempt=2 fixture.ready {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-2", "initial_order_count": 0}
L0031 2026-10-22T21:51:58.962Z t+435.667ms attempt=2 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.submit", "operation": "submit", "method": "POST", "path": "/api/checkout"}
L0032 2026-10-22T21:51:58.963Z t+436.828ms attempt=2 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.submit", "method": "POST", "path": "/api/checkout"}
L0033 2026-10-22T21:51:59.082Z t+555.748ms attempt=2 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders"}
L0034 2026-10-22T21:51:59.084Z t+557.388ms attempt=2 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.history", "method": "GET", "path": "/api/orders/my-orders"}
L0035 2026-10-22T21:51:59.085Z t+558.595ms attempt=2 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 1.197}
L0036 2026-10-22T21:51:59.085Z t+559.110ms attempt=2 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 3.345, "response": {"order_count": 0, "orders": []}}
L0037 2026-10-22T21:51:59.112Z t+586.051ms attempt=2 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 149.205}
L0038 2026-10-22T21:51:59.113Z t+586.861ms attempt=2 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.submit", "operation": "submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 151.179, "response": {"order_id": 1}}
L0039 2026-10-22T21:51:59.113Z t+587.041ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "SUBMIT_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0040 2026-10-22T21:51:59.113Z t+587.071ms attempt=2 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0041 2026-10-22T21:51:59.115Z t+588.409ms attempt=2 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0042 2026-10-22T21:51:59.116Z t+589.371ms attempt=2 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 0.952}
L0043 2026-10-22T21:51:59.116Z t+589.904ms attempt=2 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a2.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 2.815, "response": {"order_count": 1, "orders": [{"id": 1, "user_id": 2, "status": "pending", "total_amount": 30000000}]}}
L0044 2026-10-22T21:51:59.116Z t+590.029ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0045 2026-10-22T21:51:59.116Z t+590.097ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0046 2026-10-22T21:51:59.116Z t+590.185ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0047 2026-10-22T21:51:59.116Z t+590.247ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0048 2026-10-22T21:51:59.117Z t+590.346ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_COUNT", "status": "passed", "expected": 1, "actual": 1}
L0049 2026-10-22T21:51:59.117Z t+590.414ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_ID", "status": "passed", "expected": 1, "actual": 1}
L0050 2026-10-22T21:51:59.117Z t+590.617ms attempt=2 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "ERROR", "assertion_id": "HISTORY_ORDER_COUNT", "status": "failed", "expected": 1, "actual": 0}
L0051 2026-10-22T21:51:59.117Z t+590.835ms attempt=2 fixture.closed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-2"}
L0052 2026-10-22T21:51:59.117Z t+590.885ms attempt=2 attempt.finished {"test_id": "ORDER-HISTORY-01", "level": "ERROR", "status": "failed", "failure": {"category": "assertion", "code": "HISTORY_ORDER_COUNT", "message": "HISTORY_ORDER_COUNT: expected 1, received 0", "expected": 1, "actual": 0}}
L0053 2026-10-22T21:51:59.121Z t+595.119ms attempt=3 attempt.started {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-3", "retry_of_attempt": 2}
L0054 2026-10-22T21:51:59.149Z t+623.123ms attempt=3 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_CART_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0055 2026-10-22T21:51:59.150Z t+623.542ms attempt=3 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_ORDER_COUNT", "status": "passed", "expected": 0, "actual": 0}
L0056 2026-10-22T21:51:59.150Z t+623.593ms attempt=3 fixture.ready {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-3", "initial_order_count": 0}
L0057 2026-10-22T21:51:59.150Z t+623.902ms attempt=3 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.submit", "operation": "submit", "method": "POST", "path": "/api/checkout"}
L0058 2026-10-22T21:51:59.151Z t+624.749ms attempt=3 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.submit", "method": "POST", "path": "/api/checkout"}
L0059 2026-10-22T21:51:59.270Z t+744.192ms attempt=3 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders"}
L0060 2026-10-22T21:51:59.272Z t+745.489ms attempt=3 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.history", "method": "GET", "path": "/api/orders/my-orders"}
L0061 2026-10-22T21:51:59.273Z t+746.687ms attempt=3 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 1.187}
L0062 2026-10-22T21:51:59.273Z t+747.174ms attempt=3 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 2.965, "response": {"order_count": 0, "orders": []}}
L0063 2026-10-22T21:51:59.356Z t+830.106ms attempt=3 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 205.336}
L0064 2026-10-22T21:51:59.357Z t+830.772ms attempt=3 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.submit", "operation": "submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 206.855, "response": {"order_id": 1}}
L0065 2026-10-22T21:51:59.357Z t+830.965ms attempt=3 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "SUBMIT_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0066 2026-10-22T21:51:59.357Z t+830.999ms attempt=3 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0067 2026-10-22T21:51:59.358Z t+832.069ms attempt=3 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0068 2026-10-22T21:51:59.359Z t+833.009ms attempt=3 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 0.93}
L0069 2026-10-22T21:51:59.360Z t+833.400ms attempt=3 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-04.a3.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 2.381, "response": {"order_count": 1, "orders": [{"id": 1, "user_id": 2, "status": "pending", "total_amount": 30000000}]}}
L0070 2026-10-22T21:51:59.360Z t+833.601ms attempt=3 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0071 2026-10-22T21:51:59.360Z t+833.737ms attempt=3 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0072 2026-10-22T21:51:59.360Z t+833.820ms attempt=3 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0073 2026-10-22T21:51:59.360Z t+833.877ms attempt=3 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0074 2026-10-22T21:51:59.360Z t+833.928ms attempt=3 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_COUNT", "status": "passed", "expected": 1, "actual": 1}
L0075 2026-10-22T21:51:59.360Z t+833.976ms attempt=3 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_ID", "status": "passed", "expected": 1, "actual": 1}
L0076 2026-10-22T21:51:59.360Z t+834.178ms attempt=3 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "ERROR", "assertion_id": "HISTORY_ORDER_COUNT", "status": "failed", "expected": 1, "actual": 0}
L0077 2026-10-22T21:51:59.361Z t+834.401ms attempt=3 fixture.closed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-3"}
L0078 2026-10-22T21:51:59.361Z t+834.487ms attempt=3 attempt.finished {"test_id": "ORDER-HISTORY-01", "level": "ERROR", "status": "failed", "failure": {"category": "assertion", "code": "HISTORY_ORDER_COUNT", "message": "HISTORY_ORDER_COUNT: expected 1, received 0", "expected": 1, "actual": 0}}
```

## run-06

```text
L0001 2026-10-22T21:52:01.777Z t+3.206ms attempt=1 attempt.started {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-1", "retry_of_attempt": null}
L0002 2026-10-22T21:52:01.954Z t+179.479ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_CART_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0003 2026-10-22T21:52:01.956Z t+182.160ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "FIXTURE_ORDER_COUNT", "status": "passed", "expected": 0, "actual": 0}
L0004 2026-10-22T21:52:01.957Z t+182.240ms attempt=1 fixture.ready {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-1", "initial_order_count": 0}
L0005 2026-10-22T21:52:01.957Z t+183.158ms attempt=1 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.submit", "operation": "submit", "method": "POST", "path": "/api/checkout"}
L0006 2026-10-22T21:52:01.960Z t+185.673ms attempt=1 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.submit", "method": "POST", "path": "/api/checkout"}
L0007 2026-10-22T21:52:01.983Z t+208.557ms attempt=1 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 22.858}
L0008 2026-10-22T21:52:01.984Z t+209.347ms attempt=1 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.submit", "operation": "submit", "method": "POST", "path": "/api/checkout", "status_code": 200, "duration_ms": 26.101, "response": {"order_id": 1}}
L0009 2026-10-22T21:52:02.079Z t+304.250ms attempt=1 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders"}
L0010 2026-10-22T21:52:02.080Z t+306.213ms attempt=1 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.history", "method": "GET", "path": "/api/orders/my-orders"}
L0011 2026-10-22T21:52:02.082Z t+307.709ms attempt=1 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 1.481}
L0012 2026-10-22T21:52:02.083Z t+308.476ms attempt=1 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.history", "operation": "history", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 4.133, "response": {"order_count": 1, "orders": [{"id": 1, "user_id": 2, "status": "pending", "total_amount": 30000000}]}}
L0013 2026-10-22T21:52:02.083Z t+308.677ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "SUBMIT_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0014 2026-10-22T21:52:02.083Z t+308.709ms attempt=1 http.client.sent {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0015 2026-10-22T21:52:02.084Z t+309.853ms attempt=1 http.server.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.confirmation", "method": "GET", "path": "/api/orders/my-orders"}
L0016 2026-10-22T21:52:02.085Z t+310.885ms attempt=1 http.server.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 1.019}
L0017 2026-10-22T21:52:02.086Z t+311.496ms attempt=1 http.client.received {"test_id": "ORDER-HISTORY-01", "level": "INFO", "request_id": "run-06.a1.confirmation", "operation": "confirmation", "method": "GET", "path": "/api/orders/my-orders", "status_code": 200, "duration_ms": 2.767, "response": {"order_count": 1, "orders": [{"id": 1, "user_id": 2, "status": "pending", "total_amount": 30000000}]}}
L0018 2026-10-22T21:52:02.086Z t+311.640ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0019 2026-10-22T21:52:02.086Z t+311.729ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_HTTP", "status": "passed", "expected": 200, "actual": 200}
L0020 2026-10-22T21:52:02.086Z t+311.901ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0021 2026-10-22T21:52:02.086Z t+312.033ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_BODY_IS_ARRAY", "status": "passed", "expected": true, "actual": true}
L0022 2026-10-22T21:52:02.086Z t+312.139ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_COUNT", "status": "passed", "expected": 1, "actual": 1}
L0023 2026-10-22T21:52:02.087Z t+312.245ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "CONFIRMATION_ORDER_ID", "status": "passed", "expected": 1, "actual": 1}
L0024 2026-10-22T21:52:02.087Z t+312.350ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_ORDER_COUNT", "status": "passed", "expected": 1, "actual": 1}
L0025 2026-10-22T21:52:02.087Z t+312.523ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_ORDER_ID", "status": "passed", "expected": 1, "actual": 1}
L0026 2026-10-22T21:52:02.087Z t+312.655ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_ORDER_OWNER", "status": "passed", "expected": 2, "actual": 2}
L0027 2026-10-22T21:52:02.087Z t+312.729ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_ORDER_TOTAL", "status": "passed", "expected": 30000000, "actual": 30000000}
L0028 2026-10-22T21:52:02.087Z t+312.789ms attempt=1 assertion.completed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "assertion_id": "HISTORY_ORDER_STATUS", "status": "passed", "expected": "pending", "actual": "pending"}
L0029 2026-10-22T21:52:02.087Z t+313.105ms attempt=1 fixture.closed {"test_id": "ORDER-HISTORY-01", "level": "INFO", "fixture_id": "fixture-1"}
L0030 2026-10-22T21:52:02.087Z t+313.175ms attempt=1 attempt.finished {"test_id": "ORDER-HISTORY-01", "level": "INFO", "status": "passed", "failure": null}
```
