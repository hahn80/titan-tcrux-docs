# Biến đổi dữ liệu định danh sang vector.


## Categorical Transformation


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
    "action": "categorical_encode",
    "kwargs": {
      "operations": {
        "hanh_kiem": [
          {
            "alias": "hanh_kiem_encode"
          }
        ]
      }
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

Output results:

| ho_va_ten | ngay_sinh | dan_toc | hoan_canh | diem_toan | gv_toan | diem_ly | gv_ly | diem_hoa | gv_hoa | diem_van | gv_van | diem_su | gv_su | diem_dia | gv_dia | diem_sinh | gv_sinh | diem_tk | hoc_luc | hanh_kiem_encode | vang_co_phep | vang_ko_phep | danh_hieu | xep_hang | tinh_trang | tinh | xa | truong | nam_hoc | khoi_lop | gvcn |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Mẫn Hướng Xuân | 2010-07-09 | Ba Na | Trung Bình | 7.30 | Trang Gia Khiếu | 8.50 | Hùng Giao Thương | 9.80 | Tào Chấn Khoát | 5.30 | Khu Cát Thương | 9.00 | Ong Tiên Khê | 1.70 | Chế Phục Thạch | 4.80 | Âu Ánh Quân | 6.60 | Đạt | 0 | 2 | 2 | NA | 91 | Lên Lớp | An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | 10 | Hồ An Hoàn |
| Điền Quốc Phong | 2009-10-06 | Mường | Giàu | 7.60 | Trang Gia Khiếu | 8.80 | Hùng Giao Thương | 0.20 | Tào Chấn Khoát | 7.40 | Khu Cát Thương | 1.70 | Ong Tiên Khê | 1.00 | Chế Phục Thạch | 5.60 | Âu Ánh Quân | 4.60 | Chưa đạt | 0 | 1 | 1 | NA | 22 | Lưu ban | An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | 10 | Hồ An Hoàn |
| Nhậm Chiêu Trường | 2009-12-07 | Cơ Ho | Giàu | 1.30 | Trang Gia Khiếu | 5.40 | Hùng Giao Thương | 6.00 | Tào Chấn Khoát | 3.70 | Khu Cát Thương | 0.20 | Ong Tiên Khê | 0.40 | Chế Phục Thạch | 5.60 | Âu Ánh Quân | 3.20 | Chưa đạt | 0 | 2 | 1 | NA | 70 | Bỏ học | An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | 10 | Hồ An Hoàn |


## One hot encode


```json
jobs = [
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/students.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "onehot_encode",
    "kwargs": {
      "columns": [
        "hanh_kiem"
      ],
      "separator": "_"
    }
  },
  {
    "task": "saving",
    "action": "write_dataframe",
    "kwargs": {
      "output_arrow": "/data/TitanProjects/tcrux/src/tests/resources/output.arrow",
      "batch_size": 200,
      "reformat_string": true
    }
  }
]
```


Output results:

| ho_va_ten | ngay_sinh | dan_toc | hoan_canh | diem_toan | gv_toan | diem_ly | gv_ly | diem_hoa | gv_hoa | diem_van | gv_van | diem_su | gv_su | diem_dia | gv_dia | diem_sinh | gv_sinh | diem_tk | hoc_luc | hanh_kiem | vang_co_phep | vang_ko_phep | danh_hieu | xep_hang | tinh_trang | tinh | xa | truong | nam_hoc | khoi_lop | gvcn | hanh_kiem_Khá | hanh_kiem_Trung Bình | hanh_kiem_Tốt | hanh_kiem_Yếu |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Mẫn Hướng Xuân | 2010-07-09 | Ba Na | Trung Bình | 7.30 | Trang Gia Khiếu | 8.50 | Hùng Giao Thương | 9.80 | Tào Chấn Khoát | 5.30 | Khu Cát Thương | 9.00 | Ong Tiên Khê | 1.70 | Chế Phục Thạch | 4.80 | Âu Ánh Quân | 6.60 | Đạt | Khá | 2 | 2 | NA | 91 | Lên Lớp | An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | 10 | Hồ An Hoàn | 1 | 0 | 0 | 0 |
| Điền Quốc Phong | 2009-10-06 | Mường | Giàu | 7.60 | Trang Gia Khiếu | 8.80 | Hùng Giao Thương | 0.20 | Tào Chấn Khoát | 7.40 | Khu Cát Thương | 1.70 | Ong Tiên Khê | 1.00 | Chế Phục Thạch | 5.60 | Âu Ánh Quân | 4.60 | Chưa đạt | Khá | 1 | 1 | NA | 22 | Lưu ban | An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | 10 | Hồ An Hoàn | 1 | 0 | 0 | 0 |
| Nhậm Chiêu Trường | 2009-12-07 | Cơ Ho | Giàu | 1.30 | Trang Gia Khiếu | 5.40 | Hùng Giao Thương | 6.00 | Tào Chấn Khoát | 3.70 | Khu Cát Thương | 0.20 | Ong Tiên Khê | 0.40 | Chế Phục Thạch | 5.60 | Âu Ánh Quân | 3.20 | Chưa đạt | Khá | 2 | 1 | NA | 70 | Bỏ học | An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | 10 | Hồ An Hoàn | 1 | 0 | 0 | 0 |
| Hình Hiểu Hải | 2010-08-31 | Kinh | Nghèo | 8.20 | Trang Gia Khiếu | 5.40 | Hùng Giao Thương | 7.50 | Tào Chấn Khoát | 2.50 | Khu Cát Thương | 5.40 | Ong Tiên Khê | 1.70 | Chế Phục Thạch | 7.30 | Âu Ánh Quân | 5.40 | Đạt | Yếu | 0 | 2 | NA | 22 | Lên Lớp | An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | 10 | Hồ An Hoàn | 0 | 0 | 0 | 1 |
| Hy Tùng Nga | 2010-01-20 | Mường | Nghèo | 6.70 | Trang Gia Khiếu | 7.00 | Hùng Giao Thương | 1.60 | Tào Chấn Khoát | 6.10 | Khu Cát Thương | 8.90 | Ong Tiên Khê | 4.40 | Chế Phục Thạch | 2.00 | Âu Ánh Quân | 5.20 | Đạt | Yếu | 1 | 2 | NA | 71 | Lên Lớp | An Giang | An Biên | THPT Phan Châu Trinh | 2023-2024 | 10 | Hồ An Hoàn | 0 | 0 | 0 | 1 |





