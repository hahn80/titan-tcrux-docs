# Regression with Java


Tính toán chỉ số thống kê hồi qui của mô hình linear regression.

Import các gói cần thiết, chẳng hạn

```java
import jcpy.engine.PythonInterpreter;
import jcpy.engine.PythonException;
import jcpy.engine.PythonInterpreterConfig;
import jcpy.engine.object.PyObject;
```


1. Mô hình đơn giản:

Tham số để gọi service:

```java
public class LinearRegression {
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
            params.put("input", "/data/TitanProjects/tcrux/src/tests/banking.arrow");
            params.put("return_format", "java");

            // Create the task list
            List<Map<String, Object>> tasks = new ArrayList<>();

            // Create the task map
            Map<String, Object> task = new HashMap<>();
            task.put("task", "regression");
            task.put("action", "linear_regression");
            
            // Creating the kwargs HashMap
            Map<String, Object> kwargs = new HashMap<>();
            kwargs.put("columns", Arrays.asList("age", "balance", "duration"));
            kwargs.put("target", "pdays");
            kwargs.put("confidence_level", 0.95);
            
            // Adding kwargs to taskDetails
            task.put("kwargs", kwargs);
            
            // Adding tasks list
            tasks.add(task);
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

Kết quả output có dạng như sau:

| r2        | mae        | mse        | feature_names | coefficients | standard_errors | t_values   | p_values       | lower_bounds | upper_bounds |
|-----------|------------|------------|---------------|--------------|-----------------|------------|----------------|--------------|--------------|
| 0.000601  | 67.594755  | 10019.5169 | age           | -0.229497    | 0.044548        | -5.151685  | 2.592377e-07   | -0.316812    | -0.142182    |
| 0.000601  | 67.594755  | 10019.5169 | balance       | 0.000193     | 0.000155        | 1.238863   | 2.154025e-01   | -0.000112    | 0.000497     |
| 0.000601  | 67.594755  | 10019.5169 | duration      | -0.000701    | 0.001828        | -0.383603  | 7.012748e-01   | -0.004285    | 0.002882     |
| 0.000601  | 67.594755  | 10019.5169 | const         | 49.511376    | 1.935598        | 25.579364  | 0.000000e+00   | 45.717572    | 53.305181    |


**Giải thích kết quả:**

- r2: chỉ số R2 score cho mô hình, gần 1 là tốt nhất, gần 0 là bad.
- mae và mse là sai số của mô hình (càng nhỏ càng tốt)
- feature_names: List các cột được đưa vào regression (chương trình tự động thêm hệ số const). Trong bảng trên ta có thể tính:
target(pdays) = -0.229497 * age + 0.000193 * balance -0.000701 * duration + 49.511376
- p_values: dùng để kiểm định mô hình linear regression.
- lower_bounds và upper_bounds là khoảng tin cậy của hệ số regression.


2. Mô hình LASSO:

```java
public class LinearRegression {
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
            params.put("input", "/data/TitanProjects/tcrux/src/tests/banking.arrow");
            params.put("return_format", "java");

            // Create the task list
            List<Map<String, Object>> tasks = new ArrayList<>();

            // Create the task map
            Map<String, Object> task = new HashMap<>();
            task.put("task", "regression");
            task.put("action", "linear_regression");
            
            // Creating the kwargs HashMap
            Map<String, Object> kwargs = new HashMap<>();
            kwargs.put("columns", Arrays.asList("age", "balance", "duration"));
            kwargs.put("target", "pdays");
            kwargs.put("confidence_level", 0.95);
			// --
			kwargs.put("method", "lasso");
			kwargs.put("alpha", 0.1);
            
            // Adding kwargs to taskDetails
            task.put("kwargs", kwargs);
            
