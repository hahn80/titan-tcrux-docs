# RunScript — PyBox API

`RunScript` runs an external Python script from JVM.


## Quick Reference

| Method | Description |
|---|---|
| `new RunScript(String/Path)` | Construct with script path |
| `RunScript.pythonExe(String)` | Factory — set Python executable first |
| `.scriptFile(String/Path)` | Set script path (factory form only) |
| `.inputFile(String/Path)` | Add input file as positional arg |
| `.outputFile(String/Path)` | Add output file as positional arg |
| `.python(String)` | Override Python executable |
| `.timeoutSeconds(int)` | Set timeout (default: 60s) |
| `.callMain()` | Enable call-main mode, no args |
| `.mainArg(String/boolean/int/long/double/List/Map)` | Add positional arg for `main()` |
| `.mainKwarg(String, String/boolean/int/long/double/List/Map)` | Add keyword arg for `main()` |
| `.run()` | Execute — returns `Result`, never throws |


## Mode 1 — File-Based

Launches `python3 script.py [args...]`. The script handles its own argument parsing. Use `.inputFile()` / `.outputFile()` to pass file paths.

### Constructor form

```java
RunScript.Result result = new RunScript("script.py")
        .inputFile(Path.of("/data/input.arrow"))
        .outputFile(Path.of("/data/output.arrow"))
        .timeoutSeconds(30)
        .run();

if (result.success()) {
    System.out.println(result.stdout());
} else {
    System.err.printf("exit %d: %s%n", result.exitCode(), result.stderr());
}
```

### Factory form (custom Python executable)

```java
RunScript.Result result = RunScript.pythonExe("/usr/bin/python3")
        .scriptFile("/path/to/script.py")
        .inputFile(Path.of("/data/input.arrow"))
        .outputFile(Path.of("/data/output.arrow"))
        .timeoutSeconds(30)
        .run();
```

### Corresponding Python script

```python
# script.py
import argparse

def main():
    p = argparse.ArgumentParser()
    p.add_argument("input_arrow")
    p.add_argument("output_arrow")
    args = p.parse_args()
    process(args.input_arrow, args.output_arrow)

if __name__ == "__main__":
    main()
```


## Mode 2 — Call-Main

Args and kwargs are JSON-serialized on the Java side and decoded by `json.loads()` in Python, so the script receives native Python types. The return value must be a string (or `None`, which becomes `""`).

### Supported argument types

| Java | Python receives |
|---|---|
| `String` | `str` |
| `boolean` | `bool` |
| `int` / `long` | `int` |
| `double` | `float` |
| `List<?>` | `list` (elements follow the same rules, any depth) |
| `Map<?,?>` | `dict` (values follow the same rules, any depth) |
| `null` | `None` |

`Path`, `Date`, and any other types are **not supported** — pass them as `String` and let the script convert.

> **StackOverflowError:** the JSON serializer is recursive. Self-referencing or pathologically deep structures will overflow the call stack. This is not guarded — keep structures shallow and acyclic.

### No arguments

```java
RunScript.Result r = RunScript.pythonExe("python3")
        .scriptFile("script2.py")
        .callMain()
        .run();

if (r.success()) {
    System.out.println(r.returnValue());
}
```

### Positional argument only

```java
RunScript.Result r = RunScript.pythonExe("python3")
        .scriptFile("script2.py")
        .mainArg("MSFT")
        .run();
```

### Positional + keyword arguments

```java
RunScript.Result r = RunScript.pythonExe("python3")
        .scriptFile("script2.py")
        .mainArg("GOOG")
        .mainKwarg("threshold", 0.8)
        .run();

if (r.success()) {
    String value = r.returnValue();
}
```

### Multiple positional arguments

Chain `.mainArg()` once per value; they are forwarded to `main()` in order.

```java
RunScript.Result r = RunScript.pythonExe("python3")
        .scriptFile("script2.py")
        .mainArg("AAPL")   // str
        .mainArg(0.5)      // float
        .mainArg(30)       // int
        .run();
```

### Multiple keyword arguments

Chain `.mainKwarg()` once per key-value pair.

```java
RunScript.Result r = RunScript.pythonExe("python3")
        .scriptFile("script2.py")
        .mainKwarg("threshold", 0.5)
        .mainKwarg("window", 30)
        .mainKwarg("active", true)
        .run();
```

### Multiple positional + multiple keyword arguments

```java
RunScript.Result r = RunScript.pythonExe("python3")
        .scriptFile("script2.py")
        .mainArg("AAPL")
        .mainArg("NYSE")
        .mainKwarg("threshold", 0.5)
        .mainKwarg("window", 30)
        .mainKwarg("active", true)
        .run();
```

### Nested List and Map arguments

```java
List<String> symbols  = List.of("AAPL", "GOOG", "MSFT");
Map<String, Object> cfg = new LinkedHashMap<>();
cfg.put("threshold", 0.5);
cfg.put("tags", List.of("tech", "large-cap"));

RunScript.Result r = RunScript.pythonExe("python3")
        .scriptFile("script2.py")
        .mainArg(symbols)          // list[str]
        .mainKwarg("config", cfg)  // dict with nested list
        .run();
```

### Corresponding Python script

```python
# script2.py
def main(symbol: str = "AAPL", exchange: str = "NASDAQ",
         threshold: float = 0.5, window: int = 10, active: bool = True) -> str:
    return f"{symbol}@{exchange} threshold={threshold:.2f} window={window} active={active}"
```


## Result

`run()` never throws. All outcomes are reflected in the returned `Result`.

| Method | Type | Description |
|---|---|---|
| `success()` | `boolean` | `true` if exit code is 0 |
| `exitCode()` | `int` | Process exit code; `-1` on failure/timeout |
| `stdout()` | `String` | Full stdout |
| `returnValue()` | `String` | `stdout` stripped of leading/trailing whitespace; use in call-main mode |
| `stderr()` | `String` | Full stderr (includes Python tracebacks) |
| `command()` | `List<String>` | The full command that was executed |
| `status()` | `Status` | Terminal status enum value |
| `cause()` | `Throwable` | Underlying exception for `LAUNCH_FAILURE`; `null` otherwise |

### Status enum

| Value | Meaning |
|---|---|
| `SUCCESS` | Script exited with code 0 |
| `NONZERO_EXIT` | Script exited with non-zero code |
| `LAUNCH_FAILURE` | Process could not start (e.g. python3 not found) |
| `TIMEOUT` | Process did not finish within the timeout |
| `INTERRUPTED` | The waiting thread was interrupted |

### Full error-handling pattern

```java
RunScript.Result r = new RunScript("script.py").run();

switch (r.status()) {
    case SUCCESS       -> process(r.stdout());
    case NONZERO_EXIT  -> log.error("Script failed (exit {}): {}", r.exitCode(), r.stderr());
    case LAUNCH_FAILURE -> log.error("Could not start Python: {}", r.cause().getMessage());
    case TIMEOUT       -> log.error("Script timed out after {}s", 60);
    case INTERRUPTED   -> log.warn("Interrupted while waiting for script");
}
```


## Notes

- **`.inputFile()` and `.outputFile()` are additive** — each call appends one positional argument. Call order determines `sys.argv` order in the script.
- **`.mainArg()` and `.mainKwarg()` implicitly enable call-main mode** — no need to call `.callMain()` first.
- **Drain threads** are daemon threads; stdout and stderr are consumed concurrently to prevent subprocess buffer deadlock.
- **All argument values in call-main mode are strings.** The script is responsible for type conversion.
