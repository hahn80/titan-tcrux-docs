# Compute Global Outliers


## IQR Whisker Method

```json
[
  {
    "task": "batch_processing",
    "action": "global_iqr_outlier",
    "kwargs": {
      "input_arrow": "/tmp/tmpt0vpw4qj/input.arrow",
      "output_arrow": "/tmp/tmpt0vpw4qj/output.arrow",
      "operations": {
        "columns": [
          "x1",
          "x2",
          "x3"
        ],
        "batch_size": 100,
        "m_batches": 8,
        "whisker": 1.5,
        "abs_err": 5e-05,
        "compression": "zstd",
        "filter_strategy": null
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
- *whisker*: Float: the whiser value from 1 to 3 (default = 1.5)
- *abs_err*: Float: The absolute error in computing IQR (default=0.00005)
- *compression*: String: zstd, lz4
- *filter_strategy*: String: `null`, `AND`, `OR` (If `null`: return all rows, if `AND`: return all outlier rows from which all columns are trues, if `OR`: return all outlier rows for which of the columns is true)


Output result:

| x1 | x1_outlier | x2 | x2_outlier | x3 | x3_outlier |
| --- | --- | --- | --- | --- | --- |
| 29 | False | -1.54 | False | 7.52 | True |
| 66 | False | 2.05 | False | 2.74 | False |
| 41 | False | 0.51 | False | 0.17 | False |
| 89 | False | -2.78 | True | 0.15 | False |
| 16 | False | 0.40 | False | 0.69 | False |
| 52 | False | -0.16 | False | 0.63 | False |
| 41 | False | 0.29 | False | 3.92 | False |
| 68 | False | 1.19 | False | 1.20 | False |
| 51 | False | 1.05 | False | 0.18 | False |
| 78 | False | -1.18 | False | 2.55 | False |
| 61 | False | -1.36 | False | 2.77 | False |
| 47 | False | 0.82 | False | 3.93 | False |
| 58 | False | -0.72 | False | 1.66 | False |
| 5 | False | -0.12 | False | 2.12 | False |
| 13 | False | -1.56 | False | 7.72 | True |
| 73 | False | -2.00 | False | 3.78 | False |
| 11 | False | -0.65 | False | 0.79 | False |
| 3 | False | -0.33 | False | 1.44 | False |
| 41 | False | 1.14 | False | 5.74 | False |
| 37 | False | 0.15 | False | 0.42 | False |



## Z-Score Method

```json
[
  {
    "task": "batch_processing",
    "action": "global_std_outlier",
    "kwargs": {
      "input_arrow": "/tmp/tmp8_1_bxyj/input.arrow",
      "output_arrow": "/tmp/tmp8_1_bxyj/output.arrow",
      "operations": {
        "columns": [
          "x1",
          "x2",
          "x3"
        ],
        "batch_size": 100,
        "m_batches": 8,
        "threshold": 2.0,
        "compression": "zstd",
        "filter_strategy": null
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
- *threshold*: Float: the z score threshold value from 3 to 3 (default = 2.0)
- *compression*: String: zstd, lz4
- *filter_strategy*: String: `null`, `AND`, `OR` (If `null`: return all rows, if `AND`: return all outlier rows from which all columns are trues, if `OR`: return all outlier rows for which of the columns is true)


Output result:

| x1 | x1_outlier | x2 | x2_outlier | x3 | x3_outlier |
| --- | --- | --- | --- | --- | --- |
| 94 | False | -0.53 | False | 3.23 | False |
| 27 | False | 1.63 | False | 2.73 | False |
| 92 | False | -1.07 | False | 1.81 | False |
| 88 | False | -0.86 | False | 6.65 | False |
| 83 | False | -0.08 | False | 0.02 | False |
| 43 | False | 1.16 | False | 1.22 | False |
| 5 | False | 0.41 | False | 0.28 | False |
| 12 | False | -1.34 | False | 1.93 | False |
| 98 | False | 0.03 | False | 4.25 | False |
| 79 | False | 0.56 | False | 1.97 | False |
| 19 | False | 1.25 | False | 2.04 | False |
| 9 | False | -0.92 | False | 1.18 | False |
| 13 | False | 0.18 | False | 2.41 | False |
| 4 | False | -0.32 | False | 3.50 | False |
| 79 | False | 1.19 | False | 2.41 | False |
| 43 | False | -0.44 | False | 4.98 | False |
| 59 | False | -0.41 | False | 5.55 | False |
| 72 | False | 0.37 | False | 1.34 | False |
| 24 | False | 0.66 | False | 1.30 | False |
| 74 | False | 1.91 | False | 1.32 | False |






