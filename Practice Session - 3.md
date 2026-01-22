# Week 3: Loops - Repeating Code Efficiently

## Overview
This week, you'll learn how to:
- Repeat code multiple times automatically (for loop)
- Loop while a condition is true (while loop)
- Execute code at least once (do-while loop)
- Control loop flow (break and continue)
- Use nested loops for complex patterns

---

## Why Do We Need Loops?

**Without loops:**
```java
System.out.println("Hello 1");
System.out.println("Hello 2");
System.out.println("Hello 3");
System.out.println("Hello 4");
System.out.println("Hello 5");
// Tedious and repetitive!
```

**With loops:**
```java
for (int i = 1; i <= 5; i++) {
    System.out.println("Hello " + i);
}
// Much cleaner!
```

---

## Day 1-2: For Loop

### What is a For Loop?
A for loop repeats code a **specific number of times**. Perfect when you know exactly how many iterations you need.

### Basic For Loop Syntax

```java
for (initialization; condition; update) {
    // Code to repeat
}
```

### Breaking Down the For Loop

```java
for (int i = 1; i <= 5; i++) {
    System.out.println("Count: " + i);
}
```

**Parts:**
1. **Initialization:** `int i = 1` - Start with i = 1
2. **Condition:** `i <= 5` - Continue while i is 5 or less
3. **Update:** `i++` - After each loop, increase i by 1
4. **Body:** Code inside `{ }` - Runs each iteration

**Output:**
```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
```

### How It Works Step-by-Step

```java
for (int i = 0; i < 3; i++) {
    System.out.println("i = " + i);
}
```

**Execution:**
1. `int i = 0` → i is now 0
2. Check: `i < 3`? YES (0 < 3) → Execute body → Print "i = 0"
3. Update: `i++` → i is now 1
4. Check: `i < 3`? YES (1 < 3) → Execute body → Print "i = 1"
5. Update: `i++` → i is now 2
6. Check: `i < 3`? YES (2 < 3) → Execute body → Print "i = 2"
7. Update: `i++` → i is now 3
8. Check: `i < 3`? NO (3 is not < 3) → **STOP**

### Common For Loop Patterns

#### 1. Count from 1 to 10
```java
for (int i = 1; i <= 10; i++) {
    System.out.println(i);
}
```

#### 2. Count from 10 down to 1
```java
for (int i = 10; i >= 1; i--) {
    System.out.println(i);
}
```

#### 3. Count by 2s (even numbers)
```java
for (int i = 0; i <= 10; i += 2) {
    System.out.println(i);  // 0, 2, 4, 6, 8, 10
}
```

#### 4. Count by 5s
```java
for (int i = 0; i <= 50; i += 5) {
    System.out.println(i);  // 0, 5, 10, 15, 20...
}
```

### Practical Examples

#### Example 1: Sum of Numbers
```java
public class SumCalculator {
    public static void main(String[] args) {
        int sum = 0;
        
        for (int i = 1; i <= 10; i++) {
            sum += i;  // sum = sum + i
        }
        
        System.out.println("Sum of 1 to 10: " + sum);  // 55
    }
}
```

#### Example 2: Multiplication Table
```java
import java.util.Scanner;

public class MultiplicationTable {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter a number: ");
        int num = scanner.nextInt();
        
        System.out.println("Multiplication table for " + num + ":");
        
        for (int i = 1; i <= 10; i++) {
            System.out.println(num + " x " + i + " = " + (num * i));
        }
        
        scanner.close();
    }
}
```

**Output (if user enters 5):**
```
Multiplication table for 5:
5 x 1 = 5
5 x 2 = 10
5 x 3 = 15
...
5 x 10 = 50
```

#### Example 3: Factorial Calculator
```java
import java.util.Scanner;

public class Factorial {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter a number: ");
        int num = scanner.nextInt();
        
        int factorial = 1;
        
        for (int i = 1; i <= num; i++) {
            factorial *= i;  // factorial = factorial * i
        }
        
        System.out.println("Factorial of " + num + " = " + factorial);
        
        scanner.close();
    }
}
```

