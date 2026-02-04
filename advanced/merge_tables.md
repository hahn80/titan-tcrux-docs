# Advanced Merge.


Merge multiple tables.


Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_merge",
    "kwargs": {
      "input_files": [
        "/tmp/tmp31fvdsx7/input_0.arrow",
        "/tmp/tmp31fvdsx7/input_1.arrow",
        "/tmp/tmp31fvdsx7/input_2.arrow",
        "/tmp/tmp31fvdsx7/input_3.arrow",
        "/tmp/tmp31fvdsx7/input_4.arrow"
      ],
      "output_arrow": "/tmp/tmp31fvdsx7/output.arrow",
      "operations": {
        "batch_size": 400,
        "num_workers": 4
      }
    }
  }
]
```


Output result:

| id | value | name |
| --- | --- | --- |
| 1844 | 3688 | item_1844 |
| 3125 | 6250 | item_3125 |
| 2422 | 4844 | item_2422 |
| 2658 | 5316 | item_2658 |
| 332 | 664 | item_332 |
| 3755 | 7510 | item_3755 |
| 1684 | 3368 | item_1684 |
| 1575 | 3150 | item_1575 |
| 2855 | 5710 | item_2855 |
| 3247 | 6494 | item_3247 |
| 768 | 1536 | item_768 |
| 354 | 708 | item_354 |
| 3489 | 6978 | item_3489 |
| 716 | 1432 | item_716 |
| 4734 | 9468 | item_4734 |
| 3137 | 6274 | item_3137 |
| 3217 | 6434 | item_3217 |
| 2078 | 4156 | item_2078 |
| 2918 | 5836 | item_2918 |
| 2374 | 4748 | item_2374 |






