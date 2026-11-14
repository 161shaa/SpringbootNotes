# ☕ Types of Dependency Injection (DI) in Java

Dependency Injection (DI) is a technique in which a class receives its dependencies from outside rather than creating them internally.

There are **3 types of Dependency Injection:**

1. Constructor Injection
2. Setter Injection
3. Field Injection

---

## 1. Constructor Injection

**Definition:** Dependencies are provided through the class constructor.

- Injected during object creation.
- Suitable for mandatory dependencies.
- Supports `final` fields.
- Recommended approach.

## 2. Setter Injection

**Definition:** Dependencies are provided through setter methods.

- Injected after object creation.
- Useful for optional dependencies.
- Dependencies can be changed later.
- Object can exist without its dependency.

## 3. Field Injection

**Definition:** Dependencies are provided directly to the class fields.

- Injected directly into instance variables.
- No constructor or setter method required.
- Commonly demonstrated using `@Autowired` in Spring.
- Generally avoided in production code.

---

## 4. Comparison of DI Types

| Feature | Constructor | Setter | Field |
|---|---|---|---|
| Injection through | Constructor | Setter method | Field |
| Injection time | Object creation | After creation | Direct assignment / framework |
| Mandatory dependency | Yes | Not enforced | Not enforced |
| `final` support | Yes | No | No |
| Recommendation | Preferred | Optional dependencies | Generally avoid |

---

## 5. Java Code Examples

For the following examples, assume this interface and implementation are already available:

```java
interface NotificationService {
    void sendNotification();
}

class EmailService implements NotificationService {

    public void sendNotification() {
        System.out.println("Email sent!");
    }
}
```

### A. Constructor Injection

**Dependency is provided through the constructor.**

```java
class UserService {

    NotificationService service;

    UserService(NotificationService service) {
        this.service = service;
    }
}
```

**Injection:**

```java
UserService user =
    new UserService(new EmailService());
```

### B. Setter Injection

**Dependency is provided through a setter method.**

```java
class UserService {

    NotificationService service;

    void setService(NotificationService service) {
        this.service = service;
    }
}
```

**Injection:**

```java
UserService user = new UserService();

user.setService(new EmailService());
```

### C. Field Injection

**Dependency is assigned directly to the field.**

```java
class UserService {

    NotificationService service;
}
```

**Injection:**

```java
UserService user = new UserService();

user.service = new EmailService();
```

---

## 6. Quick Revision

| DI Type | Remember |
|---|---|
| Constructor Injection | Pass dependency through constructor |
| Setter Injection | Pass dependency through setter method |
| Field Injection | Assign dependency directly to field |

### ⭐ Best Practice

- **Constructor Injection:** Prefer for mandatory dependencies.
- **Setter Injection:** Use for optional dependencies.
- **Field Injection:** Generally avoid in production applications.

**Key Takeaway:** All three techniques provide dependencies externally. The difference is how and when the dependency is injected.



##  Quick Comparison – DI Syntax

### 1. Constructor Injection
Dependency is passed through the constructor.

```java
UserService user = new UserService(notify);
```

### 2. Setter Injection
Object is created first, then dependency is injected through a setter method.

```java
UserService user = new UserService();
user.setNotificationService(notify);
```

### 3. Field Injection (Direct Assignment)
Dependency is assigned directly to the instance variable.

```java
UserService user = new UserService();
user.notification = notify;
```

**Remember:**

- Constructor → Pass through constructor.
- Setter → Pass through setter method.
- Field → Assign directly to field.
