# Fill Missing Dates In Timeseries


This service can be used to fill the missing data from a timeseries.


The input data:

| OrderDate | Region | Rep | Item | Units | Unit_Cost |
| --- | --- | --- | --- | --- | --- |
| 2021-01-06 00:00:00 | East | Jones | Pencil | 95 | 1.99 |
| 2021-01-23 00:00:00 | Central | Kivell | Binder | 50 | 19.99 |
| 2021-02-09 00:00:00 | Central | Jardine | Pencil | 36 | 4.99 |
| 2021-02-26 00:00:00 | Central | Gill | Pen | 27 | 19.99 |
| 2021-03-15 00:00:00 | West | Sorvino | Pencil | 56 | 2.99 |


We can see that there are gaps in between dates. This is a really bad example as we only have two dates for each month. However, we will create a full dataset with all the dates. So we will use the ts_interval="1d".


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/orders.arrow"
    }
  },
  {
    "task": "timeseries",
    "action": "fill_missing_dates",
    "kwargs": {
      "ts_index": "OrderDate",
      "ts_interval": "1d",
      "operations": {
        "Units": {
          "alias": "Units",
          "fill_value": null,
          "strategy": "linear"
        },
        "Unit_Cost": {
          "alias": "Unit_Cost",
          "fill_value": null,
          "strategy": "min"
        }
      }
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "/data/TitanProjects/tcrux/src/tests/resources/output.arrow",
      "batch_size": 20,
      "reformat_string": true
    }
  }
]
```

Explain the parameters:

- *ts_index*: String: The name of the datetime column, we accept `Date`; `Datetime` or `Timestamp`.
- *ts_interval*: String: The format could be "2d" or "1mo"... Check the notice below.
- *operations*: Hashmap<String, Object> The object contains alias, fill_value, and strategy. If the fill_value is not null (say fill_value=10), then we will use this value to replace any null values. If fill_value is null, then we consider the stragety. There are strategies you can use: linear, mean, median, mode, min, max

| ts_interval | Meaning      | Example   | Notes                             |
| ---- | ------------ | --------- | --------------------------------- |
| `ns` | nanoseconds  | `"500ns"` | 1 ns = 10⁻⁹ seconds               |
| `us` | microseconds | `"250us"` | 1 µs = 10⁻⁶ seconds               |
| `ms` | milliseconds | `"100ms"` | 1 ms = 10⁻³ seconds               |
| `s`  | seconds      | `"30s"`   | standard                          |
| `m`  | minutes      | `"5m"`    | 60 s                              |
| `h`  | hours        | `"12h"`   | 3600 s                            |
| `d`  | days         | `"1d"`    | calendar day                      |
| `w`  | weeks        | `"2w"`    | 7 days                            |
| `mo` | months       | `"3mo"`   | calendar months (variable length) |
| `y`  | years        | `"1y"`    | 12 months (variable length)       |


**Combination examples:**


| Example    | Meaning               |
| ---------- | --------------------- |
| `"1d12h"`  | 1 day + 12 hours      |
| `"2w3d"`   | 2 weeks + 3 days      |
| `"1y6mo"`  | 1 year + 6 months     |
| `"10h30m"` | 10 hours + 30 minutes |



Output result:

| OrderDate | Region | Rep | Item | Units | Unit_Cost |
| --- | --- | --- | --- | --- | --- |
| 2021-01-06 00:00:00 | East | Jones | Pencil | 95 | 1.99 |
| 2021-01-07 00:00:00 | None | None | None | 92 | 1.29 |
| 2021-01-08 00:00:00 | None | None | None | 89 | 1.29 |
| 2021-01-09 00:00:00 | None | None | None | 87 | 1.29 |
| 2021-01-10 00:00:00 | None | None | None | 84 | 1.29 |
