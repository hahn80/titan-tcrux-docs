# Variance Inflation Factor (VIF)

The Variance Inflation Factor (VIF) measures how much the variance of a regression coefficient is inflated due to multicollinearity with other predictor variables.


```json
  {
    "task": "statistics",
    "action": "vif_score",
    "kwargs": {
      "columns": [
        "Major Depressive",
        "Dysthymia",
        "GAD",
        "Social Phobia",
        "Simple Phobia",
        "Agoraphobia",
        "Panic",
        "Alcohol Dep",
        "Drug Dep",
        "ASPD"
      ]
    }
  },
```


Output:

| variable | vif |
| --- | --- |
| Major Depressive | 5.92 |
| Dysthymia | 5.79 |
| GAD | 5.64 |
| Social Phobia | 5.55 |
| Simple Phobia | 5.92 |
| Agoraphobia | 5.79 |
| Panic | 5.40 |
| Alcohol Dep | 5.67 |
| Drug Dep | 5.87 |
| ASPD | 5.14 |


## Interpret the VIF values in your table:

- VIF = 1 → No multicollinearity
- VIF > 5 → Moderate to high multicollinearity (many researchers start to worry here)
- VIF > 10 → Severe multicollinearity (almost always considered problematic)

| Variable            | VIF   | Interpretation                              |
|---------------------|-------|------------------------------------------------|
| Major Depressive    | 5.92  | Moderate-high multicollinearity             |
| Dysthymia           | 5.79  | Moderate-high multicollinearity             |
| GAD                 | 5.64  | Moderate-high multicollinearity             |
| Social Phobia       | 5.55  | Moderate-high multicollinearity             |
| Simple Phobia       | 5.92  | Moderate-high multicollinearity             |
| Agoraphobia         | 5.79  | Moderate-high multicollinearity             |
| Panic               | 5.40  | Borderline moderate/high multicollinearity  |
| Alcohol Dependence  | 5.67  | Moderate-high multicollinearity             |
| Drug Dependence     | 5.87  | Moderate-high multicollinearity             |
| ASPD                | 5.14  | Lowest, but still above 5 → moderate concern|

## Common ways to deal with this level of multicollinearity

- Combine highly correlated disorders into composite indices (e.g., any anxiety disorder, any mood disorder, internalizing vs. externalizing psychopathology).
- Use dimension reduction techniques (PCA, factor analysis) to create uncorrelated components.
- Drop some variables (least preferred when all are theoretically important).


