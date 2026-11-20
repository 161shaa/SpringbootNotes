# Spring Boot – Database Connectivity Without a Service Layer

## 1. Introduction

In this project, we will implement database insertion using Spring Boot, Spring Data JPA, and PostgreSQL.

We will create:

- Two Entity classes.
- Two Repository interfaces.
- Two Controller classes.
- Database configuration using `application.properties`.

**Important: We will NOT create a Service class in this project.**

Instead, our Controller classes will directly communicate with their respective Repository interfaces to save data into the database.

### Our requirement

We have two controllers:

1. StudentController
2. UserController

We will create two database tables:

1. `usertable_one`
2. `usertable_two`

Each controller will insert data into its respective database table.

---

## 2. Architecture and Request Flow

```text
                  POSTMAN
                     |
             HTTP POST Request
                     |
          +----------+----------+
          |                     |
          v                     v
   StudentController      UserController
          |                     |
          v                     v
   UserTwoRepository      UserRepository
          |                     |
          v                     v
      UserTwo.java            User.java
          |                     |
          v                     v
    usertable_two          usertable_one
```

### Request flow

**Student flow:**

1. Postman sends a POST request to StudentController.
2. Controller receives JSON data.
3. `@RequestBody` converts JSON into a UserTwo object.
4. Controller calls `userTwoRepository.save(student)`.
5. Repository communicates with PostgreSQL.
6. Data gets inserted into `usertable_two`.

**User flow:**

1. Postman sends a POST request to UserController.
2. Controller receives JSON data.
3. `@RequestBody` converts JSON into a User object.
4. Controller calls `userRepository.save(user)`.
5. Repository communicates with PostgreSQL.
6. Data gets inserted into `usertable_one`.

---

## 3. Project Structure

```text
practiceAfroze/
│
├── src/
│   │
│   ├── main/
│   │   │
│   │   ├── java/
│   │   │   └── com/example/practiceAfroze/
│   │   │       │
│   │   │       ├── PracticeAfrozeApplication.java
│   │   │       │
│   │   │       ├── Entity/
│   │   │       │   ├── User.java
│   │   │       │   └── UserTwo.java
│   │   │       │
│   │   │       ├── Repository/
│   │   │       │   ├── UserRepository.java
│   │   │       │   └── UserTwoRepository.java
│   │   │       │
│   │   │       └── Controller/
│   │   │           ├── UserController.java
│   │   │           └── StudentController.java
│   │   │
│   │   └── resources/
│   │       └── application.properties
│   │
│   └── test/
│       └── java/
│
├── target/
├── pom.xml
└── README.md
```

**Note:** Java package and class names are case-sensitive. Lowercase package names such as `entity`, `repository`, and `controller` are recommended. The code below follows the package names used in this project.

---

## 4. Maven Dependencies – pom.xml

We need three main dependencies.

```xml
<dependencies>

    <!-- Spring Boot Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Data JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- PostgreSQL Driver -->
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <scope>runtime</scope>
    </dependency>

</dependencies>
```

### Why do we need these dependencies?

| Dependency | Purpose |
|---|---|
| Spring Boot Web | Creates REST APIs and handles HTTP requests |
| Spring Data JPA | Communicates with the database using repositories |
| PostgreSQL Driver | Connects the application to PostgreSQL |

Keep the existing Spring Boot parent, Java version, and Maven build plugin in your `pom.xml`.

---

## 5. Database Configuration – application.properties

Location:

```text
src/main/resources/application.properties
```

This file contains application and database configuration.

```properties
# Application Name
spring.application.name=practiceAfroze

# Server Configuration
server.port=8080

# PostgreSQL Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:1611/postgres
spring.datasource.username=YOUR_DB_USERNAME
spring.datasource.password=YOUR_DB_PASSWORD
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

Replace the username and password with your actual PostgreSQL credentials.

### Important properties

| Property | Purpose |
|---|---|
| `spring.datasource.url` | Database connection URL |
| `spring.datasource.username` | Database username |
| `spring.datasource.password` | Database password |
| `server.port` | Application port |
| `spring.jpa.hibernate.ddl-auto=update` | Updates the database schema based on entity mappings |
| `spring.jpa.show-sql=true` | Displays SQL queries in the console |

**Important:** Hibernate can automatically create the tables based on our Entity classes because we have configured `ddl-auto=update`.

Do not commit real database credentials to GitHub.

---

## 6. Creating Entity Classes

### What is an Entity?

An Entity is a Java class that represents a database table.

In our project, we need two database tables. Therefore, we create two Entity classes.

```text
User.java       ---> usertable_one