---

## Day 3-4: While Loop

### What is a While Loop?
A while loop repeats code **as long as a condition is true**. Use it when you don't know exactly how many times to loop.

### While Loop Syntax

```java
while (condition) {
    // Code to repeat
}
```

### Basic Example

```java
int count = 1;

while (count <= 5) {
    System.out.println("Count: " + count);
    count++;
}
```

**Output:**
```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
```

### For Loop vs While Loop

**Same task, different loops:**

```java
// Using FOR loop
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}

// Using WHILE loop
int i = 1;
while (i <= 5) {
    System.out.println(i);
    i++;
}
```

Both produce the same output!

### When to Use While Loop

#### Example 1: User Input Validation
```java
import java.util.Scanner;

public class PasswordChecker {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String password = "";
        
        // Keep asking until correct password is entered
        while (!password.equals("java123")) {
            System.out.print("Enter password: ");
            password = scanner.nextLine();
            
            if (!password.equals("java123")) {
                System.out.println("Wrong password! Try again.");
            }
        }
        
        System.out.println("Access granted!");
        scanner.close();
    }
}
```

#### Example 2: Guessing Game
```java
import java.util.Scanner;

public class GuessingGame {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int secretNumber = 7;
        int guess = 0;
        
        System.out.println("Guess a number between 1 and 10!");
        
        while (guess != secretNumber) {
            System.out.print("Your guess: ");
            guess = scanner.nextInt();
            
            if (guess < secretNumber) {
                System.out.println("Too low!");
            } else if (guess > secretNumber) {
                System.out.println("Too high!");
            } else {
                System.out.println("Correct! You won!");
            }
        }
        
        scanner.close();
    }
}
```

#### Example 3: Sum Until Negative Number
```java
import java.util.Scanner;

public class SumUntilNegative {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int sum = 0;
        int number = 0;
        
        System.out.println("Enter numbers to sum (negative to stop):");
        
        while (number >= 0) {
            System.out.print("Enter number: ");
            number = scanner.nextInt();
            
            if (number >= 0) {
                sum += number;
            }
        }
        
        System.out.println("Total sum: " + sum);
        scanner.close();
    }
}
```

### Infinite Loop Warning! ⚠️

```java
// BAD - This will run forever!
int i = 1;
while (i <= 5) {
    System.out.println(i);
    // Forgot to increment i!
}

// GOOD - This will stop
int i = 1;
while (i <= 5) {
    System.out.println(i);
    i++;  // Don't forget this!
}
```

---

## Day 5: Do-While Loop

### What is a Do-While Loop?
Similar to while loop, but **executes at least once** before checking the condition.

### Do-While Syntax

```java
do {
    // Code to execute
} while (condition);
```

### While vs Do-While

```java
// WHILE loop - might not execute at all
int i = 10;
while (i < 5) {
    System.out.println("This never prints");
}

// DO-WHILE loop - executes at least once
int j = 10;
do {
    System.out.println("This prints once!");
} while (j < 5);
```

### Perfect Use Case: Menu Systems

```java
import java.util.Scanner;

public class MenuSystem {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int choice;
        
        do {
            System.out.println("\n=== MENU ===");
            System.out.println("1. Option 1");
            System.out.println("2. Option 2");
            System.out.println("3. Option 3");
            System.out.println("0. Exit");
            System.out.print("Choose an option: ");
            choice = scanner.nextInt();
            
            if (choice == 1) {
                System.out.println("You selected Option 1");
            } else if (choice == 2) {
                System.out.println("You selected Option 2");
            } else if (choice == 3) {
                System.out.println("You selected Option 3");
            } else if (choice == 0) {
                System.out.println("Goodbye!");
            } else {
                System.out.println("Invalid choice!");
            }
            
        } while (choice != 0);
        
        scanner.close();
    }
}
```

---

## Day 6: Break and Continue

### Break Statement
**Immediately exits the loop**, no matter what the condition says.

