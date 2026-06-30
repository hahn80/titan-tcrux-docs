# TurboSearch: Quantized Search for Similarity

We integrate 4-bit quantized method from Google to approximatedly search the similarity of the vectors.

## Create the index (RAW).


Jobs to call `tcrux_multi_tasking`:

```json
[
  {
    "task": "querying",
    "action": "batch_index_create",
    "kwargs": {
      "input_arrow": "hdfs://172.30.0.2:9000/tmp/tmpsgsz41lx/input.arrow?user=root",
      "index_path": "hdfs://172.30.0.2:9000/tmp/tmpsgsz41lx/raw_index?user=root",
      "operations": {
        "bit_width": 4,
        "batch_size": 32000,
        "key_column": "doc_id",
        "vector_column": "embedding"
      }
    }
  }
]
```

The input arrow must have the schema: key and vector.

Params:

- *input_arrow*: String: the input arrow file or hdfs
- *index_path*: String: path to index folder or hdfs
- *bit_width*: Int: bit size for quantization (4 or 2).
- *length*: Int: How many rows to take.
- *batch_size*: Int: batch_size for the index writer.
- *key_column*: String: name of key column.
- *vector_column*: String: name of vector column.


## Build the local search index

Once we created the (raw) index, we have to build the index before searching.

```json
[
  {
    "task": "querying",
    "action": "batch_index_build",
    "kwargs": {
      "index_path": "hdfs://172.30.0.2:9000/tmp/tmpsgsz41lx/raw_index?user=root",
      "local_path": "/tmp/tmpsgsz41lx/index",
      "operations": {
        "key_column": "doc_id",
        "vector_column": "embedding"
      }
    }
  }
]
```

- *index_path*: String, path to raw index (could be on hdfs)
- *local_path*: String, path to local built index (must be local, not hdfs)


## Similarity Search

Once we created the index, we can search it.

```json
[
  {
    "task": "querying",
    "action": "batch_index_search",
    "kwargs": {
      "index_path": "/tmp/tmpsgsz41lx/index",
      "__arrow__": "query",
      "operations": {
        "k": 10,
        "min_score": 0.1
      }
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
			kw.put("operations", MapBuilder.create(ops -> {"k": 10, "min_score": 0.1}).build());
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
- `operations`: contains *k* (int) and *min_score* (float). The *k* is required, but min_score is optional, can be null. 


