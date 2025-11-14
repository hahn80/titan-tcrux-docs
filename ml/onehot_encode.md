# Onehot Encoding

Vectorize a categorical variale into vector using onehot encoding.


Consider the orders dataset as given below:


| OrderDate | Region | Rep | Item | Units | Unit Cost |
| --- | --- | --- | --- | --- | --- |
| 2021-01-06 | East | Jones | Pencil | 95 |  1.99  |
| 2021-01-23 | Central | Kivell | Binder | 50 |  19.99  |
| 2021-02-09 | Central | Jardine | Pencil | 36 |  4.99  |
| 2021-02-26 | Central | Gill | Pen | 27 |  19.99  |
| 2021-03-15 | West | Sorvino | Pencil | 56 |  2.99  |
| 2021-04-01 | East | Jones | Binder | 60 |  4.99  |
| 2021-04-18 | Central | Andrews | Pencil | 75 |  1.99  |
| 2021-05-05 | Central | Jardine | Pencil | 90 |  4.99  |
| 2021-05-22 | West | Thompson | Pencil | 32 |  1.99  |


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/filled_orders.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "onehot_encode",
    "kwargs": {
      "columns": [
        "Region",
        "Item"
      ],
      "separator": "_"
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

Output result:

| OrderDate | Region | Rep | Item | Units | Unit_Cost | Region_Central | Region_East | Region_West | Region_null | Item_Binder | Item_Desk | Item_Pen | Item_Pen Set | Item_Pencil | Item_null |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2021-01-06 00:00:00 | East | Jones | Pencil | 95 | 1.99 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 |
| 2021-01-07 00:00:00 | None | None | None | 92 | 3.05 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-08 00:00:00 | None | None | None | 89 | 4.11 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-09 00:00:00 | None | None | None | 87 | 5.17 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-10 00:00:00 | None | None | None | 84 | 6.23 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-11 00:00:00 | None | None | None | 81 | 7.28 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-12 00:00:00 | None | None | None | 79 | 8.34 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-13 00:00:00 | None | None | None | 76 | 9.40 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-14 00:00:00 | None | None | None | 73 | 10.46 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-15 00:00:00 | None | None | None | 71 | 11.52 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-16 00:00:00 | None | None | None | 68 | 12.58 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-17 00:00:00 | None | None | None | 65 | 13.64 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-18 00:00:00 | None | None | None | 63 | 14.70 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-19 00:00:00 | None | None | None | 60 | 15.75 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-20 00:00:00 | None | None | None | 57 | 16.81 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-21 00:00:00 | None | None | None | 55 | 17.87 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-22 00:00:00 | None | None | None | 52 | 18.93 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-23 00:00:00 | Central | Kivell | Binder | 50 | 19.99 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| 2021-01-24 00:00:00 | None | None | None | 49 | 19.11 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-25 00:00:00 | None | None | None | 48 | 18.23 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| 2021-01-26 00:00:00 | None | None | None | 47 | 17.34 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
