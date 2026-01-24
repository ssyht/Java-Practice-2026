# Week 2: Control Flow - Making Decisions in Java

## Overview
This week, you'll learn how to:
- Get input from users (Scanner)
- Make decisions in your code (if-else statements)
- Compare values (comparison operators)
- Combine conditions (logical operators)

---

## Day 1-2: Getting User Input with Scanner

### What is Scanner?
Scanner is a tool that lets your program read input from the user. Think of it as asking questions and waiting for answers.

### Basic Scanner Setup

```java
import java.util.Scanner;

public class UserInputDemo {
    public static void main(String[] args) {
        // Create a Scanner object
        Scanner scanner = new Scanner(System.in);
        
        // Ask for user's name
        System.out.print("Enter your name: ");
        String name = scanner.nextLine();
        
        // Ask for user's age
        System.out.print("Enter your age: ");
        int age = scanner.nextInt();
        
        // Display the information
        System.out.println("Hello, " + name + "!");
        System.out.println("You are " + age + " years old.");
        
        // Close the scanner (good practice)
        scanner.close();
    }
}
```

**Output:**
```
Enter your name: Alice
Enter your age: 25
Hello, Alice!
You are 25 years old.
```

### Scanner Methods for Different Data Types

```java
import java.util.Scanner;

public class ScannerMethods {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // Reading different types
        System.out.print("Enter a whole number: ");
        int number = scanner.nextInt();
        
        System.out.print("Enter a decimal number: ");
        double decimal = scanner.nextDouble();
        
        System.out.print("Enter true or false: ");
        boolean flag = scanner.nextBoolean();
        
        System.out.print("Enter a single word: ");
        String word = scanner.next();
        
        // Clear the buffer before nextLine()
        scanner.nextLine();
        
        System.out.print("Enter a full sentence: ");
        String sentence = scanner.nextLine();
        
        System.out.println("\nYou entered:");
        System.out.println("Number: " + number);
        System.out.println("Decimal: " + decimal);
        System.out.println("Boolean: " + flag);
        System.out.println("Word: " + word);
        System.out.println("Sentence: " + sentence);
        
        scanner.close();
    }
}
```

### Important Scanner Rules

1. **Always import Scanner at the top:**
   ```java
   import java.util.Scanner;
   ```

2. **Create Scanner object once:**
   ```java
   Scanner scanner = new Scanner(System.in);
   ```

3. **Use the right method for the data type:**
   - `nextInt()` → for int
   - `nextDouble()` → for double
   - `nextBoolean()` → for boolean
   - `next()` → for single word (String)
   - `nextLine()` → for entire line (String)

4. **Close Scanner when done:**
   ```java
   scanner.close();
   ```

### Common Scanner Issue (and Fix!)

**Problem:** After using `nextInt()`, `nextDouble()`, etc., `nextLine()` doesn't wait for input.

**Solution:** Add an extra `scanner.nextLine()` to clear the buffer:

```java
System.out.print("Enter your age: ");
int age = scanner.nextInt();

scanner.nextLine(); // Clear the buffer!

System.out.print("Enter your name: ");
String name = scanner.nextLine();
```

---

## Day 3-4: If-Else Statements

### What are If-Else Statements?
They let your program make decisions based on conditions. Like: "IF it's raining, take an umbrella, ELSE wear sunglasses."

### Basic If Statement

```java
import java.util.Scanner;

public class IfDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter your age: ");
        int age = scanner.nextInt();
        
        if (age >= 18) {
            System.out.println("You are an adult!");
        }
        
        scanner.close();
    }
}
```

### If-Else Statement

```java
import java.util.Scanner;

public class IfElseDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter your age: ");
        int age = scanner.nextInt();
        
        if (age >= 18) {
            System.out.println("You are an adult!");
        } else {
            System.out.println("You are a minor.");
        }
        
        scanner.close();
    }
}
```

### If-Else-If Ladder

```java
import java.util.Scanner;

public class GradeCalculator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter your score (0-100): ");
        int score = scanner.nextInt();
        
        if (score >= 90) {
            System.out.println("Grade: A - Excellent!");
        } else if (score >= 80) {
            System.out.println("Grade: B - Good job!");
        } else if (score >= 70) {
            System.out.println("Grade: C - Fair.");
        } else if (score >= 60) {
            System.out.println("Grade: D - Needs improvement.");
        } else {
            System.out.println("Grade: F - Failed.");
        }
        
        scanner.close();
    }
}
```