UserTwo.java    ---> usertable_two
```

### 6.1 User.java

This entity represents the first database table: `usertable_one`.

```java
package com.example.practiceAfroze.Entity;

import jakarta.persistence.*;

@Entity
@Table(name = "usertable_one")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String email;

    private int age;

    // Default constructor
    public User() {
    }

    // Parameterized constructor
    public User(String name, String email, int age) {
        this.name = name;
        this.email = email;
        this.age = age;
    }

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

### Understanding Entity annotations

| Annotation | Purpose |
|---|---|
| `@Entity` | Marks the Java class as a JPA entity |
| `@Table(name = "usertable_one")` | Specifies the database table name |
| `@Id` | Identifies the primary key |
| `@GeneratedValue` | Automatically generates the primary key |
| `GenerationType.IDENTITY` | Uses the database identity/auto-increment mechanism |

### 6.2 UserTwo.java

This entity represents the second database table: `usertable_two`.

```java
package com.example.practiceAfroze.Entity;

import jakarta.persistence.*;

@Entity
@Table(name = "usertable_two")
public class UserTwo {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String email;

    private int age;

    // Default constructor
    public UserTwo() {
    }

    // Parameterized constructor
    public UserTwo(String name, String email, int age) {
        this.name = name;
        this.email = email;
        this.age = age;
    }

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

### Entity-to-table mapping

| Entity class | Database table |
|---|---|
| User.java | usertable_one |
| UserTwo.java | usertable_two |

Both tables contain:

| Column | Data type |
|---|---|
| id | BIGINT / primary key |
| name | VARCHAR |
| email | VARCHAR |
| age | INTEGER |

**Remember:** The `@Table` annotation determines which database table an entity represents.

---

## 7. Creating Repository Interfaces

### What is a Repository?

A Repository is responsible for communicating with the database.

Spring Data JPA provides ready-made database operations through `JpaRepository`.

We will create two Repository interfaces, one for each Entity.

### 7.1 UserRepository.java

This repository works with the `User` entity.

```java
package com.example.practiceAfroze.Repository;

import com.example.practiceAfroze.Entity.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {

}
```

### 7.2 UserTwoRepository.java

This repository works with the `UserTwo` entity.

```java
package com.example.practiceAfroze.Repository;

import com.example.practiceAfroze.Entity.UserTwo;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserTwoRepository extends JpaRepository<UserTwo, Long> {

}
```

### How does Spring Data JPA know which table to use?

Look at this declaration:

```java
public interface UserRepository
        extends JpaRepository<User, Long> {
}
```

`JpaRepository` accepts two generic parameters:

```java
JpaRepository<EntityClass, PrimaryKeyDataType>
```

For example:

```java
JpaRepository<User, Long>
```

- `User` identifies the entity managed by this repository.
- `Long` identifies the data type of its primary key.

Spring Data JPA uses the entity mapping to determine the database table.

The mapping happens like this:

```text
UserRepository
      |
      v
JpaRepository<User, Long>
      |
      v
User.java
      |
      v
@Table(name = "usertable_one")
      |
      v
usertable_one
```

Similarly:

```text
UserTwoRepository
      |
      v
JpaRepository<UserTwo, Long>
      |
      v
UserTwo.java
      |
      v
@Table(name = "usertable_two")
      |
      v
usertable_two
```

### Why don't we implement the repository methods?

We only declare an interface:

```java
public interface UserRepository
        extends JpaRepository<User, Long> {
}
```

Spring Data JPA automatically creates a repository implementation at runtime.

We get methods such as:

| Method | Purpose |
|---|---|
| `save()` | Insert or update a record |
| `findAll()` | Retrieve all records |
| `findById()` | Retrieve a record by ID |
| `deleteById()` | Delete a record by ID |
| `count()` | Count records |

We don't need to write the basic SQL queries ourselves.

---

## 8. Creating Controllers – WITHOUT A SERVICE LAYER

### Important architectural decision

In this project, we are deliberately NOT creating a Service class.

Our architecture is:

```text
Controller ---> Repository ---> Entity ---> Database
```

Normally, a Spring Boot application can use a Service layer to hold business logic.

However, for this simple learning project, we will directly inject the Repository into the Controller.

We will create two controllers:

1. StudentController
2. UserController

Each controller will communicate with its respective Repository.

### 8.1 StudentController.java

Requirement:

When we call the StudentController API, data should be inserted into `usertable_two`.

```java
package com.example.practiceAfroze.Controller;

