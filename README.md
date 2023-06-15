# Sales_Prediction
import org.json.JSONObject;

import java.util.*;

public class JsonSchemaCycleDetector {

    public static boolean hasCycle(JSONObject schema) {

        Set<Object> visited = new HashSet<>();

        Stack<Object> stack = new Stack<>();

        for (String key : schema.keySet()) {

            Object value = schema.get(key);

            if (detectCycle(value, visited, stack))

                return true;

        }

        return false;

    }

    private static boolean detectCycle(Object current, Set<Object> visited, Stack<Object> stack) {

        if (visited.contains(current))

            return true;

        visited.add(current);

        stack.push(current);

        if (current instanceof JSONObject) {

            JSONObject obj = (JSONObject) current;

            for (String key : obj.keySet()) {

                Object value = obj.get(key);

                if (detectCycle(value, visited, stack))

                    return true;

            }

        }

        stack.pop();

        return false;

    }

    public static void main(String[] args) {

        // Example usage

        String jsonString = "{ \"property1\": { \"property2\": { \"property3\": { \"property1\": {} } } } }";

        JSONObject schema = new JSONObject(jsonString);

        if (hasCycle(schema))

            System.out.println("The JSON schema contains cycles.");

        else

            System.out.println("The JSON schema does not contain cycles.");

    }

}
