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

4. TBATS


**TBATS** stands for:

> **T**rigonometric seasonality, **B**ox-Cox transformation, **A**RMA errors, **T**rend, and **S**easonal components.

It’s a flexible model designed to handle **multiple seasonalities**, **non-linear trends**, and **complex error structures** — especially useful for **hourly, daily, or weekly data** with repeating cycles.


| Parameter            | Type                    | Default | Description                                                                                                                                     |
| -------------------- | ----------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| **season_length**    | Int or List<Int> | —       | Number of observations per unit of time. For example, `24` for hourly data (daily cycle), or `[24, 168]` for both daily and weekly seasonality. |
| **use_boxcox**       | `Optional[bool]`        | `True`  | Whether to apply a **Box-Cox transformation** to stabilize variance and make the series more normally distributed.                              |
| **bc_lower_bound**   | `float`                 | `0.0`   | Lower bound for Box-Cox λ (lambda).                                                                                                             |
| **bc_upper_bound**   | `float`                 | `1.0`   | Upper bound for Box-Cox λ (lambda).                                                                                                             |
| **use_trend**        | `Optional[bool]`        | `True`  | Whether to include a **trend component** in the model (linear growth or decline over time).                                                     |
| **use_damped_trend** | `Optional[bool]`        | `False` | Whether to apply **damping** to the trend, reducing its influence over time.                                                                    |
| **use_arma_errors**  | `bool`                  | `False` | Whether to use an **ARMA (AutoRegressive Moving Average)** component to model autocorrelation in the residual errors.                           |


**Example:** season length = [7, 30]: Weekly and monthly seasonality

```json
{
	"algorithm": "TBATS",
	"args": {
	  "season_length": [7, 30],
	  "use_boxcox": true,
	  "bc_lower_bound": 0.0,
	  "bc_upper_bound": 1.0,
	  "use_trend": true,
	  "use_damped_trend": false,
	  "use_arma_errors": true
	},
	"alias": "Unit_Costs_TBATS"
}
```


5. Theta


Theta model is a time series forecasting method that combines simple exponential smoothing (SES) with a decomposition approach that modifies the curvature of the time series using a parameter called θ (theta).

| Parameter | Default | Description|
| -- | -- | -- |
| **theta** | `2` | The **theta coefficient** that controls the curvature adjustment of the deseasonalized series. Cannot be `0`.  <br> - `θ = 1`: equivalent to **Simple Exponential Smoothing (SES)**.  <br> - `θ > 1`: allows the model to capture curvature and trend more strongly. |
| **seasonality_period** | *None* | User-defined **seasonality period**. <br> If not set, Darts will **automatically infer** it from the training data when you call `.fit()`.|
| **season_mode**        | `MULTIPLICATIVE` | Type of seasonal component:  <br> - `ADDITIVE`: seasonality added directly to the series. <br> - `MULTIPLICATIVE`: seasonality scales with the level of the series. <br> - `null`: disables seasonality.|

	
```json
{
	"algorithm": "Theta",
	"args": {
	  "theta": 2,
	  "seasonality_period": 7,
	  "season_mode": "MULTIPLICATIVE"
	},
	"alias": "Unit_Costs_Theta"
}
```


6. FFT

The FFT (Fast Fourier Transform) model is a frequency-domain forecasting method.
It decomposes a time series into its frequency components using Fourier Transform, keeps the most significant frequencies, and reconstructs the forecasted signal using inverse FFT.


| Parameter | Type | Default | Description|
| -- | -- | -- | -- |
| **nr_freqs_to_keep**  | Int | *None*  | Number of dominant frequencies to retain during forecasting. Smaller values smooth the signal and remove noise; larger values capture more detail.|
| **required_matches**  | String | *None*  | Specifies which attributes of `Timestamp` must align between training and prediction phases (e.g., `{'month'}` for monthly data). <br> Ensures the start of prediction aligns with a full seasonal period. <br> If not set, the model automatically infers relevant seasonal attributes. |
| **trend** | String | *None*  | Type of trend to **remove (detrend)** before applying FFT.  <br> Possible values:  `poly` -> polynomial trend<; `exp` -> exponential trend; `None` —> no detrending|
| **trend_poly_degree** |  Int | *2*     | Degree of the polynomial to fit if `trend=poly`. Higher degrees capture more complex trends but risk overfitting.|


**Example:** keep 20 dominant frequencies and use a 2nd-degree polynomial detrending.

```json
{
            "algorithm": "FFT",
            "args": {
              "nr_freqs_to_keep": 20,
              "trend": "poly",
              "trend_poly_degree": 2
            },
            "alias": "Unit_Costs_FFT"
          }
```


7. KalmanForecaster

The Kalman Filter Forecaster in Darts is a state-space model that uses the Kalman filtering algorithm to produce forecasts.

It models the time series as a latent (hidden) state process evolving over time and estimates both the hidden states and future observations based on noisy measurements.

| Parameter | Type  | Default    | Description|
| --------- | ----- | ---------- | -- |
| **dim_x** | `int` | *required* | Size (dimension) of the Kalman filter **state vector** — controls how many latent states the model uses to represent the system. A larger `dim_x` can model more complex temporal dependencies. |


* The **Kalman Filter** is ideal for **noisy, dynamic, or partially observed** systems (e.g., sensor data, control systems).
* The parameter `dim_x` roughly controls model complexity — larger values may better capture hidden dynamics but require more data.
* The model **treats future values as missing** and uses the Kalman filter’s recursive equations to estimate them.
* When you don’t specify a Kalman model, Darts uses **N4SID** to identify the best linear state-space system from your data.



```json
{
                "algorithm": "KalmanForecaster",
                "args": {"dim_x": 10},
                "alias": "Unit_Costs_KF",
            }
```



