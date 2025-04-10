# Các hàm dự báo timeseries.


Các hàm dự báo cho time series. Các hàm này được gọi với tham số tương tự nhau. Chỉ khác nhau ở 2 tham số *algorithm* và *args*.

1. Hàm ARIMA

Hàm này cần các tham số sau:

- *algorithm*: String có giá trị là `"ARIMA"`
- *args*: List Hashmap: Chứa các tham số cần thiết cho thuật toán ARIMA.
- Ý nghĩa của các tham số trong args:
	- p (int) – Order (number of time lags) of the autoregressive model (AR).
	- d (int) – The order of differentiation; i.e., the number of times the data have had past values subtracted (I).
	- q (int) – The size of the moving average window (MA).

```json
params = {
  "task": "timeseries",
  "action": "forecast",
  "kwargs": {
    "operations": {
      "Passengers": [
        {
          "algorithm": "ARIMA",
          "args": {
            "p": 12,
            "d": 1,
            "q": 2
          },
          "future": 10,
          "ts_index": "Month",
          "freq": "MS",
          "alias": "ps1"
        }
    }
  }
}
```

Kết quả output có dạng như sau:

| Month              | ps1        |
| 1961-01-01 00:00:00 | 448.519105 |
| 1961-02-01 00:00:00 | 414.329074 |
| 1961-03-01 00:00:00 | 438.896605 |
| 1961-04-01 00:00:00 | 478.230597 |
| 1961-05-01 00:00:00 | 499.402339 |
| 1961-06-01 00:00:00 | 554.148824 |
| 1961-07-01 00:00:00 | 641.556879 |
| 1961-08-01 00:00:00 | 620.202744 |
| 1961-09-01 00:00:00 | 532.193188 |
| 1961-10-01 00:00:00 | 477.381192 |


**Giải thích kết quả:**



2. Hàm AutoARIMA

- Ý nghĩa của các tham số trong args:
	- **start_p**: *INT*; starting value for the AR (autoregressive) order search  
	- **max_p**: *INT*; maximum value for the AR (p) order considered  
	- **start_q**: *INT*; starting value for the MA (moving average) order search


```json
params = {
  "task": "timeseries",
  "action": "forecast",
  "kwargs": {
    "operations": {
      "Passengers": [
        {
          "algorithm": "AutoARIMA",
          "args": {
            "start_p": 8,
            "max_p": 12,
            "start_q": 1
          },
          "future": 10,
          "ts_index": "Month",
          "freq": "MS",
          "alias": "ps2"
        }
      ]
    }
  }
}
```
