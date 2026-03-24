# Advanced Batch Join.


Jobs: Join two big tables, support inner, left, semi, anti, full, cross

```json
[
  {
    "task": "batch_processing",
    "action": "batch_join",
    "kwargs": {
      "left_arrow": "/tmp/tmp85r74nc4.arrow",
      "right_arrow": "/tmp/tmpnyatxoxk.arrow",
      "output_arrow": "/tmp/tmp5mqcvmom.arrow",
      "operations": {
        "left_keys": [
          "order_id"
        ],
        "right_keys": [
          "order_id"
        ],
        "left_columns": [
          "amount",
          "ts"
        ],
        "right_columns": [
          "product",
          "price"
        ],
        "type": "inner",
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

- *left_arrow* or *right_arrow*: String: input arrow files
- *output_arrow*: String: output arrow file
- *left_keys*: List[String].
- *right_keys*: List[String].
- *batch_size*: Int: batch_size for the output
- *memory_limit_mb*: Int: max size of memory in MB to process (avoid) OOM.
- *left_columns*: List[String]: left columns to select for the output, it will add the key columns automatically.
- *right_columns*: List[String]: right columns to select for the output, it will add the key columns automatically.
- *compression*: String: zstd, lz4
- *type*: Join type, support 6 types above (inner, left, semi, anti, full, cross)


Output result:

| order_id | amount | ts | order_id_right | product | price |
| --- | --- | --- | --- | --- | --- |
| 1 | 10.00 | 1000 | 1 | alpha | 1.10 |
| 2 | 20.00 | 1001 | 2 | beta | 2.20 |
| 3 | 30.00 | 1002 | 3 | gamma | 3.30 |
| 4 | 40.00 | 1003 | 4 | delta | 4.40 |
| 5 | 50.00 | 1004 | 5 | epsilon | 5.50 |
| 6 | 60.00 | 1005 | 6 | zeta | 6.60 |
| 7 | 70.00 | 1006 | 7 | eta | 7.70 |
| 8 | 80.00 | 1007 | 8 | theta | 8.80 |
| 8 | 80.00 | 1007 | 8 | theta2 | 8.90 |
| 9 | 90.00 | 1008 | 9 | iota | 9.90 |
| 9 | 90.00 | 1008 | 9 | iota2 | 10.00 |
| 10 | 100.00 | 1009 | 10 | kappa | 11.10 |
| 11 | 11.00 | 1010 | 11 | lambda | 12.20 |
| 12 | 22.00 | 1011 | 12 | mu | 13.30 |
| 13 | 33.00 | 1012 | 13 | nu | 14.40 |









