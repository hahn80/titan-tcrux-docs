# BÀI TOÁN GIÁO DỤC TỈNH – GIA LAI


## Thống kê đánh giá tình hình

Chạy hàm 1: Nhóm biến tinh_trang (Lên lớp, lưu ban, bỏ học) theo từng năm:

multi_tasking jobs:

```json
jobs = [
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "students.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "frequency_by",
    "kwargs": {
      "column": "tinh_trang",
      "by": [
        "tinh",
        "xa",
        "truong",
        "nam_hoc"
      ]
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```

Giải thích tham số:
- *column*: String; tên cột để xác định frequency.
- *by*: Tên cột để phân nhóm.

Kết quả output:

| tinh | xa | truong | nam_hoc | tinh_trang_val | tinh_trang_freq | tinh_trang_rel_freq |
| --- | --- | --- | --- | --- | --- | --- |
| An Giang | An Biên | THPT Phan Châu Trinh | 2025-2026 | Lên Lớp | 176 | 48.89 |
| An Giang | An Biên | THPT Phan Châu Trinh | 2025-2026 | Lưu ban | 158 | 43.89 |
| An Giang | An Biên | THPT Phan Châu Trinh | 2025-2026 | Bỏ học | 26 | 7.22 |
| An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | Lên Lớp | 200 | 55.56 |
| An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | Lưu ban | 122 | 33.89 |
| An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | Bỏ học | 38 | 10.56 |
| An Giang | An Biên | THPT Phan Châu Trinh | 2024-2025 | Lên Lớp | 193 | 53.61 |
| An Giang | An Biên | THPT Phan Châu Trinh | 2024-2025 | Lưu ban | 144 | 40.00 |
| An Giang | An Biên | THPT Phan Châu Trinh | 2024-2025 | Bỏ học | 23 | 6.39 |

Giải thích kết quả:

- Cột nam_hoc: Gồm các giá trị khác nhau theo phân nhóm của `nam_hoc`.
- `tinh_trang_val`: Chứa các giá trị khác nhau của biến `tinh_trang`.
- `tinh_trang_freq` là frequency của biến `tinh_trang`.
- `tinh_trang_rel_freq` là tỉ lệ phần trăm của biến `tinh_trang`.


