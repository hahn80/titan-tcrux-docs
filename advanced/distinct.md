# Advanced Distinct.


Remove duplicates


Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "bath_distinct",
    "kwargs": {
      "input_arrow": "/tmp/tmpzcdqrfb0/input.arrow",
      "output_arrow": "/tmp/tmpzcdqrfb0/output.arrow",
      "operations": {
        "batch_size": 200,
        "max_way": 4
      }
    }
  }
]
```


Output result:

| id | category | value |
| --- | --- | --- |
| 1605 | cat_5 | 5 |
| 2942 | cat_42 | 142 |
| 316 | cat_16 | 116 |
| 3615 | cat_15 | 15 |
| 4785 | cat_85 | 185 |
| 4867 | cat_67 | 67 |
| 2871 | cat_71 | 71 |
| 2079 | cat_79 | 79 |
| 4558 | cat_58 | 158 |
| 1900 | cat_0 | 100 |
| 2539 | cat_39 | 139 |
| 2227 | cat_27 | 27 |
| 3516 | cat_16 | 116 |
| 3992 | cat_92 | 192 |
| 3572 | cat_72 | 172 |
| 3297 | cat_97 | 97 |
| 1549 | cat_49 | 149 |
| 4479 | cat_79 | 79 |
| 471 | cat_71 | 71 |
| 1250 | cat_50 | 50 |






