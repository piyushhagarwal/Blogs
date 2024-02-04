---
title: "Java Exception Handling: A Comprehensive Guide"
seoTitle: "Java Exception Handling: A Comprehensive Guide"
seoDescription: "The blog walks through the basics of handling errors in Java. It explains how to use try and catch blocks to manage different types of errors."
datePublished: Sat Oct 21 2023 11:29:06 GMT+0000 (Coordinated Universal Time)
cuid: clnzyiy6v000808jtcovxg5vp
slug: java-exception-handling-a-comprehensive-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1697887554895/8e42b8de-79e5-4077-af67-36c9b8757faa.png
tags: java, error-handling

---

I had a lot of questions regarding this subject as a novice. I had no idea how to respond to exceptions or deal with them. The terms `try`, `catch`, `throw`, `throws` and `finally` completely overwhelmed me. I want to make sure you fully understand error handling, or "exception handling" as it is called in Java, in this blog post.

> ### Types of errors

1. ***Compile time errors***
    
    These are errors that don't let your code compile. Some of the common examples are syntactical errors or usage of undeclared variables.  
    These errors are easy to solve because the compiler helps or guides you on how to solve them.
    
    ```java
    System.out.prrint("Hello World"); // print is misspelled
    
    int a = 5;
    System.out.println(b); // Use of undeclared variable
    ```
    
2. ***Run time errors***
    
    Even if everything is working fine and your application is running, there might be some instances where your application suddenly crashes.  
    Let's discuss some common instances where run time occurs:
    
    1. Out-of-bound index error:
        
        Suppose you create an array of size 5 and your user tries to access the element at the 6th index.
        
        ```java
        int arr[] = { 1, 2, 3, 4, 5 };
        System.out.println(arr[6]);
        // Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 6 out of bounds for length 5
        ```
        
    2. Divide by zero:
        
        Suppose you create a program to divide 2 numbers and you put the denominator as 0.
        
        ```java
        int i = 10;
        int j = 0;
        int num = i / j;
        System.out.println(num);
        // Exception in thread "main" java.lang.ArithmeticException: / by zero
        ```
        
    3. Opening a non-existing file:
        
        If you try to open or access a file that doesn't exist.
        
3. ***Logical errors***
    
    Logical errors allow the program to run but lead to unexpected or incorrect results. These errors arise when the code does not fulfil its intended purpose due to faulty logic or algorithmic mistakes. We can write tests to reduce logical errors significantly.
    

---

Let's see with an example how **Run time** errors or exceptions terminate the program or application abruptly.

```java
System.out.println("Before exception");
int arr[] = { 1, 2, 3, 4, 5 };
int num = arr[6]; // Trying to access element which is out of bound
System.out.println("After exception");

//Output:

//Before exception
//Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 6 out of bounds for length 5
```

As you can see "**After exception**" was not printed as the program terminated after an exception was encountered.  
If the application is a basic application we wouldn't mind restarting it, but if the application is crucial we wouldn't want to restart it again.  
This is the reason we should handle exceptions in our code. For example, we can handle the situation when the user is trying to access the index that is out of the bound of the array.

> ### Now let's understand how to handle these exceptions

Let us see some important keywords:

1. ### `try`
    
    We should wrap the critical statements that might cause exceptions inside the try block.
    
2. ### `catch`
    
    If any exception is encountered in the try block it is handled in the catch block.
    

```java
System.out.println("Before exception");
int arr[] = { 1, 2, 3, 4, 5 };
try {
     int num = arr[6]; // Trying to access element which is out of bound
}catch (Exception e) {
     System.out.println("Some exception occured " + e );
}
System.out.println("After exception");

// Output:

//Before exception
//Some exception occured: java.lang.ArrayIndexOutOfBoundsException: Index 6 out of bounds for length 5
//After exception
```

As we can see, the program is still running even after encountering the exception.

> In the catch block, what does the object `Exception e` represent?

**Exception**: This is the base class for all exceptions in Java.  
**e**: This is just a variable name, and you can choose any name of your choice.

Many subclasses of the Exception class represent several types of exceptions. Some of the most common exception subclasses are:

1. **ArithmeticException:**  
    Thrown when an arithmetic operation has an exceptional condition, such as division by zero.
    
2. **NullPointerException:**  
    Thrown when attempting to access an object that is `null`.
    
3. **ArrayIndexOutOfBoundsException:**
    
    Thrown when attempting to access an array element with an illegal index.
    
4. **IOException:**
    
    The base class for exceptions that can occur during I/O operations.
    
5. **SQLException:**
    
    Represents an exception that is related to database access.
    

We can have multiple catch blocks with a single try block. It is advisable to specify the precise exception subclass when handling errors instead of relying on the broader Exception class.  
Now let's understand this condition with an example. Suppose we have multiple critical statements which can cause an error in a single try block.

```java
int arr[] = { 1, 2, 3, 4, 5 };
int i = 10;
int j = 0;
try {
    int num1 = arr[6]; // Trying to access element which is out of bound
    int num2 = i / j; // Trying to divide by zero
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("The index you are trying to access is out of bound");
} catch(ArithmeticException e){
    System.out.println("You are trying to divide by zero");
} catch(Exception e){
    System.out.println("Some other exception");
}
```

Whenever we encounter an exception, handling it more gracefully is possible by knowing the type of exception instead of using the generalized Exception class. This approach improves clarity and makes debugging easier.

> There are certain **checked** exceptions, such as **SQLException** and **IOException**, that must be handled compulsorily; otherwise, our code will not compile.

We can also handle exceptions gracefully instead of just displaying the error messages.

