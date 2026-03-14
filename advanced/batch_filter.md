# Advanced Filter.


Filter by batch

Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_filter",
    "kwargs": {
      "input_arrow": "/tmp/tmp3_bo94qd/input.arrow",
      "output_arrow": "/tmp/tmp3_bo94qd/output.arrow",
      "operations": {
        "where": "\n    Country NOT IN ('Mexico', 'USA')\n    AND Category == 'Beverages'\n    AND Quantity > 10.0\n    AND Email ILIKE '%@example.com'\n    AND Cost IS NULL\n    AND OrderDate BETWEEN DATE('2025-01-01', '%Y-%m-%d') AND DATE('2025-06-30', '%Y-%m-%d')\n    AND EventTime >= DATETIME('2025-05-01 00:30:00', '%Y-%m-%d %H:%M:%S')\n    ",
        "batch_size": 250,
        "m_batches": 8,
        "num_workers": 4,
        "columns": [
          "OrderDate",
          "Country",
          "Quantity"
        ],
        "compression": "zstd"
      }
    }
  }
]
```

*columns*: null or List of Strings. Null will take all columns.


Tham số `where` có kiểu là string tương tự như WHERE trong SQL. Nó hỗ trợ các tính năng sau:

- Comparision: ColName == 1; ColName >= 10.0; ColName == 'ABC' (==; !=; >=; <=; >; <)
- NULL: ColName IS NULL; ColName IS NOT NULL
- BETWEEN: ColName BETWEEN 'A' AND 'C'; ColName BETWEEN 2.1 AND 10.0;
- LIKE: ColName LIKE '%@edu.vn';
- ILIKE: ColName ILIKE '%@edu.vn'; // support case insensitive.
- Support DATE and DATETIME: DATE(String, format); DATE('2025-01-01', '%Y-%m-%d'); DATETIME(String, format); DATETIME('2025-05-01 00:30:00', '%Y-%m-%d %H:%M:%S')
- NOT: We can flip the whole statement: NOT (ColName == 10 AND ColName2 == 'a').


Output result:

| OrderDate | Country | Quantity |
| --- | --- | --- |
| 2025-04-15 | Argentina | 15 |
| 2025-06-22 | Canada | 18 |
| 2025-05-25 | Argentina | 25 |
| 2025-01-15 | Argentina | 25 |
| 2025-06-27 | Canada | 23 |
| 2025-05-05 | Argentina | 20 |
| 2025-02-24 | Argentina | 20 |
| 2025-01-05 | Argentina | 25 |
| 2025-05-23 | Canada | 13 |
| 2025-06-12 | Canada | 18 |
| 2025-03-16 | Argentina | 15 |
| 2025-03-06 | Argentina | 15 |
| 2025-02-09 | Argentina | 20 |
| 2025-05-23 | Canada | 13 |
| 2025-01-13 | Canada | 23 |
| 2025-05-20 | Argentina | 15 |
| 2025-03-21 | Argentina | 15 |
| 2025-01-05 | Argentina | 20 |
| 2025-01-23 | Canada | 13 |
| 2025-05-23 | Canada | 23 |





