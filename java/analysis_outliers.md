# Outlier Analysis


## Outlier with IQR

Hàm xác định outlier bằng IQR được chạy dưới transforming -> numeric_transform.


```json
jobs = [
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "bbby.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "numeric_transform",
    "kwargs": {
      "operations": {
        "x1": [
          {
            "func": "iqr_outlier",
            "alias": "x1_outlier",
            "args": {
              "whisker": 1.5,
              "extreme_whisker": 3.0
            }
          }
        ],
        "x2": [
          {
            "func": "iqr_outlier",
            "alias": "x2_outlier",
            "args": {
              "whisker": 1.5,
              "extreme_whisker": 3.0
            }
          }
        ]
      }
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Giải thích tham số cho hàm `iqr_outlier`:

- *whisker*: 1.5 là hệ số xác định outlier thông thường.

- *extreme_whisker*: 3.0 là hệ số xác định outlier quá mức.


Output results:

- Nếu giá trị là -2, nghĩa là outlier quá mức (quá nhỏ)
- Nếu giá trị là -1, nghĩa là outlier nhỏ thông thường
- Tương tự cho các giá trị 1 và 2 cho outlier
- Giá trị 0 nghĩa là nắm trong vùng an toàn.

| x1 | x2 | x1_outlier | x2_outlier |
| --- | --- | --- | --- |
| -100 | -120 | -2 | -2 |
| -50 | -60 | -1 | -1 |
| 0 | 2 | 0 | 0 |
| 70 | 7 | 1 | 0 |
| 10 | 80 | 0 | 1 |
| 15 | 17 | 0 | 0 |
| 20 | 22 | 0 | 0 |
| 25 | 27 | 0 | 0 |
| 5 | 12 | 0 | 0 |
| 120 | 120 | 2 | 2 |



## Outlier with z-score

```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/bbby.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "numeric_transform",
    "kwargs": {
      "operations": {
        "x2": [
          {
            "func": "z_score_outlier",
            "alias": "x2_outlier",
            "args": {
              "threshold": 2,
              "extreme_threshold": 3
            }
          }
        ]
      }
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "/data/TitanProjects/tcrux/src/tests/resources/output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Output result: (outlier with -2, -1, 0, 1, 2)

| x1 | x2 | x2_outlier |
| --- | --- | --- |
| -100 | -120 | 0 |
| -50 | -60 | 0 |
| 0 | 2 | 0 |
| 70 | 7 | 0 |
| 10 | 80 | 0 |
| 15 | 17 | 0 |
| 20 | 22 | 0 |
| 25 | 27 | 0 |
| 5 | 12 | 0 |
| 120 | 120 | 0 |
