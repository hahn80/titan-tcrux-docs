# Advanced Orderby.


Batch orderby


Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_orderby",
    "kwargs": {
      "input_arrow": "/tmp/tmpeq9ivtc9/input.arrow",
      "output_arrow": "/tmp/tmpeq9ivtc9/output.arrow",
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
        "batch_size": 32000,
        "max_way": 8
      }
    }
  }
]
```


Output result:

| id | value | score |
| --- | --- | --- |
| 8655 | 1345 | 17310 |
| 8696 | 1304 | 17392 |
| 6174 | 3826 | 12348 |
| 3402 | 6598 | 6804 |
| 422 | 9578 | 844 |
| 3274 | 6726 | 6548 |
| 7592 | 2408 | 15184 |
| 271 | 9729 | 542 |
| 3745 | 6255 | 7490 |
| 9517 | 483 | 19034 |
| 3132 | 6868 | 6264 |
| 9908 | 92 | 19816 |
| 2551 | 7449 | 5102 |
| 5593 | 4407 | 11186 |
| 8709 | 1291 | 17418 |
| 5644 | 4356 | 11288 |
| 88 | 9912 | 176 |
| 1844 | 8156 | 3688 |
| 7841 | 2159 | 15682 |
| 4320 | 5680 | 8640 |






