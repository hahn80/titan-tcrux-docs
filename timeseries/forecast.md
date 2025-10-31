# Timeseries Forecasting


This service is used to forecast the timeseries data. We can predict one column with multiple algorithms (to compare) or multiple columns with defferent algorithms.
The explanation for each algorithm is at the end of the page.


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
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/filled_orders.arrow"
    }
  },
  {
    "task": "timeseries",
    "action": "forecast",
    "kwargs": {
      "ts_index": "OrderDate",
      "freq": "D",
      "future": 10,
      "operations": {
        "Units": [
          {
            "algorithm": "ARIMA",
            "args": {
              "p": 12,
              "d": 1,
              "q": 2,
              "seasonal_order": [
                0,
                0,
                0,
                0
              ],
              "trend": "t"
            },
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

**Explanation of parameters:**

- *ts_index*: Name of the time data column
- *freq*: Frequency of the time data, for example "D" for day, "M" for month. Check the frequecy table below.
- *future*: The number or dates (by unit) to predict in the future.
- *operations*: Hashmap<String, Object> to perform the forecasting, check the explanation of each algorithm at the end.

| Frequency | Description |
|--------|-------------|
| B | business day frequency |
| D | calendar day frequency |
| W | weekly frequency |
| M | monthly frequency |
| Q | quarterly frequency |
| Y | yearly frequency |
| h | hourly frequency |
| min | minutely frequency |
| s | secondly frequency |
| ms | milliseconds |
| us | microseconds |
| ns | nanoseconds |



Output result:

| OrderDate | Units_Arima |
| --- | --- |
| 2024-12-30 00:00:00 | 74 |
| 2024-12-31 00:00:00 | 75 |
| 2025-01-01 00:00:00 | 75 |
| 2025-01-02 00:00:00 | 76 |
| 2025-01-03 00:00:00 | 76 |
| 2025-01-04 00:00:00 | 76 |
| 2025-01-05 00:00:00 | 77 |
| 2025-01-06 00:00:00 | 77 |
| 2025-01-07 00:00:00 | 77 |
| 2025-01-08 00:00:00 | 77 |


## Forecast on multiple columns


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/filled_orders.arrow"
    }
  },
  {
    "task": "timeseries",
    "action": "forecast",
    "kwargs": {
      "ts_index": "OrderDate",
      "freq": "D",
      "future": 10,
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
          },
          {
            "algorithm": "AutoARIMA",
            "args": {
              "start_p": 8,
              "max_p": 12,
              "start_q": 1
            },
            "future": 10,
            "ts_index": "OrderDate",
            "freq": "D",
            "alias": "Units_AutoArima"
          }
        ],
        "Unit_Cost": [
          {
            "algorithm": "AutoARIMA",
            "args": {
              "start_p": 8,
              "max_p": 12,
              "start_q": 1
            },
            "future": 10,
            "ts_index": "OrderDate",
            "freq": "D",
            "alias": "Unit_Costs_AutoARIMA"
          },
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

| OrderDate | Units_Arima | Units_AutoArima | Unit_Costs_AutoARIMA | Unit_Costs_ARIMA |
| --- | --- | --- | --- | --- |
| 2024-12-30 00:00:00 | 74 | 74 | 16.80 | 16.79 |
| 2024-12-31 00:00:00 | 75 | 75 | 17.54 | 17.54 |
| 2025-01-01 00:00:00 | 75 | 75 | 18.22 | 18.21 |
| 2025-01-02 00:00:00 | 76 | 74 | 18.84 | 18.82 |
| 2025-01-03 00:00:00 | 76 | 74 | 19.39 | 19.37 |
| 2025-01-04 00:00:00 | 76 | 73 | 19.87 | 19.84 |
| 2025-01-05 00:00:00 | 77 | 73 | 20.28 | 20.25 |
| 2025-01-06 00:00:00 | 77 | 72 | 20.63 | 20.59 |
| 2025-01-07 00:00:00 | 77 | 71 | 20.91 | 20.87 |
| 2025-01-08 00:00:00 | 77 | 70 | 21.13 | 21.09 |




## Argument for each algorithm

```json
{
                "algorithm": "ARIMA",
                "args": {
                    "p": 12,
                    "d": 1,
                    "q": 2,
                    "seasonal_order": [0, 0, 0, 0],
                    "trend": "t",
                },
                "alias": "Units_Arima",
            }
```

We will cover the `algorithm` and `args` in detail for each supported algorithm.

1. ARIMA

Argument has p, d, q, seasonal_order, and trend.

- p (Int): Order (number of time lags) of the autoregressive model (AR).

- d (Int): The order of differentiation; i.e., the number of times the data have had past values subtracted (I).

- q (Int):  The size of the moving average window (MA).

- seasonal_order (List of 4 Int): There are 4 integers (P,D,Q,s) order of the seasonal component for the AR parameters (P), differences (D), MA parameters (Q) and periodicity (s).
- trend (String): 'n', 'c', 't', 'ct': 'n' for no trend, 'c' for a constant term, 't' for a linear trend in time, and 'ct' for a constant term and linear trend.

2. AutoARIMA.

Automatically selects the best ARIMA (AutoRegressive Integrated Moving Average)
model using an information criterion. Default is Akaike Information Criterion (AICc).

- **d** *(Int* — Order of first-differencing.
- **D** *(Int)* — Order of seasonal-differencing.
- **start_p** *(int)* — Starting value of `p` in stepwise procedure.
- **start_q** *(int)* — Starting value of `q` in stepwise procedure.
- **start_P** *(int)* — Starting value of `P` in stepwise procedure.
- **start_Q** *(int)* — Starting value of `Q` in stepwise procedure.
- **max_p** *(int)* — Max autoregressive order `p`.
- **max_q** *(int)* — Max moving average order `q`.
- **max_P** *(int)* — Max seasonal autoregressive order `P`.
- **max_Q** *(int)* — Max seasonal moving average order `Q`.
- **max_d** *(int)* — Max number of non-seasonal differences.
- **max_D** *(int)* — Max number of seasonal differences.
- **stationary** *(bool)* — If `True`, restricts search to stationary models.
- **seasonal** *(bool)* — If `False`, restricts search to non-seasonal models.
- **season_length** *(int)* — Number of observations per unit of time (e.g., 24 for hourly data).


3. ExponentialSmoothing

- **trend** — Type of trend component. Either `ADDITIVE`, `MULTIPLICATIVE`,  or null.

- **seasonal** — Type of seasonal component.  Either `ADDITIVE`, `MULTIPLICATIVE`, `SeasonalityMode.NONE`, or null.

For example:

```json
{
	"algorithm": "ExponentialSmoothing",
	"args": {"trend": "ADDITIVE", "seasonal": "ADDITIVE"},
	"alias": "Units_EXPS",
}
```






