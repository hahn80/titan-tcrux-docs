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
      "input_arrow": "/tmp/tmpxa58y0jd.arrow",
      "output_arrow": "/tmp/tmpc_hqidwy.arrow",
      "operations": {
        "value1": [
          {
            "func": "sum",
            "alias": "v1_sum",
            "args": []
          },
          {
            "func": "count",
            "alias": "v1_count_all",
            "args": [
              "all"
            ]
          },
          {
            "func": "count",
            "alias": "v1_count_not_null",
            "args": [
              "not_null"
            ]
          },
          {
            "func": "count",
            "alias": "v1_count_null",
            "args": [
              "null"
            ]
          },
          {
            "func": "mean",
            "alias": "v1_mean",
            "args": []
          }
        ],
        "value2": [
          {
            "func": "first",
            "alias": "value2_first",
            "args": []
          },
          {
            "func": "last",
            "alias": "value2_last",
            "args": []
          }
        ]
      },
      "by": [
        "group"
      ],
      "num_partitions": 16
    }
  }
]
```


Output result:

| group | v1_sum | v1_count_all | v1_count_not_null | v1_count_null | v1_mean | value2_first | value2_last |
| --- | --- | --- | --- | --- | --- | --- | --- |
| B | 3 | 2 | 2 | 0 | 1.50 | 1 | 2 |
| A | 2 | 2 | 1 | 1 | 2.00 | 2 | 2 |
| C | None | 2 | 0 | 2 | None | None | None |


Support the following functions:

```json
{
    "sum",
    "max",
    "min",
    "count",
    "mean",
    "stddev",
    "variance",
    "count_distinct",
    "first",
    "last",
}
```






