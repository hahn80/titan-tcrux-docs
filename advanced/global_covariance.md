# Compute Global Covariance/Correlation Matrix


## Covariance Matrix Without Nulls

```json
[
  {
    "task": "batch_processing",
    "action": "global_covariance",
    "kwargs": {
      "input_arrow": "/tmp/tmpq_qx6d_4/cov_input.arrow",
      "output_arrow": "/tmp/tmpq_qx6d_4/cov.arrow",
      "operations": {
        "columns": [
          "a",
          "b",
          "c"
        ],
        "mode": "covariance",
        "m_batches": 8,
        "compression": "zstd"
      }
    }
  }
]
```

Params:

- *input_arrow*: String: input arrow file
- *output_arrow*: String: output arrow file
- *columns*: List of String
- *mode*: String: *covariance* or *correlation*
- *m_batches*: Int: number of batches to process at once.
- *compression*: String: zstd, lz4


Output result:

| column_name | a | b | c |
| --- | --- | --- | --- |
| a | 0.92 | 1.84 | 0.05 |
| b | 1.84 | 3.93 | 0.08 |
| c | 0.05 | 0.08 | 1.04 |




## Covariance Matrix Drop Nulls

```json
[
  {
    "task": "batch_processing",
    "action": "global_covariance",
    "kwargs": {
      "input_arrow": "/tmp/tmpp08fyvnf/cov_input.arrow",
      "output_arrow": "/tmp/tmpp08fyvnf/cov.arrow",
      "operations": {
        "columns": [
          "a",
          "b",
          "c"
        ],
        "mode": "covariance",
        "m_batches": 8,
        "null_strategy": "drop",
        "compression": "zstd"
      }
    }
  }
]
```

Params:

- All params are the same as above.
- *null_strategy*: String (We drop in this case)


Output result:

| column_name | a | b | c |
| --- | --- | --- | --- |
| a | 0.83 | 1.66 | 0.05 |
| b | 1.66 | 3.55 | 0.07 |
| c | 0.05 | 0.07 | 1.04 |


## Covariance Matrix Fill Nulls

```json
[
  {
    "task": "batch_processing",
    "action": "global_covariance",
    "kwargs": {
      "input_arrow": "/tmp/tmpuhhukf9d/cov_input.arrow",
      "output_arrow": "/tmp/tmpuhhukf9d/cov.arrow",
      "operations": {
        "columns": [
          "a",
          "b",
          "c"
        ],
        "mode": "covariance",
        "m_batches": 8,
        "null_strategy": "fill",
        "fill_value": 0.0,
        "compression": "zstd"
      }
    }
  }
]
```

Params:

- All params are the same as above.
- *null_strategy*: String (We *fill* in this case)
- *fill_value*: Float (The value to replace nulls)


Output result:

| column_name | a | b | c |
| --- | --- | --- | --- |
| a | 0.83 | 1.66 | 0.05 |
| b | 1.66 | 3.55 | 0.07 |
| c | 0.05 | 0.07 | 1.04 |


## Correlation Matrix

Just redo these 3 functions with *mode* = *correlation* to get the correlation matrix. For example:

```json
[
  {
    "task": "batch_processing",
    "action": "global_covariance",
    "kwargs": {
      "input_arrow": "/tmp/tmp9_kpys8p/cov_input.arrow",
      "output_arrow": "/tmp/tmp9_kpys8p/cov.arrow",
      "operations": {
        "columns": [
          "a",
          "b",
          "c"
        ],
        "mode": "correlation",
        "m_batches": 8,
        "null_strategy": "fill",
        "fill_value": 0.0,
        "compression": "zstd"
      }
    }
  }
]
```





