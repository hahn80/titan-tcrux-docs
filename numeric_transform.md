# Numeric Transform

Transforming numeric data supports all math functions and some interestingly complex functions as rolling windows.

The goal of this function is to transform a numeric data from column to a new column attached to the old dataframe. It will produce the same length dataframe with new columns.

With task transforming and action numeric_transform, we can generate a new data using all math functions: "exp", "log", "log10", "log1p", "pct_change", "rank", "sin", "tan", "cos"
cumulative function: "cum_sum", "cum_min", "cum_max"
rolling windows: "rolling_min", "rolling_max", "rolling_sum"
rolling by: This is an interesting function that will be introduced in another topic.

```java
public class NumericTransform {
    public static void main(String[] args) {
        // Create and configure the Python interpreter
        PythonInterpreterConfig config = PythonInterpreterConfig
            .newBuilder()
            .setPythonExec("python") // Specify the Python executable
            //.addPythonPaths("/data/TitanProjects/tcrux/src") // Specify if you need to add custom paths
            .build();

        // Use try-with-resources to ensure proper resource management
        try (PythonInterpreter interpreter = new PythonInterpreter(config)) {
            // Import Python worker
            interpreter.exec("from titan.tcrux.services import worker");
            
            // Create the HashMap for kwargs
            HashMap<String, Object> kwargs1 = new HashMap<>();
            kwargs1.put("input_arrow", "banking.arrow");

            // Create the main HashMap for params
            HashMap<String, Object> params1 = new HashMap<>();
            params1.put("task", "loading");
            params1.put("action", "load_dataframe");
            params1.put("kwargs", kwargs1);
            
            
            Object data_obj = interpreter.invoke("worker", params1);
            
            
            
            // Create the first operation
            Map<String, Object> operation1 = new HashMap<>();
            operation1.put("func", "cum_sum");
            operation1.put("alias", "balance_cs");
            operation1.put("args", new ArrayList<>()); // Empty list for args

            // Create the second operation
            List<Object> arg2 = Arrays.asList(7);
            
            Map<String, Object> operation2 = new HashMap<>();
            operation2.put("func", "rolling_mean");
            operation2.put("alias", "balance_rolm");
            operation2.put("args", arg2);

            // Create the list of operations for "num"
            List<Map<String, Object>> balanceOperations = new ArrayList<>();
            balanceOperations.add(operation1);
            balanceOperations.add(operation2);

            // Create the final operations map
            //duration
            Map<String, List<Map<String, Object>>> operations = new HashMap<>();
            operations.put("balance", balanceOperations);

            
            
            // Create the HashMap for kwargs
            HashMap<String, Object> kwargs2 = new HashMap<>();
            kwargs2.put("data", data_obj);
            kwargs2.put("operations", operations);

            // Create the main HashMap for params
            HashMap<String, Object> params2 = new HashMap<>();
            params2.put("task", "transforming");
            params2.put("action", "numeric_transform");
            params2.put("kwargs", kwargs2);
            
            data_obj = interpreter.invoke("worker", params2);
                        
            // Create the HashMap for kwargs
            //*
            HashMap<String, Object> kwargsN = new HashMap<>();
            kwargsN.put("data", data_obj);
            kwargsN.put("output_arrow", "output.arrow");
            kwargsN.put("batch_size", 20);

            // Create the main HashMap for params
            HashMap<String, Object> paramsN = new HashMap<>();
            paramsN.put("task", "saving");
            paramsN.put("action", "write_dataframe");
            paramsN.put("kwargs", kwargsN);
            //*/
            
            /*
            HashMap<String, Object> kwargsN = new HashMap<>();
            kwargsN.put("data", data_obj);

            // Create the main HashMap for params
            HashMap<String, Object> paramsN = new HashMap<>();
            paramsN.put("task", "testing");
            paramsN.put("action", "view_as_json");
            paramsN.put("kwargs", kwargsN);
            */
            
            
            Object file_obj = interpreter.invoke("worker", paramsN);
                
            // Now you can use the object
            System.out.println(file_obj);
            

        } catch (Exception e) {
            e.printStackTrace(); // Print any exceptions
        }
    }
}
```

