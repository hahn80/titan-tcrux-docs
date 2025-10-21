# Compute Percent Along Rows


This service compute the percent on each row (from multiple columns)


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
    "action": "row_percentage",
    "kwargs": {
      "operations": {
        "x1": {
          "alias": "x1_row_pct"
        },
        "x2": {
          "alias": "x2_row_pct"
        }
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

Explain the params:

- operations: We used two columns x1 and x2 to compute the percent on each row. For example on the first row, x1 = -100, x2 = -120. So x1 percent must be (-100)/(-100 + -120) = 45.45%.

Input data:

| x1 | x2 |
| --- | --- |
| -100 | -120 |
| -50 | -60 |
| 0 | 2 |
| 70 | 7 |
| 10 | 80 |
| 15 | 17 |
| 20 | 22 |
| 25 | 27 |
| 5 | 12 |
| 120 | 120 |


Output data:

| x1 | x2 | x1_row_pct | x2_row_pct |
| --- | --- | --- | --- |
| -100 | -120 | 45.45 | 54.55 |
| -50 | -60 | 45.45 | 54.55 |
| 0 | 2 | 0.00 | 100.00 |
| 70 | 7 | 90.91 | 9.09 |
| 10 | 80 | 11.11 | 88.89 |
| 15 | 17 | 46.88 | 53.12 |
| 20 | 22 | 47.62 | 52.38 |
| 25 | 27 | 48.08 | 51.92 |
| 5 | 12 | 29.41 | 70.59 |
| 120 | 120 | 50.00 | 50.00 |