### Nested If Statements

```java
import java.util.Scanner;

public class NestedIfDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter your age: ");
        int age = scanner.nextInt();
        
        System.out.print("Do you have a driver's license? (true/false): ");
        boolean hasLicense = scanner.nextBoolean();
        
        if (age >= 16) {
            if (hasLicense) {
                System.out.println("You can drive!");
            } else {
                System.out.println("You need to get a license first.");
            }
        } else {
            System.out.println("You're too young to drive.");
        }
        
        scanner.close();
    }
}
```

---

## Day 5: Comparison Operators

These operators compare two values and return true or false.

### The Six Comparison Operators

```java
public class ComparisonOperators {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        
        // Equal to
        System.out.println(a == b);  // false
        
        // Not equal to
        System.out.println(a != b);  // true
        
        // Greater than
        System.out.println(a > b);   // false
        
        // Less than
        System.out.println(a < b);   // true
        
        // Greater than or equal to
        System.out.println(a >= 10); // true
        
        // Less than or equal to
        System.out.println(b <= 20); // true
    }
}
```

### Comparison Operators in Action

```java
import java.util.Scanner;

public class NumberComparison {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter first number: ");
        int num1 = scanner.nextInt();
        
        System.out.print("Enter second number: ");
        int num2 = scanner.nextInt();
        
        if (num1 > num2) {
            System.out.println(num1 + " is greater than " + num2);
        } else if (num1 < num2) {
            System.out.println(num1 + " is less than " + num2);
        } else {
            System.out.println("Both numbers are equal!");
        }
        
        scanner.close();
    }
}
```

### Important: == vs =

```java
int x = 5;  // Assignment: store 5 in x

if (x == 5) {  // Comparison: check if x equals 5
    System.out.println("x is 5");
}
```

**Common Mistake:**
```java
if (x = 5) {  // ❌ WRONG! This assigns 5 to x, doesn't compare
    // This will cause an error
}
```

---

## Day 6: Logical Operators

Logical operators combine multiple conditions.

### The Three Logical Operators

```java
public class LogicalOperators {
    public static void main(String[] args) {
        int age = 25;
        boolean hasLicense = true;
        boolean hasInsurance = false;
        
        // AND (&&) - Both conditions must be true
        if (age >= 18 && hasLicense) {
            System.out.println("Can drive legally!");
        }
        
        // OR (||) - At least one condition must be true
        if (hasLicense || age >= 21) {
            System.out.println("Meets at least one requirement!");
        }
        
        // NOT (!) - Reverses the condition
        if (!hasInsurance) {
            System.out.println("You need insurance!");
        }
    }
}
```

### Truth Tables

**AND (&&):**
```
true  && true  = true
true  && false = false
false && true  = false
false && false = false
```

**OR (||):**
```
true  || true  = true
true  || false = true
false || true  = true
false || false = false
```

**NOT (!):**
```
!true  = false
!false = true
```

### Complex Conditions Example

```java
import java.util.Scanner;

public class MovieTheater {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter your age: ");
        int age = scanner.nextInt();
        
        System.out.print("Do you have a student ID? (true/false): ");
        boolean hasStudentID = scanner.nextBoolean();
        
        System.out.print("Is it Tuesday? (true/false): ");
        boolean isTuesday = scanner.nextBoolean();
        
        // Regular ticket price
        double price = 15.00;
        
        // Discount conditions
        if (age < 12 || age > 65) {
            price = 8.00;
            System.out.println("Child/Senior discount applied!");
        } else if (hasStudentID && isTuesday) {
            price = 10.00;
            System.out.println("Student Tuesday discount applied!");
        } else if (hasStudentID) {
            price = 12.00;
            System.out.println("Student discount applied!");
        } else if (isTuesday) {
            price = 11.00;
            System.out.println("Tuesday discount applied!");
        }
        
        System.out.println("Ticket price: $" + price);
        
        scanner.close();
    }
}
```

---

## Day 7: Practice Exercises

### Exercise 1: Even or Odd Checker
Create a program that:
1. Asks the user for a number
2. Checks if it's even or odd
3. Prints the result

