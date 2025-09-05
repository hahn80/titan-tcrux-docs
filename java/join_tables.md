# Join Tables

Dịch vụ này dùng để join 2 tables với nhau. Kết quả sẽ lưu ra một file arrrow mới.

Code demo:

```java
public class JoinTableTest extends BaseTest {

    @Test
    public void testJoinTable() {
        
        try (PythonInterpreter interpreter = new PythonInterpreter(config)) {
            interpreter.exec("from titan.tcrux.services import tcrux_multi_tasking");
            
            String leftTable = projectDir + "/src/tests/resources/ljoin.arrow";
            String rightTable = projectDir + "/src/tests/resources/rjoin.arrow";
            String outputArrow = projectDir + "/src/tests/resources/output.arrow";

            ArrayList<HashMap<String, Object>> jobs = new ArrayList<>();

            jobs.add(
                JobBuilder.job()
                    .task("transforming")
                    .action("join_tables")
                    .kwargs(k -> {
                        k.put("left_table", leftTable);
                        k.put("right_table", rightTable);
                        k.put("output_arrow", outputArrow);
                        k.put("on", "t1.hp_td_nhom_mon_hoc_id==t2.hp_td_nhom_mon_hoc_id");
                        k.put("how", "left");
                        k.put("join_nulls", true);
                        k.put("compression", "lz4");
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
- *how*: String (join style: "inner", "left", "outer", "semi", "anti", "cross", "outer_coalesce")
- *join_nulls*: Boolean (True if match null values)
- *compression*: String (Compressing level: "uncompressed", "lz4", "zstd")


