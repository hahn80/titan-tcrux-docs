# Numeric Transform

Transforming numeric data supports all math functions and some interestingly complex functions as rolling windows.

The goal of this function is to transform a numeric data from column to a new column attached to the old dataframe. It will produce the same length dataframe with new columns.

With task `transforming` and action `numeric_transform`, we can generate a new data using a

- math functions: "exp", "log", "log10", "log1p", "pct_change", "rank", "sin", "tan", "cos"
- cumulative function: "cum_sum", "cum_min", "cum_max"
- rolling windows: "rolling_min", "rolling_max", "rolling_sum"
- customized functions: percent_rank
- rolling by: This is an interesting function that will be introduced in another topic.

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
        "x1": [
          {
            "func": "rank",
            "alias": "x1_rank",
            "args": {
              "method": "average"
            }
          },
          {
            "func": "percent_rank",
            "alias": "x1_pct_rank",
            "args": {
              "method": "average"
            }
          },
          {
            "func": "pct_change",
            "alias": "x1_pct_change",
            "args": []
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


Output result:


| x1 | x2 | x1_rank | x1_pct_rank | x1_pct_change |
| --- | --- | --- | --- | --- |
| -100 | -120 | 1.00 | 0.10 | None |
| -50 | -60 | 2.00 | 0.20 | -0.50 |
| 0 | 2 | 3.00 | 0.30 | -1.00 |
| 70 | 7 | 9.00 | 0.90 | inf |
| 10 | 80 | 5.00 | 0.50 | -0.86 |
| 15 | 17 | 6.00 | 0.60 | 0.50 |
| 20 | 22 | 7.00 | 0.70 | 0.33 |
| 25 | 27 | 8.00 | 0.80 | 0.25 |
| 5 | 12 | 4.00 | 0.40 | -0.80 |
| 120 | 120 | 10.00 | 1.00 | 23.00 |
