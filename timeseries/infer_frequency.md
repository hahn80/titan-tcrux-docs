# Infer the frequency in Timeseries


This service will find a suitable frequency in timeseries data.


The input data:

| OrderDate | Region | Rep | Item | Units | Unit_Cost |
| --- | --- | --- | --- | --- | --- |
| 2021-01-06 00:00:00 | East | Jones | Pencil | 95 | 1.99 |
| 2021-01-23 00:00:00 | Central | Kivell | Binder | 50 | 19.99 |
| 2021-02-09 00:00:00 | Central | Jardine | Pencil | 36 | 4.99 |
| 2021-02-26 00:00:00 | Central | Gill | Pen | 27 | 19.99 |
| 2021-03-15 00:00:00 | West | Sorvino | Pencil | 56 | 2.99 |



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
    "task": "converting",
    "action": "infer_frequency",
    "kwargs": {
      "ts_index": "OrderDate",
      "strategy": "min"
    }
  }
]
```

Explain the parameters:

- *ts_index*: String: The name of the datetime column, we accept `Date`; `Datetime` or `Timestamp`.
- *strategy*: String: Either `min` or `mode`.


Output result:

```json
{'unit': 'day', 'size': 8, 'freq': '8D'}
```
