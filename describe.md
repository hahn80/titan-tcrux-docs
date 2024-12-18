# Describe Statistics


Check the ReadMe to see the preample of the interface. Below is the core of the implementation. There are 3 steps for this process.

Step 1: Load arrow to a dataframe. This can be done using task: loading; action: load_dataframe with kwargs: input_arrow to indicate the path to arrow file.
After step 1, you can obtain a dataframe object which can be use sequently for many purposes later.

Step 2: Run the decribe stats process: with task: statistics and action: describe; in this step we have to use the dataframe object from the first step.

Step 3: Save result to an arrow: This can be done with task: saving and action: write_dataframe; you can specify the batch_size if you want.

Notice: This process can be used for many tranformations in between step 1 and step 3. This makes the framework more versatile to do multiple tasks and actions continuously.

Explanation of the kwargs of describe function:
- *data*: pass a dataframe object from load_dataframe for this param.
- *metrics*: It can be null (by default) or a list of String. Here is the full list of supported metrics: ['count', 'null_count', 'mean', 'std', 'var', 'sum', 'min', '25%', '50%', '75%', 'max', 'skew', 'kurtosis', 'outliers', 'mode', 'n_unique', 'frequency']
The outliers and frequecy are more complex, and they will be mentioned at another function later.



```java
public class DescribeStats {
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
            
            
            // Create the HashMap for kwargs
            HashMap<String, Object> kwargs2 = new HashMap<>();
            kwargs2.put("data", data_obj);
            kwargs2.put("metrics", null);

            // Create the main HashMap for params
            HashMap<String, Object> params2 = new HashMap<>();
            params2.put("task", "statistics");
            params2.put("action", "describe");
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

