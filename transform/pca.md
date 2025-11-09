# Dimension Reduction


Giảm chiều dữ liệu, compact dữ liệu. Có 3 loại pca được đưa ra: pca_raw, pca_correlation, pca_covariance.

## PCA Raw

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
    "task": "reduction",
    "action": "pca_raw",
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
      "top_k": 3,
      "pc_labels": [
        "score1",
        "score2",
        "score3"
      ],
      "json_path": "/data/TitanProjects/tcrux/src/tests/resources/output.json"
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

Giải thích tham số:

- *columns*: List(String)Tên các cột cần tính toán PCA
- *top_k*: Int: Số thành phần PCA cần giữ lại.
- *pc_labels*: List(String) Tên các cột PCA sau khi tính toán
- *json_path*: String: Đường dẫn lưu file json để biết các score được tính toán theo các cột đầu vào thế nào.

Kết quả output có dạng như sau:

| CRIM | ZN | INDUS | CHAS | NOX | RM | AGE | DIS | RAD | TAX | PTRATIO | B | LSTAT | MEDV | score1 | score2 | score3 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0.25 | 0.00 | 10.59 | 0 | 0.49 | 5.78 | 72.70 | 4.35 | 4 | 277.00 | 18.60 | 389.43 | 18.06 | 22.50 | -466.42 | -127.70 | -24.95 |
| 0.54 | 0.00 | 6.20 | 0 | 0.50 | 5.98 | 68.10 | 3.67 | 8 | 307.00 | 17.40 | 378.35 | 11.65 | 24.30 | -481.86 | -100.52 | -17.19 |
| 0.03 | 0.00 | 2.18 | 0 | 0.46 | 7.00 | 45.80 | 6.06 | 3 | 222.00 | 18.70 | 394.63 | 2.94 | 33.40 | -423.39 | -167.23 | -7.73 |
| 2.33 | 0.00 | 19.58 | 0 | 0.87 | 5.19 | 93.80 | 1.53 | 5 | 403.00 | 14.70 | 356.99 | 28.32 | 17.80 | -546.26 | -22.86 | -29.77 |
| 28.66 | 0.00 | 18.10 | 0 | 0.60 | 5.16 | 100.00 | 1.59 | 24 | 666.00 | 20.20 | 210.97 | 20.08 | 16.30 | -659.31 | 256.04 | -6.73 |


Kết quả json biểu diễn của score theo các cột đầu vào:

```json
{
  "score1": {
    "CRIM": -0.007828890925998953,
    "ZN": -0.017421275249209146,
    "INDUS": -0.02111326865873321,
    "CHAS": -0.0001195658365624062,
    "NOX": -0.0009941415558105185,
    "RM": -0.010908712044404245,
    "AGE": -0.12463180268698903,
    "DIS": -0.0062575337333053635,
    "RAD": -0.019315843626551654,
    "TAX": -0.7706065923093399,
    "PTRATIO": -0.03259319423177573,
    "B": -0.6226433026194815,
    "LSTAT": -0.02329316225929451,
    "const": 0.0
  },
  "score2": {
    "CRIM": 0.029568732524283256,
    "ZN": -0.057323818466443015,
    "INDUS": 0.020037664966785492,
    "CHAS": -0.00013829926526873822,
    "NOX": -8.06698786656365e-06,
    "RM": -0.006826864416267249,
    "AGE": 0.028011345987495773,
    "DIS": -0.010439947342425976,
    "RAD": 0.03907359291234525,
    "TAX": 0.6232175175884274,
    "PTRATIO": -0.010772858097181811,
    "B": -0.7773337612284256,
    "LSTAT": 0.014396454170126522,
    "const": 0.0
  },
  "score3": {
    "CRIM": -0.010725078822033218,
    "ZN": 0.6043664611695015,
    "INDUS": -0.08733345890775722,
    "CHAS": -0.0009902710164107146,
    "NOX": -0.0020184783120294823,
    "RM": 0.0010227693765717698,
    "AGE": -0.776786825466353,
    "DIS": 0.0414420229191676,
    "RAD": 0.007567836782371164,
    "TAX": 0.10913735098077658,
    "PTRATIO": -0.018870969721387378,
    "B": 0.010557118918499307,
    "LSTAT": -0.09716947067810892,
    "const": 0.0
  }
}
```


**Giải thích kết quả:**

- Số cột pc được trả về phụ thuộc vào tham số top_k được đưa vào. Đây là vector các thành phần chính trong phân tích PCA.
- Chúng ta thay đổi `action` thành pca_correlation hoặc pca_covariance để chạy tương tự.
- Nếu dùng pca_correlation, kết quả pc thu được đã được biến đổi bằng cách chuẩn hóa dữ liệu ban đầu. Do đó kết quả thu về không phán ánh đơn vị đo từ dữ liệu gốc.

TODO: Tính score cho các cột liên quan đến các pc1, pc2, ect.

pca_explain (rotation matrix)
pc1 -> to which columns
pc2 -> ....

2 kiểu trả về:
- Trả về hết (default)
- Trả về đối tượng với threshold %
- Loai do thi mo ta ket qua PCA.
- Phan cap correlation < covariance < raw by units in docs.

	
## PCA Covariance


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
    "task": "reduction",
    "action": "pca_covariance",
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
      "top_k": 3,
      "pc_labels": [
        "score1",
        "score2",
        "score3"
      ],
      "json_path": "/data/TitanProjects/tcrux/src/tests/resources/output.json"
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

## PCA Correlation


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
    "task": "reduction",
    "action": "pca_correlation",
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
      "top_k": 3,
      "pc_labels": [
        "score1",
        "score2",
        "score3"
      ],
      "json_path": "/data/TitanProjects/tcrux/src/tests/resources/output.json"
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

Trong phần json output, nếu kết quả có dạng:

```json
'score1': {'CRIM': -0.029204025297661407, 'ZN': 0.011000909396546245, 'INDUS': -0.05058269424175899, 'CHAS': -0.019872215572386515, 'NOX': -2.9616714438055185, 'RM': 0.2696060689460453, 'AGE': -0.011154305778621712, 'DIS': 0.15285203050257756, 'RAD': -0.03676348934983933, 'TAX': -0.0020102640111909786, 'PTRATIO': -0.09475761326120753, 'B': 0.0022254644498652127, 'LSTAT': -0.04342020870332253, 'const': 3.354665880349922}
```

Nghĩa là

$$
score1 = CRIM * (-0.0292...) + ZN * (0.0110009...) + ... + LSTAT * (-0.0434...) + 3.35466
$$
