# PCA Tranformation.


There are two steps to transform $$n$$ columns of a data set into $$k$$ principal components using PCA.

Consider the following dataset (truncated)

| key | feat_a | feat_b | feat_c | feat_d |
| --- | --- | --- | --- | --- |
| 3 | 0.13 | -0.09 | 1.74 | 0.73 |
| 9 | 0.88 | 0.67 | 4.08 | -0.98 |
| 50 | -0.38 | 0.48 | 6.70 | -0.67 |
| 59 | 1.60 | 1.14 | 2.35 | -0.60 |
| 79 | -0.50 | -1.11 | 2.67 | 0.47 |
| 109 | -0.15 | -0.54 | 4.29 | 0.27 |
| 112 | 0.09 | 0.21 | 0.93 | -0.30 |
| 122 | 2.13 | 0.93 | -1.56 | -0.86 |
| 188 | -0.21 | -1.37 | 0.84 | 0.43 |
| 192 | -1.60 | -1.76 | 4.02 | 1.20 |
| 198 | 0.76 | 0.75 | 5.05 | 0.11 |
| 199 | 0.53 | 0.00 | 3.08 | 0.36 |
| 207 | -0.28 | -0.11 | 0.51 | -0.25 |
| 215 | -1.04 | -0.55 | 3.62 | 0.56 |
| 248 | -1.83 | -1.67 | 1.59 | 1.43 |
| 253 | 2.42 | 2.20 | 3.06 | -1.49 |
| 264 | 0.26 | 1.15 | 1.61 | -0.62 |
| 273 | 0.20 | -0.44 | 3.13 | -0.60 |
| 295 | -1.22 | -0.83 | 5.10 | 1.59 |
| 298 | -0.52 | 0.07 | 3.96 | 1.00 |




## Train PCA model

Jobs:

```json
[
  {
    "task": "reduction",
    "action": "pca_train",
    "kwargs": {
      "input_arrow": "/tmp/tmpi3nbf9r1/input.arrow",
      "model_path": "/tmp/tmpi3nbf9r1/pca_model.arrow",
      "operations": {
        "columns": [
          "feat_a",
          "feat_b",
          "feat_c",
          "feat_d"
        ],
        "batch_size": 128,
        "standardize": false
      }
    }
  }
]
```

- *input_arrow*: String: input arrow file.
- *output_arrow*: String: output arrow file.
- *batch_size*: Int: batch_size for the output.
- *standardize*: Boolean: True: standardize each column before applying PCA; False: Keep the raw data from input.


## Transform data using PCA

Once we trained the model and save it to `/tmp/tmpi3nbf9r1/pca_model.arrow`, then we can transform the data as follow:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_query",
    "kwargs": {
      "input_arrow": "/tmp/tmp8bv1i9tx/input.arrow",
      "output_arrow": "/tmp/tmp8bv1i9tx/output.arrow",
      "operations": {
        "query": "\n        SELECT  key, PC1, PC2\n        FROM dummy\n        TRANSFORM PCA(feat_a, feat_b, feat_c, feat_d, \"/tmp/tmp8bv1i9tx/pca_model.arrow\", 2, PC1, PC2)\n        ",
        "batch_size": 250,
        "m_batches": 10,
        "compression": "zstd"
      }
    }
  }
]
```

- *query*: In TRANSFORM PCA function, we use the following syntax: `PCA(col1, ..., colN, path_to_trained_model, k, PC1, ..., PCk)`. It means that we have $$N$$ input columns `col1` to `colN`, $$k$$ is the number of components and `PC1` to `PCk` are the name for output columns.

Output result (truncated):

| key | PC1 | PC2 |
| --- | --- | --- |
| 0 | -1.17 | -0.14 |
| 2 | -3.10 | 2.70 |
| 3 | 1.05 | 0.35 |
| 4 | 1.08 | 0.35 |
| 5 | -1.08 | -1.33 |
| 6 | -1.59 | -0.86 |
| 7 | -0.95 | -0.73 |
| 8 | 3.21 | 0.29 |
| 10 | 1.85 | 0.59 |
| 11 | -0.40 | -1.65 |
| 13 | -0.90 | -1.18 |
| 14 | 1.52 | -0.39 |
| 15 | 2.74 | -2.23 |
| 16 | -1.74 | 1.02 |
| 17 | -0.47 | -1.32 |
| 19 | -3.09 | 0.03 |
| 21 | -0.30 | 0.56 |
| 22 | -0.65 | -0.37 |
| 25 | 0.94 | -0.21 |
| 27 | -2.98 | 0.52 |