import com.example.practiceAfroze.Entity.UserTwo;
import com.example.practiceAfroze.Repository.UserTwoRepository;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/student")
public class StudentController {

    private final UserTwoRepository userTwoRepository;

    // Constructor Injection
    public StudentController(UserTwoRepository userTwoRepository) {
        this.userTwoRepository = userTwoRepository;
    }

    // Insert student into usertable_two
    @PostMapping("/save")
    public String saveStudent(@RequestBody UserTwo student) {

        userTwoRepository.save(student);

        return "Student data inserted into usertable_two";
    }
}
```

### Understanding the code

**1. `@RestController`**

```java
@RestController
```

Marks this class as a REST controller.

Spring manages this class and allows it to handle HTTP requests.

**2. `@RequestMapping`**

```java
@RequestMapping("/student")
```

Defines the common URL prefix for the controller.

**3. Constructor Injection**

```java
private final UserTwoRepository userTwoRepository;

public StudentController(UserTwoRepository userTwoRepository) {
    this.userTwoRepository = userTwoRepository;
}
```

Spring automatically provides the required repository object.

We don't manually create it using `new`.

**4. `@PostMapping`**

```java
@PostMapping("/save")
```

Maps HTTP POST requests to this method.

The complete endpoint becomes:

```text
POST http://localhost:8080/student/save
```

**5. `@RequestBody`**

```java
public String saveStudent(@RequestBody UserTwo student)
```

Receives JSON data from the request body and converts it into a Java `UserTwo` object.

**6. Repository save method**

```java
userTwoRepository.save(student);
```

Saves the entity into the database table mapped by `UserTwo`, which is `usertable_two`.

### 8.2 UserController.java

Requirement:

When we call the UserController API, data should be inserted into `usertable_one`.

```java
package com.example.practiceAfroze.Controller;

import com.example.practiceAfroze.Entity.User;
import com.example.practiceAfroze.Repository.UserRepository;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/user")
public class UserController {

    private final UserRepository userRepository;

