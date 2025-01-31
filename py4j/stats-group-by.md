# Các hàm thống kê theo nhóm.


Group và tính toán thống kê sẽ tiết kiệm được thời gian và quá trình chạy.

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

Group by hỗ trợ tất cả các hàm sau:

| func | args |
|------|------|
| abs | [] |
| arccos | [] |
| arccosh | [] |
| arcsin | [] |
| arcsinh | [] |
| arctan | [] |
| arctanh | [] |
| arg_unique | [] |
| cbrt | [] |
| cos | [] |
| cosh | [] |
| cot | [] |
| cum_count | [] |
| cum_max | [] |
| cum_min | [] |
| cum_prod | [] |
| cum_sum | [] |
| ewm_mean | [com, span, half_life] |
| ewm_mean_by | [by, half_life] |
| ewm_std | [com, span, half_life] |
| ewm_var | [com, span, half_life] |
| hist | [bins, bin_count] |
| kurtosis | [] |
| log | [base] |
| log10 | [] |
| log1p | [] |
| mode | [] |
| n_unique | [] |
| pct_change | [n] |
| peak_max | [] |
| peak_min | [] |
| rank | [method, descending, seed] |
| rolling_map | [function, window_size] |
| rolling_max | [window_size, weights] |
| rolling_max_by | [by, window_size] |
| rolling_mean | [window_size, weights] |
| rolling_mean_by | [by, window_size] |
| rolling_median | [window_size, weights] |
| rolling_median_by | [by, window_size] |
| rolling_min | [window_size, weights] |
| rolling_min_by | [by, window_size] |
| rolling_quantile | [quantile] |
| rolling_quantile_by | [by, window_size] |
| rolling_skew | [window_size, bias] |
| rolling_std | [window_size, weights] |
| rolling_std_by | [by, window_size] |
| rolling_sum | [window_size, weights] |
| rolling_sum_by | [by, window_size] |
| rolling_var | [window_size, weights] |
| rolling_var_by | [by, window_size] |
| search_sorted | [element, side] |
| sin | [] |
| sinh | [] |
| skew | [] |
| sqrt | [] |
| tan | [] |
| tanh | [] |
| unique | [] |
| unique_counts | [] |
| len | [] |
| max | [] |
| mean | [] |
| median | [] |
| min | [] |
| n_unique | [] |
| quantile | [quantile, interpolation] |
| std | [] |
| sum | [] |
| var | [] |


Các hàm bổ sung được viết thêm:


| func | args |
|------|------|
|iqr_mask| {whisker: 1.5}|
|z_score_mask| {threshold: 3.0}|
|confidence_interval| {confidence_level: 0.95}|
| range | [] |
| cvar | [] |
| pearson_index | [] |







