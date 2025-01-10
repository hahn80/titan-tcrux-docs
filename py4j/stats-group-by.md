# Các hàm thống kê theo nhóm.


Group và tính toán thống kê sẽ tiết kiệm được thời gian và quá trình chạy.

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "group_by.arrow",
    "tasks": [
        {
            "task": "transforming",
            "action": "group_by",
            "kwargs": {
                "by": ["job", "marital"],
                "operations": {
                    "age": [
                        {
                            "func": "min",
                            "alias": "age_min",
                            "args": [],
                        },
                        {"func": "max", "alias": "age_max", "args": []},
                        {"func": "std", "alias": "age_std", "args": []},
                        {"func": "cvar", "alias": "age_cvar", "args": []},
                        {"func": "confidence_interval", "alias": "age_ci", "args": []},
                    ],
                    "loan": [{"func": "frequency", "alias": "loan_freq", "args": []}],
                },
            },
        }
    ],
}
```

Kết quả output có dạng như sau:

| job             | marital   | age_min | age_max | age_std   | age_cvar  | age_ci              | loan_freq                              |
|------------------|-----------|---------|---------|-----------|-----------|---------------------|----------------------------------------|
| retired         | single    | 24      | 86      | 12.305292 | 0.223657  | -55.54014:165.57718 | [[{'val': 'no', 'freq': 89, 'rel_freq': 82.41}... |
| entrepreneur    | single    | 21      | 58      | 7.578248  | 0.214105  | -32.6929:103.48282  | [[{'val': 'yes', 'freq': 51, 'rel_freq': 21.43}... |
| admin.          | divorced  | 26      | 66      | 8.451030  | 0.196183  | -32.85216:119.00683 | [[{'val': 'no', 'freq': 582, 'rel_freq': 77.6}... |
| services        | single    | 20      | 60      | 7.057107  | 0.214715  | -30.53831:96.27287  | [[{'val': 'no', 'freq': 993, 'rel_freq': 82.89}... |
| entrepreneur    | divorced  | 29      | 61      | 8.033614  | 0.175453  | -26.39145:117.96686 | [[{'val': 'no', 'freq': 134, 'rel_freq': 74.86}... |
| admin.          | single    | 20      | 66      | 7.622093  | 0.222882  | -34.28387:102.67971 | [[{'val': 'no', 'freq': 1443, 'rel_freq': 83.5}... |
| admin.          | married   | 22      | 75      | 9.261378  | 0.223153  | -41.70776:124.71259 | [[{'val': 'no', 'freq': 2155, 'rel_freq': 80.0}... |
| services        | divorced  | 25      | 60      | 8.411953  | 0.195635  | -32.58022:118.57657 | [[{'val': 'no', 'freq': 436, 'rel_freq': 79.42}... |
| unemployed      | married   | 21      | 65      | 9.269511  | 0.213371  | -39.84002:126.72648 | [[{'val': 'no', 'freq': 652, 'rel_freq': 89.19}... |
| self-employed   | single    | 22      | 60      | 7.060741  | 0.210304  | -29.86425:97.01223  | [[{'val': 'no', 'freq': 393, 'rel_freq': 88.12}... |
| ...             | ...       | ...     | ...     | ...       | ...       | ...                 | ...                                    |



**Giải thích kết quả:**

Group by hỗ trợ tất cả các hàm đang có trong toán và arrow. Ngoài ra, một số hàm mới được hỗ trợ bao gồm:

iqr_mask; iqr_outlier; z_score; z_score_mask; z_score_outlier; confidence_interval; range; cvar; pearson_index; frequency;



