# Advanced Quantile GroupBy.


Group và tính toán thống kê cho quantile. Mục đích là tính chính xác quantiles bằng phương pháp ES.

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
    "action": "batch_quantile_groupby",
    "kwargs": {
      "input_arrow": "/tmp/tmpm8r9q2p5.arrow",
      "output_arrow": "/tmp/tmpj0iefi0q.arrow",
      "operations": {
        "keys": [
          "group"
        ],
        "aggregates": [
          {
            "input_column": "value1",
            "output_alias": "value1",
            "quantiles": [
              0.25,
              0.5,
              0.75
            ],
            "abs_err": 0.0001
          },
          {
            "input_column": "value2",
            "output_alias": "value2",
            "quantiles": [
              0.25,
              0.5,
              0.75,
              0.95
            ],
            "abs_err": 0.0001
          }
        ],
        "memory_limit_mb": 4096,
        "batch_size": 32000,
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
- *compression*: String: zstd, lz4
- Params for aggregates:
	- *quantiles*: List of floats from 0.1 to 0.99; 0.25 for Q1, 0.5 for Q2 (median) and 0.75 for Q3.
	- *abs_err*: 0.0001. Use this default value as it does not effect the calculation much. We are computing the exact quatiles in this case.


Output result:

| group | value1_Q25 | value1_Q50 | value1_Q75 | value2_Q25 | value2_Q50 | value2_Q75 | value2_Q95 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| A | 2.00 | 2.00 | 2.00 | 2.00 | 2.00 | 2.00 | 2.00 |
| B | 1.00 | 1.00 | 2.00 | 1.00 | 1.00 | 2.00 | 2.00 |
| C | None | None | None | None | None | None | None |








