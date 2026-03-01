# Compute Global Stats


Compute the describe stats for the big file.


Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "global_stats",
    "kwargs": {
      "input_arrow": "/tmp/tmp7_qzg6ep/input.arrow",
      "output_arrow": "/tmp/tmp7_qzg6ep/output.arrow",
      "operations": {
        "columns": [
          "Quantity",
          "Cost"
        ],
        "batch_size": 250,
        "m_batches": 8,
        "quantile_pcts": [
          35,
          50,
          75
        ]
      }
    }
  }
]
```


Output result:

| metric | Quantity | Cost |
| --- | --- | --- |
| count_all | 10000.00 | 10000.00 |
| count_null | 0.00 | 4000.00 |
| count_not_null | 10000.00 | 6000.00 |
| sum | 130000.00 | 140000.00 |
| mean | 13.00 | 23.33 |
| var | 52.00 | 155.56 |
| std | 7.21 | 12.47 |
| min | 1.00 | 10.00 |
| q35 | 9.00 | 20.00 |
| q50 | 13.00 | 20.00 |
| q75 | 19.00 | 40.00 |
| max | 25.00 | 40.00 |






