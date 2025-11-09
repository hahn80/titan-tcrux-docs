# Correlation Matrix.


Có hai cách tính toán correlation matrix (có đưa target vào; không đưa target vào)

## Không đưa target vào:

Tham số để gọi service:

```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/banking.arrow"
    }
  },
  {
    "task": "statistics",
    "action": "correlation_matrix",
    "kwargs": {
      "columns": null,
      "target": null,
      "feature_name": "features"
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

Kết quả output có dạng như sau:

| key | age | balance | day | duration | campaign | pdays | previous | year | features |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1.00 | 0.01 | 0.07 | -0.06 | 0.01 | -0.10 | 0.44 | 0.27 | 0.84 | key |
| 0.01 | 1.00 | 0.10 | -0.01 | -0.00 | 0.00 | -0.02 | 0.00 | -0.01 | age |
| 0.07 | 0.10 | 1.00 | 0.00 | 0.02 | -0.01 | 0.00 | 0.02 | 0.03 | balance |
| -0.06 | -0.01 | 0.00 | 1.00 | -0.03 | 0.16 | -0.09 | -0.05 | -0.17 | day |
| 0.01 | -0.00 | 0.02 | -0.03 | 1.00 | -0.08 | -0.00 | 0.00 | 0.04 | duration |
| -0.10 | 0.00 | -0.01 | 0.16 | -0.08 | 1.00 | -0.09 | -0.03 | -0.17 | campaign |
| 0.44 | -0.02 | 0.00 | -0.09 | -0.00 | -0.09 | 1.00 | 0.45 | 0.46 | pdays |
| 0.27 | 0.00 | 0.02 | -0.05 | 0.00 | -0.03 | 0.45 | 1.00 | 0.29 | previous |
| 0.84 | -0.01 | 0.03 | -0.17 | 0.04 | -0.17 | 0.46 | 0.29 | 1.00 | year |




**Giải thích kết quả:**

- Khi dữ liệu trả về, cột tên các columns là `feature_name`, mục đích ở đây là để hiểu được ở vị trí vô (i,j) là mức độ tương quan (có mối liên hệ) giữa column[i] và column[j].
- Khi columns=None, nó sẽ tự tính toán tất cả các cột mà nó có thể tính được trong dữ liệu.
- Nếu cột target được đưa vào, nó chỉ tính hệ số tương quan của các cột đã cho và cột target.

Giải thích ý nghĩa: Hệ số Correlation xác định mối quan hệ xu thế biến đổi trong mối quan hệ tương quan của hai biến, hệ số tương quan nằm trong khoảng từ -1 đến 1. Hệ số này chỉ ra rằng liệu với dữ liệu đã cho thì cặp biến này có đi đến mối quan hệ đường thẳng y = ax + b hay không. Nếu hệ số tương quan dương có nghĩa biến này tăng thì biến kia cũng tăng và ngược lại, nếu hệ số tương quan âm thì biến này tăng biến kia sẽ giảm và ngược lại. Thường người ta quy ước nếu hệ số tương quan 0.8<= r <= 1 thì được gọi là tương quan thuận mạnh, tức hai biến này có mối quan hệ thuận mật thiết với nhau;  0.4<= r < 0.8 được xem là có tương quan thuận với nhau; 0.2<= r < 0.4 tương quan thuận yếu với nhau; -0.2 <r < 0.2 thì hai biến được xem như không tương quan với nhau. Tương tự cho phần hệ số âm là tương quan nghịch.

Khi đó =1 thì ta nói là tương quan thuận tuyệt đối, tức dữ liệu của hai biến đã nằm trên một đường thằng với hệ số dẫn đầu dương ( y = ax + b, a>0),  Khi đó =-1 thì ta nói là tương quan nghịch tuyệt đối, tức dữ liệu của hai biến đã nằm trên một đường thằng với hệ số dẫn đầu âm ( y = ax + b, a<0).


## Target as a list of strings


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/banking.arrow"
    }
  },
  {
    "task": "statistics",
    "action": "correlation_matrix",
    "kwargs": {
      "columns": null,
      "target": [
        "balance",
        "pdays"
      ],
      "feature_name": "features"
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

| key | age | balance | day | duration | campaign | pdays | previous | year | features |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0.07 | 0.10 | 1.00 | 0.00 | 0.02 | -0.01 | 0.00 | 0.02 | 0.03 | balance |
| 0.44 | -0.02 | 0.00 | -0.09 | -0.00 | -0.09 | 1.00 | 0.45 | 0.46 | pdays |


