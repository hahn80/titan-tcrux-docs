# Relative Frequency

Khi có dữ liệu dạng danh mục (category), chúng ta có thể  dùng frequency để xác định phân bố của dữ liệu theo từng danh mục.

Cách gọi như sau (Giả sử dev đã quen thuộc với các jobs)

```json
jobs = [
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/path/logistic.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "frequency_by",
    "kwargs": {
      "column": "deposit",
      "by": "education"
    }
  }
]
```

Giải thích tham số:
- *column*: String; tên cột để xác định frequency.
- *by*: Tên cột để phân nhóm.

Kết quả output:

| education | deposit_val | deposit_freq | deposit_rel_freq |
| --- | --- | --- | --- |
| primary | yes | 591 | 39.40 |
| primary | no | 909 | 60.60 |
| tertiary | yes | 1996 | 54.11 |
| tertiary | no | 1693 | 45.89 |
| unknown | yes | 252 | 50.70 |
| unknown | no | 245 | 49.30 |
| secondary | yes | 2450 | 44.74 |
| secondary | no | 3026 | 55.26 |

Giải thích kết quả:

- Cột education: Gồm các giá trị khác nhau theo phân nhóm của `education`.
- `deposit_val`: Chứa các giá trị khác nhau của biến `deposit`.
- `deposit_freq` là frequency và `deposit_rel_freq` là tỉ lệ phần trăm


Với kết quả của bản đưa ra, chúng ta có thể  vẽ thành đồ thị biểu diễn:

![Data Visualization](./imgs/education-vs-deposit.png)


Mỗi biểu đồ tròn thể hiện tỷ lệ yes so với no deposits, dựa trên deposit_rel_freq. Ví dụ:

unknown: 50,70% yes, 49,30% no.

primary: 39,40% yes, 60,60% no.

tertiary: 54,11% yes, 45,89% no.

secondary: 44,74% yes, 55,26% no.

Khi di chuột vào một phần biểu đồ, hiển thị phần trăm và số lượng (ví dụ: “yes: 50,70% (Count: 252)” đối với unknown).


Phân tích insights:

- Tertiary Education: Tỷ lệ yes cao nhất (54,11%), cho thấy xu hướng gửi tiền mạnh hơn.

- Primary Education: Tỷ lệ no cao nhất (60,60%), cho thấy ít khả năng gửi tiền hơn.

- Unknown Education: Gần như cân bằng giữa yes (50,70%) và no (49,30%).

- Secondary Education: Nghiêng về no ở mức vừa phải (55,26% so với 44,74%).

