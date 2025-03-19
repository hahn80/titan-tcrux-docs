# Phân nhóm và lọc khách hàng.


Lọc khách hàng theo các tiêu chí đưa ra và phân nhóm nếu cần.

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "final.arrow",
    "tasks": [
        {
            "task": "transforming",
            "action": "purify",
            "kwargs": {"conditions": {"y": [{"relation": "==", "value": "yes"}]}},
        },
        {
            "task": "clustering",
            "action": "get_top_clients",
            "kwargs": {
                "columns": None,
                "n_clusters": 0,
                "keys": ["key"],
                "exclude": ["year", "date"],
                "percent": 10,
				"cluster_alias": "cluster",
    			"distance_alias": "distance",
            },
        },
    ],
}
```

**Giải thích các tham số:**

- *action*: `purify` để lọc khách hàng theo điều kiện column `y` == "yes".
- *conditions*" a hashmap with key is the column name and value is a list of hashmaps {"relation": one of the relations, "value": value to compare to}. 
- *action*: `get_top_clients` to extract 10% "best" clients.
- *columns*: (List[String] or Null) if null the take all columns except the keys.
- *n_clusters*: Int : the number of clusters; if n_clusters=0, then it will determine the number of groups automatically.
- *keys*: Optional -> List[String] or Null : list of columns that are used as keys.
- *exclude*: List[String] : the list of columns that exlude from doing calculations.
- *percent*: Percentage to get the best clients. Float number from 1 to 100.
- *cluster_alias*: String
- *distance_alias*: String

input data:

| key | age | job          | marital | education | default | balance | housing | loan | contact  | day | month | duration | campaign | pdays | previous | poutcome | y  | year | date       |
|-----|-----|-------------|---------|-----------|---------|---------|---------|------|----------|-----|-------|----------|----------|-------|----------|----------|----|------|------------|
| 0   | 58  | management  | married | tertiary  | no      | 2143    | yes     | no   | unknown  | 5   | may   | 261      | 1        | -1    | 0        | unknown  | no | 2010 | 2010-05-05 |
| 1   | 44  | technician  | single  | secondary | no      | 29      | yes     | no   | unknown  | 5   | may   | 151      | 1        | -1    | 0        | unknown  | no | 2010 | 2010-05-05 |
| 2   | 33  | entrepreneur| married | secondary | no      | 2       | yes     | yes  | unknown  | 5   | may   | 76       | 1        | -1    | 0        | unknown  | no | 2010 | 2010-05-05 |
| 3   | 47  | blue-collar | married | unknown   | no      | 1506    | yes     | no   | unknown  | 5   | may   | 92       | 1        | -1    | 0        | unknown  | no | 2010 | 2010-05-05 |
| 4   | 33  | unknown     | single  | unknown   | no      | 1       | no      | no   | unknown  | 5   | may   | 198      | 1        | -1    | 0        | unknown  | no | 2010 | 2010-05-05 |


Kết quả output có dạng như sau:


| key   | age | job          | marital | education | default | balance | housing | loan | contact  | ... | duration | campaign | pdays | previous | poutcome | y   | year | date       | cluster | distance  |
|-------|-----|-------------|---------|-----------|---------|---------|---------|------|----------|-----|----------|----------|-------|----------|----------|-----|------|------------|---------|-----------|
| 10157 | 49  | management  | married | tertiary  | no      | 64      | no      | no   | unknown  | ... | 586      | 1        | -1    | 0        | unknown  | yes | 2010 | 2010-06-11 | 4       | 3.987217  |
| 10403 | 40  | management  | married | secondary | no      | 406     | yes     | no   | unknown  | ... | 577      | 2        | -1    | 0        | unknown  | yes | 2010 | 2010-06-12 | 4       | 4.001134  |
| 10824 | 47  | management  | married | secondary | no      | 2892    | no      | no   | unknown  | ... | 556      | 1        | -1    | 0        | unknown  | yes | 2010 | 2010-06-17 | 4       | 3.976455  |
| 10970 | 40  | management  | married | secondary | no      | 106     | no      | no   | unknown  | ... | 676      | 2        | -1    | 0        | unknown  | yes | 2010 | 2010-06-17 | 4       | 3.884023  |
| 14101 | 49  | blue-collar | married | secondary | no      | 3728    | yes     | no   | cellular | ... | 1060     | 2        | -1    | 0        | unknown  | yes | 2010 | 2010-07-11 | 0       | 3.980042  |









