# Slice Big Files.


```json
{
  "task": "batch_processing",
  "action": "batch_slice",
  "kwargs": {
    "input_arrow": "/tests/resources/left.arrow",
    "output_arrow": "/tmp/tmp230l0g6f/output.arrow",
    "operations": {
      "columns": null,
      "offset": 10,
      "length": 5000,
      "batch_size": 3200,
      "compression": "lz4"
    }
  }
}
```

Params:

- *input_files*: List[String]: multiple input arrow files
- *output_arrow*: String: output arrow file
- *columns*: List of columns to select, columns=null -> take all columns.
- *offset*: Int: Start row to slice.
- *length*: Int: How many rows to take.
- *batch_size*: Int: batch_size for the output
- *compression*: String: zstd, lz4








