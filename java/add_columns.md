# Add New Columns


This service adds new columns with constant value. It supports INT, LONG, DOUBLE, and STRING types.


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
          "dtype": "INT"
        },
        "x4": {
          "value": 10.0,
          "dtype": "DOUBLE"
        },
        "x5": {
          "value": "A",
          "dtype": "STRING"
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

| x1 | x2 | x3 | x4 | x5 |
| --- | --- | --- | --- | --- |
| -100 | -120 | None | 10.00 | A |
| -50 | -60 | None | 10.00 | A |
| 0 | 2 | None | 10.00 | A |
| 70 | 7 | None | 10.00 | A |
| 10 | 80 | None | 10.00 | A |
| 15 | 17 | None | 10.00 | A |
| 20 | 22 | None | 10.00 | A |
| 25 | 27 | None | 10.00 | A |
| 5 | 12 | None | 10.00 | A |
| 120 | 120 | None | 10.00 | A |


