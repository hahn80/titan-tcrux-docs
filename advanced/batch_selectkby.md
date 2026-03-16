# Advanced SelectKBy.


Batch selectkby

```json
[
  {
    "task": "batch_processing",
    "action": "batch_selectkby",
    "kwargs": {
      "input_arrow": "/tmp/tmpxg1tfi90/input.arrow",
      "output_arrow": "/tmp/tmpxg1tfi90/output.arrow",
      "operations": {
        "sort_keys": [
          [
            "id",
            true
          ]
        ],
        "k": 100,
        "batch_size": 32000,
        "columns": [
          "value",
          "score"
        ],
        "memory_limit_mb": 4096,
        "compression": "zstd"
      }
    }
  }
]
```

Params:

- *input_arrow*: String: input arrow file
- *output_arrow*: String: output arrow file
- *batch_size*: Int: batch_size for the output
- *sort_keys*: The list of pairs (key, order_boolean).
- *k*: Int: The number of rows to select.
- *memory_limit_mb*: Int: max size of memory in MB to process (avoid) OOM.
- *columns*: List[String]: pushdown these column to output.
- *compression*: String: zstd, lz4


Output result:

| value | score |
| --- | --- |
| 9996 | 8 |
| 9982 | 36 |
| 9991 | 18 |
| 9960 | 80 |
| 9959 | 82 |
| 9951 | 98 |
| 9929 | 142 |
| 9972 | 56 |
| 9911 | 178 |
| 9916 | 168 |
| 9967 | 66 |
| 9971 | 58 |
| 9990 | 20 |
| 9900 | 200 |
| 9963 | 74 |
| 9984 | 32 |
| 9919 | 162 |
| 9913 | 174 |
| 9978 | 44 |
| 9976 | 48 |







