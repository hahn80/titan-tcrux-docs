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

| column_name | count_all | count_null | count_not_null | count_distinct | sum | mean | var | std | min | q25 | q50 | q75 | max |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Quantity | 10000 | 0 | 10000 | 25 | 130000.00 | 13.00 | 52.00 | 7.21 | 1.00 | 7.00 | 13.00 | 19.00 | 25.00 |
| Cost | 10000 | 4000 | 6000 | 3 | 140000.00 | 23.33 | 155.56 | 12.47 | 10.00 | 10.00 | 20.00 | 40.00 | 40.00 |
| Country | 10000 | 0 | 10000 | 5 | nan | nan | nan | nan | None | nan | nan | nan | None |








