# IQR Outlier GroupBy.


Group và tính toán xác định outlier theo nhóm. Nếu số nhóm quá lớn, cần chuẩn bị RAM cho đủ nhé.


Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_outliers_groupby",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/large_g.arrow",
      "output_arrow": "/tmp/tmpn_2zzjp7/output",
      "operations": {
        "keys": [
          "doituongbenhnhanid"
        ],
        "columns": [
          "tongtien",
          "so_phut_kham"
        ],
        "batch_size": 3200,
        "whisker": 1.5,
        "abs_err": 0.0001,
        "m_batches": 4,
        "filter_strategy": null,
        "compression": "zstd"
      }
    }
  }
]
```

Chú ý:

* Tham số:

- *input_arrow*: String: input arrow file
- *output_arrow*: String: output arrow file
- *keys*: List[String]: groupby keys.
- *columns*: List of Strings.
- *batch_size*: Int: batch_size for the output
- *m_batches*: Int: number of batches to process at one loop.
- *whisker*: Float: the whiser value from 1 to 3 (default = 1.5)
- *abs_err*: Float: The absolute error in computing IQR (default=0.00005)
- *filter_strategy*: String: `null`, `AND`, `OR` (If `null`: return all rows, if `AND`: return all outlier rows from which all columns are trues, if `OR`: return all outlier rows for which of the columns is true)
- *compression*: String: zstd, lz4,










