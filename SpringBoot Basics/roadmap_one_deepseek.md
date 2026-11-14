# Spring Boot Roadmap

## Phase 1: Fundamentals
- [ ] Core Java Refresher (OOP, Collections, Streams)
- [ ] Build Tools (Maven or Gradle)
- [ ] Inversion of Control (IoC) & Dependency Injection (DI)
- [ ] Spring Beans & Application Context

## Phase 2: Spring Boot Basics
- [ ] Spring Initializr & Project Setup
- [ ] `@SpringBootApplication` & Auto-configuration
- [ ] Configuration (`application.properties` / `application.yml`)
- [ ] Spring Profiles (dev, prod)
- [ ] Stereotype Annotations (`@Component`, `@Service`, `@Repository`)

## Phase 3: Building REST APIs
- [ ] Controllers (`@RestController`, `@GetMapping`, `@PostMapping`, etc.)
- [ ] Request Handling (`@RequestBody`, `@PathVariable`, `@RequestParam`)
- [ ] Returning Responses (`ResponseEntity`)
- [ ] Global Exception Handling (`@ControllerAdvice`)
- [ ] Data Transfer Objects (DTOs)

## Phase 4: Data Access (Database)
- [ ] Spring Data JPA & Hibernate
- [ ] Entities (`@Entity`, `@Id`, `@Column`)
- [ ] Repositories (`JpaRepository`)
- [ ] Connecting to Databases (H2, PostgreSQL, MySQL)
- [ ] Transactions (`@Transactional`)

## Phase 5: Security
- [ ] Spring Security Basics (Filter Chain)
- [ ] Authentication vs Authorization
- [ ] Password Hashing (BCrypt)
- [ ] JWT (JSON Web Tokens) for Stateless Auth

## Phase 6: Testing
- [ ] Unit Testing (JUnit 5 & Mockito)
- [ ] Integration Testing (`@SpringBootTest`)
- [ ] Slicing Tests (`@WebMvcTest`, `@DataJpaTest`)

## Phase 7: Advanced & Deployment
- [ ] Validation (`@Valid`, Hibernate Validator)
- [ ] Spring Boot Actuator (Health checks & Metrics)
- [ ] Caching (`@EnableCaching`)
- [ ] Dockerizing the App
- [ ] Deployment (AWS, Render, Railway, etc.)
