# Advanced Filter.


Filter by batch

Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_filter",
    "kwargs": {
      "input_arrow": "/tmp/tmpcd508x_z/input.arrow",
      "output_arrow": "/tmp/tmpcd508x_z/output.arrow",
      "operations": {
        "where": "\n    Country NOT IN ('Mexico', 'USA')\n    AND Category == 'Beverages'\n    AND Quantity > 10.0\n    AND Email LIKE '%@example.com'\n    AND Cost IS NULL\n    AND OrderDate BETWEEN DATE('2025-01-01', '%Y-%m-%d') AND DATE('2025-06-30', '%Y-%m-%d')\n    ",
        "batch_size": 250,
        "max_way": 8,
        "num_workers": 4
      }
    }
  }
]
```


Output result:

| OrderDate | Country | Category | Quantity | Email | Cost |
| --- | --- | --- | --- | --- | --- |
| 2025-01-03 | Canada | Beverages | 13 | test@example.com | None |
| 2025-04-28 | Canada | Beverages | 23 | test@example.com | None |
| 2025-03-06 | Argentina | Beverages | 25 | user2@example.com | None |
| 2025-02-24 | Argentina | Beverages | 25 | user2@example.com | None |
| 2025-05-23 | Canada | Beverages | 23 | test@example.com | None |
| 2025-04-20 | Argentina | Beverages | 20 | user2@example.com | None |
| 2025-04-13 | Canada | Beverages | 23 | test@example.com | None |
| 2025-02-19 | Argentina | Beverages | 20 | user2@example.com | None |
| 2025-06-14 | Argentina | Beverages | 25 | user2@example.com | None |
| 2025-02-14 | Argentina | Beverages | 20 | user2@example.com | None |
| 2025-03-26 | Argentina | Beverages | 25 | user2@example.com | None |
| 2025-05-13 | Canada | Beverages | 13 | test@example.com | None |
| 2025-05-30 | Argentina | Beverages | 25 | user2@example.com | None |
| 2025-06-24 | Argentina | Beverages | 25 | user2@example.com | None |
| 2025-04-18 | Canada | Beverages | 18 | test@example.com | None |
| 2025-01-05 | Argentina | Beverages | 25 | user2@example.com | None |
| 2025-04-23 | Canada | Beverages | 18 | test@example.com | None |
| 2025-04-05 | Argentina | Beverages | 25 | user2@example.com | None |
| 2025-02-12 | Canada | Beverages | 18 | test@example.com | None |
| 2025-03-26 | Argentina | Beverages | 25 | user2@example.com | None |





