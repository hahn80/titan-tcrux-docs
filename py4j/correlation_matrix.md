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
            },
        }
    ],
}
```

Kết quả output có dạng như sau:

|        | age       | balance   | day       | duration  | campaign  | pdays     | previous  | year      |
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

- Khi dữ liệu trả về, cột tên các columns không có, nhưng mục đích ở đây là để hiểu được ở vị trí vô (i,j) là mức độ tương quan (có mối liên hệ) giữa column[i] và column[j].
- Khi columns=None, nó sẽ tự tính toán tất cả các cột mà nó có thể tính được trong dữ liệu.
- Nếu cột target được đưa vào, nó chỉ tính hệ số tương quan của các cột đã cho và cột target.
