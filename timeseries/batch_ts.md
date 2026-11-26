# Advanced Timeseries Forecasting


This document contains the improved techniques to forecast timeseries data. Today we just support "arima" algorithm, we will add more later.

For the most of the following functions, the `input_arrow` may contain more columns than the model needs. Only the
columns named in `operations` (`target`, `xreg_columns`, `time_column`, `id_column`) are needed. Examples below use the local path for the input, but we can also use HDFS path for input arrow file.

## Train all without external regressor (xreg)

Fits the model on the whole file and saves it. It is useful for a dataset that fit in memory. (Around 3M rows)

Job params:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_ts_train_all",
    "kwargs": {
      "input_arrow": "/tmp/tmpwrsfgafv/ts_full.arrow",
      "output_model": "/tmp/tmpwrsfgafv/ts_all.afm",
      "operations": {
        "algorithm": "arima",
        "target": "y",
        "time_column": "time",
        "p": 1,
        "d": 0,
        "q": 1
      }
    }
  }
]
```

**Param explanation:**

- `input_arrow` can hold extra columns. Only the columns named in `operations` are read.
- `output_model` to save the trained model
- `operations` is a Hashmap with following information

| Key | Type | Default | Range | Meaning |
|-----|------|---------|-------|---------|
| `algorithm` | str | required | `"arima"` | Model family. |
| `target` | str | required | existing column | Column to model. |
| `time_column` | str | `""` | existing column or empty | Time ordering column. Checked to be strictly increasing. Stored so later calls can check continuity. |
| `p` | int | 0 | >= 0 | Non-seasonal AR order. Adds `ar1..arp`. |
| `d` | int | 0 | >= 0 (usually 0 to 2) | Non-seasonal differencing. |
| `q` | int | 0 | >= 0 | Non-seasonal MA order. Adds `ma1..maq`. |
| `P` | int | 0 | >= 0 | Seasonal AR order. Adds `sar1..`. |
| `D` | int | 0 | >= 0 (usually 0 or 1) | Seasonal differencing. |
| `Q` | int | 0 | >= 0 | Seasonal MA order. Adds `sma1..`. |
| `period` | int | 1 | >= 1 | Season length (12 for monthly, 7 for daily with weekly cycle). Needed when P, D or Q is above 0. |
| `include_mean` | bool | True | True/False | Fit an `intercept`. Only applied when `d = D = 0`. |
| `transform_pars` | bool | True | True/False | Reparameterize AR terms during optimization so the fit stays stationary. |
| `tol` | float | 1e-8 | > 0 | Optimizer convergence tolerance. |
| `max_iter` | int | 100 | >= 1 | Optimizer iteration limit. |


**Return result:**

It returns a Hashmap in jvm:

```json
{'algorithm': 'arima', 'kind': 'single', 'target': 'y', 'xreg_columns': [], 'spec': {'p': 1, 'd': 0, 'q': 1, 'P': 0, 'D': 0, 'Q': 0, 'period': 1, 'include_mean': True, 'transform_pars': True, 'method': 'CSS-ML', 'ssinit': 'Gardner1980', 'kappa': 1000000.0, 'tol': 1e-08, 'max_iter': 100}, 'warnings': [], 'nobs': 2000, 'sigma2': 0.9807511218423983, 'loglik': -2818.9004987798553, 'aic': 5645.800997559711, 'coef': [{'name': 'ar1', 'coef': 0.6048795257561164, 'std_error': 0.02308109031665135}, {'name': 'ma1', 'coef': 0.3214667651790137, 'std_error': 0.02775716730887021}, {'name': 'intercept', 'coef': 5.087728854624905, 'std_error': 0.07399574062234146}], 'xreg_stats': []}
```

## Train all with external regressor (xreg)

Job:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_ts_train_all",
    "kwargs": {
      "input_arrow": "/tmp/tmpzp94imri/xreg_full.arrow",
      "output_model": "/tmp/tmpzp94imri/ts_all.afm",
      "operations": {
        "algorithm": "arima",
        "target": "y",
        "time_column": "time",
        "xreg_columns": [
          "x1",
          "x2",
          "x3"
        ],
        "p": 1,
        "d": 0,
        "q": 1
      }
    }
  }
]
```

