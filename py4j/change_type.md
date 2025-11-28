# Change type of data (casting).


Tham số để gọi service:

1. Typical Types:

```json
{
	"task": "transforming",
	"action": "change_type",
	"kwargs": {
			"operations": {
			"num": {"alias": "num", "dtype": "DOUBLE"},
			"count": {"alias": "count", "dtype": "DOUBLE"},
		}
	},
}
```

Giải thích tham số:

- `dtype`: New type to cast to, suppported: "INT", "LONG", "DOUBLE", "STRING"


2. Datetime Types


Supported types:

- *TIMESTAMP*: To timestamp, need to speficy time_unit: (‘us’, ‘ns’, ‘ms’); and time_zone: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
- *DATE*: Convert the integer indicates the number of days since the Unix epoch (1970-01-01) to date.
- *DURATION*: Convert to duration, need to specify the time_unit (‘us’, ‘ns’, ‘ms’).


```JSON
[
  {
    "task": "loading",
    "action": "load_dataframe",
    "kwargs": {
      "input_arrow": "/data/TitanProjects/tcrux/src/tests/resources/time_data.arrow"
    }
  },
  {
    "task": "transforming",
    "action": "change_type",
    "kwargs": {
      "operations": {
        "date64": {
          "alias": "date64_stamp",
          "dtype": "TIMESTAMP",
          "time_unit": "ms",
          "time_zone": "UTC"
        },
        "date32": {
          "alias": "date32_int",
          "dtype": "INT"
        }
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

| date32 | date64 | date64_stamp | date32_int |
| --- | --- | --- | --- |
| 2022-01-08 | 2022-02-01 00:00:00 | 2022-02-01 00:00:00+00:00 | 19000 |
| 2023-04-03 | 2023-05-01 00:00:00 | 2023-05-01 00:00:00+00:00 | 19450 |
| 2024-10-04 | 2024-09-12 00:00:00 | 2024-09-12 00:00:00+00:00 | 20000 |
