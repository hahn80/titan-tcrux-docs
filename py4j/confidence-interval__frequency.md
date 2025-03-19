# Tính độ tin cậy và tần suất.


Tính confidence_interval và frequency. Chúng ta cũng có thể ghép nhiều hàm khác trong cùng một lúc chạy với hai hàm này.\

1. Chỉ tính confidence interval:

Tham số như sau:

```json
params = {
  "task": "statistics",
  "action": "calculate_stats",
  "kwargs": {
    "operations": {
      "value": [
        {
          "func": "ci_lower",
          "alias": "val_ci_lower",
          "args": [
            0.95
          ]
        },
        {
          "func": "ci_upper",
          "alias": "val_ci_upper",
          "args": [
            0.95
          ]
        }
      ]
    }
  }
}
```

2. Chỉ tính frequency:

Tham số như sau

```json
{
  "task": "statistics",
  "action": "calculate_stats",
  "kwargs": {
    "operations": {
      "label": [
        {
          "func": "freq_value",
          "alias": "label_freq_val",
          "args": []
        },
        {
          "func": "freq_count",
          "alias": "label_freq_count",
          "args": []
        },
        {
          "func": "freq_percent",
          "alias": "label_freq_pct",
          "args": []
        }
      ]
    }
  }
}

```

3. Kết hợp nhiều hám cùng một lúc:

Tham số để gọi service:

```json
params = {
  "task": "statistics",
  "action": "calculate_stats",
  "kwargs": {
    "operations": {
      "value": [
        {
          "func": "mean",
          "alias": "val_mean",
          "args": []
        },
        {
          "func": "cvar",
          "alias": "val_cvar",
          "args": []
        },
        {
          "func": "ci_lower",
          "alias": "val_ci_lower",
          "args": [
            0.95
          ]
        },
        {
          "func": "ci_upper",
          "alias": "val_ci_upper",
          "args": [
            0.95
          ]
        }
      ],
      "label": [
        {
          "func": "freq_value",
          "alias": "label_freq_val",
          "args": []
        },
        {
          "func": "freq_count",
          "alias": "label_freq_count",
          "args": []
        },
        {
          "func": "freq_percent",
          "alias": "label_freq_pct",
          "args": []
        }
      ]
    }
  }
}
```

Kết quả output có dạng như sau:

| age_ci          | education_freq                              |
|------------------|--------------------------------------------|
| 40.87876:40.99366 | [{'val': 'tertiary', 'freq': 13301, 'rel_freq'...] |



**Giải thích kết quả:**

- Để có được khả năng capture được dữ liệu với độ tin cậy cao. Khoảng confidence interval phải rộng hơn khi độ tin cậy cao hơn.
- Trong phần frequency, kết quả cho ta giá trị của danh mục, tần số và tần suất của từng danh mục.