```java
// Find first number divisible by 7
for (int i = 1; i <= 100; i++) {
    if (i % 7 == 0) {
        System.out.println("First number divisible by 7: " + i);
        break;  // Exit loop immediately
    }
}
// Output: First number divisible by 7: 7
```

### Continue Statement
**Skips the rest of the current iteration** and moves to the next one.

```java
// Print odd numbers only
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        continue;  // Skip even numbers
    }
    System.out.println(i);  // Only prints odd numbers
}
// Output: 1, 3, 5, 7, 9
```

### Break vs Continue

```java
// Using BREAK
System.out.println("With BREAK:");
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        break;  // Exit loop when i is 3
    }
    System.out.println(i);
}
// Output: 1, 2

// Using CONTINUE
System.out.println("\nWith CONTINUE:");
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue;  // Skip when i is 3
    }
    System.out.println(i);
}
// Output: 1, 2, 4, 5
```

### Practical Example: Search and Exit

```java
import java.util.Scanner;

public class SearchNumber {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.print("Enter a number to search for (1-100): ");
        int target = scanner.nextInt();
        
        boolean found = false;
        
        for (int i = 1; i <= 100; i++) {
            if (i == target) {
                System.out.println("Found " + target + " at position " + i);
                found = true;
                break;  // Stop searching once found
            }
        }
        
        if (!found) {
            System.out.println("Number not found");
        }
        
        scanner.close();
    }
}
```

---

## Day 7: Nested Loops

### What are Nested Loops?
A loop **inside another loop**. The inner loop runs completely for each iteration of the outer loop.

### Basic Example

```java
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        System.out.println("i = " + i + ", j = " + j);
    }
}
```

**Output:**
```
i = 1, j = 1
i = 1, j = 2
i = 1, j = 3
i = 2, j = 1
i = 2, j = 2
i = 2, j = 3
i = 3, j = 1
i = 3, j = 2
i = 3, j = 3
```

### Pattern 1: Rectangle of Stars

```java
public class StarRectangle {
    public static void main(String[] args) {
        for (int i = 1; i <= 4; i++) {      // 4 rows
            for (int j = 1; j <= 5; j++) {  // 5 stars per row
                System.out.print("* ");
            }
            System.out.println();  // New line after each row
        }
    }
}
```

**Output:**
```
* * * * * 
* * * * * 
* * * * * 
* * * * * 
```

### Pattern 2: Right Triangle

```java
public class StarTriangle {
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
}
```

**Output:**
```
* 
* * 
* * * 
* * * * 
* * * * * 
```

### Pattern 3: Multiplication Table

```java
public class MultiplicationTable {
    public static void main(String[] args) {
        System.out.println("Multiplication Table (1-10):\n");
        
        for (int i = 1; i <= 10; i++) {
            for (int j = 1; j <= 10; j++) {
                System.out.print((i * j) + "\t");  // \t = tab
            }
            System.out.println();
        }
    }
}
```

### Pattern 4: Number Pyramid

```java
public class NumberPyramid {
    public static void main(String[] args) {
        int rows = 5;
        
        for (int i = 1; i <= rows; i++) {
            // Print spaces
            for (int j = 1; j <= rows - i; j++) {
                System.out.print("  ");
            }
            // Print numbers ascending
            for (int j = 1; j <= i; j++) {
                System.out.print(j + " ");
            }
            // Print numbers descending
            for (int j = i - 1; j >= 1; j--) {
                System.out.print(j + " ");
            }
            System.out.println();
        }
    }
}
```

**Output:**
```
        1 
      1 2 1 
    1 2 3 2 1 
  1 2 3 4 3 2 1 
1 2 3 4 5 4 3 2 1 
```

---

## Week 3 Practice Exercises

### Exercise 1: Number Sum
Write a program that:
1. Asks the user for a number N
2. Calculates the sum of all numbers from 1 to N
3. Example: If N = 5, sum = 1+2+3+4+5 = 15

