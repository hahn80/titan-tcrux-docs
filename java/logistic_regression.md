# Logistic Regression


Run the service to train the logistic model for banking problem.

The output will be a Hashmap with score, recall, precision, f1 scores, and saved_model

```java
public class LogisticRegressionTest extends BaseTest {

    @Test
    public void testLogisticRegress() {

        try (PythonInterpreter interpreter = new PythonInterpreter(config)) {
            interpreter.exec("from titan.tcrux.services import tcrux_multi_tasking");

            String inputArrow = projectDir + "/src/tests/resources/logistic.arrow";
            String outputArrow = projectDir + "/src/tests/resources/output.arrow";
            String modelSavedPath = projectDir + "/src/tests/resources/logistic_model.bin";

            ArrayList<HashMap<String, Object>> jobs = new ArrayList<>();

            // Job 1: loading
            jobs.add(
                JobBuilder.job()
                    .task("loading")
                    .action("load_dataframe")
                    .kwargs(k -> k.put("input_arrow", inputArrow))
                    .build()
            );

            // Job 2: logistic regression
            jobs.add(
                JobBuilder.job()
                    .task("regression")
                    .action("logistic_regression")
                    .kwargs(k -> {
                        k.put("target_column", "deposit");
                        k.put("stratify_column", "loan"); // optional, can be ignored.
                        k.put("model_saved_path", modelSavedPath);
                        k.put(
                            "processors", PyUtils.map(
                                "numerical_columns", PyUtils.list(
                                        "age",
                                        "balance",
                                        "day",
                                        "campaign",
                                        "pdays",
                                        "previous",
                                        "duration"
                                    ),
                                "categorical_columns", PyUtils.list(
                                        "job",
                                        "marital",
                                        "education",
                                        "default",
                                        "housing",
                                        "loan"
                                    ))
                            );
                    })
                    .build()
            );


            // System.out.println(jobs);
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

Trong gói tcrux, chúng ta cần load data trước khi thực hiện các tác vụ tiếp theo. Chúng ta cũng có thể save kết quả thành arrow file.

Giải thích tham số:

Bước đầu tiên là load_dataframe với `input_arrow`.
Sau khi load, data sẽ tự động được pass cho job tiếp theo.

- *target_column*: String (tên cột để dự đoán sau này, dữ liệu kiểu category)
- *stratify_column*: String (tên cột để phân chia dữ liệu cho cân bằng theo dữ liệu đã có của cột stratify_column, tham số này là optional và có thể bỏ qua)
- *model_saved_path*: String (path to save model)
- *processors*: Là một Hashmap phải chứa một trong hai keys sau: `numerical_columns` hoặc `categorical_columns`. Values cho các key này là list các tên cột có datatypes numerical hoặc category.
- 



