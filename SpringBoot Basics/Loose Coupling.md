# Loose Coupling Using Dependency Injection in Java

## 1. What is Loose Coupling?

Solution to Tight Coupling is Loose coupling. 

Loose coupling occurs when classes have minimal dependency on each other.

A class does not need to know the internal implementation of another class. Instead, it communicates through abstractions such as interfaces.

This makes the application easier to maintain, extend, test, and modify.

## 2. How to Achieve Loose Coupling?

One of the primary techniques to achieve loose coupling is **Dependency Injection (DI)**.

Dependency Injection means providing an object with its required dependencies from outside rather than allowing it to create those dependencies internally.

### Key Concepts

- Loose Coupling = The design goal.
- Dependency Injection = A technique to achieve loose coupling.
- Interface / Abstraction = Helps classes depend on contracts rather than concrete implementations.

## 3. Example of Loose Coupling Using Dependency Injection

### Code Structure

```text
LooseCoupling/
│
├── NotificationService.java
├── EmailService.java
├── UserService.java
└── Main.java
```

### Step 1: Create an Interface

**NotificationService.java**

```java
interface NotificationService {

    void sendNotification();

}
```

The interface defines a contract that every notification service must implement.

### Step 2: Implement the Interface

**EmailService.java**

```java
class EmailService implements NotificationService {

    @Override
    public void sendNotification() {

        System.out.println("Email notification sent!");

    }
}
```

Here, EmailService provides its own implementation of the interface.

### Step 3: Inject the Dependency

**UserService.java**

```java
class UserService {

    private NotificationService notificationService;

    // Constructor Injection
    UserService(NotificationService notificationService) {

        this.notificationService = notificationService;

    }

    void registerUser() {

        System.out.println("User registered successfully!");

        notificationService.sendNotification();

    }
}
```

Notice that UserService depends on the NotificationService interface instead of directly depending on EmailService.

Also, UserService does not create the dependency itself. It receives the dependency through its constructor.

### Step 4: Main Class

**Main.java**

```java
public class Main {

    public static void main(String[] args) {

        NotificationService service = new EmailService();

        UserService userService = new UserService(service);

        userService.registerUser();

    }
}
```

### Output

```text
User registered successfully!
Email notification sent!
```

## 4. Why Is This Loosely Coupled?

Consider this line:

```java
UserService userService = new UserService(service);
```

We are creating the dependency outside the UserService class and injecting it through the constructor.

UserService only knows about the NotificationService interface.

It does not know whether the actual implementation is EmailService, SMSService, or another notification service.

### Let's Add Another Implementation

**SMSService.java**

```java
class SMSService implements NotificationService {

    @Override
    public void sendNotification() {

        System.out.println("SMS notification sent!");

    }
}
```

Now we can switch implementations without modifying UserService.

```java
public class Main {

    public static void main(String[] args) {

        NotificationService service = new SMSService();

        UserService userService = new UserService(service);

        userService.registerUser();

    }
}
```

### Output

```text
User registered successfully!
SMS notification sent!
```

**Important Observation:**

We changed the notification implementation without making any changes to the UserService class.

This is the benefit of loose coupling.

## 5. Dependency Injection Types

There are three commonly discussed types of Dependency Injection:

| Type | Explanation |
|---|---|
| Constructor Injection | Dependencies are provided through a class constructor. |
| Setter Injection | Dependencies are provided through setter methods. |
| Field Injection | Dependencies are injected directly into class fields. |

Constructor Injection is generally preferred because it makes required dependencies explicit and supports immutable fields.

## 6. Tight Coupling vs Loose Coupling

| Tight Coupling | Loose Coupling |
|---|---|
| Depends on concrete classes | Depends on abstractions |
| Creates dependencies internally | Receives dependencies externally |
| Difficult to replace implementations | Easy to replace implementations |
| Difficult to test independently | Easier to test independently |
| Less flexible | More flexible |
| Higher maintenance effort | Easier maintenance |

## 7. Advantages of Loose Coupling

1. **Maintainability:** Changes in one implementation have less impact on dependent classes.
2. **Flexibility:** Implementations can be replaced without modifying the dependent class.
3. **Testability:** Dependencies can be replaced with mock or test implementations.
4. **Reusability:** Classes can be reused with different implementations.
5. **Extensibility:** New implementations can be introduced more easily.

## 8. Conclusion

Dependency Injection helps achieve loose coupling by separating the creation of dependencies from the classes that use them.

Instead of depending on concrete implementations, classes can depend on interfaces or abstractions.

### Remember

- Tight Coupling = High dependency between classes.
- Loose Coupling = Minimal dependency between classes.
- Dependency Injection = Providing dependencies externally.
- Abstraction = Depending on contracts rather than concrete implementations.

**Important:** Dependency Injection is a design technique, not something exclusive to Spring Boot. We can implement it using plain Java, as demonstrated above.
