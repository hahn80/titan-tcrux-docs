# Advanced SelectKBy.


Batch selectkby


Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_selectkby",
    "kwargs": {
      "input_arrow": "/tmp/tmpljobkqap/input.arrow",
      "output_arrow": "/tmp/tmpljobkqap/output.arrow",
      "operations": {
        "sort_keys": [
          [
            "id",
            true
          ]
        ],
        "k": 100,
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
| 92 | 9908 | 184 |
| 21 | 9979 | 42 |
| 37 | 9963 | 74 |
| 75 | 9925 | 150 |
| 61 | 9939 | 122 |
| 78 | 9922 | 156 |
| 73 | 9927 | 146 |
| 65 | 9935 | 130 |
| 64 | 9936 | 128 |
| 58 | 9942 | 116 |
| 87 | 9913 | 174 |
| 57 | 9943 | 114 |
| 2 | 9998 | 4 |
| 67 | 9933 | 134 |
| 3 | 9997 | 6 |
| 18 | 9982 | 36 |
| 98 | 9902 | 196 |
| 47 | 9953 | 94 |
| 27 | 9973 | 54 |
| 74 | 9926 | 148 |







