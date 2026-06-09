# Advanced Query.


Query an arrow file as a real SQL query.

Jobs:

```json
[
  {
    "task": "batch_processing",
    "action": "batch_query",
    "kwargs": {
      "input_arrow": "/tmp/tmpwnw1tdsi/input.arrow",
      "output_arrow": "/tmp/tmpwnw1tdsi/output.arrow",
      "operations": {
        "query": "\n    SELECT OrderDate, Country, Category, Quantity, Email, Cost, EventTime, cost_log, quantity_sqrt, PriceTag\n    FROM Dummy\n    TRANSFORM\n        log(Cost) as cost_log, sqrt(Quantity) as quantity_sqrt, CUT(Cost, 40, 'Expensive', 20, 'Moderate', 'Cheap', True) as PriceTag\n    WHERE\n        Country NOT IN ('Mexico', 'USA')\n        AND Category == 'Beverages'\n        AND Quantity > 10.0\n        AND Email ILIKE '%@example.com'\n        AND Cost IS NOT NULL\n        AND OrderDate BETWEEN DATE('2025-01-01', '%Y-%m-%d') AND DATE('2025-06-30', '%Y-%m-%d')\n        AND EventTime >= DATETIME('2025-05-01 00:30:00', '%Y-%m-%d %H:%M:%S')\n    ",
        "batch_size": 250,
        "memory_limit_mb": 300,
        "compression": "zstd"
      }
    }
  }
]
```

- *input_arrow*: String: input arrow file.
- *output_arrow*: String: output arrow file.
- *batch_size*: Int: batch_size for the output.
- *memory_limit_mb*: Int: max size of memory in MB to process (avoid) OOM.
- *query*: support a full query similar to SQL.


Output result:

| OrderDate | Country | Category | Quantity | Email | Cost | EventTime | cost_log | quantity_sqrt | PriceTag |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2025-02-27 | Canada | Beverages | 18 | test@example.com | 43.00 | 2025-05-10 21:00:00 | 3.76 | 4.24 | Expensive |
| 2025-04-03 | Canada | Beverages | 23 | test@example.com | 43.00 | 2025-05-04 20:00:00 | 3.76 | 4.80 | Expensive |
| 2025-01-18 | Canada | Beverages | 23 | test@example.com | 43.00 | 2025-05-01 17:00:00 | 3.76 | 4.80 | Expensive |
| 2025-02-02 | Canada | Beverages | 18 | test@example.com | 43.00 | 2025-05-09 20:00:00 | 3.76 | 4.24 | Expensive |
| 2025-02-02 | Canada | Beverages | 13 | test@example.com | 43.00 | 2025-05-02 08:00:00 | 3.76 | 3.61 | Expensive |
| 2025-06-12 | Canada | Beverages | 18 | test@example.com | 43.00 | 2025-05-07 18:00:00 | 3.76 | 4.24 | Expensive |
| 2025-02-22 | Canada | Beverages | 13 | test@example.com | 43.00 | 2025-05-10 16:00:00 | 3.76 | 3.61 | Expensive |
| 2025-02-27 | Canada | Beverages | 13 | test@example.com | 43.00 | 2025-05-03 09:00:00 | 3.76 | 3.61 | Expensive |
| 2025-02-07 | Canada | Beverages | 18 | test@example.com | 43.00 | 2025-05-02 13:00:00 | 3.76 | 4.24 | Expensive |
| 2025-06-07 | Canada | Beverages | 13 | test@example.com | 43.00 | 2025-05-07 13:00:00 | 3.76 | 3.61 | Expensive |
| 2025-01-28 | Canada | Beverages | 13 | test@example.com | 43.00 | 2025-05-09 15:00:00 | 3.76 | 3.61 | Expensive |
| 2025-04-28 | Canada | Beverages | 23 | test@example.com | 43.00 | 2025-05-05 21:00:00 | 3.76 | 4.80 | Expensive |
| 2025-01-08 | Canada | Beverages | 18 | test@example.com | 43.00 | 2025-05-08 19:00:00 | 3.76 | 4.24 | Expensive |
| 2025-05-23 | Canada | Beverages | 23 | test@example.com | 43.00 | 2025-05-06 22:00:00 | 3.76 | 4.80 | Expensive |
| 2025-01-03 | Canada | Beverages | 13 | test@example.com | 43.00 | 2025-05-08 14:00:00 | 3.76 | 3.61 | Expensive |
| 2025-04-18 | Canada | Beverages | 13 | test@example.com | 43.00 | 2025-05-05 11:00:00 | 3.76 | 3.61 | Expensive |
| 2025-03-04 | Canada | Beverages | 18 | test@example.com | 43.00 | 2025-05-03 14:00:00 | 3.76 | 4.24 | Expensive |
| 2025-03-09 | Canada | Beverages | 23 | test@example.com | 43.00 | 2025-05-03 19:00:00 | 3.76 | 4.80 | Expensive |
| 2025-02-07 | Canada | Beverages | 23 | test@example.com | 43.00 | 2025-05-10 01:00:00 | 3.76 | 4.80 | Expensive |
| 2025-01-13 | Canada | Beverages | 23 | test@example.com | 43.00 | 2025-05-09 00:00:00 | 3.76 | 4.80 | Expensive |



