# Week 7: Object Relationships and Collaboration

## Overview
- Object relationships (Association, Aggregation, Composition)
- Object collaboration
- "Has-a" relationships
- Designing interacting objects
- UML Class Diagrams basics

---

## What You'll Learn This Week
- **Object Relationships** - How objects connect to each other
- **Association** - Objects that work together
- **Aggregation** - "Has-a" relationship (weak ownership)
- **Composition** - "Has-a" relationship (strong ownership)
- **Object Collaboration** - Objects calling each other's methods
- **UML Class Diagrams** - Visualizing relationships

---

## Day 1: Introduction to Object Relationships

### What Are Object Relationships?

In real life, objects don't exist in isolation. They interact with each other:
- A **Car** has an **Engine**
- A **Student** enrolls in **Courses**
- A **Library** contains **Books**
- A **Team** has **Players**

In OOP, we model these relationships!

---

### The Three Types of "Has-a" Relationships

```
Association (uses)
    ↓
Aggregation (has-a, weak)
    ↓
Composition (has-a, strong)
```

---

## Association

### What Is Association?

**Association** = Two objects that work together, but neither owns the other.

**Real-World Example:**
- A **Teacher** teaches **Students**
- A **Doctor** treats **Patients**
- The Teacher doesn't "own" the Students
- The Students don't "own" the Teacher
- They just **work together**

### Code Example: Teacher and Student

```java
public class Student {
    private String name;
    
    public Student(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
}

public class Teacher {
    private String name;
    
    public Teacher(String name) {
        this.name = name;
    }
    
    // Association: Teacher works with Student
    public void teach(Student student) {
        System.out.println(name + " is teaching " + student.getName());
    }
}
```

**Using it:**
```java
Student alice = new Student("Alice");
Student bob = new Student("Bob");
Teacher mrSmith = new Teacher("Mr. Smith");

mrSmith.teach(alice);  // Mr. Smith is teaching Alice
mrSmith.teach(bob);    // Mr. Smith is teaching Bob
```

**Key Point:** Teacher and Student are independent. If the Teacher object is destroyed, Students still exist!

---

### UML Diagram for Association

```
┌─────────┐             ┌─────────┐
│ Teacher │────────────>│ Student │
└─────────┘  teaches    └─────────┘
```

The arrow shows the relationship direction.

---

## Aggregation (Weak "Has-a")

### What Is Aggregation?

**Aggregation** = One object "has" another, but the contained object can exist independently.

**Real-World Examples:**
- A **Department** has **Employees** (Employees can exist without the Department)
- A **Library** has **Books** (Books can exist without the Library)
- A **Team** has **Players** (Players can exist without the Team)

**Key:** The "part" can survive without the "whole"

### Code Example: Team and Player

```java
public class Player {
    private String name;
    private int jerseyNumber;
    
    public Player(String name, int jerseyNumber) {
        this.name = name;
        this.jerseyNumber = jerseyNumber;
    }
    
    public String getName() {
        return name;
    }
    
    public void displayInfo() {
        System.out.println("Player: " + name + " #" + jerseyNumber);
    }
}

public class Team {
    private String teamName;
    private Player[] players;  // Team HAS Players
    private int playerCount;
    
    public Team(String teamName, int maxPlayers) {
        this.teamName = teamName;
        this.players = new Player[maxPlayers];
        this.playerCount = 0;
    }
    
    // Add a player to the team
    public void addPlayer(Player player) {
        if (playerCount < players.length) {
            players[playerCount] = player;
            playerCount++;
            System.out.println(player.getName() + " joined " + teamName);
        }
    }
    
    // Display all players
    public void displayRoster() {
        System.out.println("\n=== " + teamName + " Roster ===");
        for (int i = 0; i < playerCount; i++) {
            players[i].displayInfo();
        }
    }
}
```

**Using it:**
```java
// Create players (exist independently)
Player p1 = new Player("Alice", 10);
Player p2 = new Player("Bob", 23);
Player p3 = new Player("Charlie", 7);

// Create team
Team basketball = new Team("Eagles", 10);

// Add players to team
basketball.addPlayer(p1);
basketball.addPlayer(p2);
basketball.addPlayer(p3);

basketball.displayRoster();

// IMPORTANT: If we destroy the team, players still exist!
basketball = null;  // Team is gone
p1.displayInfo();   // But Alice still exists!
```

**Output:**
```
Alice joined Eagles
Bob joined Eagles
Charlie joined Eagles

=== Eagles Roster ===
Player: Alice #10
Player: Bob #23
Player: Charlie #7
Player: Alice #10
```

**Key Point:** Players exist independently! Destroying the Team doesn't destroy the Players.

---

### UML Diagram for Aggregation

