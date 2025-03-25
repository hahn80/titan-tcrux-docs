# Change type of data (casting).


Tham số để gọi service:

```json
params = {
    "input": "input.arrow",
    "output": "output.arrow",
    "tasks": [
        {
            "task": "transforming",
            "action": "change_type",
            "kwargs": {
					"operations": {
					"num": {"alias": "num", "dtype": "DOUBLE"},
					"count": {"alias": "count", "dtype": "DOUBLE"},
				}
            },
        }
    ],
}
```

Giải thích tham số:

- `dtype`: New type to cast to, suppported: "INT", "LONG", "DOUBLE", "STRING"
