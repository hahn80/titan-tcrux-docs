# Batch Join Tables

Dịch vụ này dùng để join 2 tables với nhau. Kết quả sẽ lưu ra một file arrrow mới.
Các file input nên được ghi dưới dạng nhiều batch để chạy tiết kiệm bộ nhớ.

## Type 1: inner/left/anti/semi

```java
public class BatchJoinTest extends BaseTest {

    @Test
    public void testBatchJoin() {
        
        try (PythonInterpreter interpreter = new PythonInterpreter(config)) {
            interpreter.exec("from titan.tcrux.services import tcrux_multi_tasking");
            
            String leftTable = projectDir + "/src/tests/resources/left.arrow";
            String rightTable = projectDir + "/src/tests/resources/right.arrow";
            String outputArrow = projectDir + "/src/tests/resources/output.arrow";

            ArrayList<HashMap<String, Object>> jobs = new ArrayList<>();

            jobs.add(
                JobBuilder.job()
                    .task("transforming")
                    .action("batch_join")
                    .kwargs(k -> {
                        k.put("left_table", leftTable);
                        k.put("right_table", rightTable);
                        k.put("output_arrow", outputArrow);
                        k.put("on", "left.x==right.x and left.y==right.y");
                        k.put("how", "inner"); // inner/left/anti/semi
                        k.put("batch_size", 32000); // batch_size to write output
                        k.put("compression", "lz4");
                        //k.put("compression", "zstd");
                        k.put("max_workers", 4); // number of working workers
                    })
                    .build()
            );
            
            long startTime = System.currentTimeMillis();
            Object result = interpreter.invoke("tcrux_multi_tasking", jobs);
            long endTime = System.currentTimeMillis();

            logger.info("Testing time: " + (endTime - startTime) + " ms");
            logger.info("Returned object: " + String.valueOf(result));

        } catch (Exception e) {
            e.printStackTrace();
            Assertions.fail("Python interpreter failed: " + e.getMessage());
        }
    }

}
```

Giải thích các tham số:

- *left_table*: String (Path to left arrow file)
- *right_table*: String (Path to right arrow file)
- *output_arrow*: String (Path to output arrow file)
- *on*: String (Logic condition to match)
- *how*: String (join style: "inner", "left")
- *batch_size*: INT (batch size to write arrow file)
- *compression*: String (Compressing level: "lz4", "zstd" or null)
- *max_workers*: INT (workers to work in parallel)



## Type 2: cross join

```java
public class BatchJoinTest extends BaseTest {

    @Test
    public void testBatchJoin() {
        
        try (PythonInterpreter interpreter = new PythonInterpreter(config)) {
            interpreter.exec("from titan.tcrux.services import tcrux_multi_tasking");
            
            String leftTable = projectDir + "/src/tests/resources/ljoin.arrow";
            String rightTable = projectDir + "/src/tests/resources/rjoin.arrow";
            String outputArrow = projectDir + "/src/tests/resources/output.arrow";

            ArrayList<HashMap<String, Object>> jobs = new ArrayList<>();

            jobs.add(
                JobBuilder.job()
                    .task("transforming")
                    .action("batch_join")
                    .kwargs(k -> {
                        k.put("left_table", leftTable);
                        k.put("right_table", rightTable);
                        k.put("output_arrow", outputArrow);
                        k.put("how", "cross");
                        k.put("batch_size", 32000); // batch_size to write output
                        k.put("compression", "lz4");
                        //k.put("compression", "zstd");
                        k.put("max_workers", 4); // number of working workers
                        k.put("resize_batch", 2000); // only need for cross join
                    })
                    .build()
            );
            
            long startTime = System.currentTimeMillis();
            Object result = interpreter.invoke("tcrux_multi_tasking", jobs);
            long endTime = System.currentTimeMillis();

            logger.info("Testing time: " + (endTime - startTime) + " ms");
            logger.info("Returned object: " + String.valueOf(result));

        } catch (Exception e) {
            e.printStackTrace();
            Assertions.fail("Python interpreter failed: " + e.getMessage());
        }
    }

}
```

Notice: we need param `resize_batch` (<= 5000) to run cross join!

