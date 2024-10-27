```java
// This is a simple Java program
public class HelloWorld {
    // Main method: Entry point of the program
    public static void main(String[] args) {
        // Print statement
        System.out.println("Hello, World!");  // Output: Hello, World!
    }
}

```
# Variables and datatypes
```java
public class VariablesExample {
    public static void main(String[] args) {
        int age = 25;
        double height = 5.9;
        char grade = 'A';
        boolean isJavaFun = true;
        String name = "John Doe";
        
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Height: " + height);
        System.out.println("Grade: " + grade);
        System.out.println("Is Java Fun? " + isJavaFun);
    }
}

```
# Conditions
```java
public class ConditionExample {
    public static void main(String[] args) {
        int number = 10;

        if (number > 0) {
            System.out.println("Positive number");
        } else if (number < 0) {
            System.out.println("Negative number");
        } else {
            System.out.println("Zero");
        }
    }
}

```
# Loops
```java
public class ForLoopExample {
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Count: " + i);
        }
    }
}

```
```java
public class WhileLoopExample {
    public static void main(String[] args) {
        int i = 1;
        while (i <= 5) {
            System.out.println("Count: " + i);
            i++;
        }
    }
}

```
# Functions (Methods)
```java
public class MethodExample {
    public static void main(String[ ] args) {
        int result = addNumbers(5, 10);
        System.out.println("Result: " + result);
    }
    
    // Method to add two numbers
    public static int addNumbers(int a, int b) {
        return a + b;
    }
}

```
# Static Variables (Class Variables)
A static variable belongs to the class rather than any specific instance (object) of the class. This means all instances of the class share the same copy of the static variable, as opposed to each object having its own independent copy.

Example:
```java
public class Car {
    // Static variable
    static int numberOfCars = 0;
    
    // Instance variable
    String model;

    // Constructor
    public Car(String model) {
        this.model = model;
        numberOfCars++;  // Increment the static variable
    }

    public static void main(String[] args) {
        Car car1 = new Car("Toyota");
        Car car2 = new Car("Honda");
        
        // Accessing the static variable
        System.out.println("Number of cars: " + Car.numberOfCars);
    }
}

```
# Static Methods
A **static method** belongs to the class rather than any object of the class. Static methods can be called without creating an object of the class and can only access static data (i.e., static variables or other static methods) directly.

```java
public class Calculator {
    // Static method
    public static int add(int a, int b) {
        return a + b;
    }

    public static void main(String[] args) {
        // No need to create an object to call a static method
        int result = Calculator.add(5, 10);
        System.out.println("Sum: " + result);
    }
}

```
# Static Block 

A static block (also known as a static initializer) is used to initialize static variables or execute any code that needs to run once when the class is loaded into memory. It runs before any static method or variable is accessed.

Example:
```java
public class StaticBlockExample {
    // Static variable
    static int value;

    // Static block
    static {
        value = 100;
        System.out.println("Static block executed. Value initialized to " + value);
    }

    public static void main(String[] args) {
        System.out.println("Main method executed. Value: " + value);
    }
}

```
```OUTPUT
Static block executed. Value initialized to 100
Main method executed. Value: 100

```
# Arrays

```java
// Declaration: dataType[] arrayName;
int[] numbers;

// Instantiation: arrayName = new dataType[size];
numbers = new int[5];  // Array of 5 integers

//or
int[] numbers = new int[5];  // Declare and instantiate an array of size 5

//or

int[] numbers = {10, 20, 30, 40, 50};  // Array with 5 elements


```
# 3. Accessing Elements in an Array
```java
System.out.println("First element: " + numbers[0]);  // Output: 10
```
Length
```java
int[] numbers = {10, 20, 30, 40, 50};
System.out.println("Array length: " + numbers.length);  // Output: 5

```

For Each 
```java
public class EnhancedForLoopArray {
    public static void main(String[] args) {
        int[] numbers = {10, 20, 30, 40, 50};
        
        // Enhanced for loop
        for (int number : numbers) {
            System.out.println("Element: " + number);
        }
    }
}

```
# 6. Multidimensional Arrays
```java
int[][] matrix = new int[3][3];  // 3x3 array (matrix)

```
You can also init a 2D array with values:
```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

```
Accessing 2D array
```java
public class MultiDimensionalArray {
    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        
        // Access element at row 1, column 2
        System.out.println("Element at row 1, column 2: " + matrix[1][2]);  // Output: 6
    }
}

```
#### Iterating Over a 2D Array:
```java
public class Iterate2DArray {
    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        
        // Nested loops to iterate over a 2D array
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();  // Newline after each row
        }
    }
}

```
# 1. Copying an Array:
```java
int[] original = {1, 2, 3};
int[] copy = new int[original.length];
System.arraycopy(original, 0, copy, 0, original.length);

for (int i : copy) {
    System.out.println(i);  // Output: 1 2 3
}

```
# 2. Sorting an array
```java
import java.util.Arrays;

public class SortArray {
    public static void main(String[] args) {
        int[] numbers = {5, 2, 8, 1, 3};
        Arrays.sort(numbers);
        
        for (int number : numbers) {
            System.out.println(number);  // Output: 1 2 3 5 8
        }
    }
}

```
