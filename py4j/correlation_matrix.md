# Correlation Matrix.


Có hai cách tính toán correlation matrix (có đưa target vào; không đưa target vào)

## Không đưa target vào:

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "corrlation_matrix.arrow",
    "tasks": [
        {
            "task": "statistics",
            "action": "correlation_matrix",
            "kwargs": {
                "columns": None,
                "target": None,
				"feature_name": "features"
            },
        }
    ],
}
```

Kết quả output có dạng như sau:

| features | age       | balance   | day       | duration  | campaign  | pdays     | previous  | year      |
|--------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|-----------|
| age    | 1.000000  | 0.097783  | -0.009120 | -0.004648 | 0.004760  | -0.023758 | 0.001288  | -0.008022 |
| balance| 0.097783  | 1.000000  | 0.004503  | 0.021560  | -0.014578 | 0.003435  | 0.016674  | 0.031499  |
| day    | -0.009120 | 0.004503  | 1.000000  | -0.030206 | 0.162490  | -0.093044 | -0.051710 | -0.170216 |
| duration| -0.004648| 0.021560  | -0.030206 | 1.000000  | -0.084570 | -0.001565 | 0.001203  | 0.037030  |
| campaign| 0.004760 | -0.014578 | 0.162490  | -0.084570 | 1.000000  | -0.088628 | -0.032855 | -0.166185 |
| pdays   | -0.023758| 0.003435  | -0.093044 | -0.001565 | -0.088628 | 1.000000  | 0.454820  | 0.462736  |
| previous| 0.001288 | 0.016674  | -0.051710 | 0.001203  | -0.032855 | 0.454820  | 1.000000  | 0.293186  |
| year    | -0.008022| 0.031499  | -0.170216 | 0.037030  | -0.166185 | 0.462736  | 0.293186  | 1.000000  |




**Giải thích kết quả:**

- Khi dữ liệu trả về, cột tên các columns là `feature_name`, mục đích ở đây là để hiểu được ở vị trí vô (i,j) là mức độ tương quan (có mối liên hệ) giữa column[i] và column[j].
- Khi columns=None, nó sẽ tự tính toán tất cả các cột mà nó có thể tính được trong dữ liệu.
- Nếu cột target được đưa vào, nó chỉ tính hệ số tương quan của các cột đã cho và cột target.

Giải thích ý nghĩa: Hệ số Correlation xác định mối quan hệ xu thế biến đổi trong mối quan hệ tương quan của hai biến, hệ số tương quan nằm trong khoảng từ -1 đến 1. Hệ số này chỉ ra rằng liệu với dữ liệu đã cho thì cặp biến này có đi đến mối quan hệ đường thẳng y = ax + b hay không. Nếu hệ số tương quan dương có nghĩa biến này tăng thì biến kia cũng tăng và ngược lại, nếu hệ số tương quan âm thì biến này tăng biến kia sẽ giảm và ngược lại. Thường người ta quy ước nếu hệ số tương quan 0.8<= r <= 1 thì được gọi là tương quan thuận mạnh, tức hai biến này có mối quan hệ thuận mật thiết với nhau;  0.4<= r < 0.8 được xem là có tương quan thuận với nhau; 0.2<= r < 0.4 tương quan thuận yếu với nhau; -0.2 <r < 0.2 thì hai biến được xem như không tương quan với nhau. Tương tự cho phần hệ số âm là tương quan nghịch.

Khi đó =1 thì ta nói là tương quan thuận tuyệt đối, tức dữ liệu của hai biến đã nằm trên một đường thằng với hệ số dẫn đầu dương ( y = ax + b, a>0),  Khi đó =-1 thì ta nói là tương quan nghịch tuyệt đối, tức dữ liệu của hai biến đã nằm trên một đường thằng với hệ số dẫn đầu âm ( y = ax + b, a<0). 


