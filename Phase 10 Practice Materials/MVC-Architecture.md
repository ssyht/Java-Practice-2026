# L13 – MVC Architecture
**CMP_SC 3330 – Object-Oriented Programming | Dr. Ekincan Ufuktepe**

---

## What Is MVC?

**MVC** stands for **Model-View-Controller**. It is an **architectural pattern** — a way of *organizing* your entire application into three distinct, separated parts.

The core idea is called **Separation of Concerns**: each part of your app has one clear job and doesn't interfere with the others.

> Think of a restaurant: the **kitchen** (Model) prepares the food, the **dining room** (View) is what customers see, and the **waiter** (Controller) takes orders from customers and delivers them to the kitchen. Each has a distinct role.

---

## The Three Components

### 1. Model — *"The Data & Logic"*

The Model is the brain of the application. It handles:
- **Storing data** (e.g., a user's username and password)
- **Business logic** (e.g., rules about what counts as a valid password)

Crucially, the Model knows **nothing about the user interface**. It doesn't care how data is displayed — that's someone else's job.

**Examples in Java:** POJOs (Plain Old Java Objects), Data Access Objects (DAOs), Service Classes

---

### 2. View — *"What the User Sees"*

The View is the presentation layer — it's the UI. It handles:
- **Displaying data** from the Model to the user
- **Collecting input** from the user (text fields, buttons, forms)

The View should be **"dumb"** — it just shows things. It contains no business logic. It doesn't process data or make decisions.

**Examples in Java:** Swing components (`JFrame`, `JTextField`), JSP (JavaServer Pages), HTML forms

---

### 3. Controller — *"The Go-Between"*

The Controller is the mediator. It handles:
- **Receiving user input** from the View
- **Telling the Model** to update based on that input
- **Deciding** which View to show next

The Controller is the glue between the Model and View. It should be **lean** — it orchestrates things but doesn't contain heavy logic itself.

**Examples in Java:** Servlets, Spring MVC Controllers, `ActionListener` implementations

---

## How They Interact

```
          User interacts with
                  ↓
[ VIEW ]  ──── sends request ──→  [ CONTROLLER ]
   ↑                                    │
   │  renders/displays                  │ manipulates
   │                                    ↓
   └──────────────────────────── [ MODEL ]
```

1. User interacts with the **View** (e.g., clicks "Register")
2. View notifies the **Controller**
3. Controller reads input, updates the **Model**
4. Model's data changes
5. **View** re-renders to show the updated data

---

## Why Use MVC?

| Benefit | Explanation |
|---|---|
| **Maintainability** | Each component can be changed independently. Update the UI without touching business logic. |
| **Scalability** | Easy to add new views (e.g., mobile vs. web) for the same Model. |
| **Reusability** | The same Model can be used across multiple different Views. |
| **Testability** | Each component can be unit tested in isolation. |

---

## Java Code Walkthrough

The lecture uses a **User Registration** app to demonstrate MVC. Let's walk through all three pieces:

---

### Model – `UserModel.java`

```java
public class UserModel {
    private String username;
    private String password;

    // Getters and setters
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }

    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
}
```

**What it does:** Just holds the user's data. No UI, no buttons, no display logic — pure data.

---

### View – `UserView.java` (Swing)

```java
public class UserView extends JFrame {
    private JTextField usernameField;
    private JPasswordField passwordField;
    private JButton registerButton;

    public UserView() {
        initialize();
    }

    private void initialize() {
        setTitle("User Registration");
        setSize(300, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        getContentPane().setLayout(null);

        usernameField = new JTextField();
        usernameField.setBounds(30, 30, 200, 25);
        getContentPane().add(usernameField);

        passwordField = new JPasswordField();
        passwordField.setBounds(30, 70, 200, 25);
        getContentPane().add(passwordField);

        registerButton = new JButton("Register");
        registerButton.setBounds(80, 120, 100, 30);
        getContentPane().add(registerButton);

        setVisible(true);
    }

    // Getters for the Controller to read input
    public String getUsername() {
        return usernameField.getText();
    }

    public String getPassword() {
        return new String(passwordField.getPassword());
    }

    // Lets the Controller attach a listener to the button
    public void addRegisterListener(ActionListener listener) {
        registerButton.addActionListener(listener);
    }
}
```

**What it does:** Displays fields and a button. Exposes getter methods so the Controller can read what the user typed. Does NOT process any data itself.

> **Note:** Alternatively, the View can be an HTML/JSP form instead of Swing:
> ```html
> <form action="register" method="post">
>     Username: <input type="text" name="username"><br>
>     Password: <input type="password" name="password"><br>
>     <input type="submit" value="Register">
> </form>
> ```

---

### Controller – `RegistrationController.java`

```java
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class RegistrationController {
    private UserModel userModel;
    private UserView userView;

    public RegistrationController(UserModel userModel, UserView userView) {
        this.userModel = userModel;
        this.userView = userView;

        // Attach the listener to the View's button
        this.userView.addRegisterListener(new RegisterListener());
    }
}

class RegisterListener implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e) {
        // 1. Read input from View
        String username = userView.getUsername();
        String password = userView.getPassword();

        // 2. Push it into the Model
        userModel.setUsername(username);
        userModel.setPassword(password);

        // 3. Process registration logic, interact with DAOs, etc.
    }
}
```

**What it does:** When the user clicks "Register", `RegisterListener` fires. It:
1. Reads the username/password from the View
2. Pushes them into the Model
3. Could then call a DAO to save to a database, etc.

The Controller is the only class that holds references to both `UserModel` and `UserView`.

---

## How All Three Fit Together

```java
public class Main {
    public static void main(String[] args) {
        UserModel model = new UserModel();
        UserView view = new UserView();
        RegistrationController controller = new RegistrationController(model, view);
    }
}
```

You create one of each, wire them together through the Controller, and MVC takes care of the rest.

---

## Best Practices

| Component | Rule |
|---|---|
| **Model** | Keep it lightweight — only data and business rules |
| **View** | Keep it "dumb" — display only, no logic |
| **Controller** | Keep it lean — delegate logic, don't over-stuff it |
| **Tooling** | Use GUI builders like **WindowBuilder** to streamline Swing UI development |

---

## MVC vs. Adapter Pattern (Quick Comparison)

| | Adapter Pattern | MVC Architecture |
|---|---|---|
| **Type** | Design Pattern (structural) | Architectural Pattern |
| **Scope** | Single class-to-class bridge | Entire application structure |
| **Goal** | Make incompatible interfaces work together | Separate data, UI, and control logic |

---

## Key Takeaways

| Concept | Summary |
|---|---|
| **MVC stands for** | Model, View, Controller |
| **Model** | Data + business logic, no UI |
| **View** | UI only, no business logic ("dumb") |
| **Controller** | The mediator — reads View input, updates Model |
| **Main benefit** | Separation of concerns → easier to maintain, test, scale |
| **Java examples** | Model = POJO, View = Swing/JSP, Controller = ActionListener/Servlet |