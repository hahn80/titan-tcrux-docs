# Functions for Analysis


## Percent Change


Xét dữ liệu stock của Amazon:

| Date | Close | Volume | Open | High | Low |
| --- | --- | --- | --- | --- | --- |
| 2025-10-03 | 219.51 | 43639030 | 223.44 | 224.20 | 219.34 |
| 2025-10-02 | 222.41 | 41258590 | 221.01 | 222.81 | 218.94 |
| 2025-10-01 | 220.63 | 43933830 | 217.36 | 222.15 | 216.61 |
| 2025-09-30 | 219.57 | 48396370 | 222.03 | 222.24 | 217.89 |
| 2025-09-29 | 222.17 | 44259180 | 220.08 | 222.60 | 219.30 |
| 2025-09-26 | 219.78 | 41650100 | 219.08 | 221.05 | 218.02 |
| 2025-09-25 | 218.15 | 52226330 | 220.06 | 220.67 | 216.47 |
| 2025-09-24 | 220.21 | 49509030 | 224.15 | 224.56 | 219.45 |
| 2025-09-23 | 220.71 | 70956190 | 227.83 | 227.86 | 220.07 |
| 2025-09-22 | 227.63 | 45914510 | 230.56 | 230.56 | 227.51 |
| 2025-09-19 | 231.48 | 97943170 | 232.37 | 234.16 | 229.70 |
| 2025-09-18 | 231.23 | 37931740 | 232.50 | 233.48 | 228.79 |
| 2025-09-17 | 231.62 | 42815230 | 233.77 | 234.30 | 228.71 |
| 2025-09-16 | 234.05 | 38203910 | 232.94 | 235.90 | 232.23 |
| 2025-09-15 | 231.43 | 33243330 | 230.62 | 233.73 | 230.32 |
| 2025-09-12 | 228.15 | 38496220 | 230.35 | 230.79 | 226.29 |
| 2025-09-11 | 229.95 | 37485600 | 231.49 | 231.53 | 229.34 |
| 2025-09-10 | 230.33 | 60907710 | 237.51 | 237.68 | 229.10 |

Chú ý rằng dữ liệu không được sắp theo thứ tự tăng dần theo ngày. Do đó khi tính pct_change cần phải sắp xếp lại.


Params to get pct_change:

