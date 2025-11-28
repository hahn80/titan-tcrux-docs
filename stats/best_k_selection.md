# Select Best K Features:


This service is used to determine the best k number of features before running dominance.


```json
  {
    "task": "statistics",
    "action": "select_best_k",
    "kwargs": {
      "target": "balance",
      "columns": [
        "age",
        "default",
        "balance",
        "housing",
        "loan",
        "day",
        "campaign",
        "pdays",
        "previous",
        "job_admin.",
        "job_blue-collar",
        "job_entrepreneur",
        "job_housemaid",
        "job_management",
        "job_retired",
        "job_self-employed",
        "job_services",
        "job_student",
        "job_technician",
        "job_unemployed",
        "job_unknown",
        "marital_divorced",
        "marital_married",
        "marital_single",
        "education_primary",
        "education_secondary",
        "education_tertiary",
        "education_unknown"
      ],
      "top_k": 10
    }
  }
```

Parameters:

- *target*: String: The target column to compute the correlation with
- *columns*: List(String): List of input columns to filter out the best scores.
- *top_k*: Int: The number of features to keep for the best score.


Output:

| variable | r_value | statistic | p_value | selected |
| --- | --- | --- | --- | --- |
| age | 0.11 | 142.54 | 0.00 | True |
| default | -0.06 | 41.62 | 0.00 | True |
| balance | 1.00 | 2792231768969696256.00 | 0.00 | True |
| housing | -0.08 | 66.72 | 0.00 | True |
| loan | -0.08 | 80.43 | 0.00 | True |
| day | 0.01 | 1.22 | 0.27 | False |
| campaign | -0.01 | 2.15 | 0.14 | False |
| pdays | 0.02 | 3.38 | 0.07 | False |
| previous | 0.03 | 10.60 | 0.00 | False |
| job_admin. | -0.04 | 16.14 | 0.00 | False |
| job_blue-collar | -0.05 | 23.89 | 0.00 | True |
| job_entrepreneur | 0.01 | 0.28 | 0.59 | False |
| job_housemaid | -0.01 | 0.71 | 0.40 | False |
| job_management | 0.04 | 22.56 | 0.00 | True |
| job_retired | 0.08 | 63.85 | 0.00 | True |
| job_self-employed | 0.02 | 4.58 | 0.03 | False |
| job_services | -0.04 | 19.39 | 0.00 | False |
| job_student | -0.00 | 0.03 | 0.87 | False |
| job_technician | 0.00 | 0.16 | 0.69 | False |
| job_unemployed | -0.01 | 1.62 | 0.20 | False |
| job_unknown | 0.01 | 1.18 | 0.28 | False |
| marital_divorced | -0.02 | 3.45 | 0.06 | False |
| marital_married | 0.03 | 7.22 | 0.01 | False |
| marital_single | -0.01 | 2.51 | 0.11 | False |
| education_primary | -0.00 | 0.01 | 0.94 | False |
| education_secondary | -0.07 | 55.92 | 0.00 | True |
| education_tertiary | 0.07 | 53.59 | 0.00 | True |
| education_unknown | 0.01 | 2.38 | 0.12 | False |

