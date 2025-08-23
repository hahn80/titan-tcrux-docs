# Logistic Regression


Run the service to train the logistic model for banking problem.

The output will be a Hashmap with score, recall, precision, and saved_model

```Python
from titan.tcrux.services import tcrux_multi_tasking
```


Params to run to train the model:

```json
params_train = [
    {
        "task": "loading",
        "action": "load_dataframe",
        "kwargs": {"input_arrow": "tests/resources/logistic.arrow"},
    },
    {
        "task": "regression",
        "action": "logistic_regression",
        "kwargs": {
            "target": "deposit",
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
                    "duration",
                ],
                "categorical_columns": [
                    "job",
                    "marital",
                    "education",
                    "default",
                    "housing",
                    "loan",
                ],
            },
        },
    },
]
```




