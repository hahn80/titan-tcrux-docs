# Advanced Query.


Query an arrow file as a real SQL query.

Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_query",
    "kwargs": {
      "input_arrow": "/tmp/tmpd5ojde0e/input.arrow",
      "output_arrow": "/tmp/tmpd5ojde0e/output.arrow",
      "operations": {
        "query": "\n    SELECT OrderDate, Country, Category, Quantity, Email, Cost, EventTime, cost_log, quantity_sqrt \n    FROM Dummy\n    TRANSFORM\n        log(Cost) as cost_log, sqrt(Quantity) as quantity_sqrt    \n    WHERE\n        Country NOT IN ('Mexico', 'USA')\n        AND Category == 'Beverages'\n        AND Quantity > 10.0\n        AND Email ILIKE '%@example.com'\n        AND Cost IS NULL\n        AND OrderDate BETWEEN DATE('2025-01-01', '%Y-%m-%d') AND DATE('2025-06-30', '%Y-%m-%d')\n        AND EventTime >= DATETIME('2025-05-01 00:30:00', '%Y-%m-%d %H:%M:%S')\n    ",
        "batch_size": 250,
        "memory_limit_mb": 300,
        "compression": "zstd"
      }
    }
  }
]
```

- *input_arrow*: String: input arrow file.
- *output_arrow*: String: output arrow file.
- *batch_size*: Int: batch_size for the output.
- *memory_limit_mb*: Int: max size of memory in MB to process (avoid) OOM.
- *query*: support a full query similar to SQL.


Output result:

| OrderDate | Country | Category | Quantity | Email | Cost | EventTime | cost_log | quantity_sqrt |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2025-01-05 | Argentina | Beverages | 15 | user2@example.com | None | 2025-06-15 04:00:00 | None | 3.87 |
| 2025-05-23 | Canada | Beverages | 23 | test@example.com | None | 2025-05-06 22:00:00 | None | 4.80 |
| 2025-01-20 | Argentina | Beverages | 25 | user2@example.com | None | 2025-06-08 07:00:00 | None | 5.00 |
| 2025-05-15 | Argentina | Beverages | 20 | user2@example.com | None | 2025-05-21 14:00:00 | None | 4.47 |
| 2025-05-18 | Canada | Beverages | 13 | test@example.com | None | 2025-06-05 17:00:00 | None | 3.61 |
| 2025-01-18 | Canada | Beverages | 23 | test@example.com | None | 2025-06-08 05:00:00 | None | 4.80 |
| 2025-02-17 | Canada | Beverages | 23 | test@example.com | None | 2025-06-01 23:00:00 | None | 4.80 |
| 2025-04-28 | Canada | Beverages | 18 | test@example.com | None | 2025-06-04 21:00:00 | None | 4.24 |
| 2025-02-19 | Argentina | Beverages | 15 | user2@example.com | None | 2025-05-25 13:00:00 | None | 3.87 |
| 2025-04-13 | Canada | Beverages | 23 | test@example.com | None | 2025-05-27 18:00:00 | None | 4.80 |
| 2025-05-30 | Argentina | Beverages | 15 | user2@example.com | None | 2025-06-28 17:00:00 | None | 3.87 |
| 2025-06-12 | Canada | Beverages | 18 | test@example.com | None | 2025-05-15 06:00:00 | None | 4.24 |
| 2025-02-07 | Canada | Beverages | 13 | test@example.com | None | 2025-06-09 01:00:00 | None | 3.61 |
| 2025-05-08 | Canada | Beverages | 18 | test@example.com | None | 2025-06-27 19:00:00 | None | 4.24 |
| 2025-05-08 | Canada | Beverages | 13 | test@example.com | None | 2025-05-21 07:00:00 | None | 3.61 |
| 2025-03-31 | Argentina | Beverages | 20 | user2@example.com | None | 2025-06-11 05:00:00 | None | 4.47 |
| 2025-01-25 | Argentina | Beverages | 15 | user2@example.com | None | 2025-05-24 12:00:00 | None | 3.87 |
| 2025-02-27 | Canada | Beverages | 13 | test@example.com | None | 2025-06-17 09:00:00 | None | 3.61 |
| 2025-01-30 | Argentina | Beverages | 25 | user2@example.com | None | 2025-06-01 05:00:00 | None | 5.00 |
| 2025-03-26 | Argentina | Beverages | 15 | user2@example.com | None | 2025-06-18 12:00:00 | None | 3.87 |





