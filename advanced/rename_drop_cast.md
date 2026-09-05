# Rename/Drop/Cast


We can use rename, drop, and cast together or each of them separately.

Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_query",
    "kwargs": {
      "input_arrow": "/tmp/tmp7na3g_4h/input.arrow",
      "output_arrow": "/tmp/tmp7na3g_4h/output.arrow",
      "operations": {
        "query": "\n        SELECT * EXCLUDE col1, col2\n        FROM Dummy\n        TRANSFORM\n        rename(col1) as new_col1,\n        rename(col2) as new_col2,\n        cast(col3, \"string\") as new_col3,\n        ADD_COLUMN(10, \"int64\") as col4,\n        ADD_COLUMN('2026/01/01', \"DATE\", \"%Y/%m/%d\") as col9,\n        ADD_COLUMN('2025-10-20 12:00:00', \"DATETIME\", \"%Y-%m-%d %H:%M:%S\", \"ms\") as col10,\n        ADD_COLUMN('23:59:59', \"TIME\", \"%H:%M:%S\", \"ms\") as col11,\n        ADD_COLUMN(5520, \"DURATION\", \"ms\") as col12,\n        ADD_COLUMN(\"100.00\", \"DECIMAL32\", 5, 2) as col5,\n        ADD_COLUMN(\"100.00\", \"DECIMAL64\", 5, 2) as col6,\n        ADD_COLUMN(\"100.00\", \"DECIMAL128\", 5, 2) as col7,\n        ADD_COLUMN(\"100.00\", \"DECIMAL256\", 5, 2) as col8\n        ",
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

## Temporal Data types

We support the following types:

- Date: `ADD_COLUMN(value, "DATE", format)` and the value is NULL or string. For example: ADD_COLUMN('2026/01/01', "DATE", "%Y/%m/%d") as col9
- Datetime: `ADD_COLUMN(value, "DATETIME", format, unit)` and the value is NULL or string, format is string, unit is string of the form "ms", "us", "ns". For example: ADD_COLUMN('2025-10-20 12:00:00', "DATETIME", "%Y-%m-%d %H:%M:%S", "ms") as col10
- Time: `ADD_COLUMN(value, "TIME", format, unit)`. For example: ADD_COLUMN('23:59:59', "TIME", "%H:%M:%S", "ms") as col11
- Duration: ADD_COLUMN(5520, "DURATION", "ms") as col12, where the value is INT.
