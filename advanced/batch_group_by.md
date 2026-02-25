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
      "input_arrow": "/tmp/tmp1xbj03wz.arrow",
      "output_arrow": "/tmp/tmpz4wrlycz.arrow",
      "operations": {
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
            "func": "stddev",
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
            "alias": "value2_quantile",
            "args": {
              "quantiles": [
                25,
                50,
                75
              ]
            }
          },
          {
            "func": "quantile",
            "alias": "value2_median",
            "args": {
              "quantiles": 50
            }
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

Chú ý:

* Tham số `num_partitions: 16` dùng để tách các nhóm thành 16 file nhỏ, nếu số lượng group nhiều thì nên tăng số partitions lên. `num_partitions` là một luỹ thừa của 2. VD: 16, 32, 64, 128,...

* Nếu chỉ tính median thì dùng tham số:
```json
{
            "func": "quantile",
            "alias": "value2_median",
            "args": {
              "quantiles": 50
            }
          }
```
* Nếu muốn tính nhiều quantiles thì dùng list các giá trị
```json
{
            "func": "quantile",
            "alias": "value2_quantile",
            "args": {
              "quantiles": [
                25,
                50,
                75
              ]
            }
          }
```


Output result:

| group | v1_sum | v1_min | v1_max | v1_count_all | v1_count_not_null | v1_count_null | v1_count_distinct | value2_first | value2_first_not_null | value2_last | value2_last_not_null | value2_quantile_q25 | value2_quantile_q50 | value2_quantile_q75 | value2_median | v1_stddev | v1_variance | v1_mean |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| C | None | None | None | 2 | 0 | 2 | 0 | None | None | None | None | None | None | None | None | None | None | None |
| B | 3 | 1 | 2 | 2 | 2 | 0 | 2 | 1 | 1 | 2 | 2 | 1.00 | 1.00 | 1.00 | 1.00 | 0.71 | 0.50 | 1.50 |
| A | 2 | 2 | 2 | 2 | 1 | 1 | 1 | None | 2 | 2 | 2 | 2.00 | 2.00 | 2.00 | 2.00 | None | None | 2.00 |


Support the following functions:

Các hàm này có thể bị crash nếu một group nào đó quá lớn: count_distinct, quantile. Các hàm còn lại sẽ hoạt động trên file lớn tuỳ ý.

```json
{
    "basic_stats",
    "sum",
    "max",
    "min",
    "count_all",
    "count_not_null",
    "count_null",
    "mean",
    "stddev",
    "variance",
    "count_distinct",
    "first",
    "first_not_null",
    "last",
    "last_not_null",
    "quantile",
}
```