            // Adding tasks list
            tasks.add(task);
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

Chú thích tham số:

- *method*: String = "lasso"
- *alpha*: Tham số là regularization có giá trị float từ nhỏ 0.01 đến lớn 100. Default value = 0.1


3. Mô hình RIDGE:


```java
public class LinearRegression {
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
            params.put("input", "/data/TitanProjects/tcrux/src/tests/banking.arrow");
            params.put("return_format", "java");

            // Create the task list
            List<Map<String, Object>> tasks = new ArrayList<>();

            // Create the task map
            Map<String, Object> task = new HashMap<>();
            task.put("task", "regression");
            task.put("action", "linear_regression");
            
            // Creating the kwargs HashMap
            Map<String, Object> kwargs = new HashMap<>();
            kwargs.put("columns", Arrays.asList("age", "balance", "duration"));
            kwargs.put("target", "pdays");
            kwargs.put("confidence_level", 0.95);
			// --
			kwargs.put("method", "ridge");
			kwargs.put("alpha", 1.0);
            
            // Adding kwargs to taskDetails
            task.put("kwargs", kwargs);
            
            // Adding tasks list
            tasks.add(task);
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


Chú thích tham số:

- *method*: String = "ridge"
- *alpha*: Tham số là regularization có giá trị float từ nhỏ 0.01 đến lớn 100. Default value = 1.0



4. Mô hình Elastic Net:

```java
public class LinearRegression {
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
            params.put("input", "/data/TitanProjects/tcrux/src/tests/banking.arrow");
            params.put("return_format", "java");

            // Create the task list
            List<Map<String, Object>> tasks = new ArrayList<>();

            // Create the task map
            Map<String, Object> task = new HashMap<>();
            task.put("task", "regression");
            task.put("action", "linear_regression");
            
            // Creating the kwargs HashMap
            Map<String, Object> kwargs = new HashMap<>();
            kwargs.put("columns", Arrays.asList("age", "balance", "duration"));
            kwargs.put("target", "pdays");
            kwargs.put("confidence_level", 0.95);
			// --
			kwargs.put("method", "elastic_net");
			kwargs.put("alpha", 1.0);
            
            // Adding kwargs to taskDetails
            task.put("kwargs", kwargs);
            
            // Adding tasks list
            tasks.add(task);
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

Chú thích tham số:

- *method*: String = "elastic_net"
- *alpha*: Tham số là regularization có giá trị float từ nhỏ 0.01 đến lớn 100. Default value = 1.0



5. Phân tích covariance cho các mô hình hồi qui:

```java
public class LinearRegression {
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
            params.put("input", "/data/TitanProjects/tcrux/src/tests/banking.arrow");
            params.put("return_format", "java");

            // Create the task list
            List<Map<String, Object>> tasks = new ArrayList<>();

            // Create the task map
            Map<String, Object> task = new HashMap<>();
            task.put("task", "regression");
            task.put("action", "linear_regression");
            
            // Creating the kwargs HashMap
            Map<String, Object> kwargs = new HashMap<>();
            kwargs.put("columns", Arrays.asList("age", "balance", "duration"));
            kwargs.put("target", "pdays");
            kwargs.put("confidence_level", 0.95);
			// --
			kwargs.put("mode", "elastic_net");
			kwargs.put("alpha", 1.0);
            
            // Adding kwargs to taskDetails
            task.put("kwargs", kwargs);
            
            // Adding tasks list
            tasks.add(task);
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

Chú thích tham số:

- *mode*: String = "covariance_inverse"
- *alpha*: Tham số là regularization có giá trị float từ nhỏ 0.0 đến lớn 100. Default value = 0.01. Nếu giá trị alpha = 0 thì nó trở thành phân tích covariance cho mô hình hồi qui thông thường.

Result:

```
{duration=[5.4504190338126846E-11, -6.273649648674255E-13, 3.336792492883095E-10, -8.751828011303263E-8], sigma_squared=[10019.517002819226, 10019.517002819226, 10019.517002819226, 10019.517002819226], balance=[-6.764836375261917E-11, 2.410142486457621E-12, -6.273649648674255E-13, -3.520318672103922E-10], const=[-8.027174301130891E-6, -3.520318672103922E-10, -8.751828011303263E-8, 3.7378588991995144E-4], degrees_of_freedom=[45210.99962601577, 45210.99962601577, 45210.99962601577, 45210.99962601577], coefficients=[-0.22910355662944387, 1.925338392563619E-4, -6.971242524555053E-4, 49.493052879247564], feature_names=[age, balance, duration, const], age=[1.9800163019209837E-7, -6.764836375261917E-11, 5.4504190338126846E-11, -8.027174301130891E-6]}
```
