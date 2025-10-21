# Add New Columns


This service adds new columns with constant value. It supported types: DECIMAL, FLOAT32, FLOAT64, INT8, INT16, INT32, INT64, INT128, UINT8, UINT16, UINT32, UINT64, DATE, DATETIME, DURATION, TIME, STRING, BOOLEAN.


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/bbby.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "add_columns",
    "kwargs": {
      "operations": {
        "x3": {
          "value": null,
          "dtype": "INT32"
        },
        "x4": {
          "value": 10.0,
          "dtype": "FLOAT64"
        },
        "x5": {
          "value": "A",
          "dtype": "STRING"
        },
        "x6": {
          "value": true,
          "dtype": "BOOLEAN"
        },
        "x7": {
          "value": "2026/01/01",
          "dtype": "DATE",
          "format": "%Y/%m/%d"
        },
        "x8": {
          "value": "2025-10-20 12:00:00",
          "dtype": "DATETIME",
          "format": "%Y-%m-%d %H:%M:%S"
        },
        "x9": {
          "value": "23:59:59",
          "dtype": "TIME",
          "format": "%H:%M:%S"
        },
        "x10": {
          "value": 5520,
          "dtype": "DURATION",
          "time_unit": "ms"
        }
      }
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "/data/TitanProjects/tcrux/src/tests/resources/output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Explain the params:

- operations: Hashmap with key is a new column name and value is anothe Hashmap with `value` for that column, and `dtype` for the column. Supported dtypes are: INT, LONG, DOUBLE, STRING.

Input data:

| x1 | x2 |
| --- | --- |
| -100 | -120 |
| -50 | -60 |
| 0 | 2 |
| 70 | 7 |
| 10 | 80 |
| 15 | 17 |
| 20 | 22 |
| 25 | 27 |
| 5 | 12 |
| 120 | 120 |


Output data:

| x1 | x2 | x3 | x4 | x5 | x6 | x7 | x8 | x9 | x10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| -100 | -120 | None | 10.00 | A | True | 2026-01-01 | 2025-10-20 12:00:00 | 23:59:59 | 0:00:05.520000 |
| -50 | -60 | None | 10.00 | A | True | 2026-01-01 | 2025-10-20 12:00:00 | 23:59:59 | 0:00:05.520000 |
| 0 | 2 | None | 10.00 | A | True | 2026-01-01 | 2025-10-20 12:00:00 | 23:59:59 | 0:00:05.520000 |
| 70 | 7 | None | 10.00 | A | True | 2026-01-01 | 2025-10-20 12:00:00 | 23:59:59 | 0:00:05.520000 |
| 10 | 80 | None | 10.00 | A | True | 2026-01-01 | 2025-10-20 12:00:00 | 23:59:59 | 0:00:05.520000 |
| 15 | 17 | None | 10.00 | A | True | 2026-01-01 | 2025-10-20 12:00:00 | 23:59:59 | 0:00:05.520000 |
| 20 | 22 | None | 10.00 | A | True | 2026-01-01 | 2025-10-20 12:00:00 | 23:59:59 | 0:00:05.520000 |
| 25 | 27 | None | 10.00 | A | True | 2026-01-01 | 2025-10-20 12:00:00 | 23:59:59 | 0:00:05.520000 |
| 5 | 12 | None | 10.00 | A | True | 2026-01-01 | 2025-10-20 12:00:00 | 23:59:59 | 0:00:05.520000 |
| 120 | 120 | None | 10.00 | A | True | 2026-01-01 | 2025-10-20 12:00:00 | 23:59:59 | 0:00:05.520000 |



