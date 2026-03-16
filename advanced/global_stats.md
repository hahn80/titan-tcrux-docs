# Compute Global Stats


Compute the describe stats for the big file.

```json
[
  {
    "task": "batch_processing",
    "action": "global_stats",
    "kwargs": {
      "input_arrow": "/tmp/tmpk4neue7k/input.arrow",
      "output_arrow": "/tmp/tmpk4neue7k/output.arrow",
      "operations": {
        "columns": [
          "Quantity",
          "Cost"
        ],
        "batch_size": 250,
        "m_batches": 8,
        "quantile_pcts": [
          25,
          50,
          75
        ]
      }
    }
  }
]
```

Params:

- *input_arrow*: String: input arrow file
- *output_arrow*: String: output arrow file
- *batch_size*: Int: batch_size for the output
- *m_batches*: Int: number of batches to process at once.
- *memory_limit_mb*: Int: max size of memory in MB to process (avoid) OOM.
- *columns*: List[String]: pushdown these column to output.
- *quantile_pcts*: List[Int]: List of percentile for quantiles.
- *compression*: String: zstd, lz4


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
| q25 | 9.00 | 20.00 |
| q50 | 13.00 | 20.00 |
| q75 | 19.00 | 40.00 |
| max | 25.00 | 40.00 |







