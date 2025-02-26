# Percentile Ranking.


1. Tính phần trăm của điểm dữ liệu theo cột:

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "output.arrow",
    "tasks": [
        {
            "task": "transforming",
            "action": "numeric_transform",
            "kwargs": {
                "operations": {"col_name": [{'func': 'percent_rank', 'alias': 'col_pct_rank', 'args': {'method': 'average'}}]}
            },
        }
    ]
}
```

Giải thích tham số:
- `operations`: Hashmap of key-values, each key is a column name, value is a list of functions.

2. Tính phần trăm theo hàng

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "output.arrow",
    "tasks": [
        {
            "task": "transforming",
            "action": "row_percent_rank",
            "kwargs": {
                "columns": ["ten_col1", "ten_col2"],
				"skip_negative": true,
				"method": "average",
            },
        }
    ],
}
```

Giải thích tham số:
- `columns`: Array of String -> key for multiple input column names
- `skip_negative`: boolean: Set TRUE nếu không tính các giá trị âm hoặc =0 vào quá trình ranking. Chương trình sẽ tự động loại các giá trị âm trước khi tính.
- `method`: String: phương pháp để tính ranking (average, dense).
