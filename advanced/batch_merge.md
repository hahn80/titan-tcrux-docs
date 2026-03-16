# Advanced Merge.


Merge multiple tables.

```json
[
  {
    "task": "batch_processing",
    "action": "batch_merge",
    "kwargs": {
      "input_files": [
        "/tmp/tmpci4v1pap/input_0.arrow",
        "/tmp/tmpci4v1pap/input_1.arrow",
        "/tmp/tmpci4v1pap/input_2.arrow",
        "/tmp/tmpci4v1pap/input_3.arrow",
        "/tmp/tmpci4v1pap/input_4.arrow"
      ],
      "output_arrow": "/tmp/tmpci4v1pap/output.arrow",
      "operations": {
        "batch_size": 400,
        "num_workers": 4,
        "maintain_order": false,
        "m_batches": 8,
        "compression": "zstd"
      }
    }
  }
]
```

Params:

- *input_files*: List[String]: multiple input arrow files
- *output_arrow*: String: output arrow file
- *batch_size*: Int: batch_size for the output
- *num_workers*: Int: number of workers.
- *maintain_order*: Bool: Keep the order of batches from input files or not.
- *m_batches*: Int: number of batches to read at once in memory.
- *compression*: String: zstd, lz4


Output result:

| id | value | name |
| --- | --- | --- |
| 2881 | 5762 | item_2881 |
| 1783 | 3566 | item_1783 |
| 2984 | 5968 | item_2984 |
| 1487 | 2974 | item_1487 |
| 4614 | 9228 | item_4614 |
| 3172 | 6344 | item_3172 |
| 875 | 1750 | item_875 |
| 1635 | 3270 | item_1635 |
| 832 | 1664 | item_832 |
| 2059 | 4118 | item_2059 |
| 831 | 1662 | item_831 |
| 3656 | 7312 | item_3656 |
| 9 | 18 | item_9 |
| 4292 | 8584 | item_4292 |
| 4553 | 9106 | item_4553 |
| 3449 | 6898 | item_3449 |
| 2702 | 5404 | item_2702 |
| 1290 | 2580 | item_1290 |
| 2712 | 5424 | item_2712 |
| 1259 | 2518 | item_1259 |







