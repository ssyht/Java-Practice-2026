# Week 1: Java Fundamentals - Getting Started

## Day 1-2: Setup & Your First Program

### What is Java?
Java is a programming language that runs on almost any device. Think of it as a language computers understand. When you write Java code, you're giving instructions to the computer.

### Setting Up Java

**Option 1: Online (Easiest for beginners)**
- Use an online compiler like:
  - https://www.programiz.com/java-programming/online-compiler/
  - https://www.jdoodle.com/online-java-compiler/
  
**Option 2: Install on your computer**
1. Download JDK (Java Development Kit) from Oracle or use OpenJDK
2. Install an IDE (Integrated Development Environment):
   - **IntelliJ IDEA Community Edition** (Recommended)
   - VS Code with Java extensions
   - Eclipse

### Your First Program: Hello World

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

**Let's break this down line by line:**

1. `public class HelloWorld` - This creates a class (a container for code) named HelloWorld
   - The class name MUST match the file name (HelloWorld.java)
   
2. `public static void main(String[] args)` - This is the starting point of every Java program
   - Think of it as the "START" button
   - Every program needs exactly one main method
   
3. `System.out.println("Hello, World!");` - This prints text to the screen
   - `System.out` = the output system
   - `println` = print line (prints and moves to next line)
   - Text goes inside double quotes `""`
   - Every statement ends with a semicolon `;`

**Practice:**
1. Type this program and run it
2. Change "Hello, World!" to your name
3. Add another println statement to print your favorite color

---

## Day 3-4: Variables and Data Types

### What are Variables?
Variables are containers that store data. Think of them as labeled boxes where you can put different types of information.

### Basic Data Types

#### 1. **int** - Stores whole numbers
```java
int age = 25;
int year = 2024;
int temperature = -5;
```

#### 2. **double** - Stores decimal numbers
```java
double price = 19.99;
double pi = 3.14159;
double temperature = 98.6;
```

#### 3. **boolean** - Stores true or false
```java
boolean isRaining = true;
boolean isWeekend = false;
boolean hasPassed = true;
```

#### 4. **char** - Stores a single character
```java
char grade = 'A';
char initial = 'J';
char symbol = '$';
```
Note: Single quotes for char, double quotes for strings!

#### 5. **String** - Stores text (multiple characters)
```java
String name = "John Doe";
String message = "Welcome to Java!";
String email = "user@example.com";
```

### Complete Example Program

```java
public class VariablesDemo {
    public static void main(String[] args) {
        // Declaring and initializing variables
        int age = 23;
        double height = 5.9;
        char grade = 'A';
        boolean isStudent = true;
        String name = "Alice";
        
        // Printing variables
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Height: " + height + " feet");
        System.out.println("Grade: " + grade);
        System.out.println("Is Student: " + isStudent);
    }
}
```

**Output:**
```
Name: Alice
Age: 23
Height: 5.9 feet
Grade: A
Is Student: true
```

### Variable Naming Rules

✅ **Allowed:**
- Must start with a letter, underscore `_`, or dollar sign `$`
- Can contain letters, digits, underscores, dollar signs
- Examples: `age`, `firstName`, `total_price`, `_count`

❌ **Not Allowed:**
- Cannot start with a digit: `1stPlace` ❌
- Cannot use Java keywords: `int`, `class`, `public` ❌
- Cannot have spaces: `first name` ❌

**Best Practices:**
- Use camelCase: `firstName`, `totalAmount`, `isValid`
- Use meaningful names: `age` ✅ instead of `a` ❌
- Constants in UPPERCASE: `final int MAX_SIZE = 100;`

---

## Day 5-6: Basic Operations

### Arithmetic Operators

