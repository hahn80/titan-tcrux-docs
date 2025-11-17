# Correlation Services


There are 5 levels of correlation in this service:

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



## 1. Basic service



```JSON
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/boston.arrow"
    }
  },
  {
    "task": "statistics",
    "action": "basic_corr",
    "kwargs": {
      "columns": [
        "INDUS",
        "RM",
        "AGE",
        "DIS",
        "PTRATIO",
        "LSTAT"
      ],
      "target": "MEDV"
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

**Chú thích:**

- *columns*: List(String): List of input columns
- *target*: String: Name of the target column


Ouput results:

| variable | r_value | statistic | p_value | ranking |
| --- | --- | --- | --- | --- |
| INDUS | -0.48 | 153.95 | 0.00 | D |
| RM | 0.70 | 471.85 | 0.00 | A |
| AGE | -0.38 | 83.48 | 0.00 | D |
| DIS | 0.25 | 33.58 | 0.00 | B |
| PTRATIO | -0.51 | 175.11 | 0.00 | F |
| LSTAT | -0.74 | 601.62 | 0.00 | F |


Các correlation được phân chia theo *ranking*, nếu `rank: A` nghĩa là nó cao nhất, tiếp theo là *B,C,D,F*. Giống như cho điểm học sinh vậy nhé. Điểm A là nhất, còn F thì lo thi lại.
Các hệ số của `data` cho ta biết mức độ tương quan của các biến. Nó giao động từ 0 đến 1.

- Rank A: Tác động tích cực mạnh: Hệ số từ 0.5 đến 1.0.
- Rank B: Tác động tích cực: Hệ số từ 0.2 đến 0.5.
- Rank C: Không tác động: Hệ số từ -0.2 đến 0.2
- Rank D: Tác động xấu: Hệ số từ -0.5 đến -0.2
- Rank F: Tác động cực xấu: Hệ số từ -1.0 đến -.05.


## 2. Low Intermediate service (intermediate1)


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
    "task": "statistics",
    "action": "percentage_corr",
    "kwargs": {
      "columns": [
        "INDUS",
        "RM",
        "AGE",
        "DIS",
        "PTRATIO",
        "LSTAT"
      ],
      "target": "MEDV",
      "top_pct": 0.1,
      "bottom_pct": 0.1,
      "min_abs_score": 0.2
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

**Chú thích:**

- *top_pct*: Float (0->1): For example, *top_pct=0.1*: Take 10% of the highest correlation values
- *bottom_pct*: Float (0->1): For example, *bottom_pct=0.2*: Take 10% of the lowest correlation values.
- It is requirement that *top_pct + bottom_pct < 1.0*.
- min_abs_score: Float. If the correlation scores are really small near zero, for example correlations = [0.01, 0.02, 0.03, 0.04], we have to set *min_abs_score=0.2* to filter the case we take top 10%, but score must be strong enough.

**Kết quả:**


| variable | r_value | statistic | p_value | ranking |
| --- | --- | --- | --- | --- |
| INDUS | -0.48 | 153.95 | 0.00 | C |
| RM | 0.70 | 471.85 | 0.00 | C |
| AGE | -0.38 | 83.48 | 0.00 | C |
| DIS | 0.25 | 33.58 | 0.00 | C |
| PTRATIO | -0.51 | 175.11 | 0.00 | C |
| LSTAT | -0.74 | 601.62 | 0.00 | F |


- Rank A: Tác động mạnh
- Rank F: Tác động rất xấu.
- Rank C: Không có tác động hoặc tác động không đáng kể.


## 3. High Intermediate service (intermediate2)


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
    "task": "statistics",
    "action": "basic_corr",
    "kwargs": {
      "columns": [
        "INDUS",
        "RM",
        "AGE",
        "DIS",
        "PTRATIO",
        "LSTAT"
      ],
      "target": "MEDV",
      "lags": [
        1,
        2,
        3,
        4,
        5,
        6
      ]
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

**Chú thích:**

- `lags`: là list các độ dài để lùi cho các biến đầu vào trong `columns`. Kiểu dữ liệu là int.
- Độ dài của `lags` phải bằng độ dài của `columns`. Nếu không thì nó sẽ báo lỗi.

**Kết quả:**

| r_value | statistic | p_value | variable | shift | ranking |
| --- | --- | --- | --- | --- | --- |
| -0.47 | 139.92 | 0.00 | INDUS | 1 | D |
| 0.33 | 62.68 | 0.00 | RM | 2 | B |
| -0.32 | 56.34 | 0.00 | AGE | 3 | D |
| 0.26 | 37.60 | 0.00 | DIS | 4 | B |
| -0.44 | 118.53 | 0.00 | PTRATIO | 5 | D |
| -0.33 | 59.21 | 0.00 | LSTAT | 6 | D |

Trường thông tin *shift* chỉ ra độ lùi của các biến tương ứng. Nếu dữ liệu đưa vào có frequency là giờ (H) thì nó lùi 5 giờ, 6 giờ, 7 giờ.
Các số trong *shift* là thông tin người dùng đưa vào. Dịch vụ *intermediate2* không tìm độ lùi tối ưu cho khách hàng.

- Rank A: Tác động tích cực mạnh: Hệ số từ 0.5 đến 1.0.
- Rank B: Tác động tích cực: Hệ số từ 0.2 đến 0.5.
- Rank C: Không tác động: Hệ số từ -0.2 đến 0.2
- Rank D: Tác động xấu: Hệ số từ -0.5 đến -0.2
- Rank F: Tác động cực xấu: Hệ số từ -1.0 đến -.05.


## 4. Low Advance service (advance1)


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
    "task": "statistics",
    "action": "percentage_corr",
    "kwargs": {
      "columns": [
        "INDUS",
        "RM",
        "AGE",
        "DIS",
        "PTRATIO",
        "LSTAT"
      ],
      "target": "MEDV",
      "lags": [
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "top_pct": 0.1,
      "bottom_pct": 0.1,
      "min_abs_score": 0.2
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
**Chú thích:**

- `lags`: là list các độ dài để lùi cho các biến đầu vào trong `columns`. Kiểu dữ liệu là int.
- Độ dài của `lags` phải bằng độ dài của `columns`. Nếu không thì nó sẽ báo lỗi.


**Kết quả:**

| variable | r_value | statistic | p_value | shift | ranking |
| --- | --- | --- | --- | --- | --- |
| INDUS | -0.47 | 139.92 | 0.00 | 1 | F |
| RM | 0.33 | 62.68 | 0.00 | 2 | C |
| AGE | -0.32 | 56.34 | 0.00 | 3 | C |
| DIS | 0.26 | 37.60 | 0.00 | 4 | C |
| PTRATIO | -0.44 | 118.53 | 0.00 | 5 | C |
| LSTAT | -0.33 | 59.21 | 0.00 | 6 | C |


Trường thông tin *shift* chỉ ra độ lùi của các biến tương ứng. Nếu dữ liệu đưa vào có frequency là giờ (H) thì nó lùi 5 giờ, 6 giờ, 7 giờ.
Các số trong *shift* là thông tin người dùng đưa vào. Dịch vụ *advance1* không tìm độ lùi tối ưu cho khách hàng.

- Rank A: Tác động mạnh
- Rank F: Tác động rất xấu.
- Rank C: Không có tác động hoặc tác động không đáng kể.


## 5. High Advance service (advance2)

Automatically determine the best shift to obtain the higher correlation.

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
    "task": "statistics",
    "action": "advance_corr",
    "kwargs": {
      "columns": [
        "INDUS",
        "RM",
        "AGE",
        "DIS",
        "PTRATIO",
        "LSTAT"
      ],
      "target": "MEDV",
      "max_lags": [
        4,
        5,
        6,
        4,
        7,
        6
      ]
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

**Chú thích:**

- `max_lags`: là list các độ dài (max) để lùi cho các biến đầu vào trong `columns`. Kiểu dữ liệu là int.
- Độ dài của `max_lags` phải bằng độ dài của `columns`. Nếu không thì nó sẽ báo lỗi.
- Thuật toán sẽ tìm ra độ lùi tối ưu để có hệ số correlation cao nhất.

**Kết quả:**

| variable | r_value | statistic | p_value | ranking | shift |
| --- | --- | --- | --- | --- | --- |
| INDUS | -0.40 | 96.76 | 0.00 | D | 4 |
| RM | 0.70 | 467.57 | 0.00 | A | 0 |
| AGE | -0.24 | 30.61 | 0.00 | D | 5 |
| DIS | 0.26 | 37.34 | 0.00 | B | 4 |
| PTRATIO | -0.40 | 96.25 | 0.00 | D | 7 |
| LSTAT | -0.33 | 59.49 | 0.00 | D | 6 |


Thuật toán *advance2* sẽ tìm ra độ lùi tối ưu cho các biến tương ứng. Với kết quả ở trên, thuật toán chỉ cho người dùng thấy rằng để có hệ số tương quan cao nhất, độ lùi của các biến.

- Rank A: Tác động tích cực mạnh: Hệ số từ 0.5 đến 1.0.
- Rank B: Tác động tích cực: Hệ số từ 0.2 đến 0.5.
- Rank C: Không tác động: Hệ số từ -0.2 đến 0.2
- Rank D: Tác động xấu: Hệ số từ -0.5 đến -0.2
- Rank F: Tác động cực xấu: Hệ số từ -1.0 đến -.05.
