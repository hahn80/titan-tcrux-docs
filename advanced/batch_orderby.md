# Advanced Orderby.


Batch orderby:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_orderby",
    "kwargs": {
      "input_arrow": "/tmp/tmpno1oe5y8/input.arrow",
      "output_arrow": "/tmp/tmpno1oe5y8/output.arrow",
      "operations": {
        "sort_keys": [
          [
            "id",
            true
          ],
          [
            "value",
            false
          ]
        ],
        "columns": [
          "value",
          "score"
        ],
        "batch_size": 32000,
        "memory_limit_mb": 200,
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
- *memory_limit_mb*: Int: max size of memory in MB to process (avoid) OOM.
- *sort_keys*: The list of pairs (key, order_boolean).
- *columns*: List[String]: pushdown these column to output.
- *compression*: String: zstd, lz4


Output result:

| value | score |
| --- | --- |
| 8754 | 2492 |
| 6057 | 7886 |
| 1768 | 16464 |
| 1017 | 17966 |
| 7808 | 4384 |
| 4772 | 10456 |
| 996 | 18008 |
| 5591 | 8818 |
| 7259 | 5482 |
| 1494 | 17012 |
| 4808 | 10384 |
| 3657 | 12686 |
| 5694 | 8612 |
| 2668 | 14664 |
| 6180 | 7640 |
| 8755 | 2490 |
| 886 | 18228 |
| 7641 | 4718 |
| 113 | 19774 |
| 6252 | 7496 |







