# Compute Global Stats


Compute the describe stats for the big file.

```json
[
  {
    "task": "batch_processing",
    "action": "global_stats",
    "kwargs": {
      "input_arrow": "/tmp/tmp3oo6jbjr/input.arrow",
      "output_arrow": "/tmp/tmp3oo6jbjr/output.arrow",
      "operations": {
        "columns": [
          "Quantity",
          "Cost",
          "Country"
        ],
        "batch_size": 250,
        "m_batches": 8,
        "quantile_pcts": [
          0.25,
          0.5,
          0.75
        ],
        "with_count_distinct": true
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
- *quantile_pcts*: List[float]: List of percentile for quantiles (between 0 and 1).
- *with_count_distinct*: true/false to add count_distinct to output.
- *compression*: String: zstd, lz4


Output result:

| metric | Quantity | Cost | Country |
| --- | --- | --- | --- |
| count_all | 10000.00 | 10000.00 | 10000.00 |
| count_null | 0.00 | 0.00 | 4000.00 |
| count_not_null | 10000.00 | 10000.00 | 6000.00 |
| count_distinct | 5.00 | 25.00 | 3.00 |
| sum | nan | 130000.00 | 140000.00 |
| mean | nan | 13.00 | 23.33 |
| var | nan | 52.00 | 155.56 |
| std | nan | 7.21 | 12.47 |
| min | nan | 1.00 | 10.00 |
| q25 | nan | nan | 10.00 |
| q50 | nan | nan | 20.00 |
| q75 | nan | nan | 40.00 |
| max | nan | 25.00 | 40.00 |








