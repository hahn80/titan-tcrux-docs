# Advanced GroupBy.


Group và tính toán thống kê.

Input data:

| group | value1 | value2 |
| --- | --- | --- |
| A | None | None |
| A | 2 | 2 |
| B | 1 | 1 |
| B | 2 | 2 |
| C | None | None |
| C | None | None |


Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_groupby",
    "kwargs": {
      "input_arrow": "/tmp/tmpglegw3oe.arrow",
      "output_arrow": "/tmp/tmpk5osntn7.arrow",
      "operations": {
        "keys": [
          "group"
        ],
        "batch_size": 500,
        "memory_limit_mb": 1024,
        "aggregates": {
          "value1": [
            {
              "func": "sum",
              "alias": "v1_sum"
            },
            {
              "func": "min",
              "alias": "v1_min"
            },
            {
              "func": "max",
              "alias": "v1_max"
            },
            {
              "func": "count_all",
              "alias": "v1_count_all"
            },
            {
              "func": "count_not_null",
              "alias": "v1_count_not_null"
            },
            {
              "func": "count_null",
              "alias": "v1_count_null"
            },
            {
              "func": "count_distinct",
              "alias": "v1_count_distinct"
            },
            {
              "func": "mean",
              "alias": "v1_mean"
            },
            {
              "func": "std",
              "alias": "v1_stddev"
            },
            {
              "func": "variance",
              "alias": "v1_variance"
            }
          ],
          "value2": [
            {
              "func": "first",
              "alias": "value2_first"
            },
            {
              "func": "first_not_null",
              "alias": "value2_first_not_null"
            },
            {
              "func": "last",
              "alias": "value2_last"
            },
            {
              "func": "last_not_null",
              "alias": "value2_last_not_null"
            },
            {
              "func": "quantile",
              "alias": "spline_es",
              "args": {
                "quantiles": [
                  0.25,
                  0.5,
                  0.75
                ],
                "method": "es_hybrid",
                "abs_err": 0.0001
              }
            },
            {
              "func": "quantile",
              "alias": "spline_sp",
              "args": {
                "quantiles": [
                  0.5
                ],
                "max_samples": 300,
                "method": "spline"
              }
            }
          ]
        },
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
- *batch_size*: Int: batch_size for the output
- *memory_limit_mb*: Int: max size of memory in MB to process (avoid) OOM.
- *columns*: List[String]: pushdown these column to output.
- *compression*: String: zstd, lz4
- Params for quantile:
	- *method*: Supported `es_hybrid` and `spline` (these are approximated quantiles, not exact ones)
	- *quantiles*: List of floats from 0.1 to 0.99; 0.25 for Q1, 0.5 for Q2 (median) and 0.75 for Q3.
	- If method == `es_hybrid`, we need param `abs_err`: 0.0001. This is the relative error to approximate the quantile, do not set it too small as it runs slower. Anything from 0.001 to 0.00001 will be okay.
	- If method == `spline`, we need param `max_samples` to approximate the spline curve for quantile. This value is INT and varies from 200 to 500.


Output result:

| group | v1_sum | v1_min | v1_max | v1_count_all | v1_count_not_null | v1_count_null | v1_count_distinct | v1_mean | v1_stddev | v1_variance | value2_first | value2_first_not_null | value2_last | value2_last_not_null | spline_es_Q25 | spline_es_Q50 | spline_es_Q75 | spline_sp_Q50 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A | 2 | 2 | 2 | 2 | 1 | 1 | 2 | 2.00 | None | None | None | 2 | 2 | 2 | 2.00 | 2.00 | 2.00 | 2.00 |
| B | 3 | 1 | 2 | 2 | 2 | 0 | 2 | 1.50 | 0.71 | 0.50 | 1 | 1 | 2 | 2 | 1.00 | 1.00 | 2.00 | 1.50 |
| C | None | None | None | 2 | 0 | 2 | 1 | None | None | None | None | None | None | None | None | None | None | None |


Support the following functions:
- sum; max; min; mean; stddev; variance
- count_all; count_not_null; count_null
- first; first_not_null; last; last_not_null
- count_distinct
- quantile







