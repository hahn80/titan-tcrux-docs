# Compute Global Pearson Correlation


## Pearson correlation

```json
[
  {
    "task": "batch_processing",
    "action": "global_pearson_corr",
    "kwargs": {
      "input_arrow": "/tmp/tmp19jy8nsg/cov_input.arrow",
      "output_arrow": "/tmp/tmp19jy8nsg/cov.arrow",
      "operations": {
        "columns": [
          "a",
          "b"
        ],
        "target": "c",
        "null_strategy": "drop",
        "compression": "zstd"
      }
    }
  }
]
```

Params:

- *input_arrow*: String: input arrow file
- *output_arrow*: String: output arrow file
- *columns*: List of String
- *target*: String: the target column to compute correlation against *columns*.
- *null_strategy*: String: Accept two values: `drop` or `fill`. If `null_strategy=fill`, then we need to add *fill_value = float value*.
- *compression*: String: zstd, lz4


Output result:

| a_rval | a_tval | a_pval | b_rval | b_tval | b_pval |
| --- | --- | --- | --- | --- | --- |
| 0.05 | 1.15 | 0.25 | 0.04 | 0.90 | 0.37 |


- *rval*: the correlation value
- *tval*: statistic value for correlation test
- *pval*: p-value for correlation test.



