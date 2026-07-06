# Rename/Drop/Cast


We can use rename, drop, and cast together or each of them separately.

Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_query",
    "kwargs": {
      "input_arrow": "/tmp/tmpss95qm56/input.arrow",
      "output_arrow": "/tmp/tmpss95qm56/output.arrow",
      "operations": {
        "query": "SELECT * EXCLUDE col1, col2\n FROM Dummy\n        TRANSFORM\n rename(col1) as new_col1,\n rename(col2) as new_col2,\n cast(col3, \"string\") as new_col3,\n ADD_COLUMN(10, \"int64\") as col4",
        "batch_size": 250,
        "memory_limit_mb": 300,
        "compression": "zstd"
      }
    }
  }
]
```

- *input_arrow*: String: input arrow file.
- *output_arrow*: String: output arrow file.
- *batch_size*: Int: batch_size for the output.
- *memory_limit_mb*: Int: max size of memory in MB to process (avoid) OOM.
- *query*: support the following syntax.
	- `rename(old_col) as new_col` to rename old_col as new_col
	- `SELECT * EXCLUDE col1, col2, ..., colN` drop col1 to colN
	- `cast(col, dtype) as new_col`: cast the col to new type. dtype is string (lowercase) putting inside quotes. Supported: 

| String | Arrow type |
|--------|-----------|
| `"int8"` | `int8` |
| `"int16"` | `int16` |
| `"int32"` | `int32` |
| `"int64"` | `int64` |
| `"float16"` | `float16` |
| `"float32"` | `float32` |
| `"float64"` | `float64` |
| `"utf8"` or `"string"` | UTF-8 string |
| `"boolean"` or `"bool"` | boolean |

**Example:**
```SQL
cast(col2, "float64")
cast(col1, "string")
```

