# Chọn các members theo phần trăm / số lựa chọn.


1. Chọn theo phần trăm:

Tham số để gọi service:

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
    "action": "best_percent_members",
    "kwargs": {
      "column": "x1",
      "percent": 20.0,
      "direction": "bottom"
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

Giải thích tham số:
- `column`: String, input column name
- `percent`: Float: From 1 to 99 pertentage
- `direction`: either 'top' or 'bottom'. Top là chọn 10% cao nhất, bottom là chọn 10% thấp nhất.

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


Output result:

| x1 | x2 |
| --- | --- |
| -100 | -120 |
| -50 | -60 |
| 0 | 2 |


2. Chọn theo số thành viên

Tham số để gọi service:

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
    "action": "best_k_members",
    "kwargs": {
      "columns": [
        "x1"
      ],
      "k": 2,
      "direction": "top"
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

Giải thích tham số:
- `columns`: Array of String -> key for multiple input column names
- `k`: Int: VD: k=2, chọn 2 phần tử tốt nhất.
- `direction`: either 'top' or 'bottom'. Top là chọn 2 cao nhất, bottom là chọn 2 thấp nhất.


Output result:

| x1 | x2 |
| --- | --- |
| 120 | 120 |
| 70 | 7 |


3. Chọn theo joint-probability

VD: Chọn khách hàng nhỏ tuổi nhất nhưng `duration` cao nhất. Trong đó chúng ta đặt trọng số 2.0 cho `duration` và 1.0 cho `age`.

Tham số để gọi service:

```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/banking.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "best_probability",
    "kwargs": {
      "operations": {
        "duration": {
          "direction": "top",
          "weight": 2.0
        },
        "age": {
          "direction": "bottom",
          "weight": 1.0
        }
      },
      "percent": 10
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

OR

```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/banking.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "best_probability",
    "kwargs": {
      "operations": {
        "duration": {
          "direction": "top",
          "weight": 2.0
        },
        "age": {
          "direction": "bottom",
          "weight": 1.0
        }
      },
      "k": 2
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

Giải thích tham số:
- `operations`: Hashmap contains keys as column names and values as hashmap of direction and weight. Default weight is 1.0
- `percent`: This is a float number 0 < percent < 100.
- `k`: Int: VD: k=2, chọn 2 phần tử tốt nhất.
- Lưu ý, chỉ một trong hai tham số `percent` hoặc `k` được đưa vào; không được đưa cả 2 cùng một lúc.


Input data:

| key | age | job | marital | education | default | balance | housing | loan | contact | day | month | duration | campaign | pdays | previous | poutcome | y | year | date |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 26194 | 34 | admin. | single | secondary | no | 296 | no | no | cellular | 20 | nov | 68 | 1 | 136 | 4 | failure | no | 2010 | 2010-11-20 |
| 25952 | 32 | technician | single | tertiary | yes | 0 | no | no | cellular | 19 | nov | 122 | 2 | -1 | 0 | unknown | no | 2010 | 2010-11-19 |
| 40849 | 64 | management | married | secondary | no | 1646 | no | no | cellular | 12 | aug | 68 | 3 | -1 | 0 | unknown | no | 2011 | 2011-08-12 |
| 9221 | 43 | management | divorced | secondary | no | 130 | yes | no | unknown | 5 | jun | 63 | 3 | -1 | 0 | unknown | no | 2010 | 2010-06-05 |
| 25644 | 39 | entrepreneur | married | primary | no | 3373 | yes | no | cellular | 19 | nov | 419 | 5 | -1 | 0 | unknown | no | 2010 | 2010-11-19 |
| 40158 | 38 | management | married | tertiary | no | 1187 | yes | yes | cellular | 5 | jun | 76 | 2 | 123 | 1 | failure | no | 2011 | 2011-06-05 |
| 19955 | 30 | technician | single | secondary | no | 0 | no | no | cellular | 8 | aug | 216 | 2 | -1 | 0 | unknown | no | 2010 | 2010-08-08 |
| 4834 | 39 | technician | married | secondary | no | 3528 | yes | no | unknown | 21 | may | 226 | 1 | -1 | 0 | unknown | no | 2010 | 2010-05-21 |
| 13013 | 36 | blue-collar | married | secondary | no | -308 | yes | no | cellular | 7 | jul | 279 | 11 | -1 | 0 | unknown | no | 2010 | 2010-07-07 |
| 2174 | 35 | management | single | tertiary | no | 244 | yes | no | unknown | 12 | may | 225 | 1 | -1 | 0 | unknown | no | 2010 | 2010-05-12 |


Output result:

| key | age | job | marital | education | default | balance | housing | loan | contact | day | month | duration | campaign | pdays | previous | poutcome | y | year | date |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 27141 | 35 | services | single | secondary | no | 1354 | no | yes | cellular | 21 | nov | 278 | 6 | -1 | 0 | unknown | no | 2010 | 2010-11-21 |
| 39179 | 38 | management | single | tertiary | no | 2368 | yes | no | cellular | 18 | may | 241 | 3 | -1 | 0 | unknown | no | 2011 | 2011-05-18 |
| 26064 | 44 | technician | married | secondary | yes | 16486 | yes | yes | cellular | 19 | nov | 275 | 3 | -1 | 0 | unknown | no | 2010 | 2010-11-19 |
| 31727 | 35 | management | single | tertiary | no | 714 | yes | no | cellular | 7 | apr | 263 | 2 | 224 | 22 | failure | no | 2011 | 2011-04-07 |
| 14897 | 45 | management | married | tertiary | no | 2 | yes | no | cellular | 16 | jul | 192 | 2 | -1 | 0 | unknown | no | 2010 | 2010-07-16 |
| 13210 | 36 | unemployed | divorced | secondary | no | 5 | yes | no | cellular | 8 | jul | 305 | 1 | -1 | 0 | unknown | no | 2010 | 2010-07-08 |
| 7325 | 39 | management | married | tertiary | no | 0 | yes | no | unknown | 29 | may | 184 | 2 | -1 | 0 | unknown | no | 2010 | 2010-05-29 |
| 8934 | 39 | admin. | married | secondary | no | 1577 | yes | no | unknown | 4 | jun | 282 | 15 | -1 | 0 | unknown | no | 2010 | 2010-06-04 |
| 16800 | 44 | blue-collar | married | primary | no | 658 | yes | no | telephone | 24 | jul | 236 | 2 | -1 | 0 | unknown | no | 2010 | 2010-07-24 |
| 31700 | 46 | technician | married | secondary | no | 138 | yes | no | cellular | 7 | apr | 226 | 2 | -1 | 0 | unknown | no | 2011 | 2011-04-07 |
