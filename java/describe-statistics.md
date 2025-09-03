# Describe statistics

Trước khi nghiên cứu, phân tích, hoặc hiểu về dữ liệu, chúng ta cần biết một số thông tin về dữ liệu. Công cụ cở bản nhất là xác định thống kê mô tả.

Trong ngữ cảnh này, giả sử người dùng có một dataset nhưng chưa rõ các cột nó có cấu trúc, dạng dữ liệu thế nào. Hàm describe sẽ tách các cột dạng số và tính các thông kê cơ bản.

Cách gọi như sau (Giả sử dev đã quen thuộc với các jobs)

```json
jobs = [
        {
            "task": "loading",
            "action": "load_dataframe",
            "kwargs": {"input_arrow": "/path/to/arrrow/file"},
        },
        {
            "task": "statistics",
            "action": "describe",
            "kwargs": {"output_format": "arrow"},
        },
    ]
```

Giải thích tham số:
- *output_format*: Có giá trị là `arrow` hoặc `json`. Tuỳ vào việc dev muốn output để tính toán tiếp hay dùng cho GUI.

Kết quả output:

| column | count | null_count | mean | std | min | Q1 | Q2 | Q3 | max | range |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| age | 11162 | 0 | 41.23 | 11.91 | 18 | 32.00 | 39.00 | 49.00 | 95 | 77 |
| balance | 11162 | 0 | 1528.54 | 3225.41 | -6847 | 122.00 | 550.00 | 1708.00 | 81204 | 88051 |
| day | 11162 | 0 | 15.66 | 8.42 | 1 | 8.00 | 15.00 | 22.00 | 31 | 30 |
| duration | 11162 | 0 | 371.99 | 347.13 | 2 | 138.00 | 255.00 | 496.00 | 3881 | 3879 |
| campaign | 11162 | 0 | 2.51 | 2.72 | 1 | 1.00 | 2.00 | 3.00 | 63 | 62 |
| pdays | 11162 | 0 | 51.33 | 108.76 | -1 | -1.00 | -1.00 | 21.00 | 854 | 855 |
| previous | 11162 | 0 | 0.83 | 2.29 | 0 | 0.00 | 0.00 | 1.00 | 58 | 58 |

Giải thích kết quả:

- Cột column: Gồm các cột với biến giá trị số cần được phân tích sơ khởi từ file arrow input.
- Trong headers, chúng ta có các metrics `count | null_count | mean | std | min | Q1 | Q2 | Q3 | max | range`. Cần giải thích cụ thể ý nghĩa này từ nhóm nghiên cứu.


