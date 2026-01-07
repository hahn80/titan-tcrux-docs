# Transpose Table

The service to transpose a table.

Consider the following table:

| abc_ct | # n2023 | # n2024 | # n2025 | # a2026 |
| --- | --- | --- | --- | --- |
| CT4 | 4 | 13 | 103 | A |
| CT1 | 1 | 10 | 100 | B |
| CT5 | 5 | 14 | 104 | C |
| CT3 | 3 | 12 | 102 | D |
| CT2 | 2 | 11 | 101 | E |


1. Transpose the above table without any options:


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/tmp/tmpr9qcxfuv.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "transpose",
    "kwargs": {
      "include_header": true,
      "header_name": "new_headers",
      "header_columns": null
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "/tmp/tmpigfzz711.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

The output result will be:

| new_headers | column_0 | column_1 | column_2 | column_3 | column_4 |
| --- | --- | --- | --- | --- | --- |
| # n2023 | 4 | 1 | 5 | 3 | 2 |
| # n2024 | 13 | 10 | 14 | 12 | 11 |
| # n2025 | 103 | 100 | 104 | 102 | 101 |


2. We use the values from a column to make a new headers

In this case, we use use the values from column named `abc_ct` to name the headers.

```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/tmp/tmpb_xrgaom.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "transpose",
    "kwargs": {
      "include_header": true,
      "header_name": "new_headers",
      "header_columns": "abc_ct"
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "/tmp/tmppmncfja6.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Output result:

| abc_ct | # n2023 | # n2024 | # n2025 | # a2026 |
| --- | --- | --- | --- | --- |
| CT4 | 4 | 13 | 103 | A |
| CT1 | 1 | 10 | 100 | B |
| CT5 | 5 | 14 | 104 | C |
| CT3 | 3 | 12 | 102 | D |
| CT2 | 2 | 11 | 101 | E |


3. We want the header names are specific

```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/tmp/tmp8n6gnwyp.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "transpose",
    "kwargs": {
      "include_header": true,
      "header_name": "new_headers",
      "header_columns": [
        "name_1",
        "name_2",
        "name_3",
        "name_4",
        "name_5"
      ]
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "/tmp/tmpw_yd2piz.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Output result:

| new_headers | name_1 | name_2 | name_3 | name_4 | name_5 |
| --- | --- | --- | --- | --- | --- |
| # n2023 | 4 | 1 | 5 | 3 | 2 |
| # n2024 | 13 | 10 | 14 | 12 | 11 |
| # n2025 | 103 | 100 | 104 | 102 | 101 |