```java
int i = 10;
int j = 0;
int num = 0;
try {
    num = i / j; // Trying to divide by zero
} catch(ArithmeticException e){
    //Dividing the number by 1 if the denominator is 0
    num = i/1;
}
System.out.println("The value of num is " + num);

// Output 
// The value of num is 10
```

---

### `throw` Keyword

> What if you want to explicitly throw an error in your code?

You use `throw` when you want to create and throw a specific exception yourself. This is useful when a method encounters a situation it knows it cannot handle, and it wants to communicate this to the calling code.

```java
public static int performDivision(int dividend, int divisor) {
    if (divisor == 0) {
        // Explicitly throwing an ArithmeticException with a custom message
        throw new ArithmeticException("Cannot divide by zero");
     }
     // Perform the division if divisor is not zero
     return dividend / divisor;
}
```

Here, you're not relying on the automatic throwing of an exception by Java; instead, you're deciding when and what type of exception to throw. This approach provides greater control over the code and enhances its understandability and readability.

You have the flexibility to create custom exception classes when needed. For instance, consider a scenario where you want to validate the age of a person and throw an exception if a user attempts to input a negative age.

```java
// Custom exception class for invalid age
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

// Main class to demonstrate age validation
public class AgeValidationExample {

    public static void main(String[] args) {
        try {
            // Simulating user input for age
            int userAge = -5;

            // Validate the age
            if (userAge < 0) {
                // If the age is negative, throw InvalidAgeException
                throw new InvalidAgeException("Age cannot be negative");
            }

            // If the age is valid, proceed with further processing
            System.out.println("Age is valid. Proceeding with further processing.");

        } catch (InvalidAgeException e) {
            // Catch and handle the custom exception
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

In this example, the code may throw a custom exception (`InvalidAgeException`) based on a condition. The `throw` keyword is used inside the conditional block to throw the exception when the condition is met.

### `throws` Keyword

When creating a function or method with critical statements, you might choose not to handle exceptions within the method itself. Instead, you can handle them where the method is being called, and the `throws` keyword is used for this purpose.

```java
public class Example {

    // A method that declares it may throw an exception
    public static void divide(int numerator, int denominator) throws ArithmeticException {

        // This statement is a critical statement and may know an exception
        int result = numerator / denominator;
        System.out.println("Result: " + result);
    }

    public static void main(String[] args) {
        try {           
            // Try dividing by zero to trigger the exception
            divide(5, 0);
        } catch (ArithmeticException e) {
            // Handle the exception at the calling location
            System.out.println("Exception caught: " + e);
        }
    }
}

// Output
//Exception caught: java.lang.ArithmeticException: / by zero
```

In this example, the `divide` method declares that it may throw an `ArithmeticException` by using the `throws` keyword.  
Another way to express this is that we are effectively ***ducking*** exceptions by using the `throws` keyword. However, it's worth noting that using the throws keyword with the `main` method of the code is generally considered bad practice.

### `finally` Keyword

The `finally` block is used in association with a `try-catch` block. It contains code that will be executed regardless of whether an exception is thrown or not.

```java
int i = 10;
int j = 0;
int num = 0;
try {
    num = i / j; // Trying to divide by zero
} catch(ArithmeticException e){
    System.out.println("Exception caught: " + e);
} finally{
    System.out.println("Finally block executed");
}


// Output 
// Exception caught: java.lang.ArithmeticException: / by zero
// Finally block executed
```

The `finally` block is commonly used for cleanup or resource release operations. This is crucial for managing resources efficiently and preventing resource leaks.  
Let's consider an example involving file handling where you need to close a file regardless of whether an exception occurs or not.

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class FileReadExample {

    public static void main(String[] args) {
        BufferedReader reader = null;

        try {
            // Opening a file for reading
            reader = new BufferedReader(new FileReader("example.txt"));
        } catch (IOException e) {
            // Handling IOException
            System.out.println("Error reading the file: " + e.getMessage());
        } finally {
            try {
                // Closing the file in the finally block to ensure it happens regardless of exceptions
                if (reader != null) {
                    reader.close();
                    System.out.println("File closed successfully.");
                }
            } catch (IOException e) {
                // Handling IOException when closing the file
                System.out.println("Error closing the file: " + e.getMessage());
            }
        }
    }
}
```

In Java, starting from Java 7, you can use the try-with-resources statement to simplify resource management. This allows you to automatically close resources (like files) when they are no longer needed, and you don't need to explicitly use a `finally` block.

You can modify the above example using try-with-resources

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class FileReadExample {

    public static void main(String[] args) {
        // Using try-with-resources to automatically close the BufferedReader
        try (BufferedReader reader = new BufferedReader(new FileReader("example.txt"))) {
            // Code for reading from the file
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            // Handling IOException
            System.out.println("Error reading the file: " + e.getMessage());
        }
    }
}
```

When the `try` block is exited, either normally or due to an exception, the `close` method of the `BufferedReader` is automatically called, ensuring that the resource is closed properly. This is possible because `BufferedReader` implements the `AutoCloseable` interface.

---

In writing this blog, I drew inspiration and valuable insights from this [YouTube Playlist](https://youtube.com/playlist?list=PLsyeobzWxl7pe_IiTfNyr55kwJPWbgxB5&si=_T6cesBjRFbAnTmu).

### Summary

The blog walks through the basics of handling errors in Java. It explains how to use `try` and `catch` blocks to manage different types of errors, and introduces terms like `throw` and `throws`. The blog emphasizes the importance of dealing with errors explicitly, especially in critical programs. By using these techniques, Java developers can prevent crashes and avoid common mistakes. The blog also highlights the `finally` block for cleanup and mentions a modern approach called try-with-resources for better resource management.

**Happy coding!**