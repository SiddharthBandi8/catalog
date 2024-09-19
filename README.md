# catalog
import org.json.JSONObject;

public class PolynomialConstantTerm {

    public static void main(String[] args) {
        // Sample JSON input
        String jsonInput = "{ \"keys\": { \"n\": 4, \"k\": 3 }, \"1\": { \"base\": \"10\", \"value\": \"4\" }, " +
                           "\"2\": { \"base\": \"2\", \"value\": \"111\" }, \"3\": { \"base\": \"10\", \"value\": \"12\" }, " +
                           "\"6\": { \"base\": \"4\", \"value\": \"213\" } }";
        
        JSONObject json = new JSONObject(jsonInput);
        
        // Extract number of roots
        int n = json.getJSONObject("keys").getInt("n");

        // Array to hold roots
        double[] roots = new double[n];

        // Parse roots from JSON
        for (int i = 1; i <= n; i++) {
            JSONObject rootObj = json.getJSONObject(String.valueOf(i));
            int base = rootObj.getInt("base");
            String value = rootObj.getString("value");

            // Convert the value from the specified base to decimal
            roots[i - 1] = convertToDecimal(value, base);
        }

        // Calculate the constant term c
        double constantTerm = calculateConstantTerm(roots);
        
        // Output the result
        System.out.println("The constant term c of the polynomial is: " + constantTerm);
    }

    // Method to convert a number from a given base to decimal
    private static double convertToDecimal(String value, int base) {
        double decimalValue = 0;
        int length = value.length();
        
        for (int i = 0; i < length; i++) {
            int digit = Character.getNumericValue(value.charAt(length - 1 - i));
            decimalValue += digit * Math.pow(base, i);
        }
        
        return decimalValue;
    }

    // Method to calculate the constant term of the polynomial
    private static double calculateConstantTerm(double[] roots) {
        double sumOfProducts = 0;

        // Calculate the sum of products of roots taken two at a time
        for (int i = 0; i < roots.length; i++) {
            for (int j = i + 1; j < roots.length; j++) {
                sumOfProducts += roots[i] * roots[j];
            }
        }

        // The constant term c is the negative sum of the products
        return -sumOfProducts;
    }
}
