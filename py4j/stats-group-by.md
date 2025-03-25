# Các hàm thống kê theo nhóm.


Group và tính toán thống kê sẽ tiết kiệm được thời gian và quá trình chạy.

1. Một ví dụ phức tạp về group by:

Tham số mẫu:

```json
{
  "task": "transforming",
  "action": "group_by",
  "kwargs": {
    "by": [
      "text",
      "cat"
    ],
    "operations": {
      "num": [
        {
          "func": "sum",
          "alias": "num_total",
          "args": []
        },
        {
          "func": "mean",
          "alias": "num_avg",
          "args": []
        },
        {
          "func": "ci_lower",
          "alias": "num_ci_lower",
          "args": [
            0.95
          ]
        },
        {
          "func": "ci_upper",
          "alias": "num_ci_upper",
          "args": [
            0.95
          ]
        }
      ],
      "count": [
        {
          "func": "max",
          "alias": "count_max",
          "args": []
        }
      ],
      "name": [
        {
          "func": "freq_value",
          "alias": "bool_freq_val",
          "args": []
        },
        {
          "func": "freq_count",
          "alias": "bool_freq_count",
          "args": []
        },
        {
          "func": "freq_percent",
          "alias": "bool_freq_pct",
          "args": []
        }
      ]
    },
	  "explode": true
  }
}
```

2. Có thể dùng các hàm mở rộng khác có trong Polars.

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "group_by.arrow",
    "tasks": [
        {
            "task": "transforming",
            "action": "group_by",
            "kwargs": {
				"by": [
				  "text",
				  "cat"
				],
				"operations": {
				  "num": [
					{
					  "func": "sum",
					  "alias": "num_total",
					  "args": []
					},
					{
					  "func": "mean",
					  "alias": "num_avg",
					  "args": []
					}
				  ],
				  "count": [
					{
					  "func": "max",
					  "alias": "count_max",
					  "args": []
					}
				  ]
				}
            },
        }
    ],
}
```

Kết quả output có dạng như sau:


| text | cat | num_total | num_avg | count_max |
|------|-----|-----------|---------|-----------|
| B    | B   | 1         | 1.0     | 4         |
| B    | A   | 2         | 2.0     | 6         |
| A    | A   | 3         | 3.0     | 4         |



**Giải thích các tham số:**

- *by*: List of Strings. Danh sách các cột cần group theo nhóm.
- *operations*" List of HashMap, each key is the column name and value has the form as a list of hashmaps {"func": ten_ham, "alias": new_name, "args": []}. 

3. Group by hỗ trợ tất cả các hàm sau:

3.a Các hàm không cần tham số, nghĩa là `args=[]`

abs; arccos; arccosh; arcsin; arcsinh; arctan; arctanh; arg_unique; cbrt; cos; cosh; cot; cum_count; cum_max; cum_min; cum_prod; cum_sum; kurtosis; log; log10; log1p; mode; n_unique; peak_max; peak_min; sin; sinh; skew; sqrt; tan; tanh; unique; unique_counts; len; max; mean; median; min; std; sum; var


3.b Các hàm cần tham số:

Here are the Polars functions along with their arguments:

- **pct_change**: Computes the percentage change between values.
  - `n` (int): Number of periods to shift for forming percent change. Default is 1. 

- **ewm_mean**: Computes the exponentially weighted moving average.
  - `com` (float, optional): Specify decay in terms of center of mass.
  - `span` (float, optional): Specify decay in terms of span.
  - `half_life` (float, optional): Specify decay in terms of half-life.
  - `alpha` (float, optional): Specify smoothing factor directly.
  - `adjust` (bool, default=True): If True, use the weights $w_i = (1 - \alpha)^i$; if False, calculate recursively.
  - `min_samples` (int, default=1): Minimum number of observations in window required to have a value (otherwise result is null).
  - `ignore_nulls` (bool, default=False): Ignore missing values when calculating weights.

- **ewm_mean_by**: Calculates time-based exponentially weighted moving average.
  - `by` (IntoExpr): Expression to group by.
  - `half_life` (str or timedelta): Specify decay in terms of half-life.

- **ewm_std**: Computes exponentially weighted moving standard deviation.
  - `com` (float, optional): Specify decay in terms of center of mass.
  - `span` (float, optional): Specify decay in terms of span.
  - `half_life` (float, optional): Specify decay in terms of half-life.
  - `alpha` (float, optional): Specify smoothing factor directly.
  - `adjust` (bool, default=True): If True, use the weights $w_i = (1 - \alpha)^i$; if False, calculate recursively.
  - `bias` (bool, default=False): If False, apply a correction to make the estimate unbiased.
  - `min_samples` (int, default=1): Minimum number of observations in window required to have a value (otherwise result is null).
  - `ignore_nulls` (bool, default=False): Ignore missing values when calculating weights.

- **ewm_var**: Computes exponentially weighted moving variance.
  - `com` (float, optional): Specify decay in terms of center of mass.
  - `span` (float, optional): Specify decay in terms of span.
  - `half_life` (float, optional): Specify decay in terms of half-life.
  - `alpha` (float, optional): Specify smoothing factor directly.
  - `adjust` (bool, default=True): If True, use the weights $w_i = (1 - \alpha)^i$; if False, calculate recursively.
  - `bias` (bool, default=False): If False, apply a correction to make the estimate unbiased.
  - `min_samples` (int, default=1): Minimum number of observations in window required to have a value (otherwise result is null).
  - `ignore_nulls` (bool, default=False): Ignore missing values when calculating weights.

- **hist**: Bins values into buckets and counts their occurrences.
  - `bins` (list of float, optional): Discretizations to make. If None, boundaries are determined based on the data.
  - `bin_count` (int, optional): Number of bins to create if `bins` is None.
  - `include_category` (bool, default=True): Whether to include the category column in the output.
  - `include_breakpoint` (bool, default=True): Whether to include the breakpoint column in the output.

- **rank**: Assigns ranks to data, dealing with ties appropriately.
  - `method` (str): Method used to assign ranks to tied elements. Options are 'average', 'min', 'max', 'dense', 'ordinal', 'random'. Default is 'average'.
  - `descending` (bool, default=False): If True, ranks are assigned in descending order.
  - `seed` (int, optional): Seed used for random number generator when `method` is 'random'.



3.c Các hàm bổ sung được viết thêm:


| func | args |
|------|------|
|iqr_mask| {whisker: 1.5}|
|z_score_mask| {threshold: 3.0}|
|confidence_interval| {confidence_level: 0.95}|
| range | [] | ->? Float (double)
| cvar | [] | ->? Float (double)
| pearson_index | [] | ->? Float type (double)

Tách các hàm cho visualize data: pearson_index, cvar, std, mean, ... lên graph
Phan chia thanh 2 nhom:
- More technical
- Simple







