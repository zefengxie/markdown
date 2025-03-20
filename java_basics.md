# **Java Basics: Syntax, Data Types, and Control Flow Statements**

## 1. Variables and Data Types
In Java, a **variable** is a container that holds data. Every variable has a data type, which defines what kind of values it can store.

### Primitive Data Types in Java
Java has 8 primitive data types:

| Data Type | Size | Example | Description |
|-----------|------|---------|-------------|
| `byte` | 1 byte | `byte b = 10;` | Stores whole numbers from -128 to 127 |
| `short` | 2 bytes | `short s = 1000;` | Stores whole numbers from -32,768 to 32,767 |
| `int` | 4 bytes | `int i = 50000;` | Stores whole numbers from -2,147,483,648 to 2,147,483,647 |
| `long` | 8 bytes | `long l = 100000L;` | Stores very large whole numbers, must end with `L` |
| `float` | 4 bytes | `float f = 5.75f;` | Stores decimal numbers, must end with `f` |
| `double` | 8 bytes | `double d = 19.99;` | Stores decimal numbers with higher precision |
| `boolean` | 1 bit | `boolean isJavaFun = true;` | Stores `true` or `false` values |
| `char` | 2 bytes | `char letter = 'A';` | Stores a single character |

### Example: Declaring Variables
```java
public class DataTypesExample {
    public static void main(String[] args) {
        int age = 25;
        double price = 19.99;
        char grade = 'A';
        boolean isPassed = true;

        System.out.println("Age: " + age);
        System.out.println("Price: " + price);
        System.out.println("Grade: " + grade);
        System.out.println("Passed: " + isPassed);
    }
}
```

---

## 2. Operators in Java
### Arithmetic Operators
| Operator | Description | Example |
|----------|-------------|---------|
| `+` | Addition | `int sum = 5 + 3;` → `8` |
| `-` | Subtraction | `int diff = 5 - 3;` → `2` |
| `*` | Multiplication | `int prod = 5 * 3;` → `15` |
| `/` | Division | `int div = 10 / 2;` → `5` |
| `%` | Modulus (Remainder) | `int mod = 10 % 3;` → `1` |

### Logical Operators
| Operator | Description | Example |
|----------|-------------|---------|
| `&&` | Logical AND | `(5 > 3) && (8 > 5)` → `true` |
| `||` | Logical OR | `(5 > 3) || (8 < 5)` → `true` |
| `!` | Logical NOT | `!(5 > 3)` → `false` |

---

## 3. Control Flow Statements

### *.1 Conditional Statements

#### `if` Statement
```java
if (condition) {
    // Code to execute if condition is true
}
```

#### Example:
```java
public class IfExample {
    public static void main(String[] args) {
        int age = 20;
        
        if (age >= 18) {
            System.out.println("You are an adult.");
        }
    }
}
```

#### `if-else` Statement
```java
if (condition) {
    // Code to execute if condition is true
} else {
    // Code to execute if condition is false
}
```

#### `switch` Statement
```java
switch (expression) {
    case value1:
        // Code to execute
        break;
    case value2:
        // Code to execute
        break;
    default:
        // Code to execute if no cases match
}
```

---

### 3.2 Looping Statements

#### `for` Loop
```java
for (initialization; condition; update) {
    // Code to execute
}
```

#### `while` Loop
```java
while (condition) {
    // Code to execute
}
```

#### `do-while` Loop
```java
do {
    // Code to execute
} while (condition);
```

---

## 4. Input and Output in Java
Java provides the `Scanner` class to take input from the user.

```java
import java.util.Scanner;

public class UserInputExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter your name: ");
        String name = scanner.nextLine();

        System.out.print("Enter your age: ");
        int age = scanner.nextInt();

        System.out.println("Hello, " + name + "! You are " + age + " years old.");
        
        scanner.close();
    }
}
```

---

## 5. String Operations
### Basic String Methods
| Method | Description | Example |
|--------|-------------|---------|
| `length()` | Returns length of the string | `"Java".length()` → `4` |
| `toUpperCase()` | Converts to uppercase | `"java".toUpperCase()` → `"JAVA"` |
| `toLowerCase()` | Converts to lowercase | `"JAVA".toLowerCase()` → `"java"` |
| `charAt(index)` | Returns character at position | `"Java".charAt(1)` → `'a'` |
| `substring(start, end)` | Extracts part of a string | `"Java".substring(1,3)` → `"av"` |
| `contains("text")` | Checks if contains text | `"Hello".contains("el")` → `true` |

---

## 6. Type Casting
Type casting is converting one data type to another.

### Implicit Casting (Automatic)
```java
int num = 10;
double newNum = num; // int to double (automatic)
System.out.println(newNum); // Output: 10.0
```

### Explicit Casting (Manual)
```java
double price = 99.99;
int newPrice = (int) price; // double to int (manual)
System.out.println(newPrice); // Output: 99
```

---
