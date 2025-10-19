# Logistic Regression


## Train model


Run the service to train the logistic model for banking problem.

The output will be a Hashmap with score, recall, precision, f1 scores, and saved_model

Input data:

| age | job | marital | education | default | balance | housing | loan | contact | day | month | duration | campaign | pdays | previous | poutcome | deposit |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 28 | technician | single | secondary | no | 250 | no | yes | cellular | 29 | jan | 133 | 1 | -1 | 0 | unknown | no |
| 27 | unknown | single | secondary | no | 1187 | no | no | telephone | 26 | feb | 232 | 1 | 101 | 1 | failure | yes |
| 34 | blue-collar | married | primary | no | 3527 | yes | no | cellular | 21 | nov | 1022 | 1 | -1 | 0 | unknown | yes |
| 60 | retired | divorced | tertiary | no | 2166 | no | no | unknown | 20 | jun | 8 | 9 | -1 | 0 | unknown | no |
| 51 | technician | divorced | secondary | no | 2323 | yes | yes | cellular | 18 | aug | 151 | 10 | -1 | 0 | unknown | no |
| 36 | management | single | tertiary | no | 243 | yes | no | cellular | 7 | may | 160 | 2 | 360 | 3 | failure | no |
| 48 | blue-collar | single | secondary | no | 8982 | yes | no | unknown | 14 | may | 628 | 1 | -1 | 0 | unknown | no |
| 38 | technician | single | secondary | no | 565 | yes | no | cellular | 30 | apr | 1143 | 1 | -1 | 0 | unknown | yes |
| 31 | management | married | tertiary | no | 1384 | yes | no | cellular | 30 | jan | 203 | 2 | 2 | 3 | other | no |
| 23 | technician | single | secondary | no | 356 | yes | no | cellular | 18 | may | 224 | 2 | 356 | 2 | failure | no |

Trong du lieu nay, chung ta co ca bien dinh tinh numeric va bien dinh danh categorical.

Tham so de chay chuong trinh:


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "logistic.arrow",
      "use_pyarrow": true
    }
  },
  {
    "task": "grpc",
    "action": "logistic_regression",
    "kwargs": {
      "target_column": "deposit",
      "stratify_column": "loan",
      "model_saved_path": "logistic_model.pkl",
      "processors": {
        "numerical_columns": [
          "age",
          "balance",
          "day",
          "campaign",
          "pdays",
          "previous",
          "duration"
        ],
        "categorical_columns": [
          "job",
          "marital",
          "education",
          "default",
          "housing",
          "loan"
        ]
      }
    }
  }
]
```

Trong gói tcrux, chúng ta cần load data trước khi thực hiện các tác vụ tiếp theo. Chúng ta cũng có thể save kết quả thành arrow file.

Giải thích tham số:

Bước đầu tiên là load_dataframe với `input_arrow`.
Sau khi load, data sẽ tự động được pass cho job tiếp theo.

- *target_column*: String (tên cột để dự đoán sau này, dữ liệu kiểu category, co the co nhieu categories khac nhau: Yes, No, Undetermined,...)
- *stratify_column*: String (tên cột để phân chia dữ liệu cho cân bằng theo dữ liệu đã có của cột stratify_column, tham số này là optional và có thể bỏ qua)
- *model_saved_path*: String (path to save model)
- *processors*: Là một Hashmap phải chứa một trong hai keys sau: `numerical_columns` hoặc `categorical_columns`. Values cho các key này là list các tên cột có datatypes numerical hoặc category.


Ket qua output:

{'accuracy_score': 0.7877295118674429, 'precision_score': 0.7882175522450627, 'recall_score': 0.7877295118674429, 'f1_score': 0.78723746945414, 'model_saved_path': '//logistic_model.pkl'}


## Predict from the model.


Jobs to run with multitasking

```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "logistic_test.arrow"
    }
  },
  {
    "task": "grpc",
    "action": "predict",
    "kwargs": {
      "model_saved_path": "logistic_model.pkl",
      "target_column": "predicted_deposit"
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


Input data:

| age | job | marital | education | default | balance | housing | loan | contact | day | month | duration | campaign | pdays | previous | poutcome |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 34 | blue-collar | divorced | secondary | no | 0 | yes | no | cellular | 5 | may | 150 | 1 | -1 | 0 | unknown |
| 36 | blue-collar | married | secondary | no | 29 | yes | yes | cellular | 31 | jul | 345 | 1 | -1 | 0 | unknown |
| 31 | blue-collar | married | secondary | no | 435 | yes | no | unknown | 21 | may | 204 | 1 | -1 | 0 | unknown |
| 36 | blue-collar | divorced | secondary | no | 7051 | no | yes | cellular | 15 | jul | 106 | 13 | -1 | 0 | unknown |
| 31 | admin. | married | secondary | no | 8781 | yes | no | cellular | 14 | may | 680 | 1 | 177 | 1 | success |
| 66 | housemaid | married | primary | no | 40 | no | no | telephone | 14 | oct | 290 | 2 | -1 | 0 | unknown |
| 26 | blue-collar | married | secondary | no | 557 | yes | yes | unknown | 27 | may | 282 | 2 | -1 | 0 | unknown |
| 39 | services | married | secondary | no | 20928 | no | no | cellular | 14 | may | 1166 | 1 | 352 | 1 | other |
| 25 | blue-collar | married | secondary | no | 101 | no | yes | cellular | 8 | jul | 460 | 3 | -1 | 0 | unknown |
| 53 | blue-collar | married | primary | no | 2295 | no | no | cellular | 21 | aug | 65 | 10 | -1 | 0 | unknown |


Now we can predict the output:

| age | job | marital | education | default | balance | housing | loan | contact | day | month | duration | campaign | pdays | previous | poutcome | predicted_deposit |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 34 | blue-collar | divorced | secondary | no | 0 | yes | no | cellular | 5 | may | 150 | 1 | -1 | 0 | unknown | no |
| 36 | blue-collar | married | secondary | no | 29 | yes | yes | cellular | 31 | jul | 345 | 1 | -1 | 0 | unknown | no |
| 31 | blue-collar | married | secondary | no | 435 | yes | no | unknown | 21 | may | 204 | 1 | -1 | 0 | unknown | no |
| 36 | blue-collar | divorced | secondary | no | 7051 | no | yes | cellular | 15 | jul | 106 | 13 | -1 | 0 | unknown | no |
| 31 | admin. | married | secondary | no | 8781 | yes | no | cellular | 14 | may | 680 | 1 | 177 | 1 | success | yes |
| 66 | housemaid | married | primary | no | 40 | no | no | telephone | 14 | oct | 290 | 2 | -1 | 0 | unknown | no |
| 26 | blue-collar | married | secondary | no | 557 | yes | yes | unknown | 27 | may | 282 | 2 | -1 | 0 | unknown | no |
| 39 | services | married | secondary | no | 20928 | no | no | cellular | 14 | may | 1166 | 1 | 352 | 1 | other | yes |
| 25 | blue-collar | married | secondary | no | 101 | no | yes | cellular | 8 | jul | 460 | 3 | -1 | 0 | unknown | no |
| 53 | blue-collar | married | primary | no | 2295 | no | no | cellular | 21 | aug | 65 | 10 | -1 | 0 | unknown | no |