```json
jobs = [
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "amazon_stock.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "numeric_transform",
    "kwargs": {
      "operations": {
        "Close": [
          {
            "func": "pct_change",
            "alias": "Close_pct_change"
          }
        ],
        "Volume": [
          {
            "func": "pct_change",
            "alias": "Volume_pct_change"
          }
        ]
      },
      "sorted_by": [
        "Date"
      ]
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Kết quả output:

| Date | Close | Volume | Open | High | Low | Close_pct_change | Volume_pct_change |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2024-10-07 | 180.80 | 42364200 | 182.95 | 183.60 | 180.25 | None | None |
| 2024-10-08 | 182.72 | 26372090 | 181.91 | 183.09 | 180.92 | 0.01 | -0.38 |
| 2024-10-09 | 185.17 | 26343120 | 182.82 | 185.84 | 182.05 | 0.01 | -0.00 |
| 2024-10-10 | 186.65 | 27785040 | 187.13 | 188.13 | 185.83 | 0.01 | 0.05 |
| 2024-10-11 | 188.82 | 25751560 | 186.63 | 189.93 | 186.30 | 0.01 | -0.07 |
| 2024-10-14 | 187.54 | 22614410 | 189.78 | 189.83 | 187.36 | -0.01 | -0.12 |
| 2024-10-15 | 187.69 | 32178930 | 187.63 | 188.41 | 184.58 | 0.00 | 0.42 |
| 2024-10-16 | 186.89 | 23456810 | 187.05 | 187.78 | 185.61 | -0.00 | -0.27 |
| 2024-10-17 | 187.53 | 25039410 | 188.22 | 188.94 | 186.00 | 0.00 | 0.07 |
| 2024-10-18 | 188.99 | 37417670 | 187.15 | 190.74 | 186.28 | 0.01 | 0.49 |
| 2024-10-21 | 189.07 | 24639390 | 188.05 | 189.46 | 186.40 | 0.00 | -0.34 |
| 2024-10-22 | 189.70 | 29650590 | 188.35 | 191.52 | 186.97 | 0.00 | 0.20 |
| 2024-10-23 | 184.71 | 31937090 | 188.85 | 189.16 | 183.69 | -0.03 | 0.08 |
| 2024-10-24 | 186.38 | 21647400 | 185.25 | 187.11 | 183.86 | 0.01 | -0.32 |


Giải thích kết quả: (trong hàng thứ 2)

- Nếu pct_change kết quả là  0.01: nghĩa là ngày 2024-10-08 Close tăng tăng 1% so với ngày 2024-10-07.
- Nếu pct_change là -0.38: nghĩa là 2024-10-08 Volume giảm 38% so với ngày 2024-10-07.


## Percent Change with group_by


Tiếp theo chúng ta sẽ tính giá trung bình theo tháng và sau đó tính pct_change theo tháng.

### Extract the year, month from date


```json
jobs = [
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "amazon_stock.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "datetime_transform",
    "kwargs": {
      "operations": {
        "Date": [
          {
            "func": "month",
            "alias": "Month"
          },
          {
            "func": "year",
            "alias": "Year"
          }
        ]
      }
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Output results:

| Date | Close | Volume | Open | High | Low | Month | Year |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2025-10-03 | 219.51 | 43639030 | 223.44 | 224.20 | 219.34 | 10 | 2025 |
| 2025-10-02 | 222.41 | 41258590 | 221.01 | 222.81 | 218.94 | 10 | 2025 |
| 2025-10-01 | 220.63 | 43933830 | 217.36 | 222.15 | 216.61 | 10 | 2025 |
| 2025-09-30 | 219.57 | 48396370 | 222.03 | 222.24 | 217.89 | 9 | 2025 |
| 2025-09-29 | 222.17 | 44259180 | 220.08 | 222.60 | 219.30 | 9 | 2025 |
| 2025-09-26 | 219.78 | 41650100 | 219.08 | 221.05 | 218.02 | 9 | 2025 |
| 2025-09-25 | 218.15 | 52226330 | 220.06 | 220.67 | 216.47 | 9 | 2025 |
| 2025-09-24 | 220.21 | 49509030 | 224.15 | 224.56 | 219.45 | 9 | 2025 |
| 2025-09-23 | 220.71 | 70956190 | 227.83 | 227.86 | 220.07 | 9 | 2025 |
| 2025-09-22 | 227.63 | 45914510 | 230.56 | 230.56 | 227.51 | 9 | 2025 |


### Compute the mean and group_by

Bây giờ chúng ta sẽ nhóm theo ngày tháng để tính trung bình:

```json
jobs = [
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "amazon_stock.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "datetime_transform",
    "kwargs": {
      "operations": {
        "Date": [
          {
            "func": "month",
            "alias": "Month"
          },
          {
            "func": "year",
            "alias": "Year"
          }
        ]
      }
    }
  },
  {
    "task": "transforming",
    "action": "group_by",
    "kwargs": {
      "by": [
        "Year",
        "Month"
      ],
      "operations": {
        "Close": [
          {
            "func": "mean",
            "alias": "Close_avg"
          }
        ],
        "Open": [
          {
            "func": "mean",
            "alias": "Open_avg"
          }
        ]
      },
      "order_by": [
        "Year",
        "Month"
      ]
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Output results:

| Year | Month | Close_avg | Open_avg |
| --- | --- | --- | --- |
| 2024 | 10 | 187.31 | 187.53 |
| 2024 | 11 | 204.51 | 203.90 |
| 2024 | 12 | 224.10 | 223.82 |
| 2025 | 1 | 228.02 | 227.91 |
| 2025 | 2 | 225.85 | 226.58 |
| 2025 | 3 | 198.51 | 198.83 |
| 2025 | 4 | 181.21 | 179.93 |
| 2025 | 5 | 200.16 | 200.00 |
| 2025 | 6 | 213.02 | 213.08 |
| 2025 | 7 | 226.30 | 226.37 |
| 2025 | 8 | 224.67 | 224.22 |
| 2025 | 9 | 228.09 | 229.13 |
| 2025 | 10 | 220.85 | 220.60 |



### Compute the pct_change mean and group_by

Bây giờ chúng ta sẽ kết hợp tất cả các phương án ở trên để tính toán pct_change theo nhóm.

```json
jobs = [
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "amazon_stock.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "datetime_transform",
    "kwargs": {
      "operations": {
        "Date": [
          {
            "func": "month",
            "alias": "Month"
          },
          {
            "func": "year",
            "alias": "Year"
          }
        ]
      }
    }
  },
  {
    "task": "transforming",
    "action": "group_by",
    "kwargs": {
      "by": [
        "Year",
        "Month"
      ],
      "operations": {
        "Close": [
          {
            "func": "mean",
            "alias": "Close_avg"
          }
        ],
        "Open": [
          {
            "func": "mean",
            "alias": "Open_avg"
          }
        ]
      },
      "order_by": [
        "Year",
        "Month"
      ]
    }
  },
  {
    "task": "transforming",
    "action": "numeric_transform",
    "kwargs": {
      "operations": {
        "Close_avg": [
          {
            "func": "pct_change",
            "alias": "Close_avg_pct_change"
          }
        ],
        "Open_avg": [
          {
            "func": "pct_change",
            "alias": "Open_avg_pct_change"
          }
        ]
      }
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Output results:

| Year | Month | Close_avg | Open_avg | Close_avg_pct_change | Open_avg_pct_change |
| --- | --- | --- | --- | --- | --- |
| 2024 | 10 | 187.31 | 187.53 | None | None |
| 2024 | 11 | 204.51 | 203.90 | 0.09 | 0.09 |
| 2024 | 12 | 224.10 | 223.82 | 0.10 | 0.10 |
| 2025 | 1 | 228.02 | 227.91 | 0.02 | 0.02 |
| 2025 | 2 | 225.85 | 226.58 | -0.01 | -0.01 |
| 2025 | 3 | 198.51 | 198.83 | -0.12 | -0.12 |
| 2025 | 4 | 181.21 | 179.93 | -0.09 | -0.10 |
| 2025 | 5 | 200.16 | 200.00 | 0.10 | 0.11 |
| 2025 | 6 | 213.02 | 213.08 | 0.06 | 0.07 |
| 2025 | 7 | 226.30 | 226.37 | 0.06 | 0.06 |
| 2025 | 8 | 224.67 | 224.22 | -0.01 | -0.01 |
| 2025 | 9 | 228.09 | 229.13 | 0.02 | 0.02 |
| 2025 | 10 | 220.85 | 220.60 | -0.03 | -0.04 |
