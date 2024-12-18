# Calculate the statistical information of the given data


Below is the core of the implementation. Let's ignore the loading and saving tasks. We will explain the middle step to calculate the statistical information:

Use the task: `statistics` and the action: `calculate_stats`. We need to pass a dataframe object to this function and the `operations` is a HashMap that maps a name of a column to the list of operators. Here is the json data of the operations:

```json
{
	"col_1": [{"func": "mean", "alias": "col_1_mean_alias", "args": []}, {"func": "std", "alias": "col_1_std_alias", "args": []}],
	"col_2": [{"func": "quantile", "alias": "col_1_quan_alias", "args": [0.25, "linear"]}, {"func": "median", "alias": "col_2_med_alias", "args": []}],
}
```

For each column, we can pass multiple operators to do the stats computation for that column. Make sure you alias the name to void the error of getting duplicate column names.
Here is the list of supported function for statistics.
["median", "mean", "mode", "std"]
(will be updated more later)



```java
public class CalculateStats {
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
            Map<String, Object> meanOperation = new HashMap<>();
            meanOperation.put("func", "mean");
            meanOperation.put("alias", "balance_mean");
            meanOperation.put("args", new ArrayList<>()); // Empty list for args

            // Create the second operation
            List<Object> quantileArgs = Arrays.asList(0.3, "linear");
            
            Map<String, Object> quantileOperation = new HashMap<>();
            quantileOperation.put("func", "quantile");
            quantileOperation.put("alias", "balance_quantile");
            quantileOperation.put("args", quantileArgs);

            // Create the list of operations for "num"
            List<Map<String, Object>> numOperations = new ArrayList<>();
            numOperations.add(meanOperation);
            numOperations.add(quantileOperation);

            // Create the final operations map
            //duration
            Map<String, List<Map<String, Object>>> operations = new HashMap<>();
            operations.put("balance", numOperations);

            
            
            // Create the HashMap for kwargs
            HashMap<String, Object> kwargs2 = new HashMap<>();
            kwargs2.put("data", data_obj);
            kwargs2.put("operations", operations);

            // Create the main HashMap for params
            HashMap<String, Object> params2 = new HashMap<>();
            params2.put("task", "statistics");
            params2.put("action", "calculate_stats");
            params2.put("kwargs", kwargs2);
            
            data_obj = interpreter.invoke("worker", params2);
                        
            // Create the HashMap for kwargs
            HashMap<String, Object> kwargsN = new HashMap<>();
            kwargsN.put("data", data_obj);
            kwargsN.put("output_arrow", "output.arrow");
            kwargsN.put("batch_size", 20);

            // Create the main HashMap for params
            HashMap<String, Object> paramsN = new HashMap<>();
            paramsN.put("task", "saving");
            paramsN.put("action", "write_dataframe");
            paramsN.put("kwargs", kwargsN);
            
            
            Object file_obj = interpreter.invoke("worker", paramsN);
                
            // Now you can use the object
            System.out.println(file_obj);
            

        } catch (Exception e) {
            e.printStackTrace(); // Print any exceptions
        }
    }
}
```
