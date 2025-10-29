# Cross Table

The service to compute the cross table from indices and columns with aggregation on any other columns.


## One index and One column:


```json
jobs = [
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/banking.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "cross_table",
    "kwargs": {
      "index": [
        "education"
      ],
      "columns": [
        "job"
      ],
      "operations": {
        "job": [
          {
            "func": "len",
            "suffix": "",
            "args": []
          }
        ]
      }
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

Mục đích: Trong bài toán trên, chúng ta xét tất cả các khả năng phân nhóm theo `job` và `education`. Trong các nhóm này, ta cấn tính toán số lượng (hoặc trung bình, tổng...) theo một biến khác; hoặc chính các biến này.


Ý nghĩa: Tính toán giao nhau giữa các nhóm đối tượng trong các trường dữ liệu có nhiều thuộc tính. Chẳng hạn khi chúng ta nói về đối tượng khách hàng được phân theo hai nhóm thuộc tính là Job và Education. Trong Job có nhiều thuộc tính khác nhau và trong Educcation cũng có nhiều thuộc tính khác nhau. VD: 7801 chỉ số lượng khách hàng có Educaion thuộc nhóm tertiary và lĩnh vực Job là management.


Giải thích tham số:

- *index*: List of Strings: The values to form the index column.
- *columns*: List of String: The values to form the column names. Joining by `::`. For example `management::len(job)`. (len -> count job value for management.
- *operations*: HashMap to aggregate. For example, compute the `len` function on column `job`.



Output result:

| education | management::len(job) | technician::len(job) | entrepreneur::len(job) | blue-collar::len(job) | unknown::len(job) | retired::len(job) | admin.::len(job) | services::len(job) | self-employed::len(job) | unemployed::len(job) | housemaid::len(job) | student::len(job) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| tertiary | 7801 | 1968 | 686 | 149 | 39 | 366 | 572 | 202 | 833 | 289 | 173 | 223 |
| secondary | 1121 | 5229 | 542 | 5371 | 71 | 984 | 4219 | 3457 | 577 | 728 | 395 | 508 |
| unknown | 242 | 242 | 76 | 454 | 127 | 119 | 171 | 150 | 39 | 29 | 45 | 163 |
| primary | 294 | 158 | 183 | 3758 | 51 | 795 | 209 | 345 | 130 | 257 | 627 | 44 |


## Multiple indices and columns:


```json
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/banking.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "cross_table",
    "kwargs": {
      "index": [
        "marital",
        "education"
      ],
      "columns": [
        "job",
        "loan"
      ],
      "operations": {
        "duration": [
          {
            "func": "mean",
            "suffix": "",
            "args": []
          },
          {
            "func": "max",
            "suffix": "",
            "args": []
          }
        ],
        "education": [
          {
            "func": "len",
            "suffix": "",
            "args": []
          }
        ],
        "job": [
          {
            "func": "len",
            "suffix": "",
            "args": []
          }
        ]
      }
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

Output result:

| marital | education | management:no::mean(duration) | technician:no::mean(duration) | entrepreneur:yes::mean(duration) | blue-collar:no::mean(duration) | unknown:no::mean(duration) | management:yes::mean(duration) | entrepreneur:no::mean(duration) | retired:no::mean(duration) | admin.:no::mean(duration) | services:no::mean(duration) | blue-collar:yes::mean(duration) | retired:yes::mean(duration) | technician:yes::mean(duration) | admin.:yes::mean(duration) | self-employed:no::mean(duration) | services:yes::mean(duration) | self-employed:yes::mean(duration) | unemployed:no::mean(duration) | housemaid:no::mean(duration) | student:no::mean(duration) | housemaid:yes::mean(duration) | unemployed:yes::mean(duration) | unknown:yes::mean(duration) | student:yes::mean(duration) | management:no::max(duration) | technician:no::max(duration) | entrepreneur:yes::max(duration) | blue-collar:no::max(duration) | unknown:no::max(duration) | management:yes::max(duration) | entrepreneur:no::max(duration) | retired:no::max(duration) | admin.:no::max(duration) | services:no::max(duration) | blue-collar:yes::max(duration) | retired:yes::max(duration) | technician:yes::max(duration) | admin.:yes::max(duration) | self-employed:no::max(duration) | services:yes::max(duration) | self-employed:yes::max(duration) | unemployed:no::max(duration) | housemaid:no::max(duration) | student:no::max(duration) | housemaid:yes::max(duration) | unemployed:yes::max(duration) | unknown:yes::max(duration) | student:yes::max(duration) | management:no::len(education) | technician:no::len(education) | entrepreneur:yes::len(education) | blue-collar:no::len(education) | unknown:no::len(education) | management:yes::len(education) | entrepreneur:no::len(education) | retired:no::len(education) | admin.:no::len(education) | services:no::len(education) | blue-collar:yes::len(education) | retired:yes::len(education) | technician:yes::len(education) | admin.:yes::len(education) | self-employed:no::len(education) | services:yes::len(education) | self-employed:yes::len(education) | unemployed:no::len(education) | housemaid:no::len(education) | student:no::len(education) | housemaid:yes::len(education) | unemployed:yes::len(education) | unknown:yes::len(education) | student:yes::len(education) | management:no::len(job) | technician:no::len(job) | entrepreneur:yes::len(job) | blue-collar:no::len(job) | unknown:no::len(job) | management:yes::len(job) | entrepreneur:no::len(job) | retired:no::len(job) | admin.:no::len(job) | services:no::len(job) | blue-collar:yes::len(job) | retired:yes::len(job) | technician:yes::len(job) | admin.:yes::len(job) | self-employed:no::len(job) | services:yes::len(job) | self-employed:yes::len(job) | unemployed:no::len(job) | housemaid:no::len(job) | student:no::len(job) | housemaid:yes::len(job) | unemployed:yes::len(job) | unknown:yes::len(job) | student:yes::len(job) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| single | primary | 170.12 | 260.19 | 377.00 | 277.72 | 168.12 | 167.67 | 201.33 | 193.36 | 232.04 | 288.71 | 255.12 | 123.33 | None | 180.33 | 294.11 | 316.18 | 302.00 | 283.19 | 379.00 | 195.73 | 163.33 | 583.00 | None | None | 475 | 1096 | 1051 | 2653 | 465 | 317 | 396 | 1003 | 670 | 1576 | 1975 | 177 | None | 267 | 1307 | 686 | 632 | 1205 | 2093 | 819 | 222 | 919 | None | None | 16 | 16 | 6 | 445 | 8 | 3 | 12 | 28 | 27 | 41 | 73 | 3 | 0 | 3 | 9 | 11 | 3 | 63 | 39 | 41 | 3 | 3 | 0 | 0 | 16 | 16 | 6 | 445 | 8 | 3 | 12 | 28 | 27 | 41 | 73 | 3 | 0 | 3 | 9 | 11 | 3 | 63 | 39 | 41 | 3 | 3 | 0 | 0 |
| divorced | primary | 192.55 | 266.92 | 392.00 | 291.64 | 247.00 | 297.75 | 266.71 | 273.90 | 199.08 | 246.34 | 258.96 | 296.53 | 259.00 | 273.00 | 287.80 | 194.50 | 215.00 | 309.48 | 265.84 | 177.00 | 443.20 | 149.50 | None | None | 472 | 489 | 476 | 2456 | 422 | 508 | 688 | 1156 | 625 | 1156 | 1080 | 1448 | 977 | 542 | 642 | 591 | 215 | 924 | 1558 | 177 | 1368 | 207 | None | None | 22 | 12 | 2 | 271 | 4 | 4 | 7 | 145 | 25 | 50 | 49 | 17 | 5 | 5 | 5 | 10 | 1 | 25 | 85 | 1 | 5 | 2 | 0 | 0 | 22 | 12 | 2 | 271 | 4 | 4 | 7 | 145 | 25 | 50 | 49 | 17 | 5 | 5 | 5 | 10 | 1 | 25 | 85 | 1 | 5 | 2 | 0 | 0 |
| single | tertiary | 261.28 | 276.57 | 298.15 | 278.06 | 177.27 | 242.56 | 269.50 | 269.27 | 218.28 | 293.48 | 259.00 | 724.00 | 247.25 | 162.71 | 307.09 | 376.85 | 351.16 | 325.83 | 314.36 | 262.26 | 177.50 | 318.00 | None | 188.00 | 1914 | 2516 | 922 | 1422 | 426 | 1347 | 1916 | 847 | 1290 | 2219 | 389 | 724 | 1184 | 461 | 1877 | 1532 | 3253 | 1837 | 1461 | 1571 | 227 | 703 | None | 257 | 2312 | 789 | 33 | 80 | 11 | 272 | 103 | 26 | 242 | 84 | 5 | 1 | 107 | 34 | 275 | 13 | 32 | 130 | 39 | 196 | 2 | 4 | 0 | 2 | 2312 | 789 | 33 | 80 | 11 | 272 | 103 | 26 | 242 | 84 | 5 | 1 | 107 | 34 | 275 | 13 | 32 | 130 | 39 | 196 | 2 | 4 | 0 | 2 |
| divorced | unknown | 310.57 | 343.13 | 181.00 | 200.85 | 257.86 | 210.67 | 654.00 | 229.85 | 391.67 | 327.20 | 343.83 | None | 91.00 | None | None | 138.00 | 245.00 | 749.00 | 64.00 | 15.00 | None | None | None | None | 1563 | 1419 | 181 | 967 | 626 | 279 | 988 | 562 | 1310 | 1409 | 715 | None | 158 | None | None | 138 | 245 | 749 | 152 | 15 | None | None | None | None | 28 | 23 | 1 | 33 | 7 | 3 | 5 | 13 | 18 | 20 | 6 | 0 | 2 | 0 | 0 | 1 | 1 | 1 | 6 | 1 | 0 | 0 | 0 | 0 | 28 | 23 | 1 | 33 | 7 | 3 | 5 | 13 | 18 | 20 | 6 | 0 | 2 | 0 | 0 | 1 | 1 | 1 | 6 | 1 | 0 | 0 | 0 | 0 |
| married | primary | 231.84 | 235.88 | 191.22 | 245.57 | 201.38 | 179.92 | 270.32 | 295.77 | 222.18 | 246.27 | 252.30 | 203.42 | 215.38 | 294.93 | 239.47 | 271.89 | 269.13 | 345.74 | 234.59 | 259.00 | 251.76 | 235.00 | 595.00 | None | 1550 | 1332 | 586 | 3078 | 759 | 518 | 1503 | 2187 | 1041 | 1934 | 1574 | 836 | 829 | 1039 | 1438 | 1877 | 816 | 3025 | 2692 | 487 | 1339 | 472 | 670 | None | 200 | 104 | 32 | 2461 | 37 | 49 | 124 | 526 | 119 | 187 | 459 | 76 | 21 | 30 | 97 | 46 | 15 | 152 | 421 | 2 | 74 | 12 | 2 | 0 | 200 | 104 | 32 | 2461 | 37 | 49 | 124 | 526 | 119 | 187 | 459 | 76 | 21 | 30 | 97 | 46 | 15 | 152 | 421 | 2 | 74 | 12 | 2 | 0 |
| divorced | secondary | 272.82 | 227.06 | 270.63 | 263.74 | 277.67 | 278.96 | 202.21 | 313.60 | 263.24 | 259.29 | 286.03 | 277.48 | 240.12 | 263.16 | 301.77 | 288.67 | 191.33 | 248.57 | 247.31 | 406.00 | 411.83 | 138.11 | None | None | 1973 | 1642 | 769 | 2231 | 556 | 1271 | 652 | 2055 | 1776 | 1880 | 2201 | 1417 | 2150 | 1658 | 1584 | 3094 | 387 | 1697 | 1200 | 406 | 760 | 248 | None | None | 113 | 561 | 19 | 309 | 3 | 24 | 48 | 138 | 500 | 355 | 78 | 27 | 147 | 148 | 48 | 99 | 9 | 109 | 64 | 1 | 6 | 9 | 0 | 0 | 113 | 561 | 19 | 309 | 3 | 24 | 48 | 138 | 500 | 355 | 78 | 27 | 147 | 148 | 48 | 99 | 9 | 109 | 64 | 1 | 6 | 9 | 0 | 0 |
| single | secondary | 251.65 | 263.77 | 252.82 | 286.16 | 274.45 | 248.43 | 306.61 | 259.28 | 254.40 | 265.91 | 305.81 | 210.08 | 256.93 | 251.77 | 283.23 | 274.90 | 343.17 | 297.12 | 219.35 | 246.27 | 105.00 | 314.00 | 537.00 | 223.80 | 2770 | 2420 | 936 | 2260 | 1487 | 1165 | 1404 | 1063 | 2301 | 3785 | 2078 | 867 | 1203 | 3183 | 1707 | 1105 | 1210 | 1606 | 1008 | 1730 | 149 | 739 | 966 | 402 | 252 | 1367 | 11 | 1093 | 22 | 42 | 61 | 32 | 1128 | 836 | 219 | 13 | 270 | 248 | 94 | 178 | 18 | 178 | 51 | 482 | 4 | 11 | 2 | 5 | 252 | 1367 | 11 | 1093 | 22 | 42 | 61 | 32 | 1128 | 836 | 219 | 13 | 270 | 248 | 94 | 178 | 18 | 178 | 51 | 482 | 4 | 11 | 2 | 5 |
| married | tertiary | 254.08 | 261.40 | 228.56 | 313.06 | 231.28 | 245.12 | 240.53 | 303.46 | 215.18 | 232.33 | 505.50 | 276.45 | 234.35 | 142.76 | 240.82 | 234.69 | 244.25 | 252.24 | 237.44 | 283.90 | 348.73 | 232.36 | None | 172.00 | 2870 | 4918 | 1156 | 1972 | 966 | 2256 | 2769 | 2027 | 1138 | 912 | 3422 | 975 | 1451 | 389 | 3322 | 514 | 1449 | 988 | 1391 | 1024 | 1269 | 673 | None | 266 | 3703 | 749 | 106 | 48 | 25 | 597 | 347 | 225 | 200 | 75 | 12 | 29 | 148 | 42 | 389 | 16 | 61 | 116 | 103 | 20 | 11 | 14 | 0 | 2 | 3703 | 749 | 106 | 48 | 25 | 597 | 347 | 225 | 200 | 75 | 12 | 29 | 148 | 42 | 389 | 16 | 61 | 116 | 103 | 20 | 11 | 14 | 0 | 2 |
| divorced | tertiary | 256.75 | 267.42 | 284.78 | 217.25 | 238.67 | 236.91 | 248.58 | 360.18 | 210.10 | 179.45 | None | 359.83 | 298.52 | 187.00 | 312.58 | 252.67 | 214.82 | 356.96 | 235.56 | 251.67 | 96.50 | 140.00 | None | None | 2087 | 1199 | 1082 | 289 | 385 | 1554 | 1164 | 1357 | 690 | 662 | None | 854 | 1992 | 426 | 1723 | 344 | 545 | 1960 | 870 | 605 | 136 | 140 | None | None | 772 | 146 | 23 | 4 | 3 | 145 | 74 | 73 | 39 | 11 | 0 | 12 | 29 | 15 | 65 | 3 | 11 | 24 | 16 | 3 | 2 | 1 | 0 | 0 | 772 | 146 | 23 | 4 | 3 | 145 | 74 | 73 | 39 | 11 | 0 | 12 | 29 | 15 | 65 | 3 | 11 | 24 | 16 | 3 | 2 | 1 | 0 | 0 |
| married | unknown | 255.15 | 224.45 | 263.00 | 257.72 | 248.28 | 156.20 | 224.58 | 309.14 | 230.61 | 267.30 | 189.04 | 253.29 | 182.00 | 302.33 | 253.13 | 330.17 | None | 388.31 | 213.19 | 155.40 | 130.00 | None | None | None | 1103 | 1018 | 804 | 1486 | 1186 | 319 | 1403 | 1348 | 1106 | 1812 | 586 | 611 | 665 | 567 | 933 | 1171 | None | 810 | 703 | 385 | 166 | None | None | None | 151 | 130 | 13 | 292 | 95 | 10 | 45 | 94 | 101 | 82 | 24 | 7 | 16 | 6 | 23 | 12 | 0 | 16 | 31 | 10 | 2 | 0 | 0 | 0 | 151 | 130 | 13 | 292 | 95 | 10 | 45 | 94 | 101 | 82 | 24 | 7 | 16 | 6 | 23 | 12 | 0 | 16 | 31 | 10 | 2 | 0 | 0 | 0 |