The params are similar to the above case, except we need `xreg_columns` where it includes all extra columns to use for training and prediction.


**Return Values:**

```json
{'algorithm': 'arima', 'kind': 'single', 'target': 'y', 'xreg_columns': ['x1', 'x2', 'x3'], 'spec': {'p': 1, 'd': 0, 'q': 1, 'P': 0, 'D': 0, 'Q': 0, 'period': 1, 'include_mean': True, 'transform_pars': True, 'method': 'CSS-ML', 'ssinit': 'Gardner1980', 'kappa': 1000000.0, 'tol': 1e-08, 'max_iter': 100}, 'warnings': [], 'nobs': 3000, 'sigma2': 0.9735969555058218, 'loglik': -4217.138288091686, 'aic': 8448.276576183373, 'coef': [{'name': 'ar1', 'coef': 0.5880171966542872, 'std_error': 0.018849410838312316}, {'name': 'ma1', 'coef': 0.3441718717518746, 'std_error': 0.021604117025194314}, {'name': 'intercept', 'coef': 4.964610402416968, 'std_error': 0.058737756758727874}, {'name': 'x1', 'coef': 1.9959319271341076, 'std_error': 0.012590525649303132}, {'name': 'x2', 'coef': -1.493019020399558, 'std_error': 0.013201982586799735}, {'name': 'x3', 'coef': 0.5156911992247248, 'std_error': 0.012753883867304729}], 'xreg_stats': [{'name': 'x1', 'coef': 1.9959319271341076, 'std_error': 0.012590525649303132, 'z': 158.5264970445916, 'p_value': 0.0}, {'name': 'x2', 'coef': -1.493019020399558, 'std_error': 0.013201982586799735, 'z': -113.09051580573835, 'p_value': 0.0}, {'name': 'x3', 'coef': 0.5156911992247248, 'std_error': 0.012753883867304729, 'z': 40.43405166536972, 'p_value': 0.0}]}
```


## Train update without external regressors

Suppose we have the old data, we can train it with train all, then we use this trained model to continue training on the new coming data and save it to a new model.

Job:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_ts_train_update",
    "kwargs": {
      "input_arrow": "/tmp/tmpwrsfgafv/ts_part2.arrow",
      "output_model": "/tmp/tmpwrsfgafv/ts_update.afm",
      "operations": {
        "algorithm": "arima",
        "model_path": "/tmp/tmpwrsfgafv/ts_base.afm",
        "time_column": "time"
      }
    }
  }
]
```

**Param explanation:**

- *input_arrow*: String for input data
- *output_model*: String path to a new model to be saved after updating
- *operations*: See the Hashmap below:

| Key | Type | Default | Range | Meaning |
|-----|------|---------|-------|---------|
| `model_path` | str | required | existing model file | Model to update. Single or panel is detected from the file. |
| `algorithm` | str | read from model | `"arima"` | Optional. |
| `time_column` | str | `""` | existing column or empty | Time column of the new rows. When set and the model recorded a last time, new times must be greater than it. |


## Train update with external regressors

Job:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_ts_train_update",
    "kwargs": {
      "input_arrow": "/tmp/tmpjaygdiff/xreg_part2.arrow",
      "output_model": "/tmp/tmpjaygdiff/ts_update.afm",
      "operations": {
        "algorithm": "arima",
        "model_path": "/tmp/tmpjaygdiff/ts_base.afm",
        "time_column": "time",
        "xreg_columns": [
          "x1",
          "x2",
          "x3"
        ]
      }
    }
  }
]
```

In this case, it is similar to train update, but we need xreg_columns as a list of strings for external column names.

**Return Values:**

