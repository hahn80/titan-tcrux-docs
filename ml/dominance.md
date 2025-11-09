# Dominance Analysis


Xét dữ liệu giá nhà ở Boston:

Variables in order:

- CRIM     per capita crime rate by town
- ZN       proportion of residential land zoned for lots over 25,000 sq.ft.
- INDUS    proportion of non-retail business acres per town
- CHAS     Charles River dummy variable (= 1 if tract bounds river; 0 otherwise)
- NOX      nitric oxides concentration (parts per 10 million)
- RM       average number of rooms per dwelling
- AGE      proportion of owner-occupied units built prior to 1940
- DIS      weighted distances to five Boston employment centres
- RAD      index of accessibility to radial highways
- TAX      full-value property-tax rate per $10,000
- PTRATIO  pupil-teacher ratio by town
- B        1000(Bk - 0.63)^2 where Bk is the proportion of blacks by town
- LSTAT    % lower status of the population
- MEDV     Median value of owner-occupied homes in $1000's


## 1. Level a: Dominance without shifts

Jobs to run dominance:

```json
jobs = [
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "boston.arrow"
    }
  },
  {
    "task": "regression",
    "action": "dominance",
    "kwargs": {
      "columns": [
        "CRIM",
        "ZN",
        "INDUS",
        "CHAS",
        "NOX",
        "RM",
        "AGE",
        "DIS",
        "RAD",
        "TAX",
        "PTRATIO",
        "B",
        "LSTAT"
      ],
      "target": "MEDV",
      "impact_percentage": [
        5,
        40
      ],
      "confidence_level": 0.75,
      "keep_constant": false
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

Giải thích tham số:

- *impact_percentage*: VD: 5, 40: Nghĩa là chia `Importance Percentage` thành các khoảng 0, <5% là tác động yếu nên bỏ qua. Từ 5% đến <40% là tác động đáng kể, và từ 40% trở lên 100% là cao.

- *confidence_level*: Mức độ tin cậy để tính hệ số tác động trên mỗi biến. Chú ý những biến nào tác động dưới 5% như ở trên sẽ bị loại bỏ, chỉ xét các biến có tác động cao hơn 5% cho quá trình xác định hệ số `Impact Coefficient`, `Lower Coefficient` và `Upper Coefficient`.

Output results:

| __measures__ | CRIM | ZN | INDUS | CHAS | NOX | RM | AGE | DIS | RAD | TAX | PTRATIO | B | LSTAT |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Individual Dominance | 0.15 | 0.13 | 0.23 | 0.03 | 0.18 | 0.48 | 0.14 | 0.06 | 0.15 | 0.22 | 0.26 | 0.11 | 0.54 |
| Partial Dominance | 0.01 | 0.01 | 0.01 | 0.01 | 0.01 | 0.16 | 0.01 | 0.03 | 0.01 | 0.01 | 0.06 | 0.01 | 0.17 |
| Interactional Dominance | 0.01 | 0.01 | 0.00 | 0.01 | 0.01 | 0.04 | 0.00 | 0.03 | 0.01 | 0.01 | 0.03 | 0.01 | 0.06 |
| Total Dominance | 0.01 | 0.01 | 0.01 | 0.01 | 0.01 | 0.16 | 0.01 | 0.03 | 0.01 | 0.01 | 0.06 | 0.01 | 0.17 |
| Importance Percentage | 2.26 | 2.50 | 2.35 | 2.73 | 2.80 | 30.69 | 1.66 | 5.91 | 1.63 | 2.49 | 10.91 | 2.36 | 31.72 |
| Impact Coefficient | None | None | None | None | None | 4.22 | None | -0.55 | None | None | -0.97 | None | -0.67 |
| Lower Coefficient | None | None | None | None | None | 3.74 | None | -0.70 | None | None | -1.11 | None | -0.72 |
| Upper Coefficient | None | None | None | None | None | 4.71 | None | -0.41 | None | None | -0.84 | None | -0.61 |**

Giải thích ý nghĩa:

(1) Importance Percentage: Mức ảnh hưởng của biến lên biến mục tiêu.

(2) Impact Coefficent: Hệ số tác động của biến lên biến mục tiêu. None có nghĩa là khi giữ nguyên các biến khác nếu tăng biến này lên 1 đơn vị thì biến mục tiêu cơ bản không thay đổi, hệ số dương thể hiện tác động tăng và hệ số âm thể hiện tác động ngược. VD: Khi các biến khác không đổi, RM tăng lên 1 đơn vị thì biến mục tiêu MEDV tăng 4.22 đơn vị; khi các biến khác không đổi nếu PTRATIO tăng lên 1 đơn vị thì biến mục tiêu MEDV giảm 0.97 đơn vị.

(3) Importance Percentage: Thể hiện mức độ ảnh hưởng của mỗi biến tổng thể lên mô hình làm ảnh hưởng đến biến mục tiêu. VD: Biến RM đóng góp 30.69% - theo chiều thuận (vì Impact Coefficient tương ứng dương) lên biến mục tiêu trong mô hình, Biến PTRATIO đóng góp 10.91% - theo chiều nghịch (vì Impact Coefficient tương ứng âm) lên biến mục tiêu trong mô hình.

(4) Lower Coefficient - Upper Coefficient: Thể hiện khoảng biến động của biến mục tiêu với độ chính xác 75% khi giữ nguyên các biến khác và chỉ biến đó tăng lên 1 đơn vị. VD: Nếu giữ nguyên các biến khác mà tăng biến RM lên 1 đơn vị thì biến mục tiêu PTATOO tăng tối thiểu 3.74 đơn vị và tăng tối đa 4.71 đơn vị; Nếu giữ nguyên các biến khác mà tăng biến PTRATIO lên 1 đơn vị thì biến mục tiêu PTATOO giảm tối thiểu 0.84 đơn vị và giảm tối đa 1.11 đơn vị


## 2. Level a: Dominance with **shifts**


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/boston.arrow"
    }
  },
  {
    "task": "regression",
    "action": "dominance",
    "kwargs": {
      "columns": [
        "CRIM",
        "ZN",
        "INDUS",
        "CHAS",
        "NOX",
        "RM",
        "AGE",
        "DIS",
        "RAD",
        "TAX",
        "PTRATIO",
        "B",
        "LSTAT"
      ],
      "target": "MEDV",
      "impact_percentage": [
        5,
        40
      ],
      "confidence_level": 0.75,
      "keep_constant": false,
      "shifts": [
        1,
        2,
        3,
        4,
        5,
        6,
        7,
        8,
        9,
        10,
        11,
        12,
        13
      ]
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "/data/TitanProjects/tcrux/src/tests/resources/output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```