**Hint:** Use the modulus operator `%`. If `number % 2 == 0`, it's even.

```java
import java.util.Scanner;

public class EvenOddChecker {
    public static void main(String[] args) {
        // Your code here!
    }
}
```

### Exercise 2: Simple Calculator
Create a calculator that:
1. Asks for two numbers
2. Asks for an operation (+, -, *, /)
3. Performs the operation and displays the result

```java
import java.util.Scanner;

public class SimpleCalculator {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter first number: ");
        // Your code here!
        
        System.out.print("Enter operator (+, -, *, /): ");
        // Your code here!
        
        System.out.print("Enter second number: ");
        // Your code here!
        
        // Use if-else to check operator and perform calculation
        // Your code here!
        
        scanner.close();
    }
}
```

### Exercise 3: BMI Calculator
Create a BMI calculator that:
1. Asks for weight (in kg) and height (in meters)
2. Calculates BMI: `weight / (height * height)`
3. Categorizes the result:
   - BMI < 18.5: Underweight
   - BMI 18.5-24.9: Normal weight
   - BMI 25-29.9: Overweight
   - BMI >= 30: Obese

```java
import java.util.Scanner;

public class BMICalculator {
    public static void main(String[] args) {
        // Your code here!
    }
}
```

### Exercise 4: Password Validator
Create a simple password validator that:
1. Asks the user to create a password
2. Checks if:
   - Password is at least 8 characters long
   - (Use `password.length()` to get length)
3. If valid, prints "Strong password!"
4. If invalid, prints "Password too short!"

```java
import java.util.Scanner;

public class PasswordValidator {
    public static void main(String[] args) {
        // Your code here!
    }
}
```

### Exercise 5: Grade Calculator with Multiple Subjects
Create a program that:
1. Asks for scores in 3 subjects (Math, Science, English)
2. Calculates the average
3. Determines the grade based on average:
   - 90+: A
   - 80-89: B
   - 70-79: C
   - 60-69: D
   - Below 60: F
4. If average >= 80 AND all subjects >= 70, print "Honor Roll!"

```java
import java.util.Scanner;

public class GradeCalculator {
    public static void main(String[] args) {
        // Your code here!
    }
}
```

---

## Common Mistakes to Avoid

### 1. Using = instead of ==
```java
if (age = 18) { }  // ❌ WRONG - Assignment
if (age == 18) { } // ✅ CORRECT - Comparison
```

### 2. Missing braces in if-else
```java
// Confusing (works but unclear):
if (age >= 18)
    System.out.println("Adult");
else
    System.out.println("Minor");

// Better (always use braces):
if (age >= 18) {
    System.out.println("Adult");
} else {
    System.out.println("Minor");
}
```

### 3. Wrong logical operator
```java
// If you need BOTH conditions to be true, use &&
if (age >= 18 && hasLicense) { }

// If you need AT LEAST ONE to be true, use ||
if (age >= 18 || hasPermit) { }
```

### 4. Not closing Scanner
```java
Scanner scanner = new Scanner(System.in);
// ... use scanner ...
scanner.close(); // Don't forget this!
```

---

## Week 2 Checklist

By the end of this week, you should be able to:
- [ ] Use Scanner to get user input
- [ ] Write if, if-else, and if-else-if statements
- [ ] Use comparison operators (==, !=, >, <, >=, <=)
- [ ] Use logical operators (&&, ||, !)
- [ ] Combine multiple conditions
- [ ] Complete all 5 practice exercises

---

## Next Week Preview

In Week 3, we'll learn:
- **Loops** (for, while, do-while)
- How to repeat code multiple times
- Break and continue statements
- Nested loops

---

## Tips for Success

1. **Test your conditions:** Try different inputs to see if your if-statements work correctly
2. **Use meaningful variable names:** `hasLicense` is better than `h`
3. **Format your code:** Use Ctrl+Shift+F in Eclipse to keep it neat
4. **Add comments:** Explain what your conditions check for
5. **Debug:** Use `System.out.println()` to check variable values

---

**Remember:** Decision-making is fundamental to programming. Every app, game, or website uses if-statements constantly. Master this and you're building a strong foundation!

Ready for Week 3? Let me know when you've completed the Week 2 exercises! 🚀