    // Constructor Injection
    public UserController(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // Insert user into usertable_one
    @PostMapping("/save")
    public String saveUser(@RequestBody User user) {

        userRepository.save(user);

        return "User data inserted into usertable_one";
    }
}
```

### Understanding the code

This controller follows the same approach as StudentController.

The only differences are:

| StudentController | UserController |
|---|---|
| `UserTwo` entity | `User` entity |
| `UserTwoRepository` | `UserRepository` |
| `/student/save` | `/user/save` |
| `usertable_two` | `usertable_one` |

**Important:** The controller does not decide which database table receives the data.

The repository's Entity type identifies the entity, and the Entity's `@Table` annotation identifies the database table.

---

## 9. Testing with Postman

Now our application is ready.

Start your Spring Boot application.

We will test both controllers using Postman.

### 9.1 Insert data into usertable_two

**HTTP Method:** POST

**URL:**

```text
http://localhost:8080/student/save
```

In Postman:

1. Select POST.
2. Enter the URL.
3. Open the Body tab.
4. Select raw.
5. Select JSON.
6. Enter the following request body.
7. Click Send.

**JSON Request:**

```json
{
    "name": "Rahul",
    "email": "rahul@gmail.com",
    "age": 23
}
```

**Expected Response:**

```text
Student data inserted into usertable_two
```

### 9.2 Insert data into usertable_one

**HTTP Method:** POST

**URL:**

```text
http://localhost:8080/user/save
```

**JSON Request:**

```json
{
    "name": "Afroze",
    "email": "afroze@gmail.com",
    "age": 25
}
```

**Expected Response:**

```text
User data inserted into usertable_one
```

### Postman testing summary

| Controller | HTTP Method | Endpoint | Database table |
|---|---|---|---|
| StudentController | POST | `/student/save` | usertable_two |
| UserController | POST | `/user/save` | usertable_one |

---

## 10. Verify the Data in PostgreSQL

After sending both requests, open DBeaver or your PostgreSQL SQL console.

Execute:

```sql
SELECT * FROM usertable_one;

SELECT * FROM usertable_two;
```

Expected result:

### usertable_one

| id | name | email | age |
|---|---|---|---|
| 1 | Afroze | afroze@gmail.com | 25 |

### usertable_two

| id | name | email | age |
|---|---|---|---|
| 1 | Rahul | rahul@gmail.com | 23 |

Your actual IDs may differ if these tables already contain records.

---

## 11. How Does Everything Work Internally?

Let's understand what happens when we send a POST request.

Example:

```text
POST /student/save
```

### Step 1 – Postman

Postman sends JSON data:

```json
{
    "name": "Rahul",
    "email": "rahul@gmail.com",
    "age": 23
}
```

### Step 2 – Controller

Spring receives the request and identifies the matching controller method.

```java
@PostMapping("/save")
public String saveStudent(@RequestBody UserTwo student)
```

### Step 3 – JSON to Java Object

Spring uses its HTTP message converters (typically Jackson) to convert JSON into a Java object.

Conceptually:

```java
UserTwo student = new UserTwo();

student.setName("Rahul");
student.setEmail("rahul@gmail.com");
student.setAge(23);
```

### Step 4 – Repository

The controller calls:

```java
userTwoRepository.save(student);
```

### Step 5 – Spring Data JPA and Hibernate

Spring Data JPA provides the repository implementation.

Hibernate handles the entity persistence and generates the appropriate SQL.

Conceptually, the SQL operation is:

```sql
INSERT INTO usertable_two (name, email, age)
VALUES ('Rahul', 'rahul@gmail.com', 23);
```

### Step 6 – PostgreSQL

PostgreSQL executes the INSERT statement and stores the record.

### Step 7 – Response

The controller returns the success message to Postman.

---

## 12. Important Concepts – Quick Revision

| Concept | Explanation |
|---|---|
| Entity | Java class mapped to a database table |
| `@Entity` | Marks a class as a JPA entity |
| `@Table` | Specifies the database table name |
| Repository | Provides database access operations |
| `JpaRepository` | Provides ready-made CRUD methods |
| Controller | Handles HTTP requests |
| `@RestController` | Marks a REST API controller |
| `@RequestMapping` | Defines a common URL prefix |
| `@PostMapping` | Maps HTTP POST requests |
| `@RequestBody` | Converts request JSON into a Java object |
| `save()` | Saves an entity using the repository |
| Constructor Injection | Injects dependencies through a constructor |
| `application.properties` | Stores application and database configuration |

---

## 13. Why Are We Not Using a Service Layer?

In this project, our Controller directly communicates with the Repository.

```text
Controller ---> Repository ---> Database
```

This is perfectly valid for a simple application.

However, in larger production applications, we generally introduce a Service layer:

```text
Controller ---> Service ---> Repository ---> Database
```

The Service layer is useful for:

- Business logic.
- Transaction management across multiple operations.
- Validation and processing.
- Reusing business logic across multiple controllers.
- Keeping controllers lightweight.

**For this exercise, we intentionally skip the Service layer to understand how Controllers and Repositories work together.**

---

## 14. Final Project Checklist

- [ ] Created Spring Boot project.
- [ ] Added Spring Web dependency.
- [ ] Added Spring Data JPA dependency.
- [ ] Added PostgreSQL driver.
- [ ] Configured `application.properties`.
- [ ] Created `User.java` entity.
- [ ] Created `UserTwo.java` entity.
- [ ] Created `UserRepository.java`.
- [ ] Created `UserTwoRepository.java`.
- [ ] Created `StudentController.java`.
- [ ] Created `UserController.java`.
- [ ] Tested StudentController using POST in Postman.
- [ ] Tested UserController using POST in Postman.
- [ ] Verified both tables in PostgreSQL.

### Final takeaway

For this project:

```text
StudentController
       |
       v
UserTwoRepository
       |
       v
UserTwo Entity
       |
       v
usertable_two
```

```text
UserController
       |
       v
UserRepository
       |
       v
User Entity
       |
       v
usertable_one
```

**No Service class is created or used anywhere in this implementation.**
