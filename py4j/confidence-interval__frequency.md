# Tính độ tin cậy và tần suất.


Tính confidence_interval và frequency. Chúng ta cũng có thể ghép nhiều hàm khác trong cùng một lúc chạy với hai hàm này.

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "calc-stats.arrow",
    "tasks": [
        {
            "task": "statistics",
            "action": "calculate_stats",
            "kwargs": {
                "operations": {
                    "age": [
                        {
                            "func": "confidence_interval",
                            "alias": "age_ci",
                            "args": {"confidence_level": 0.75},
                        }
                    ],
                    "education": [
                        {"func": "frequency", "alias": "education_freq", "args": []}
                    ],
                }
            },
        }
    ],
}
```

Kết quả output có dạng như sau:

| age_ci          | education_freq                              |
|------------------|--------------------------------------------|
| 40.87876:40.99366 | [{'val': 'tertiary', 'freq': 13301, 'rel_freq'...] |



**Giải thích kết quả:**

- Để có được khả năng capture được dữ liệu với độ tin cậy cao. Khoảng confidence interval phải rộng hơn khi độ tin cậy cao hơn.
- Trong phần frequency, kết quả cho ta giá trị của danh mục, tần số và tần suất của từng danh mục.
