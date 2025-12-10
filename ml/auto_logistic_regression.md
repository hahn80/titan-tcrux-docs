# Automatic Logistic Regression


## Train model


Run the service with jobs:


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/loan_data.arrow",
      "use_pyarrow": true
    }
  },
  {
    "task": "regression",
    "action": "auto_logistic_train",
    "kwargs": {
      "target_column": "loan_status",
      "stratify_column": null,
      "model_saved_path": "/data/TitanProjects/tcrux/src/tests/resources/loan_log.bin",
      "numerical_columns": [
        "person_age",
        "person_income",
        "person_emp_exp",
        "loan_amnt",
        "loan_int_rate",
        "loan_percent_income",
        "cb_person_cred_hist_length",
        "credit_score"
      ],
      "categorical_columns": [
        "person_gender",
        "person_education",
        "person_home_ownership",
        "loan_intent",
        "previous_loan_defaults_on_file"
      ]
    }
  }
]
```

Giải thích tham số:

- *target_column*: String (tên cột để dự đoán sau này, dữ liệu kiểu category, co the co nhieu categories khac nhau: Yes, No, Undetermined,...)
- *stratify_column*: String (tên cột để phân chia dữ liệu cho cân bằng theo dữ liệu đã có của cột stratify_column, tham số này là optional và có thể bỏ qua)
- *model_saved_path*: String (path to save model)
- *numerical_columns*: List(String): List of numerical variables
- *categorical_columns*: List(String): List of categorical variables


Output result:

```json
{
  'selected_features': [
    'num__loan_amnt',
    'num__loan_int_rate',
    'num__loan_percent_income',
    'num__credit_score',
    'cat__person_home_ownership_OTHER',
    'cat__person_home_ownership_OWN',
    'cat__person_home_ownership_RENT',
    'cat__loan_intent_MEDICAL',
    'cat__loan_intent_PERSONAL',
    'cat__loan_intent_VENTURE'
  ],
  'auc': 0.16133214285714287,
  'classes': [
    0,
    1
  ],
  'classification_report': {
    '0': {
      'precision': 0.9289232934553132,
      'recall': 0.9428571428571428,
      'f1-score': 0.9358383551931939,
      'support': 7000.0
    },
    '1': {
      'precision': 0.7889182058047494,
      'recall': 0.7475,
      'f1-score': 0.7676508344030809,
      'support': 2000.0
    },
    'accuracy': 0.8994444444444445,
    'macro avg': {
      'precision': 0.8589207496300313,
      'recall': 0.8451785714285714,
      'f1-score': 0.8517445947981375,
      'support': 9000.0
    },
    'weighted avg': {
      'precision': 0.8978110517551879,
      'recall': 0.8994444444444445,
      'f1-score': 0.8984633505731688,
      'support': 9000.0
    }
  },
  'model_path': '/data/TitanProjects/tcrux/src/tests/resources/loan_log.bin'
}
```

**Explanation:**

- *selected_features*: The important columns after the automatic processing.
- *auc*: Area under the curve: the metric to measure the performance of the model.
	* 0.9 - 1.0: Outstanding.
	* 0.8 - 0.9: Excellent.
	* 0.7 - 0.8: Acceptable/Good.
- *classes*: List(String) The list of categories for the output.
- *classification_report*: The report to understand the performance of the model.



## Predict from the model.


Jobs to run with multitasking

```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/loan_data.arrow"
    }
  },
  {
    "task": "regression",
    "action": "auto_logistic_predict",
    "kwargs": {
      "model_saved_path": "/data/TitanProjects/tcrux/src/tests/resources/loan_log.bin",
      "target_column": "predicted"
    }
  }
]
```



Output result:

| person_age | person_gender | person_education | person_income | person_emp_exp | person_home_ownership | loan_amnt | loan_intent | loan_int_rate | loan_percent_income | cb_person_cred_hist_length | credit_score | previous_loan_defaults_on_file | loan_status | probability | predicted |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 24.00 | female | Bachelor | 82048.00 | 6 | MORTGAGE | 3000.00 | PERSONAL | 7.88 | 0.04 | 4.00 | 610 | No | 0 | 0.95 | 0 |
| 24.00 | male | Associate | 66074.00 | 0 | RENT | 16750.00 | PERSONAL | 20.00 | 0.25 | 2.00 | 660 | No | 1 | 0.96 | 1 |
| 21.00 | male | High School | 73125.00 | 0 | RENT | 10000.00 | EDUCATION | 12.18 | 0.14 | 2.00 | 655 | No | 0 | 0.62 | 0 |
| 27.00 | male | High School | 120997.00 | 5 | MORTGAGE | 16000.00 | MEDICAL | 6.54 | 0.13 | 5.00 | 655 | Yes | 0 | 1.00 | 0 |
| 23.00 | male | High School | 73001.00 | 3 | MORTGAGE | 25850.00 | EDUCATION | 7.90 | 0.35 | 4.00 | 621 | Yes | 0 | 1.00 | 0 |

**Explanation:** The output data contains the predicted labels and also their probability for predicting those labels.
