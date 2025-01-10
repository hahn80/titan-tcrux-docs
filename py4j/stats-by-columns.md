# Các hàm thống kê theo cột.


Tính toán các hàm thống kê của các cột: z_score, iqr_outlier, z_score_outlier, confidence_interval, frequency, iqr_mask, z_score_mask

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
                        {"func": "z_score", "alias": "age_zscore", "args": []},
                        {"func": "iqr_mask", "alias": "age_iqr_mask", "args": []},
                    ],
                    "balance": [
                        {"func": "z_score", "alias": "balance_zscore", "args": []},
                        {
                            "func": "z_score_mask",
                            "alias": "balance_zscore_mask",
                            "args": [],
                        },
                    ],
                }
            },
        }
    ],
}
```

Kết quả output có dạng như sau:

| age_zscore | age_iqr_mask | balance_zscore | balance_zscore_mask |
|------------|--------------|----------------|----------------------|
| 1.606947   | True         | 0.256416       | True                |
| 0.288526   | True         | -0.437890      | True                |
| -0.747376  | True         | -0.446758      | True                |
| 0.571045   | True         | 0.047205       | True                |
| -0.747376  | True         | -0.447086      | True                |
| ...        | ...          | ...            | ...                 |
| 0.947737   | True         | -0.176458      | True                |
| 2.831195   | False        | 0.120445       | True                |
| 2.925368   | False        | 1.429577       | True                |
| 1.512774   | True         | -0.228021      | True                |
| -0.370684  | True         | 0.528359       | True                |


**Giải thích kết quả:**

các hàm iqr_mask và z_score_mask dùng để check xem dữ liệu có nằm trong vùng an toàn của outlier hay không? Nếu là True thì trong vùng an toàn, nếu là False thì thuộc outlier.

