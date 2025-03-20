# **Java Basics: Functions and Methods**

## **1. What is a Method?**
A **method** is a block of code that performs a specific task. Methods are used to:
- **Avoid code repetition** by reusing blocks of code.
- **Improve readability** by breaking code into smaller logical units.
- **Facilitate debugging** by isolating different functionalities.

---

## **2. Defining a Method**
A method consists of:
- **Method Name** – A unique identifier.
- **Return Type** – The data type of the value returned (or `void` if no value is returned).
- **Parameters (Optional)** – Values passed to the method.
- **Method Body** – The block of code executed when the method is called.

### **Syntax of a Method**
```java
returnType methodName(parameters) {
    // Code to execute
    return value; // If returnType is not void
}
```

---

## **3. Calling a Method**
```java
public class MethodExample {
    static void sayHello() {
        System.out.println("Hello, Java!");
    }

    public static void main(String[] args) {
        sayHello(); // Calling the method
    }
}
```

---

## **4. Methods with Parameters**
```java
public class MethodWithParams {
    static void greet(String name) {
        System.out.println("Hello, " + name + "!");
    }

    public static void main(String[] args) {
        greet("Zack");
        greet("Alice");
    }
}
```

---

## **5. Methods with Return Values**
```java
public class ReturnExample {
    static int add(int a, int b) {
        return a + b;
    }

    public static void main(String[] args) {
        int sum = add(5, 3);
        System.out.println("Sum: " + sum);
    }
}
```

---

## **6. Method Overloading**
```java
public class OverloadingExample {
    static int multiply(int a, int b) {
        return a * b;
    }

    static double multiply(double a, double b) {
        return a * b;
    }

    public static void main(String[] args) {
        System.out.println("Multiply Integers: " + multiply(3, 4));
        System.out.println("Multiply Doubles: " + multiply(2.5, 3.5));
    }
}
```

---

## **7. Recursion (Calling a Method Within Itself)**
```java
public class RecursionExample {
    static int factorial(int n) {
        if (n == 1) return 1; // Base case
        return n * factorial(n - 1); // Recursive call
    }

    public static void main(String[] args) {
        System.out.println("Factorial of 5: " + factorial(5));
    }
}
```

---

## **8. Access Modifiers in Methods**
| Modifier | Description | Example |
|----------|-------------|---------|
| `public` | Accessible from anywhere | `public void display()` |
| `private` | Accessible only within the same class | `private void show()` |
| `protected` | Accessible in the same package and subclasses | `protected void run()` |
| No modifier (default) | Accessible only within the same package | `void test()` |

```java
public class AccessModifiersExample {
    public void publicMethod() {
        System.out.println("This is a public method.");
    }

    private void privateMethod() {
        System.out.println("This is a private method.");
    }

    public static void main(String[] args) {
        AccessModifiersExample obj = new AccessModifiersExample();
        obj.publicMethod();
        // obj.privateMethod(); // This will cause an error
    }
}
```

