# Advanced Filter.


Filter by batch

Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_filter",
    "kwargs": {
      "input_arrow": "/tmp/tmp9umktjnn/input.arrow",
      "output_arrow": "/tmp/tmp9umktjnn/output.arrow",
      "operations": {
        "where": "\n    Country NOT IN ('Mexico', 'USA')\n    AND Category == 'Beverages'\n    AND Quantity > 10.0\n    AND Email ILIKE '%@example.com'\n    AND Cost IS NULL\n    AND OrderDate BETWEEN DATE('2025-01-01', '%Y-%m-%d') AND DATE('2025-06-30', '%Y-%m-%d')\n    AND EventTime >= DATETIME('2025-05-01 00:30:00', '%Y-%m-%d %H:%M:%S')\n    ",
        "batch_size": 250,
        "m_batches": 8,
        "num_workers": 4,
        "columns": null,
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

| OrderDate | Country | Category | Quantity | Email | Cost | EventTime |
| --- | --- | --- | --- | --- | --- | --- |
| 2025-01-08 | Canada | Beverages | 18 | test@example.com | None | 2025-06-22 19:00:00 |
| 2025-02-02 | Canada | Beverages | 23 | test@example.com | None | 2025-05-17 08:00:00 |
| 2025-05-18 | Canada | Beverages | 13 | test@example.com | None | 2025-05-06 17:00:00 |
| 2025-05-25 | Argentina | Beverages | 20 | user2@example.com | None | 2025-05-07 00:00:00 |
| 2025-04-10 | Argentina | Beverages | 15 | user2@example.com | None | 2025-05-20 03:00:00 |
| 2025-05-13 | Canada | Beverages | 23 | test@example.com | None | 2025-06-28 00:00:00 |
| 2025-06-29 | Argentina | Beverages | 15 | user2@example.com | None | 2025-05-15 23:00:00 |
| 2025-05-03 | Canada | Beverages | 13 | test@example.com | None | 2025-05-28 14:00:00 |
| 2025-03-04 | Canada | Beverages | 18 | test@example.com | None | 2025-06-17 14:00:00 |
| 2025-05-25 | Argentina | Beverages | 15 | user2@example.com | None | 2025-06-06 00:00:00 |
| 2025-06-04 | Argentina | Beverages | 15 | user2@example.com | None | 2025-06-28 22:00:00 |
| 2025-01-28 | Canada | Beverages | 18 | test@example.com | None | 2025-05-17 03:00:00 |
| 2025-04-10 | Argentina | Beverages | 25 | user2@example.com | None | 2025-06-04 03:00:00 |
| 2025-03-14 | Canada | Beverages | 18 | test@example.com | None | 2025-05-26 12:00:00 |
| 2025-04-25 | Argentina | Beverages | 20 | user2@example.com | None | 2025-06-12 06:00:00 |
| 2025-01-20 | Argentina | Beverages | 15 | user2@example.com | None | 2025-05-24 07:00:00 |
| 2025-05-28 | Canada | Beverages | 18 | test@example.com | None | 2025-05-29 15:00:00 |
| 2025-03-11 | Argentina | Beverages | 25 | user2@example.com | None | 2025-06-10 09:00:00 |
| 2025-04-15 | Argentina | Beverages | 25 | user2@example.com | None | 2025-05-27 20:00:00 |
| 2025-01-28 | Canada | Beverages | 13 | test@example.com | None | 2025-05-17 03:00:00 |