```json
{'algorithm': 'arima', 'kind': 'single', 'target': 'y', 'xreg_columns': ['x1', 'x2', 'x3'], 'spec': {'p': 1, 'd': 0, 'q': 1, 'P': 0, 'D': 0, 'Q': 0, 'period': 1, 'include_mean': True, 'transform_pars': True, 'method': 'CSS-ML', 'ssinit': 'Gardner1980', 'kappa': 1000000.0, 'tol': 1e-08, 'max_iter': 100}, 'warnings': [], 'nobs': 3000, 'sigma2': 0.9692343419574265, 'loglik': -3368.414614834458, 'aic': 6750.829229668916, 'coef': [{'name': 'ar1', 'coef': 0.5934694329803777, 'std_error': 0.021035657439103395}, {'name': 'ma1', 'coef': 0.3382289067517509, 'std_error': 0.024437091453519556}, {'name': 'intercept', 'coef': 4.943639415055771, 'std_error': 0.06609836813103688}, {'name': 'x1', 'coef': 2.009788664309469, 'std_error': 0.014288808208398935}, {'name': 'x2', 'coef': -1.4827347758922067, 'std_error': 0.014919099963858203}, {'name': 'x3', 'coef': 0.5232676401646713, 'std_error': 0.01424412096367251}], 'xreg_stats': [{'name': 'x1', 'coef': 2.009788664309469, 'std_error': 0.014288808208398935, 'z': 140.6547442583853, 'p_value': 0.0}, {'name': 'x2', 'coef': -1.4827347758922067, 'std_error': 0.014919099963858203, 'z': -99.38500174167069, 'p_value': 0.0}, {'name': 'x3', 'coef': 0.5232676401646713, 'std_error': 0.01424412096367251, 'z': 36.735691974196705, 'p_value': 1.967552160248443e-295}]}
```

## Train stream without external regressors

This function is good to handle big file (more than memory).


Job:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_ts_train_stream",
    "kwargs": {
      "input_arrow": "/tmp/tmpwrsfgafv/ts_full.arrow",
      "output_model": "/tmp/tmpwrsfgafv/ts_stream.afm",
      "operations": {
        "algorithm": "arima",
        "target": "y",
        "time_column": "time",
        "p": 1,
        "d": 0,
        "q": 1,
        "init_rows": 500,
        "chunk_rows": 300
      }
    }
  }
]
```

**Param explanation:**

Similar to others above, except two below:

| Key | Type | Default | Range | Meaning |
|-----|------|---------|-------|---------|
| `init_rows` | int | 10000 | >= 1 | Head rows used to fit coefficients. Pick a value large enough to be representative. |
| `chunk_rows` | int | 65536 | >= 1 | Rows per batch when streaming the rest. Affects memory only, not the result. |


**Return values:**

```json
{'algorithm': 'arima', 'kind': 'single', 'target': 'y', 'xreg_columns': [], 'spec': {'p': 1, 'd': 0, 'q': 1, 'P': 0, 'D': 0, 'Q': 0, 'period': 1, 'include_mean': True, 'transform_pars': True, 'method': 'CSS-ML', 'ssinit': 'Gardner1980', 'kappa': 1000000.0, 'tol': 1e-08, 'max_iter': 100}, 'warnings': [], 'nobs': 2000, 'sigma2': 0.986269459338001, 'loglik': -706.4461449217221, 'aic': 1420.8922898434441, 'coef': [{'name': 'ar1', 'coef': 0.6236226860465766, 'std_error': 0.045888017351057485}, {'name': 'ma1', 'coef': 0.26218535497345563, 'std_error': 0.05718598027178406}, {'name': 'intercept', 'coef': 5.169415596161348, 'std_error': 0.14838991233780244}], 'xreg_stats': [], 'window_rows': 500, 'stream_rows': 1500, 'stream_batches': 8}
```


## Train stream with external regressors


Job:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_ts_train_stream",
    "kwargs": {
      "input_arrow": "/tmp/tmpjaygdiff/xreg_full.arrow",
      "output_model": "/tmp/tmpjaygdiff/ts_stream.afm",
      "operations": {
        "algorithm": "arima",
        "target": "y",
        "time_column": "time",
        "xreg_columns": [
          "x1",
          "x2",
          "x3"
        ],
        "p": 1,
        "d": 0,
        "q": 1,
        "init_rows": 800,
        "chunk_rows": 300
      }
    }
  }
]
```

