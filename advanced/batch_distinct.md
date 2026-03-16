# Advanced Distinct.


Remove duplicates


Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_distinct",
    "kwargs": {
      "input_arrow": "/tmp/tmpltz0xc1f/input.arrow",
      "output_arrow": "/tmp/tmpltz0xc1f/output.arrow",
      "operations": {
        "batch_size": 200,
        "memory_limit_mb": 400,
        "columns": [
          "category",
          "value"
        ],
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
- *columns*: List[String]: pushdown these column to output.
- *compression*: String: zstd, lz4


Output result:

| category | value |
| --- | --- |
| cat_88 | 188 |
| cat_55 | 155 |
| cat_9 | 109 |
| cat_77 | 177 |
| cat_37 | 137 |
| cat_21 | 121 |
| cat_7 | 7 |
| cat_52 | 152 |
| cat_11 | 111 |
| cat_8 | 8 |
| cat_14 | 114 |
| cat_84 | 84 |
| cat_71 | 71 |
| cat_13 | 13 |
| cat_97 | 197 |
| cat_38 | 138 |
| cat_51 | 151 |
| cat_30 | 30 |
| cat_99 | 199 |
| cat_79 | 79 |






