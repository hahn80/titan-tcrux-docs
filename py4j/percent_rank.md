# Percentile Ranking.


1. Tính phần trăm của điểm dữ liệu theo cột:

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "grp_by_bnk.arrow",
    "tasks":
    [
        {
            "task": "transforming",
            "action": "group_by",
            "kwargs":
            {
                "operations":
                {
                    "age":
                    [
                        {
                            "func": "percent_rank",
                            "alias": "age_pct_rank",
                            "method": "average"
                        }
                    ],
                    "duration":
                    [
                        {
                            "func": "percent_rank",
                            "alias": "duration_pct_rank",
                            "method": "average"
                        }
                    ]
                },
                "by":
                [
                    "job",
                    "marital"
                ],
                "explode": true
            }
        }
    ]
}
```

Giải thích tham số:
- `operations`: Hashmap of key-values, each key is a column name, value is a list of functions.
- `by`: List of group-by column names
- `explode`: True/False: to unpack the list type, if False, it returns dataframe with List Type.

2. Tính phần trăm theo hàng

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "banking_grp_by.arrow",
    "tasks":
    [
        {
            "task": "transforming",
            "action": "row_percent_rank",
            "kwargs":
            {
                "operations":
                {
                    "age":
                    {
                        "alias": "age_pct_rank"
                    },
                    "duration":
                    {
                        "alias": "duration_pct_rank"
                    }
                },
                "by":
                [
                    "job",
                    "marital"
                ],
                "skip_negative": true,
                "method": "average"
            }
        }
    ]
}
```

Giải thích tham số:
- `operations`: Hashmap of key, value: key=column name; value is a hashmap of alias name.
- `skip_negative`: boolean: Set TRUE nếu không tính các giá trị âm hoặc =0 vào quá trình ranking. Chương trình sẽ tự động loại các giá trị âm trước khi tính.
- `method`: String: phương pháp để tính ranking (average, dense).
- `by`: List of Strings (names of group-by columns)
