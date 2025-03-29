# Run analysis service with java


Mục tiêu: Chạy phân tích trực tiếp trên Java, kết quả trả về sẽ tự động kiểu Primitive của Java, hoặc Array, hoặc Hashmap.


## Chạy kết quả trả về là một giá trị.

Áp dụng được cho tất cả các hàm chỉ trả về một giá trị duy nhất, ví dụ `sum`, `max`, `min`, `mean`...


Mẫu code như sau:

```java
import jcpy.engine.PythonInterpreter;
import jcpy.engine.PythonException;
import jcpy.engine.PythonInterpreterConfig;
import jcpy.engine.object.PyObject;

public class JavaTransform {
    public static void main(String[] args) {
        // Create and configure the Python interpreter
        PythonInterpreterConfig config = PythonInterpreterConfig
            .newBuilder()
            .setPythonExec("python")
            .build();

        // Use try-with-resources to ensure proper resource management
        try (PythonInterpreter interpreter = new PythonInterpreter(config)) {
            // Import Python worker
            interpreter.exec("from titan.tcrux.services import tcrux_java");
            
            // Create the HashMap for kwargs
            Map<String, Object> params = new HashMap<>();

            // Add basic key-value pairs
            params.put("input", "banking.arrow");

            // Create the task list
            List<Map<String, Object>> tasks = new ArrayList<>();

            // Create the task map
            Map<String, Object> task = new HashMap<>();
            task.put("task", "transforming");
            task.put("action", "java_transform");

            // Create the kwargs map
            Map<String, Object> kwargs = new HashMap<>();

            // Create operations map
            Map<String, List<Map<String, String>>> operations = new HashMap<>();

            // Define the operations for age
            List<Map<String, String>> ageOps = new ArrayList<>();
            Map<String, String> ageOp = new HashMap<>();

            //*******************************//

            //-- Single function --
            // sum; mean; max; min; first; last

            ageOp.put("func", "mean");
            ageOp.put("alias", "new_name");
            
            ageOps.add(ageOp);
            
            operations.put("age", ageOps);
            
            //*******************************//

            // Add operations map to kwargs
            kwargs.put("operations", operations);

            // Add kwargs to task
            task.put("kwargs", kwargs);

            // Add task to tasks list
            tasks.add(task);

            // Add tasks to params
            params.put("tasks", tasks);
            
            
            Object obj = interpreter.invoke("tcrux_java", params);
                
            // Now you can use the object
            System.out.println(obj);
            System.out.println(obj.getClass().getName());
            

        } catch (Exception e) {
            e.printStackTrace(); // Print any exceptions
        }
    }
}
```

**Output:**

40.93621021432837
java.lang.Double


**Danh sách các hàm hỗ trợ:**

- No params: max, min, sum, mean, first, last, mode, count, len, median, n_unique, var, std, skew, kurtosis
- Support all math functions
- Customized functions: range, cvar, pearson_index

- Có tham số: Cập nhật sau


## Chạy kết quả trả về là một array.


```java
public class JavaTransform {
    public static void main(String[] args) {
        // Create and configure the Python interpreter
        PythonInterpreterConfig config = PythonInterpreterConfig
            .newBuilder()
            .setPythonExec("python")
            .build();

        // Use try-with-resources to ensure proper resource management
        try (PythonInterpreter interpreter = new PythonInterpreter(config)) {
            // Import Python worker
            interpreter.exec("from titan.tcrux.services import tcrux_java");
            
            // Create the HashMap for kwargs
            Map<String, Object> params = new HashMap<>();

            // Add basic key-value pairs
            params.put("input", "banking.arrow");

            // Create the task list
            List<Map<String, Object>> tasks = new ArrayList<>();

            // Create the task map
            Map<String, Object> task = new HashMap<>();
            task.put("task", "transforming");
            task.put("action", "java_transform");

            // Create the kwargs map
            Map<String, Object> kwargs = new HashMap<>();

            // Create operations map
            Map<String, List<Map<String, String>>> operations = new HashMap<>();

            // Define the operations for age
            List<Map<String, String>> ageOps = new ArrayList<>();
            Map<String, String> ageOp = new HashMap<>();

            //*******************************//

            //-- Single function --
            // sum; mean; max; min; first; last

            ageOp.put("func", "unique");
            ageOp.put("alias", "new_name");
            
            ageOps.add(ageOp);
            
            operations.put("education", ageOps);
            
            //*******************************//

            // Add operations map to kwargs
            kwargs.put("operations", operations);

            // Add kwargs to task
            task.put("kwargs", kwargs);

            // Add task to tasks list
            tasks.add(task);

            // Add tasks to params
            params.put("tasks", tasks);
            
            
            Object obj = interpreter.invoke("tcrux_java", params);
                
            // Now you can use the object
            System.out.println(obj);
            System.out.println(obj.getClass().getName());
            

        } catch (Exception e) {
            e.printStackTrace(); // Print any exceptions
        }
    }
}
```

