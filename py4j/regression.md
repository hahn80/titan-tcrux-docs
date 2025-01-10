# Regression


Tính toán chỉ số thống kê hồi qui của mô hình linear regression.

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "regression.arrow",
    "tasks": [
        {
            "task": "regression",
            "action": "linear_regression",
            "kwargs": {
                "columns": ["age", "balance", "duration"],
                "target": "pdays",
                "confidence_level": 0.95,
            },
        }
    ],
}
```

Kết quả output có dạng như sau:

| r2        | mae        | mse        | feature_names | coefficients | standard_errors | t_values   | p_values       | lower_bounds | upper_bounds |
|-----------|------------|------------|---------------|--------------|-----------------|------------|----------------|--------------|--------------|
| 0.000601  | 67.594755  | 10019.5169 | age           | -0.229497    | 0.044548        | -5.151685  | 2.592377e-07   | -0.316812    | -0.142182    |
| 0.000601  | 67.594755  | 10019.5169 | balance       | 0.000193     | 0.000155        | 1.238863   | 2.154025e-01   | -0.000112    | 0.000497     |
| 0.000601  | 67.594755  | 10019.5169 | duration      | -0.000701    | 0.001828        | -0.383603  | 7.012748e-01   | -0.004285    | 0.002882     |
| 0.000601  | 67.594755  | 10019.5169 | const         | 49.511376    | 1.935598        | 25.579364  | 0.000000e+00   | 45.717572    | 53.305181    |


**Giải thích kết quả:**

- r2: chỉ số R2 score cho mô hình, gần 1 là tốt nhất, gần 0 là bad.
- mae và mse là sai số của mô hình (càng nhỏ càng tốt)
- feature_names: List các cột được đưa vào regression (chương trình tự động thêm hệ số const). Trong bảng trên ta có thể tính:
target(pdays) = -0.229497 * age + 0.000193 * balance -0.000701 * duration + 49.511376
- p_values: dùng để kiểm định mô hình linear regression.
- lower_bounds và upper_bounds là khoảng tin cậy của hệ số regression.
