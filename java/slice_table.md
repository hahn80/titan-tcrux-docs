# Slice Table

Dịch vụ này dùng để  slice table from a big file.


```java
public class SliceTableTest extends BaseTest {

    @Test
    public void testSliceTable() {
        
        try (PythonInterpreter interpreter = new PythonInterpreter(config)) {
            interpreter.exec("from titan.tcrux.services import tcrux_multi_tasking");

            String inputArrow = projectDir + "/src/tests/resources/left.arrow";
            String outputArrow = projectDir + "/src/tests/resources/output.arrow";

            ArrayList<HashMap<String, Object>> jobs = new ArrayList<>();

            jobs.add(
                JobBuilder.job()
                    .task("transforming")
                    .action("slice_table")
                    .kwargs(k -> {
                        k.put("input_arrow", inputArrow);
                        k.put("output_arrow", outputArrow);
                        k.put("start_index", 1000);
                        k.put("end_index", 300000);
                        k.put("batch_size", 32000);
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

- *input_arrow*: String (Path to input arrow file)
- *output_arrow*: String (Path to output arrow file)
- *start_index*: INT (start index row)
- *end_index*: INT (end index row; can be bigger than actual number of rows)
- *batch_size*: INT (batch size to write arrow file)
- *compression*: String (Compressing level: "lz4", "zstd" or null)



