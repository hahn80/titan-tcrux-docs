# Timeseries Forecasting


This service is used to forecast the timeseries data. We can predict one column with multiple algorithms (to compare) or multiple columns with defferent algorithms.


## Forecast on single column

Consider the orders dataset as given below:


| OrderDate | Region | Rep | Item | Units | Unit Cost |
| --- | --- | --- | --- | --- | --- |
| 2021-01-06 | East | Jones | Pencil | 95 |  1.99  |
| 2021-01-23 | Central | Kivell | Binder | 50 |  19.99  |
| 2021-02-09 | Central | Jardine | Pencil | 36 |  4.99  |
| 2021-02-26 | Central | Gill | Pen | 27 |  19.99  |
| 2021-03-15 | West | Sorvino | Pencil | 56 |  2.99  |
| 2021-04-01 | East | Jones | Binder | 60 |  4.99  |
| 2021-04-18 | Central | Andrews | Pencil | 75 |  1.99  |
| 2021-05-05 | Central | Jardine | Pencil | 90 |  4.99  |
| 2021-05-22 | West | Thompson | Pencil | 32 |  1.99  |
| 2021-06-08 | East | Jones | Binder | 60 |  8.99  |
| 2021-06-25 | Central | Morgan | Pencil | 90 |  4.99  |
| 2021-07-12 | East | Howard | Binder | 29 |  1.99  |
| 2021-07-29 | East | Parent | Binder | 81 |  19.99  |
| 2021-08-15 | East | Jones | Pencil | 35 |  4.99  |
| 2021-09-01 | Central | Smith | Desk | 2 |  125.00  |
| 2021-09-18 | East | Jones | Pen Set | 16 |  15.99  |
| 2021-10-05 | Central | Morgan | Binder | 28 |  8.99  |
| 2021-10-22 | East | Jones | Pen | 64 |  8.99  |
| 2021-11-08 | East | Parent | Pen | 15 |  19.99  |


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
    "action": "forecast",
    "kwargs": {
      "operations": {
        "Units": [
          {
            "algorithm": "ARIMA",
            "args": {
              "p": 12,
              "d": 1,
              "q": 2
            },
            "future": 10,
            "ts_index": "OrderDate",
            "freq": "D",
            "alias": "Units_Arima"
          }
        ]
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


Output result:

| OrderDate | Units_Arima |
| --- | --- |
| 2024-12-30 00:00:00 | 75.28 |
| 2024-12-31 00:00:00 | 76.67 |
| 2025-01-01 00:00:00 | 78.72 |
| 2025-01-02 00:00:00 | 81.69 |
| 2025-01-03 00:00:00 | 85.28 |
| 2025-01-04 00:00:00 | 88.84 |
| 2025-01-05 00:00:00 | 91.69 |
| 2025-01-06 00:00:00 | 93.32 |
| 2025-01-07 00:00:00 | 93.55 |
| 2025-01-08 00:00:00 | 92.62 |


## Forecast on multiple columns


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
    "action": "forecast",
    "kwargs": {
      "operations": {
        "Units": [
          {
            "algorithm": "ARIMA",
            "args": {
              "p": 12,
              "d": 1,
              "q": 2
            },
            "future": 10,
            "ts_index": "OrderDate",
            "freq": "D",
            "alias": "Units_Arima"
          }
        ],
        "Unit_Cost": [
          {
            "algorithm": "ARIMA",
            "args": {
              "p": 8,
              "d": 1,
              "q": 1
            },
            "future": 10,
            "ts_index": "OrderDate",
            "freq": "D",
            "alias": "Unit_Costs_ARIMA"
          }
        ]
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

Output results:

| OrderDate | Units_Arima | Unit_Costs_ARIMA |
| --- | --- | --- |
| 2024-12-30 00:00:00 | 75.28 | 15.79 |
| 2024-12-31 00:00:00 | 76.67 | 15.28 |
| 2025-01-01 00:00:00 | 78.72 | 14.56 |
| 2025-01-02 00:00:00 | 81.69 | 13.84 |
| 2025-01-03 00:00:00 | 85.28 | 13.23 |
| 2025-01-04 00:00:00 | 88.84 | 12.63 |
| 2025-01-05 00:00:00 | 91.69 | 11.87 |
| 2025-01-06 00:00:00 | 93.32 | 10.96 |
| 2025-01-07 00:00:00 | 93.55 | 10.12 |
| 2025-01-08 00:00:00 | 92.62 | 9.48 |