**Output:**

[unknown, tertiary, primary, secondary]
java.util.ArrayList

**Danh sách các hàm hỗ trợ:**

Chú ý, hàm này chỉ chạy trên một column và kết quả trả về là một array:

- Các hàm không có tham số: unique, cum_count, cum_max, cum_min, cum_sum
- Các hàm được viết bổ sung: iqr_mask, iqr_outlier, z_score, z_score_mask, z_score_outlier, ci_lower, ci_upper, freq_value, freq_count, freq_percent, percent_rank


## Chạy kết quả trả về là một hashmap.

Hàm này hỗ trợ analysis nhiều cột cùng một lúc, dùng các hàm khác nhau trên một cột hoặc các cột khác nhau. Công cụ này rất hữu ích nếu muốn phân tích hoặc thực hiện đa tác vụ.

```java
public class JavaTransform {
    public static void main(String[] args) {
        // Create and configure the Python interpreter
        PythonInterpreterConfig config = PythonInterpreterConfig
            .newBuilder()
            .setPythonExec("python")
            .build();

        // Use try-with-resources to ensure proper resource management
        try (PythonInterpreter interpreter = new PythonInterpreter(config)) {
            // Import Python worker
            interpreter.exec("from titan.tcrux.services import tcrux_java");
            
            // Create the HashMap for kwargs
            Map<String, Object> params = new HashMap<>();

            // Add basic key-value pairs
            params.put("input", "banking.arrow");

            // Create the task list
            List<Map<String, Object>> tasks = new ArrayList<>();

            // Create the task map
            Map<String, Object> task = new HashMap<>();
            task.put("task", "transforming");
            task.put("action", "java_transform");

            // Create the kwargs map
            Map<String, Object> kwargs = new HashMap<>();

            // Create operations map
            Map<String, List<Map<String, String>>> operations = new HashMap<>();

            // Define the operations for age
            List<Map<String, String>> ageOps = new ArrayList<>();
            Map<String, String> ageOp = new HashMap<>();

            //*******************************//

            //-- Single function --
            // sum; mean; max; min; first; last

            ageOp.put("func", "mean");
            ageOp.put("alias", "new_name");
            
            ageOps.add(ageOp);
            
            operations.put("age", ageOps);
			
			// Define the operations for education
            List<Map<String, String>> educationOps = new ArrayList<>();
            Map<String, String> educationOp = new HashMap<>();
            educationOp.put("func", "mode");
            educationOp.put("alias", "edu_mode");
            educationOps.add(educationOp);

            operations.put("education", educationOps);
            
            //*******************************//

            // Add operations map to kwargs
            kwargs.put("operations", operations);

            // Add kwargs to task
            task.put("kwargs", kwargs);

            // Add task to tasks list
            tasks.add(task);

            // Add tasks to params
            params.put("tasks", tasks);
            
            
            Object obj = interpreter.invoke("tcrux_java", params);
                
            // Now you can use the object
            System.out.println(obj);
            System.out.println(obj.getClass().getName());
            

        } catch (Exception e) {
            e.printStackTrace(); // Print any exceptions
        }
    }
}
```

**Output:**

{edu_mode=secondary, new_name=40.93621021432837}
java.util.HashMap
