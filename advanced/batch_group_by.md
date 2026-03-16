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
      "input_arrow": "/tmp/tmp6e4t3t09.arrow",
      "output_arrow": "/tmp/tmpptwqurna.arrow",
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


Output result:

| group | v1_sum | v1_min | v1_max | v1_count_all | v1_count_not_null | v1_count_null | v1_count_distinct | v1_mean | v1_stddev | v1_variance | value2_first | value2_first_not_null | value2_last | value2_last_not_null |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A | 2 | 2 | 2 | 2 | 1 | 1 | 2 | 2.00 | None | None | None | 2 | 2 | 2 |
| B | 3 | 1 | 2 | 2 | 2 | 0 | 2 | 1.50 | 0.71 | 0.50 | 1 | 1 | 2 | 2 |
| C | None | None | None | 2 | 0 | 2 | 1 | None | None | None | None | None | None | None |


Support the following functions:
- sum; max; min; mean; stddev; variance
- count_all; count_not_null; count_null
- first; first_not_null; last; last_not_null
- count_distinct







