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
    "task": "statistics",
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
| Upper Coefficient | None | None | None | None | None | 4.71 | None | -0.41 | None | None | -0.84 | None | -0.61 |