*shifts*: the *shift* units for each columns (integer) and must have same length as *columns*.

For example, if 

"columns": [ "CRIM", "ZN", "INDUS", "CHAS", "NOX", "RM", "AGE", "DIS", "RAD", "TAX", "PTRATIO", "B", "LSTAT" ]

and

"shifts": [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13 ]

It means: shift CRIM column 1 row forward, ZN 2 rows forward and so on.
We can also shift backward with negative numbers.



Output result:

| __measures__ | CRIM | ZN | INDUS | CHAS | NOX | RM | AGE | DIS | RAD | TAX | PTRATIO | B | LSTAT |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Individual Dominance | 0.14 | 0.09 | 0.19 | 0.02 | 0.11 | 0.08 | 0.07 | 0.07 | 0.17 | 0.19 | 0.17 | 0.06 | 0.11 |
| Partial Dominance | 0.03 | 0.01 | 0.03 | 0.01 | 0.00 | 0.01 | 0.00 | 0.00 | 0.01 | 0.02 | 0.04 | 0.00 | 0.01 |
| Interactional Dominance | 0.02 | 0.00 | 0.02 | 0.01 | 0.00 | 0.01 | 0.00 | 0.00 | 0.00 | 0.00 | 0.02 | 0.00 | 0.00 |
| Total Dominance | 0.03 | 0.01 | 0.03 | 0.01 | 0.00 | 0.01 | 0.00 | 0.00 | 0.01 | 0.02 | 0.04 | 0.00 | 0.01 |
| Importance Percentage | 14.27 | 6.43 | 16.26 | 8.23 | 1.59 | 8.30 | 0.56 | 1.12 | 4.57 | 8.96 | 22.74 | 2.67 | 4.32 |
| Impact Coefficient | -0.18 | 0.03 | -0.26 | 4.27 | None | 1.20 | None | None | None | -0.00 | -0.82 | None | None |
| Lower Coefficient | -0.23 | 0.01 | -0.34 | 2.68 | None | 0.58 | None | None | None | -0.01 | -1.04 | None | None |
| Upper Coefficient | -0.12 | 0.05 | -0.17 | 5.86 | None | 1.82 | None | None | None | 0.00 | -0.60 | None | None |
