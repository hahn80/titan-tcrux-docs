# Contingency Table.


Tính bảng phân bố xác suất.

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "contingency_table.arrow",
    "tasks": [
        {
            "task": "statistics",
            "action": "contingency_table",
            "kwargs": {
                "index": ["job", "marital"],
                "values": "y",
                "show_percentage": True,
            },
        }
    ],
}
```

Kết quả output có dạng như sau:

| job             | marital   | no   | yes  | total | no_pct | yes_pct |
|------------------|-----------|------|------|-------|--------|--------|
| management      | married   | 4719 | 681  | 5400  | 87.39  | 12.61  |
| technician      | single    | 2273 | 347  | 2620  | 86.76  | 13.24  |
| entrepreneur    | married   | 989  | 81   | 1070  | 92.43  | 7.57   |
| blue-collar     | married   | 6531 | 437  | 6968  | 93.73  | 6.27   |
| unknown         | single    | 56   | 12   | 68    | 82.35  | 17.65  |
| management      | single    | 2469 | 478  | 2947  | 83.78  | 16.22  |
| entrepreneur    | divorced  | 164  | 15   | 179   | 91.62  | 8.38   |
| retired         | married   | 1349 | 382  | 1731  | 77.93  | 22.07  |
| admin.          | divorced  | 660  | 90   | 750   | 88.00  | 12.00  |
| admin.          | single    | 1493 | 235  | 1728  | 86.40  | 13.60  |
| technician      | married   | 3636 | 416  | 4052  | 89.73  | 10.27  |



**Giải thích kết quả:**

- Nếu tham số show_percentage=true, chúng ta nhận được cả tần số và phần trăm của các tần số này.
