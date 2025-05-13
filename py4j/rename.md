# Rename Dataframe.


Tham số để gọi service:

```json
params = {
    "input": "input.arrow",
    "output": "output.arrow",
    "tasks": [
        {
            "task": "transforming",
            "action": "rename",
            "kwargs": {
					"operations": {
					"num": {"alias": "num_new"},
					"count": {"alias": "count_new"},
				}
            },
        }
    ],
}
```

Giải thích tham số:

- `alias`: new name will be assigned to the alias values.