# Compute Transform Functions


## Arithmetic

| Function | Args | Description |
|----------|------|-------------|
| `abs` | `(col)` | Absolute value. Output type matches input. |
| `negate` | `(col)` | Arithmetic negation (`-x`). |
| `sign` | `(col)` | Sign of `x`: `-1`, `0`, or `1`. Output type matches input. |
| `add` | `(col1, col2\|const)` | Element-wise addition. |
| `subtract` | `(col1, col2\|const)` | Element-wise subtraction (`col1 - col2`). |
| `multiply` | `(col1, col2\|const)` | Element-wise multiplication. |
| `divide` | `(col1, col2\|const)` | Element-wise division (`col1 / col2`). Integer division truncates. |
| `power` | `(col, col2\|const)` | `col ^ exponent`. |
| `sqrt` | `(col)` | Square root. Output is float64. |
| `exp` | `(col)` | `e^x`. Output is float64. |
| `expm1` | `(col)` | `e^x - 1`. More precise than `exp(x) - 1` near zero. Output is float64. |

**Supported input types:** all numeric types (int8/16/32/64, uint8/16/32/64, float32, float64).


## Rounding

| Function | Args | Description |
|----------|------|-------------|
| `ceil` | `(col)` | Round up to nearest integer. |
| `floor` | `(col)` | Round down to nearest integer. |
| `trunc` | `(col)` | Truncate fractional digits (round toward zero). |
| `round` | `(col [, ndigits [, round_mode]])` | Round to `ndigits` decimal places using `round_mode`. |

**`round` arguments:**

- `ndigits` (int constant, default `0`) — number of decimal places to keep. Negative values round to tens, hundreds, etc.
- `round_mode` (string constant, default `"HALF_TO_EVEN"`) — tie-breaking rule.

**Supported `round_mode` values:**