```java
import java.util.Scanner;

public class NumberSum {
    public static void main(String[] args) {
        // Your code here!
    }
}
```

### Exercise 2: Reverse Number
Write a program that:
1. Asks for a number (e.g., 12345)
2. Prints it in reverse (54321)
3. Hint: Use `num % 10` to get last digit, then `num / 10` to remove it

```java
import java.util.Scanner;

public class ReverseNumber {
    public static void main(String[] args) {
        // Your code here!
    }
}
```

### Exercise 3: Prime Number Checker
Write a program that:
1. Asks for a number
2. Checks if it's prime (only divisible by 1 and itself)
3. Use a loop to check if any number from 2 to N-1 divides it evenly

```java
import java.util.Scanner;

public class PrimeChecker {
    public static void main(String[] args) {
        // Your code here!
    }
}
```

### Exercise 4: Fibonacci Series
Print the first N numbers of the Fibonacci series:
- Series: 0, 1, 1, 2, 3, 5, 8, 13, 21...
- Each number is the sum of the previous two

```java
import java.util.Scanner;

public class Fibonacci {
    public static void main(String[] args) {
        // Your code here!
    }
}
```

### Exercise 5: Star Patterns
Create a program that prints this pattern:
```
    *
   ***
  *****
 *******
*********
```
(Pyramid with 5 rows)

```java
public class StarPyramid {
    public static void main(String[] args) {
        // Your code here!
    }
}
```

### Exercise 6: Login System
Create a login system that:
1. Gives user 3 attempts to enter correct password
2. Password is "java2024"
3. If successful, print "Login successful"
4. If 3 failed attempts, print "Account locked"

```java
import java.util.Scanner;

public class LoginSystem {
    public static void main(String[] args) {
        // Your code here!
    }
}
```

---

## Loop Selection Guide

**Use FOR loop when:**
- You know exactly how many times to loop
- Counting/iterating through a range
- Example: Print 1 to 100

**Use WHILE loop when:**
- You don't know how many iterations needed
- Loop depends on user input or condition
- Example: Keep asking until correct password

**Use DO-WHILE loop when:**
- You need to execute at least once
- Menu systems
- Example: Show menu, then check if user wants to continue

---

## Common Loop Mistakes to Avoid

### 1. Infinite Loop
```java
// BAD
for (int i = 1; i <= 10; i--) {  // i gets smaller!
    System.out.println(i);
}
```

### 2. Off-by-One Error
```java
// Prints 0-9 (10 numbers)
for (int i = 0; i < 10; i++) { }

// Prints 0-10 (11 numbers)
for (int i = 0; i <= 10; i++) { }
```

### 3. Using Wrong Variable
```java
// BAD
for (int i = 0; i < 5; i++) {
    for (int j = 0; j < 5; i++) {  // Should be j++
        System.out.print("*");
    }
}
```

---

## Week 3 Checklist

By the end of this week, you should be able to:
- [ ] Write for loops to repeat code N times
- [ ] Use while loops for conditional repetition
- [ ] Understand when to use do-while loops
- [ ] Use break to exit loops early
- [ ] Use continue to skip iterations
- [ ] Create nested loops for patterns
- [ ] Complete all 6 practice exercises

---

## Next Week Preview

In Week 4, we'll learn:
- **Arrays** - Storing multiple values in one variable
- Array declaration and initialization
- Accessing and modifying array elements
- Looping through arrays
- **Strings** - Advanced string manipulation
- Multi-dimensional arrays

---

## Tips for Success

1. **Start simple:** Master basic loops before nested loops
2. **Use pen and paper:** Draw out what each iteration does
3. **Debug with print:** Add `System.out.println()` to see values
4. **Practice patterns:** They help you think about loops creatively
5. **Avoid infinite loops:** Always make sure your loop will end!

---

**Loops are EVERYWHERE in programming!** Games, animations, data processing - they all use loops. Master this and you'll have a powerful tool in your coding toolkit! 🚀

Ready for Week 4? Let me know when you've completed the Week 3 exercises!