# Chọn các members theo phần trăm / số lựa chọn.


1. Chọn theo phần trăm:

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "output.arrow",
    "tasks": [
        {
            "task": "statistics",
            "action": "best_percent_members",
            "kwargs": {
                "column": "ten_col",
				"percent": 10.0,
				"direction": "top",
            },
        }
    ],
}
```

Giải thích tham số:
- `column`: String -> key for input column name
- `percent`: Float: From 1 to 99 pertentage
- `direction`: either 'top' or 'bottom'. Top là chọn 10% cao nhất, bottom là chọn 10% thấp nhất.

2. Chọn theo số thành viên

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "output.arrow",
    "tasks": [
        {
            "task": "statistics",
            "action": "best_k_members",
            "kwargs": {
                "columns": ["ten_col1", "ten_col2"],
				"k": 2,
				"direction": "top",
            },
        }
    ],
}
```

Giải thích tham số:
- `columns`: Array of String -> key for multiple input column names
- `k`: Int: VD: k=2, chọn 2 phần tử tốt nhất.
- `direction`: either 'top' or 'bottom'. Top là chọn 2 cao nhất, bottom là chọn 2 thấp nhất.


3. Chọn theo joint-probability

VD: Chọn khách hàng nhỏ tuổi nhất nhưng thu nhập cao nhất.

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "output.arrow",
    "tasks": [
        {
            "task": "statistics",
            "action": "best_probability",
            "kwargs": {
                "operations": {"col_1": {"direction": "top", "weight": 1.0}, "col_2": {"direction": "bottom", "weight": 1.1}}
				"percent": 10,
				// "k": 2
            },
        }
    ],
}
```

Giải thích tham số:
- `operations`: Hashmap contains keys as column names and values as hashmap of direction and weight. Default weight is 1.0
- `percent`: This is a float number 0 < percent < 100.
- `k`: Int: VD: k=2, chọn 2 phần tử tốt nhất.
- Lưu ý, chỉ một trong hai tham số `percent` hoặc `k` được đưa vào; không được đưa cả 2 cùng một lúc.