```
┌──────┐              ┌────────┐
│ Team │◇────────────>│ Player │
└──────┘   has        └────────┘
```

The empty diamond (◇) means aggregation (weak ownership).

---

## Composition (Strong "Has-a")

### What Is Composition?

**Composition** = One object "has" another, and the contained object CANNOT exist independently.

**Real-World Examples:**
- A **House** has **Rooms** (Rooms can't exist without the House)
- A **Car** has an **Engine** (Engine is part of the Car)
- A **Book** has **Chapters** (Chapters are part of the Book)

**Key:** The "part" dies with the "whole"

### Code Example: House and Room

```java
public class Room {
    private String type;  // bedroom, bathroom, kitchen
    private double size;
    
    public Room(String type, double size) {
        this.type = type;
        this.size = size;
    }
    
    public void displayInfo() {
        System.out.println(type + " (" + size + " sq ft)");
    }
}

public class House {
    private String address;
    private Room[] rooms;  // House OWNS Rooms
    private int roomCount;
    
    public House(String address, int maxRooms) {
        this.address = address;
        this.rooms = new Room[maxRooms];
        this.roomCount = 0;
    }
    
    // Create a room inside the house
    public void addRoom(String type, double size) {
        if (roomCount < rooms.length) {
            // Room is created BY the House
            rooms[roomCount] = new Room(type, size);
            roomCount++;
        }
    }
    
    public void displayHouseInfo() {
        System.out.println("\nHouse at: " + address);
        System.out.println("Rooms:");
        for (int i = 0; i < roomCount; i++) {
            rooms[i].displayInfo();
        }
    }
}
```

**Using it:**
```java
House myHouse = new House("123 Main St", 5);

// Rooms are created BY the house
myHouse.addRoom("Living Room", 300);
myHouse.addRoom("Bedroom", 200);
myHouse.addRoom("Kitchen", 150);

myHouse.displayHouseInfo();

// If house is destroyed, rooms are destroyed too!
myHouse = null;  // House AND all its rooms are gone!
```

**Key Difference from Aggregation:**
- In **Aggregation:** Players exist separately, then join the Team
- In **Composition:** Rooms are created BY the House and belong to it

---

### UML Diagram for Composition

```
┌───────┐              ┌──────┐
│ House │♦────────────>│ Room │
└───────┘   owns       └──────┘
```

The filled diamond (♦) means composition (strong ownership).

---

## Comparing All Three Relationships

### Visual Summary

```
Association (uses)
Teacher ────────> Student
(Independent objects that interact)

Aggregation (has, weak)
Team ◇────────> Player
(Has-a, but parts can exist independently)

Composition (owns, strong)
House ♦────────> Room
(Has-a, parts die with the whole)
```

### Decision Guide

**Ask yourself:** "If I destroy the container, what happens to the contents?"

| Relationship | Container Dies → Contents? | Example |
|-------------|---------------------------|---------|
| **Association** | Contents are independent | Teacher-Student |
| **Aggregation** | Contents survive | Team-Player |
| **Composition** | Contents die too | House-Room |

---

## Day 2-3: Object Collaboration

### What Is Object Collaboration?

**Object Collaboration** = Objects working together by calling each other's methods.

Objects don't work alone - they collaborate to accomplish tasks!

### Example: Library System

```java
public class Book {
    private String title;
    private String author;
    private boolean checkedOut;
    
    public Book(String title, String author) {
        this.title = title;
        this.author = author;
        this.checkedOut = false;
    }
    
    public String getTitle() {
        return title;
    }
    
    public boolean isCheckedOut() {
        return checkedOut;
    }
    
    public void checkOut() {
        if (!checkedOut) {
            checkedOut = true;
            System.out.println("Checked out: " + title);
        } else {
            System.out.println(title + " is already checked out!");
        }
    }
    
    public void returnBook() {
        if (checkedOut) {
            checkedOut = false;
            System.out.println("Returned: " + title);
        }
    }
}

public class Member {
    private String name;
    private int memberId;
    
    public Member(String name, int memberId) {
        this.name = name;
        this.memberId = memberId;
    }
    
    public String getName() {
        return name;
    }
    
    // Member collaborates with Book
    public void borrowBook(Book book) {
        System.out.println(name + " is borrowing " + book.getTitle());
        book.checkOut();  // Calling Book's method!
    }
    
    public void returnBook(Book book) {
        System.out.println(name + " is returning " + book.getTitle());
        book.returnBook();  // Calling Book's method!
    }
}

public class Library {
    private String name;
    private Book[] books;
    private int bookCount;
    
    public Library(String name, int capacity) {
        this.name = name;
        this.books = new Book[capacity];
        this.bookCount = 0;
    }
    
    public void addBook(Book book) {
        if (bookCount < books.length) {
            books[bookCount] = book;
            bookCount++;
            System.out.println("Added to library: " + book.getTitle());
        }
    }
    
    public void displayAvailableBooks() {
        System.out.println("\n=== Available Books at " + name + " ===");
        for (int i = 0; i < bookCount; i++) {
            if (!books[i].isCheckedOut()) {
                System.out.println("- " + books[i].getTitle());
            }
        }
    }
}
```

**Using it (objects collaborating):**
```java
// Create library
Library cityLibrary = new Library("City Library", 100);

// Create books
Book book1 = new Book("Java Programming", "John Doe");
Book book2 = new Book("Python Basics", "Jane Smith");

// Add books to library
cityLibrary.addBook(book1);
cityLibrary.addBook(book2);

// Create member
Member alice = new Member("Alice", 1001);

// Show available books
cityLibrary.displayAvailableBooks();

// Alice borrows a book (collaboration!)
alice.borrowBook(book1);

// Show available books again
cityLibrary.displayAvailableBooks();

// Alice returns the book
alice.returnBook(book1);

cityLibrary.displayAvailableBooks();
```

**What's Happening:**
1. **Library** manages **Books** (Aggregation)
2. **Member** interacts with **Books** (Association)
3. Objects collaborate by calling each other's methods!

---

## Day 4: More Complex Collaboration

### Example: Order System

```java
public class Product {
    private String name;
    private double price;
    
    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
    
    public String getName() {
        return name;
    }
    
    public double getPrice() {
        return price;
    }
}

public class OrderItem {
    private Product product;
    private int quantity;
    
    public OrderItem(Product product, int quantity) {
        this.product = product;
        this.quantity = quantity;
    }
    
    public double getTotal() {
        return product.getPrice() * quantity;
    }
    
    public void displayItem() {
        System.out.println(product.getName() + " x" + quantity + " = $" + getTotal());
    }
}

public class Order {
    private int orderId;
    private OrderItem[] items;
    private int itemCount;
    
    public Order(int orderId, int maxItems) {
        this.orderId = orderId;
        this.items = new OrderItem[maxItems];
        this.itemCount = 0;
    }
    
    public void addItem(Product product, int quantity) {
        if (itemCount < items.length) {
            items[itemCount] = new OrderItem(product, quantity);
            itemCount++;
        }
    }
    
    public double calculateTotal() {
        double total = 0;
        for (int i = 0; i < itemCount; i++) {
            total += items[i].getTotal();
        }
        return total;
    }
    
    public void displayOrder() {
        System.out.println("\n=== Order #" + orderId + " ===");
        for (int i = 0; i < itemCount; i++) {
            items[i].displayItem();
        }
        System.out.println("Total: $" + calculateTotal());
    }
}
```

**Using it:**
```java
// Create products
Product laptop = new Product("Laptop", 999.99);
Product mouse = new Product("Mouse", 29.99);
Product keyboard = new Product("Keyboard", 79.99);

// Create order
Order order = new Order(1001, 10);

// Add items to order (collaboration!)
order.addItem(laptop, 1);
order.addItem(mouse, 2);
order.addItem(keyboard, 1);

// Display order
order.displayOrder();
```

**Relationships:**
- Order **has** OrderItems (Composition - items die with order)
- OrderItem **uses** Product (Association - product is independent)

---

## Day 5: UML Class Diagrams

### What Are UML Class Diagrams?

**UML (Unified Modeling Language)** = A visual way to show class structure and relationships.

**From your professor:** "UML is a communication tool, not busywork."

### Basic Class Diagram

```
┌─────────────────┐
│   ClassName     │
├─────────────────┤
│ - field1: type  │  ← Fields (- means private)
│ - field2: type  │
├─────────────────┤
│ + method1()     │  ← Methods (+ means public)
│ + method2()     │
└─────────────────┘
```

### Example: Book Class

```
┌──────────────────────┐
│       Book           │
├──────────────────────┤
│ - title: String      │
│ - author: String     │
│ - pages: int         │
├──────────────────────┤
│ + getTitle(): String │
│ + getAuthor(): String│
│ + isLongBook(): bool │
└──────────────────────┘
```

### Relationship Symbols

```
Association:    ClassA ────────> ClassB
Aggregation:    ClassA ◇───────> ClassB
Composition:    ClassA ♦───────> ClassB
Inheritance:    ClassB ─────▷ ClassA (Week 8)
```

### Complete Example: Library System

```
┌─────────────┐              ┌──────────────┐
│  Library    │              │    Book      │
├─────────────┤              ├──────────────┤
│ - name      │◇────────────>│ - title      │
│ - books[]   │   has        │ - author     │
├─────────────┤              │ - checkedOut │
│ + addBook() │              ├──────────────┤
└─────────────┘              │ + checkOut() │
                             │ + return()   │
                             └──────────────┘
                                    ↑
                                    │ uses
                                    │
                             ┌──────────────┐
                             │   Member     │
                             ├──────────────┤
                             │ - name       │
                             │ - memberId   │
                             ├──────────────┤
                             │ + borrow()   │
                             └──────────────┘
```

---

## Week 7 Practice Exercises

### Exercise 1: University System

Create a university system with these relationships:

**Classes needed:**
- `Student` (name, id, major)
- `Course` (courseCode, courseName, credits)
- `Department` (name, courses array)

**Requirements:**
- Department **aggregates** Courses (has courses)
- Student can enroll in Courses (association)
- Create methods for enrolling students, displaying course roster

---

### Exercise 2: Car and Engine

Create a car system demonstrating composition:

**Classes needed:**
- `Engine` (horsepower, type)
- `Car` (make, model, engine)

**Requirements:**
- Car **owns** Engine (composition)
- Engine is created inside Car constructor
- When Car is destroyed, Engine is destroyed
- Methods: start(), stop(), displayInfo()

---

### Exercise 3: Shopping Cart

Create a shopping system:

**Classes needed:**
- `Product` (name, price)
- `CartItem` (product, quantity)
- `ShoppingCart` (items array)
- `Customer` (name, cart)

**Requirements:**
- ShoppingCart **owns** CartItems (composition)
- CartItem **uses** Product (association)
- Customer **has** ShoppingCart (composition)
- Methods: addToCart(), removeFromCart(), checkout()

---

### Exercise 4: School System

Create a complete school management system:

**Classes needed:**
- `Teacher` (name, subject)
- `Student` (name, grade)
- `Classroom` (teacher, students array, room number)

**Requirements:**
- Classroom **aggregates** Students
- Classroom **has** one Teacher (association)
- Methods: addStudent(), assignTeacher(), displayClassInfo()

---

### Exercise 5: Bank System

Create a banking system:

**Classes needed:**
- `BankAccount` (accountNumber, balance)
- `Transaction` (type, amount, date)
- `Customer` (name, accounts array)
- `Bank` (name, customers array)

**Requirements:**
- Customer **owns** BankAccounts (composition)
- BankAccount **owns** Transactions (composition)
- Bank **has** Customers (aggregation)
- Objects should collaborate for deposits, withdrawals

---

## Common Mistakes to Avoid

### Mistake 1: Confusing Aggregation and Composition

```java
// ❌ WRONG - This looks like composition but acts like aggregation
public class Team {
    private Player[] players;
    
    public void addPlayer(Player p) {
        // Player exists independently - this is AGGREGATION
    }
}

// ✅ CORRECT - True composition
public class House {
    private Room[] rooms;
    
    public void addRoom(String type) {
        // Room is created HERE - this is COMPOSITION
        rooms[count] = new Room(type);
    }
}
```

### Mistake 2: Creating Circular Dependencies

```java
// ❌ BAD - Circular dependency
public class A {
    private B b;
    
    public A() {
        b = new B(this);  // A creates B, passes itself
    }
}

public class B {
    private A a;
    
    public B(A a) {
        this.a = a;  // B holds reference to A
    }
}
// This creates tight coupling and is hard to maintain!
```

### Mistake 3: Not Thinking About Ownership

Ask: "If I destroy object A, should object B be destroyed?"
- YES → Composition
- NO → Aggregation or Association

---

## Week 7 Checklist

By the end of this week, you should be able to:
- [ ] Explain the difference between Association, Aggregation, and Composition
- [ ] Identify which relationship to use in a given scenario
- [ ] Design classes with proper relationships
- [ ] Make objects collaborate by calling each other's methods
- [ ] Read basic UML class diagrams
- [ ] Draw simple UML diagrams showing relationships
- [ ] Complete all 5 practice exercises

---

## Key Takeaways

### Relationships:
- **Association** = Objects work together (Teacher teaches Student)
- **Aggregation** = Has-a, weak (Team has Players)
- **Composition** = Has-a, strong (House has Rooms)

### Collaboration:
- Objects call each other's methods
- Design objects to work together
- Think about responsibilities

### UML:
- Visual tool for communication
- Shows class structure and relationships
- Use symbols: ───>, ◇───>, ♦───>

---

## Next Week Preview

In Week 8, we'll learn:
- **Inheritance** - "Is-a" relationships
- **Method Overriding** - Changing parent behavior
- **Polymorphism** - Objects of different types
- **Abstract Classes** - Templates for subclasses
- **Interfaces** - Contracts that classes must follow

---

You've now learned how objects relate to and collaborate with each other! This is crucial for building real applications where many objects work together! 🚀

Ready for Week 8 (Inheritance and Polymorphism)? Let me know!