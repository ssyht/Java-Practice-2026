# Getting Started with Eclipse - Your First Java Program

## Step-by-Step Guide to Create Your First Project

### Step 1: Open Eclipse
1. Launch Eclipse
2. You'll see a "Workspace Launcher" dialog
   - This asks where to save your projects
   - The default location is fine, or choose your own folder
   - Click "Launch"

### Step 2: Create a New Java Project
1. Go to **File → New → Java Project**
   - If you don't see "Java Project", click **File → New → Project → Java → Java Project**

2. In the "New Java Project" window:
   - **Project name:** Type `JavaBasics` (or any name you like)
   - **JRE:** Should automatically show "JavaSE-17" or similar
   - Click **Finish**

3. If a module dialog appears, click **Don't Create** (we'll keep it simple for now)

### Step 3: Create Your First Java Class
1. In the **Package Explorer** (left side), right-click on your project name `JavaBasics`

2. Select **New → Class**

3. In the "New Java Class" window:
   - **Package:** Leave empty for now (we'll learn about packages later)
   - **Name:** Type `HelloWorld`
   - ✅ Check the box: **public static void main(String[] args)**
   - Click **Finish**

Eclipse will create a file that looks like this:

```java
public class HelloWorld {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

	}

}
```

### Step 4: Write Your First Code
1. Delete the comment line `// TODO Auto-generated method stub`

2. Inside the `main` method (between the curly braces `{}`), type:
```java
System.out.println("Hello, World!");
```

Your complete code should look like this:

```java
public class HelloWorld {

	public static void main(String[] args) {
		System.out.println("Hello, World!");
	}

}
```

### Step 5: Run Your Program
1. **Right-click** anywhere in your code editor

2. Select **Run As → Java Application**

3. Look at the **Console** tab at the bottom of Eclipse
   - You should see: `Hello, World!`

**Congratulations! You've just run your first Java program! 🎉**

---

## Understanding Eclipse's Interface

### Main Areas:
1. **Package Explorer (Left):** Shows all your projects and files
2. **Editor (Center):** Where you write your code
3. **Console (Bottom):** Shows output when you run programs
4. **Outline (Right):** Shows structure of your code

### Useful Shortcuts:
- **Ctrl + Space:** Auto-complete (VERY USEFUL!)
- **Ctrl + S:** Save file
- **Ctrl + F11:** Run program
- **Ctrl + /:** Comment/uncomment line
- **Ctrl + Shift + F:** Format code (makes it neat)

---

## Practice: Create Your Second Program

Let's practice creating another program from scratch:

### Create a New Class
1. Right-click on `JavaBasics` project
2. **New → Class**
3. Name it `MyInfo`
4. Check the `main` method box
5. Click **Finish**

### Write This Code:

```java
public class MyInfo {

	public static void main(String[] args) {
		// Personal information
		String name = "Your Name";
		int age = 25;
		double height = 5.9;
		boolean isStudent = true;
		
		// Display information
		System.out.println("Name: " + name);
		System.out.println("Age: " + age);
		System.out.println("Height: " + height + " feet");
		System.out.println("Student: " + isStudent);
	}

}
```

### Modify it:
- Change the name to your actual name
- Change age to your age
- Change height to your height
- Run it!

---

## Eclipse Tips for Beginners

### 1. Auto-Complete is Your Friend
When typing, press **Ctrl + Space** to see suggestions:
- Type `sysout` then press **Ctrl + Space** → Eclipse automatically writes `System.out.println();`
- Type `main` then press **Ctrl + Space** → Eclipse writes the entire main method

### 2. Eclipse Shows Errors
- **Red underline:** Error - your code won't run
- **Yellow underline:** Warning - might have issues
- **Hover your mouse** over the error to see what's wrong

### 3. Save Often
- Eclipse auto-saves, but pressing **Ctrl + S** is a good habit
- The white dot on the file tab means unsaved changes

### 4. Multiple Classes in One Project
- You can have many classes in one project
- Each class can have its own `main` method
- Right-click the class you want to run → **Run As → Java Application**

---

## Common Eclipse Issues (and Solutions)

### Issue 1: "Cannot find main method"
**Solution:** Make sure your main method looks exactly like this:
```java
public static void main(String[] args) {
```

### Issue 2: Red X on project
**Solution:** 
- Right-click project → **Properties**
- Check that Java compiler is set to 17 or compatible version

### Issue 3: Console doesn't show
**Solution:** 
- Go to **Window → Show View → Console**

### Issue 4: Code looks messy
**Solution:** 
- Select all code (**Ctrl + A**)
- Press **Ctrl + Shift + F** to auto-format

---

## Your First Week Assignments in Eclipse

Create these programs in your `JavaBasics` project. Create a **new class** for each exercise:

### Exercise 1: Calculator (Class name: `Calculator`)
```java
public class Calculator {
	public static void main(String[] args) {
		int num1 = 15;
		int num2 = 4;
		
		// Calculate and print:
		// - Sum
		// - Difference  
		// - Product
		// - Quotient
		// - Remainder
		
		// Your code here!
	}
}
```

### Exercise 2: TemperatureConverter (Class name: `TemperatureConverter`)
```java
public class TemperatureConverter {
	public static void main(String[] args) {
		double celsius = 25.0;
		
		// Convert to Fahrenheit using: F = (C * 9/5) + 32
		// Store result in a variable
		// Print the result
		
		// Your code here!
	}
}
```

### Exercise 3: CircleCalculations (Class name: `CircleCalculations`)
```java
public class CircleCalculations {
	public static void main(String[] args) {
		double radius = 5.0;
		double pi = 3.14159;
		
		// Calculate area: π * r * r
		// Calculate circumference: 2 * π * r
		// Print both results
		
		// Your code here!
	}
}
```

---

## Organizing Your Learning

### Project Structure Suggestion:
```
JavaBasics (Project)
├── HelloWorld.java
├── MyInfo.java
├── Calculator.java
├── TemperatureConverter.java
└── CircleCalculations.java
```

As you progress through weeks, you can create new projects:
- `JavaBasics` (Week 1)
- `ControlFlow` (Week 2)
- `LoopsAndArrays` (Week 3)
- etc.

---

## Next Steps

1. ✅ Complete the 3 exercises above
2. ✅ Experiment with changing values
3. ✅ Try the `sysout` + Ctrl+Space trick
4. ✅ Practice creating new classes
5. ✅ Get comfortable with Eclipse

**When you're done:**
- Share your solutions with me (copy-paste the code)
- I'll review them and give feedback
- Then we'll move to Week 2!

---

## Quick Reference Card

**Creating a new class:**
Right-click project → New → Class → Name it → Check main method → Finish

**Running your code:**
Right-click in editor → Run As → Java Application

**Quick print statement:**
Type `sysout` → Ctrl + Space → Enter

**Format code:**
Ctrl + A → Ctrl + Shift + F

**Save:**
Ctrl + S

---

You're all set! Eclipse is ready, Java 17 is installed, and you have everything you need to start coding. Take your time with the exercises and let me know how it goes! 💪
