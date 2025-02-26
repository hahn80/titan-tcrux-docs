# Dimension Reduction


Giảm chiều dữ liệu, compact dữ liệu. Có 3 loại pca được đưa ra: pca_raw, pca_correlation, pca_covariance.

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "reduction.arrow",
    "tasks": [
        {
            "task": "reduction",
            "action": "pca_raw",
            "kwargs": {
                "columns": ["age", "balance", "duration", "pdays"],
                "top_k": 2,
            },
        }
    ],
}
```

Kết quả output có dạng như sau:

| pc_1         | pc_2         |
|--------------|--------------|
| -190.479678  | -2150.782788 |
| -152.162820  | -34.267486   |
| -77.754032   | -4.713829    |
| -43.426507   | -1508.440391 |
| -199.040067  | -7.798970    |



**Giải thích kết quả:**

- Số cột pc được trả về phụ thuộc vào tham số top_k được đưa vào. Đây là vector các thành phần chính trong phân tích PCA.
- Chúng ta thay đổi `action` thành pca_correlation hoặc pca_covariance để chạy tương tự.
- Nếu dùng pca_correlation, kết quả pc thu được đã được biến đổi bằng cách chuẩn hóa dữ liệu ban đầu. Do đó kết quả thu về không phán ánh đơn vị đo từ dữ liệu gốc.

TODO: Tính score cho các cột liên quan đến các pc1, pc2, ect.

pca_explain (rotation matrix)
pc1 -> to which columns
pc2 -> ....

2 kiểu trả về:
- Trả về hết (default)
- Trả về đối tượng với threshold %
- Loai do thi mo ta ket qua PCA.
- Phan cap correlation < covariance < raw by units in docs.

