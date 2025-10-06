# Các hàm thống kê cơ bản.

Trước tiên, chúng ta cần kết nối tcrux với py4j để gọi services.

```python
```PYTHON
from py4j.clientserver import (
    ClientServer,
    PythonParameters,
    JavaParameters
)
from py4j.java_collections import JavaMap
from titan.tcrux.services import executor


class Tcrux(object):
    @staticmethod
    def execute(service: str, params: JavaMap) -> str:
        result = executor(service=service, params=params)
        return result

    class Java:
        implements = ["com.titan.tcrux.Tcrux"]
```

Chạy các hàm thống kê cơ bản: mean, std, min, max, count, sum, quantile, mode, cvar, range, pearson_index, confidence_interval, skew, kurt

Tham số để gọi service:

```json
params = {
    "input": "banking.arrow",
    "output": "basic.arrow",
    "tasks": [
        {
            "task": "statistics",
            "action": "describe",
            "kwargs": {
                "metrics": [
                    "mean",
                    "std",
                    "min",
                    "max",
                    "cvar",
                    "skew",
                    "kurt",
                    "range",
                    "pearson_index",
                    "confidence_interval",
                ],
            },
        }
    ],
}
```

Kết quả output có dạng như sau:

| metrics        | age       | balance       | day       | duration     | campaign    | pdays       | previous    | year       |
|----------------|-----------|---------------|-----------|--------------|-------------|-------------|-------------|------------|
| mean           | 40.936210 | 1362.272058   | 15.806419 | 258.163080   | 2.763841    | 40.197828   | 0.580323    | 2010.444626 |
| std            | 10.618762 | 3044.765829   | 8.322476  | 257.527812   | 3.098021    | 100.128746  | 2.303441    | 0.602364    |
| min            | 18.000000 | -8019.000000  | 1.000000  | 0.000000     | 1.000000    | -1.000000   | 0.000000    | 2010.000000 |
| max            | 95.000000 | 102127.000000 | 31.000000 | 4918.000000  | 63.000000   | 871.000000  | 275.000000  | 2012.000000 |
| cvar           | 0.259398  | 2.235064      | 0.526525  | 0.997539     | 1.120912    | 2.490899    | 3.969237    | 0.000300    |
| skew           | 0.684795  | 8.360031      | 0.093076  | 3.144214     | 4.898488    | 2.615629    | 41.845066   | 1.008675    |
| range          | 77.000000 | 110146.000000 | 30.000000 | 4918.000000  | 62.000000   | 872.000000  | 275.000000  | 2.000000    |
| pearson_index  | 0.547016  | 0.900830      | -0.069780 | 0.910539     | 0.739673    | 1.234346    | 0.755813    | 2.214406    |

Giải thích ý nghĩa:
(1) Mean: Giá trị trung bình.

(2) std (Standard Deviation): Là đại lượng dùng để đo độ phân tán của dữ liệu (quanh giá trị trung bình) đối với hai trường dữ liệu CÙNG ĐƠN VỊ ĐO. Nếu hai trường dữ liệu cùng đơn vị đo, nếu dữ liệu nào có std lớn thì độ phân tán của nó lớn và ngược lại. std còn dùng để tính toán các đại lượng khác trong thống kê như confidence interval,...

(3) cvar (Coefficient variation - Hệ số phương sai): cvar dùng để xác định sự phân tán của dữ liệu khi không quan tâm đến đơn vị đo. Với hai trường dữ liệu có các đơn vị đo khác nhau thì dùng cvar để so sánh độ phân tán của chúng, trường dữ liệu nào có cvar lớn hơn thì độ phân tán nó lớn hơn và ngược lại

(4) Min: Là giá trị nhỏ nhất của dữ liệu trong trường đó.

(5) Max: Giá trị lớn nhất của dữ liệu trong trường đó.

(6) skew: Đo độ lệch trong phân bố của dữ liệu. Nếu skew = 0 thì dữ liệu cân đối (mean = mode = median), Skew > 0 thì mean lệch về bên phải của dữ liệu (chiều cao của dữ liệu lệch về bên trái) và ngược lại.
<img width="330" height="450" alt="image" src="https://github.com/user-attachments/assets/dddd15bd-bfff-4a36-a160-0781cbc14d35" />

