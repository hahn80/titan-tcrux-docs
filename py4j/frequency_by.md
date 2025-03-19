# Hàm thống kê frequencey by.


Tính thống kê frequency kết hợp với group by.

Tham số để gọi service:

```json
params = {
        "input": "banking.arrow",
		"output": "group_by.arrow",
		"tasks": [
			{
            "task": "transforming",
            "action": "frequency_by",
			"kwargs": {
					"column": "color",
					"alias": {"val": "color_val", "req": "color_freq", "rel": "color_rel"},
					"by": "model",
			},
    }
```

Kết quả input/output có dạng như sau:

Input: 

| color | model |
|-------|-------|
| red   | 1     |
| blue  | 1     |
| red   | 1     |
| green | 1     |
| blue  | 1     |
| …     | …     |
| blue  | 2     |
| red   | 2     |
| green | 2     |
| blue  | 2     |
| blue  | 2     |

Output:

| model | color_val | color_freq | color_rel |
|-------|-----------|------------|------------|
| 2     | red       | 2          | 33.33      |
| 2     | blue      | 3          | 50.0       |
| 2     | green     | 1          | 16.67      |
| 1     | red       | 2          | 33.33      |
| 1     | blue      | 3          | 50.0       |
| 1     | green     | 1          | 16.67      |


**Giải thích kết quả:**

*column*: String, name of the column to calculate the frequency
*by*: String, name of the column to group_by. If we do not have any column to group_by, we set by=null.


