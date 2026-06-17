# TurboSearch: Quantized Search for Similarity

We integrate 4-bit quantized method from Google to approximatedly search the similarity of the vectors.

## Create the index.


Jobs to call `tcrux_multi_tasking`:

```json
[
  {
    "task": "querying",
    "action": "batch_index_create",
    "kwargs": {
      "input_arrow": "/tmp/tests/resources/fake_vectors.arrow",
      "index_path": "/tmp/tmprthphl4q/index",
      "operations": {
        "bit_width": 4,
        "batch_size": 32000,
        "max_rows_per_file": 640000
      }
    }
  }
]

```

Params:

- *input_arrow*: String: the input arrow file
- *index_path*: String: path to index folder (create this path before calling)
- *bit_width*: Int: bit size for quantization (4 or 2).
- *length*: Int: How many rows to take.
- *batch_size*: Int: batch_size for the index writer.
- *max_rows_per_file*: Int: The max number or rows for each index file. The index can have multiple files.



## Similarity Search

Once we created the index, we can search it.

```json
[
  {
    "task": "querying",
    "action": "batch_index_search",
    "kwargs": {
      "index_path": "/tmp/tmprthphl4q/index",
      "query": arrow.RecordBatch or Table,
	  "operations": {}
    }
  }
]
```

Now, we will talk about how to pass arrow RecordBatch or Table to *query* from Java:

```java
private static final int N_QUERIES = 10;
private static final int DIM = 1024;
byte[] ipc;
try (ByteArrayOutputStream out = new ByteArrayOutputStream();
	 ArrowStreamWriter writer =
			 new ArrowStreamWriter(root, null, Channels.newChannel(out))) {
	writer.start();
	writer.writeBatch();
	writer.end();
	ipc = out.toByteArray();
}

List<Object> jobs = ListBuilder.create(l ->
	l.add(JobBuilder.job()
		.task("querying")
		.action("batch_index_search")
		.kwargs(kw -> {
			kw.put("index_path", indexPath);
			kw.put("__arrow__", "query");
			kw.put("operations", MapBuilder.create(ops -> {}).build());
		})
		.build())
).build();

Map<String, Object> result = peer.sendArrow("tcrux.execute",
		MapBuilder.create(m -> m.put("jobs", jobs)).build(), ipc, 30_000);
```

Notice:

- *ipc* data will be sent to Python from Java
- *index_path*: String: Path to the turbo index
- `__arrow__` = `query`. This is the main idea, it tells Python to receives ipc data and set `query` = ipc data once it obtained.
- `operations`: Just empty Hashmap for now.