The params are explained in the previous cases


## Make prediction without external regressors

Once we train the model and save to a file. We can use it to predict.

Job:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_ts_predict",
    "kwargs": {
      "input_model": "/tmp/tmpwrsfgafv/ts_stream.afm",
      "output_arrow": "/tmp/tmpwrsfgafv/ts_forecast.arrow",
      "operations": {
        "algorithm": "arima",
        "h": 12,
        "levels": [
          95
        ]
      }
    }
  }
]
```

| Key | Type | Default | Range | Meaning |
|-----|------|---------|-------|---------|
| `h` | int | 0 | >= 1 | Forecast horizon in steps. 0 raises an error. |
| `levels` | list[float] | `[80, 95]` | each strictly between 0 and 100 | Prediction interval levels in percent. Each adds `lo_<level>` and `hi_<level>` columns. |


**Predicted Results:**

| step | mean | se | lo_95 | hi_95 |
| --- | --- | --- | --- | --- |
| 1 | 4.13 | 0.99 | 2.19 | 6.08 |
| 2 | 4.52 | 1.33 | 1.92 | 7.12 |
| 3 | 4.77 | 1.44 | 1.95 | 7.58 |
| 4 | 4.92 | 1.48 | 2.03 | 7.81 |
| 5 | 5.01 | 1.49 | 2.09 | 7.94 |
| 6 | 5.07 | 1.50 | 2.14 | 8.01 |
| 7 | 5.11 | 1.50 | 2.17 | 8.05 |
| 8 | 5.13 | 1.50 | 2.19 | 8.07 |
| 9 | 5.15 | 1.50 | 2.20 | 8.09 |
| 10 | 5.15 | 1.50 | 2.21 | 8.10 |
| 11 | 5.16 | 1.50 | 2.22 | 8.10 |
| 12 | 5.16 | 1.50 | 2.22 | 8.11 |


## Make prediction with external regressors

Since we build model with external regressor, we have to specify the future values for these external regressors so that the model can use them to make prediction.

Job:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_ts_predict",
    "kwargs": {
      "input_model": "/tmp/tmpjaygdiff/ts_stream.afm",
      "output_arrow": "/tmp/tmpjaygdiff/ts_xreg_forecast.arrow",
      "operations": {
        "algorithm": "arima",
        "h": 8,
        "levels": [
          95
        ],
        "newxreg": "/tmp/tmpjaygdiff/xreg_future.arrow",
        "time_column": "time"
      }
    }
  }
]
```

Notice: The `newxreg` arrow must have enough xreg columns as we used to train the model.

| Key | Type | Default | Range | Meaning |
|-----|------|---------|-------|---------|
| `newxreg` | str | `""` | Arrow file, exactly `h` rows | Future regressor values. Required when the model has regressors, and an error when it has none. No missing values. Columns are matched by name, so order does not matter. A missing regressor column raises `RuntimeError`. |

**Predicted Results:**

| step | mean | se | lo_95 | hi_95 |
| --- | --- | --- | --- | --- |
| 1 | 10.09 | 0.95 | 8.23 | 11.95 |
| 2 | 5.62 | 1.34 | 2.99 | 8.24 |
| 3 | 10.85 | 1.46 | 7.99 | 13.70 |
| 4 | -0.15 | 1.50 | -3.09 | 2.79 |
| 5 | 5.17 | 1.52 | 2.20 | 8.14 |
| 6 | 2.85 | 1.52 | -0.13 | 5.83 |
| 7 | 7.10 | 1.52 | 4.11 | 10.08 |
| 8 | 8.20 | 1.53 | 5.21 | 11.19 |