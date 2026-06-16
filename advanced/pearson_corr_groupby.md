# Compute Pearson Correlation by Groups


## Pearson correlation groupby

```json
[
  {
    "task": "batch_processing",
    "action": "batch_pearson_corr_groupby",
    "kwargs": {
      "input_arrow": "/tmp/tests/resources/large_g.arrow",
      "output_arrow": "/tmp/tmpb05pv0q0/output",
      "operations": {
        "keys": [
          "doituongbenhnhanid"
        ],
        "columns": [
          "tongtien",
          "so_phut_kham"
        ],
        "target": "tongtien",
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
- *keys*: List of String
- *columns*: List of String
- *target*: String: the target column to compute correlation against *columns*.
- *null_strategy*: String: Accept two values: `drop` or `fill`. If `null_strategy=fill`, then we need to add *fill_value = float value*.
- *compression*: String: zstd, lz4


Output result:

| doituongbenhnhanid | tongtien_rval | tongtien_tval | tongtien_pval | so_phut_kham_rval | so_phut_kham_tval | so_phut_kham_pval |
| --- | --- | --- | --- | --- | --- | --- |
| 2 | 1.00 | 35538785039.71 | 0.00 | -0.01 | -8.85 | 0.00 |
| 3 | 1.00 | inf | nan | -0.02 | -10.33 | 0.00 |
| 1 | 1.00 | 72622465768.70 | 0.00 | -0.00 | -4.48 | 0.00 |
| 5 | 1.00 | inf | nan | -0.12 | -21.50 | 0.00 |



