# TCRUX Documentation

A new framework to process data and do analysis and statistics for iTitan.

Here is the list of implemented functions:

1. Transforming data for numeric, datetime data, and also categorical data (of the text form)
2. New function for describe statistis
3. Collective functions to do analysis and statistics from given dataframe.


The interface of the framework: Below is the snip code to run a basic welcome function test with the framework.

Import necessary java libs and then jcpy:

```java
import jcpy.engine.PythonInterpreter;
import jcpy.engine.PythonException;
import jcpy.engine.PythonInterpreterConfig;
import jcpy.engine.object.PyObject;
```


Now we can interact with API as follows:

```java
public class Welcome {
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
            HashMap<String, String> kwargs = new HashMap<>();
            kwargs.put("name", "iTitan");

            // Create the main HashMap for params
            HashMap<String, Object> params = new HashMap<>();
            params.put("task", "testing");
            params.put("action", "welcome");
            params.put("kwargs", kwargs);
            
            
            Object result = interpreter.invoke("worker", params);
                
            // Now you can use the object
            System.out.println(result);
            

        } catch (Exception e) {
            e.printStackTrace(); // Print any exceptions
        }
    }
}
```
