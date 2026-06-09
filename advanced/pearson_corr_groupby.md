# Compute Pearson Correlation by Groups


## Pearson correlation groupby

```json
[
  {
    "task": "batch_processing",
    "action": "batch_pearson_corr_groupby",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/large_g.arrow",
      "output_arrow": "/tmp/tmpafn5qvtd/output",
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

| doituongbenhnhanid | tongtien | so_phut_kham |
| --- | --- | --- |
| 2 | 1.00 | -0.01 |
| 3 | 1.00 | -0.02 |
| 1 | 1.00 | -0.00 |
| 5 | 1.00 | -0.12 |



