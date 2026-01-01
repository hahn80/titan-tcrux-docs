# Pivot Table

The service to transform a tall table to wide fat table.

Consider the input data:


| City | Category | Quantity |
| --- | --- | --- |
| Phoenix | Food | 6 |
| Houston | Food | 2 |
| New York | Electronics | 11 |
| Houston | Furniture | 4 |
| New York | Clothing | 10 |
| Houston | Toys | 12 |
| Los Angeles | Clothing | 9 |
| Phoenix | Furniture | 6 |
| Phoenix | Food | 6 |
| Houston | Clothing | 4 |
| Houston | Toys | 6 |
| Chicago | Furniture | 2 |
| New York | Clothing | 6 |
| Los Angeles | Furniture | 12 |
| Los Angeles | Clothing | 5 |
| New York | Toys | 5 |
| Houston | Toys | 10 |
| Phoenix | Toys | 16 |
| Phoenix | Food | 12 |
| New York | Furniture | 12 |


## Simple Pivot:


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/tmp/tmpaxez8r58/input.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "pivot",
    "kwargs": {
      "index": [
        "City"
      ],
      "columns": [
        "Category"
      ],
      "values": [
        "Quantity"
      ],
      "agg_func": "max"
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "/tmp/tmpaxez8r58/output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

***Chú thích:***

- *index*: the index column (List of ONE String Type or String).
- *columns*: the list of columns List<ONE String>.
- *values*: the list of values List<ONE String>.
- *agg_func*: Support only one agg function of the form {‘min’, ‘max’, ‘first’, ‘last’, ‘sum’, ‘mean’, ‘median’, ‘len’, ‘count_all’, 'count_null', 'count_not_null'}. 


Output result:

| City | Food | Toys | Clothing | Furniture | Electronics |
| --- | --- | --- | --- | --- | --- |
| Chicago | 14 | 18 | 18 | 14 | 17 |
| Phoenix | 12 | 19 | 17 | 18 | 12 |
| New York | 17 | 18 | 19 | 12 | 11 |
| Houston | 6 | 18 | 17 | 17 | 16 |
| Los Angeles | 7 | 5 | 12 | 12 | 18 |


## Multiple Pivot:


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/tmp/tmp9rudxqr2/input.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "multiple_pivot",
    "kwargs": {
      "index": [
        "City"
      ],
      "columns": [
        "Category"
      ],
      "operations": {
        "Quantity": [
          {
            "func": "max",
            "args": [],
            "alias": null
          },
          {
            "func": "sum",
            "args": [],
            "alias": null
          }
        ],
        "Sales": [
          {
            "func": "sum",
            "args": [],
            "alias": null
          }
        ]
      }
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "/tmp/tmp9rudxqr2/output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Output result:

| City | Clothing_Quantity::max | Electronics_Quantity::max | Food_Quantity::max | Furniture_Quantity::max | Toys_Quantity::max | Clothing_Quantity::sum | Electronics_Quantity::sum | Food_Quantity::sum | Furniture_Quantity::sum | Toys_Quantity::sum | Clothing_Sales::sum | Electronics_Sales::sum | Food_Sales::sum | Furniture_Sales::sum | Toys_Sales::sum |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Los Angeles | None | 18 | None | None | None | None | 26 | None | None | None | None | 1985.31 | None | None | None |
| Chicago | 18 | None | None | None | None | 53 | None | None | None | None | 3406.00 | None | None | None | None |
| New York | 19 | None | None | None | None | 68 | None | None | None | None | 2866.81 | None | None | None | None |
| Houston | None | None | 6 | None | None | None | None | 13 | None | None | None | None | 1733.25 | None | None |
| Phoenix | None | None | 12 | None | None | None | None | 24 | None | None | None | None | 1026.09 | None | None |

