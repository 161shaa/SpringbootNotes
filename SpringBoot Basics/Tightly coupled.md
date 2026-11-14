# Tight Coupling in Java

## 1. What is Tight Coupling?

Tight coupling occurs when one class is highly dependent on another concrete class to perform its operations.

If we make changes to one class, those changes may force us to modify other dependent classes as well.

## 2. Example of Tight Coupling

### Code Structure

```text
TightCoupling/
│
├── EmailService.java
├── UserService.java
└── Main.java
```

### EmailService.java

```java
class EmailService {

    void sendEmail() {
        System.out.println("Sending email notification...");
    }
}
```

### UserService.java

```java
class UserService {

    // Direct dependency on a concrete class
    EmailService emailService = new EmailService();

    void registerUser() {

        System.out.println("User registered successfully!");

        emailService.sendEmail();
    }
}
```

### Main.java

```java
public class Main {

    public static void main(String[] args) {

        UserService userService = new UserService();

        userService.registerUser();
    }
}
```

### Output

```text
User registered successfully!
Sending email notification...
```

## 3. Why Is This Tightly Coupled?

Look at this line inside `UserService`:

```java
EmailService emailService = new EmailService();
```

Here, `UserService` is directly creating an object of `EmailService`.

This means:

1. `UserService` knows about the concrete `EmailService` class.
2. `UserService` is responsible for creating its dependency.
3. `UserService` cannot easily switch to another notification service.
4. Both classes are strongly dependent on each other.

For example, suppose we want to replace email notifications with SMS notifications.

We would need to modify the `UserService` class.

```java
class UserService {

    SMSService smsService = new SMSService();

    void registerUser() {

        System.out.println("User registered successfully!");

        smsService.sendSMS();
    }
}
```

We have to modify the existing class just to change the notification mechanism.

This demonstrates tight coupling.

## 4. Drawbacks of Tight Coupling

| Drawback | Explanation |
|---|---|
| Difficult to maintain | Changes in one class can affect other dependent classes. |
| Poor flexibility | Replacing one implementation with another requires modifying dependent classes. |
| Difficult to test | Testing a class independently becomes difficult because it creates its own dependencies. |
| Code reusability | Classes become less reusable because they depend on specific implementations. |
| Difficult to extend | Adding new functionality may require modifying existing classes. |
| High dependency | Classes become heavily dependent on concrete implementations. |
| Violates Open/Closed Principle | Frequent modifications may be required when introducing new implementations. |

## 5. Conclusion

Tight coupling makes classes highly dependent on concrete implementations.

When one class directly creates and manages objects of another class, changing the dependency can require changes in the dependent class.

**Key takeaway:**

Tight Coupling = High Dependency + Low Flexibility + Difficult Maintenance.

In the next step, we can understand how Loose Coupling solves these problems using interfaces and Dependency Injection.