```java
public class ArithmeticDemo {
    public static void main(String[] args) {
        int a = 10;
        int b = 3;
        
        // Addition
        int sum = a + b;          // 13
        System.out.println("Sum: " + sum);
        
        // Subtraction
        int difference = a - b;   // 7
        System.out.println("Difference: " + difference);
        
        // Multiplication
        int product = a * b;      // 30
        System.out.println("Product: " + product);
        
        // Division
        int quotient = a / b;     // 3 (integer division!)
        System.out.println("Quotient: " + quotient);
        
        // Modulus (remainder)
        int remainder = a % b;    // 1
        System.out.println("Remainder: " + remainder);
        
        // Division with decimals
        double preciseDiv = (double) a / b;  // 3.333...
        System.out.println("Precise Division: " + preciseDiv);
    }
}
```

### Important: Integer Division!
```java
int result = 10 / 3;      // result = 3 (not 3.333!)
double result2 = 10.0 / 3;  // result2 = 3.333...
```

### Increment and Decrement

```java
int count = 5;

count++;    // count = 6 (same as count = count + 1)
count--;    // count = 5 (same as count = count - 1)

count += 3; // count = 8 (same as count = count + 3)
count -= 2; // count = 6 (same as count = count - 2)
count *= 2; // count = 12 (same as count = count * 2)
count /= 3; // count = 4 (same as count = count / 3)
```

### String Concatenation

```java
String firstName = "John";
String lastName = "Doe";
String fullName = firstName + " " + lastName;  // "John Doe"

int age = 25;
String message = "I am " + age + " years old";  // "I am 25 years old"
```

---

## Day 7: Practice Exercises

### Exercise 1: Personal Information
Write a program that stores and displays:
- Your name (String)
- Your age (int)
- Your height in feet (double)
- Whether you're a student (boolean)
- Your grade (char)

### Exercise 2: Simple Calculator
Write a program that:
1. Creates two int variables: `num1 = 15` and `num2 = 4`
2. Calculates and prints:
   - Sum
   - Difference
   - Product
   - Quotient
   - Remainder

### Exercise 3: Temperature Converter
Write a program that converts Celsius to Fahrenheit.
- Formula: `F = (C * 9/5) + 32`
- Test with: C = 25 (should give F = 77)

### Exercise 4: Circle Calculations
Given radius = 5.0:
- Calculate area: `π * r * r` (use 3.14159 for π)
- Calculate circumference: `2 * π * r`
- Print both results

---

## Common Beginner Mistakes to Avoid

1. **Forgetting semicolons**
   ```java
   int age = 25  // ❌ Missing semicolon
   int age = 25; // ✅ Correct
   ```

2. **Mismatching data types**
   ```java
   int price = 19.99;     // ❌ Can't store decimal in int
   double price = 19.99;  // ✅ Correct
   ```

3. **Using single quotes for strings**
   ```java
   String name = 'John';  // ❌ Wrong quotes
   String name = "John";  // ✅ Correct
   ```

4. **Class name doesn't match file name**
   ```java
   // File: Hello.java
   public class World { } // ❌ Names don't match
   ```

---

## Week 1 Checklist

By the end of this week, you should be able to:
- [ ] Write and run a basic Java program
- [ ] Understand what classes and the main method are
- [ ] Declare variables of different types (int, double, boolean, char, String)
- [ ] Use System.out.println() to display output
- [ ] Perform basic arithmetic operations
- [ ] Concatenate strings
- [ ] Complete all 4 practice exercises

---

## Next Week Preview

In Week 2, we'll learn:
- Taking user input with Scanner
- If-else statements (making decisions)
- Comparison operators (>, <, ==, !=)
- Logical operators (&&, ||, !)

---

## Tips for Success

1. **Type everything yourself** - Don't copy-paste. Your muscle memory matters!
2. **Experiment** - Change values, break things, see what happens
3. **Practice daily** - Even 30 minutes a day is better than 3 hours once a week
4. **Read error messages** - They tell you what's wrong
5. **Take notes** - Write down things that confuse you

---

**Remember:** Everyone starts from zero. The fact that you're taking this step is already progress. Take your time, be patient with yourself, and don't hesitate to ask questions!

Ready for Week 2? Let me know when you've completed the Week 1 exercises!