| Value | Meaning |
|-------|---------|
| `"DOWN"` | Round toward negative infinity (floor) |
| `"UP"` | Round toward positive infinity (ceil) |
| `"TOWARDS_ZERO"` | Truncate (remove fractional part) |
| `"TOWARDS_INFINITY"` | Round away from zero |
| `"HALF_DOWN"` | Ties round toward negative infinity |
| `"HALF_UP"` | Ties round toward positive infinity |
| `"HALF_TOWARDS_ZERO"` | Ties truncate |
| `"HALF_TOWARDS_INFINITY"` | Ties round away from zero |
| `"HALF_TO_EVEN"` | Ties round to nearest even digit (banker's rounding) — **default** |
| `"HALF_TO_ODD"` | Ties round to nearest odd digit |

**Examples:**
```python
round(col1)                  # 0 decimal places, HALF_TO_EVEN
round(col1, 3)               # 3 decimal places, HALF_TO_EVEN
round(col1, 3, 'DOWN')       # 3 decimal places, round toward -inf
round(col1, -2)              # round to nearest 100
```


## Logarithms

| Function | Args | Description |
|----------|------|-------------|
| `ln` | `(col)` | Natural logarithm `ln(x)`. |
| `log` | `(col [, base])` | Logarithm with arbitrary base. Omit `base` for natural log (base `e`). |
| `log2` | `(col)` | Base-2 logarithm. |
| `log10` | `(col)` | Base-10 logarithm. |
| `log1p` | `(col)` | `ln(1 + x)`. More precise than `ln(x+1)` for `x` near zero. |
| `logb` | `(col, base_col)` | Logarithm with base from another column. Both args must be the same float type. |

**`log` input types:** float32, float64, and all integer types. Integer input produces float64 output.  
**`logb` input types:** float32 or float64 only; both arguments must be the same type. For mixed-type or integer bases, use `log(col, base)` instead.

**Examples:**
```python
ln(col1)           # natural log
log(col1)          # natural log (same as ln)
log(col1, 3)       # log base 3
log10(col1)        # log base 10
logb(col1, col2)   # log base col2 (both float64)
```


## Trigonometric

All trig functions operate on float32 or float64 columns and return the same type. Input is expected in **radians**.

| Function | Args | Description |
|----------|------|-------------|
| `sin` | `(col)` | Sine. |
| `cos` | `(col)` | Cosine. |
| `tan` | `(col)` | Tangent. |
| `asin` | `(col)` | Arcsine. Input domain `[-1, 1]`; output in `[-π/2, π/2]`. |
| `acos` | `(col)` | Arccosine. Input domain `[-1, 1]`; output in `[0, π]`. |
| `atan` | `(col)` | Arctangent. Output in `(-π/2, π/2)`. |
| `atan2` | `(y_col, x_col)` | `atan(y/x)` using signs of both args to determine quadrant. Output in `(-π, π]`. |


## Hyperbolic

All hyperbolic functions operate on float32 or float64 and return the same type.

| Function | Args | Description |
|----------|------|-------------|
| `sinh` | `(col)` | Hyperbolic sine. |
| `cosh` | `(col)` | Hyperbolic cosine. |
| `tanh` | `(col)` | Hyperbolic tangent. Output in `(-1, 1)`. |
| `asinh` | `(col)` | Inverse hyperbolic sine. |
| `acosh` | `(col)` | Inverse hyperbolic cosine. Input domain `[1, ∞)`. |
| `atanh` | `(col)` | Inverse hyperbolic tangent. Input domain `(-1, 1)`. |


## Type Casting

```python
cast(col, type_string)
```

Casts a column to the specified Arrow type. Uses safe casting (raises on overflow or invalid conversion).

**Supported type strings:**

| String | Arrow type |
|--------|-----------|
| `"int8"` | `int8` |
| `"int16"` | `int16` |
| `"int32"` | `int32` |
| `"int64"` | `int64` |
| `"float16"` | `float16` |
| `"float32"` | `float32` |
| `"float64"` | `float64` |
| `"utf8"` or `"string"` | UTF-8 string |
| `"boolean"` or `"bool"` | boolean |

**Example:**
```python
cast(col2, "float64")   # convert int column to float64
cast(col1, "string")    # numeric to string
```


## CUT — Bin into Categories

Assigns a string label to each row based on numeric thresholds. Equivalent to `pd.cut`.

```python
CUT(col, thresh1, label1, thresh2, label2, ..., default_label, inclusive)
```

**Arguments:**

| Position | Type | Description |
|----------|------|-------------|
| 1 | COLUMN | Numeric column to bin (int or float). |
| 2, 4, 6, … | CONSTANT (numeric) | Thresholds in **descending** order (highest first). |
| 3, 5, 7, … | CONSTANT (string) | Label assigned when value meets the corresponding threshold. |
| second-to-last | CONSTANT (string) | Default label when no threshold is matched. |
| last | CONSTANT (bool/int) | `True`/`1` = inclusive (`>=`); `False`/`0` = exclusive (`>`). |

**Minimum:** 1 column + 1 threshold/label pair + default label + inclusive flag = **5 args total**.  
**Additional bins:** add pairs of `(threshold, label)` before the default label. Total args must satisfy `(total - 3) % 2 == 0`.

**Output type:** `utf8` string column.

**Example:**
```python
CUT(col2, 90, "Excellent", 80, "Good", 70, "Pass", "Failed", True)
# inclusive=True:
#   col2 >= 90  →  "Excellent"
#   col2 >= 80  →  "Good"
#   col2 >= 70  →  "Pass"
#   otherwise  →  "Failed"
```

With `inclusive=False`:
```
  col2 > 90  →  "Excellent"
  col2 > 80  →  "Good"
  col2 > 70  →  "Pass"
  otherwise  →  "Failed"
```

**Supported column types:** int8, int16, int32, int64, uint8, uint16, uint32, uint64, float32, float64.


## One crazy example:

```SQL
SELECT *
FROM Dummy
TRANSFORM abs(col1) as abs_col1, add(col1, col2) as sum_col12,
divide(col2, 10) as col2_div10, exp(col1) as col1_exp, expm1(col1) as col1_expm1,
multiply(col1, col3) as mult_col13, negate(col1) as col1_neg, power(col1, 3) as col1_cubed,
sign(col1) as col1_sign, sqrt(col1) as col1_sqrt, subtract(col1, col3) as diff_col13,
ceil(col1) as col1_ceil, floor(col1) as col1_floor,
round(col1) as col1_round, round(col1, 3, 'DOWN') as col1_round3,
trunc(col1) as col1_trunc,
ln(col1) as col1_ln, log(col1) as col1_log, log10(col1) as col1_log10, log1p(col1) as col1_log1p,
log2(col1) as col1_log2, logb(col1, 3) as col1_log3,
acos(col1) as col1_acos, asin(col1) as col1_asin, atan(col1) as col1_atan, atan2(col1, col2) as col12_atan2,
cos(col1) as col1_cos, sin(col1) as col1_sin, tan(col1) as col1_tan,
acosh(col1) as col1_acosh, asinh(col1) as col1_asinh, atanh(col1) as col1_atanh,
cosh(col1) as col1_cosh, sinh(col1) as col1_sinh, tanh(col1) as col1_tanh,
cast(col2, "float64") as col1_cast,
CUT(col2, 90, "Excellent", 80, "Good", 70, "Pass", "Failed", True) as grade
WHERE col1 > 0.5 and col3 > 0.75
```

