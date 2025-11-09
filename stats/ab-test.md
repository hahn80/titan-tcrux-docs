# AB Testing.


AB testing thường được sử dụng trong việc so sánh kết quả của 2 hoặc nhiều chiến dịch khác nhau.

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "basic.arrow",
    "tasks": [
        {
            "task": "statistics",
            "action": "ab_test",
            "kwargs": {
                "group": "name_of_campains",
				"value": "gia_tri_thu_nhap",
				"alpha": 0.05
            },
        }
    ],
}
```

| campains | expense |
| A | 10 |
| B | 15 |
| A | 5 |
| B | 20 |
| C | 40 |
| D | 20 |



Giải thích tham số:

- "group": String (name of the grouped column)
- "value": String (name of value column to comare),
- "alpha": Float (from 0.01 to 0.1) significant level.

TODO:
- input data
- output result and meaning
- 2 options: simple and pca on which variables?

