# Dominance Analysis


Tính toán các thống kê trong dominance analysis.


## 1. Level a: Dominance without shifts

Tham số để gọi service:
```json
params = {
    "input": "banking.arrow",
    "output": "calc-stats.arrow",
    "tasks": [
        {
            "task": "statistics",
            "action": "dominance",
            "kwargs": {
                "columns": [
                "mpg",
                "rep78",
                "headroom",
                "trunk",
                "weight",
                "length",
                "turn",
                "displacement",
                "gear_ratio"
				],
				"target": "price",
				"impact_percentage": [5, 20],
				"confidence_level": 0.75,
            },
        }
    ],
}
```

**Parameters**:

- columns: list of input columns.
- target: name of the output column (revenue).
- impact_percentage: list of dominance percentages:
For example, impact_percentage: [5, 20], that means we split the dominance levels into 3 groups: 0%-5% (0 = weak positive); 5%-20% (1 or - = moderate positive); and 20%-100% (2 or -2 = strong positive); if the value is negative, it means that the impact in the negative direction.
- `confidence_level`: a number from 0.5 to 0.95 (this number represent 50% to 95% confidence)


**Output result:** The output will be an arrow file.


## 2. Level a: Dominance with **shifts**


Tham số để gọi service:
```json
params = {
    "input": "banking.arrow",
    "output": "calc-stats.arrow",
    "tasks": [
        {
            "task": "statistics",
            "action": "dominance",
            "kwargs": {
                "columns": [
                "mpg",
                "rep78",
                "headroom",
                "trunk",
                "weight",
                "length",
                "turn",
                "displacement",
                "gear_ratio"
				],
				"target": "price",
				"impact_percentage": [5, 20],
				"confidence_level": 0.75,
				"shifts": [
					1,
					2,
					3,
					2,
					1,
					2,
					4,
					2,
					3
				]
            },
        }
    ],
}
```

Parameters:

*shifts*: the *shift* units for each columns (integer) and must have same length as *columns*.
