# Clean Architecture in Java with Spring Boot

Clean Architecture, proposed by Robert C. Martin (Uncle Bob), is a software design philosophy that emphasizes separation of concerns through well-defined layers.
These layers enforce dependency inversion, ensuring that higher-level policies (business rules) do not depend on lower-level details (frameworks, databases, etc.).

---

## **Overview of Clean Architecture Layers**

### **1. Entities (Core Business Logic)**
- Represents the core of the application.
- Contains business rules and domain models.
- Independent of frameworks, databases, or external systems.

#### Example:
```java
public class User {
    private Long id;
    private String name;
    private String email;

    // Business rules or core methods
    public boolean isEmailValid() {
        return email != null && email.contains("@");
    }

    // Getters and setters
}
```

---

### **2. Use Cases (Application Logic)**
- Encapsulates the application's specific business rules.
- Coordinates the interaction between entities and interface adapters.

#### Example:
```java
public class CreateUserUseCase {
    private final UserRepository userRepository;

    public CreateUserUseCase(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User createUser(String name, String email) {
        User user = new User();
        user.setName(name);
        user.setEmail(email);

        if (!user.isEmailValid()) {
            throw new IllegalArgumentException("Invalid email address");
        }

        return userRepository.save(user);
    }
}
```

---

### **3. Interface Adapters (Adapters)**
- Translates data between external systems and use cases.
- Includes controllers for handling HTTP requests and repositories for data interaction.

#### Example: Controller
```java
@RestController
@RequestMapping("/users")
public class UserController {
    private final CreateUserUseCase createUserUseCase;

    public UserController(CreateUserUseCase createUserUseCase) {
        this.createUserUseCase = createUserUseCase;
    }

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody UserRequest request) {
        User user = createUserUseCase.createUser(request.getName(), request.getEmail());
        return ResponseEntity.ok(user);
    }
}
```

#### Example: Repository Interface
```java
public interface UserRepository {
    User save(User user);
    Optional<User> findById(Long id);
}
```

---

### **4. Frameworks and Drivers**
- Implements lower-level technical details like databases and third-party libraries.
- Should depend on interfaces defined in the higher layers (Dependency Inversion).

#### Example: Spring JPA Repository Implementation
```java
@Repository
public class JpaUserRepository implements UserRepository {
    private final JpaUserRepositoryInterface repository;

    public JpaUserRepository(JpaUserRepositoryInterface repository) {
        this.repository = repository;
    }

    @Override
    public User save(User user) {
        return repository.save(user);
    }

    @Override
    public Optional<User> findById(Long id) {
        return repository.findById(id);
    }

    interface JpaUserRepositoryInterface extends JpaRepository<User, Long> {}
}
```

---

## **Dependency Rules**
- **Outer layers** (Frameworks and Drivers) depend on **inner layers** (Entities, Use Cases).
- **Entities** do not depend on any other layer.
- Use **Dependency Injection** to manage dependencies.

---

## **Directory Structure**
```plaintext
src/main/java/com/example/cleanarchitecture
├── entities          # Core Business Logic
│   └── User.java
├── usecases          # Application Logic
│   └── CreateUserUseCase.java
├── adapters          # Interface Adapters
│   ├── controllers
│   │   └── UserController.java
│   ├── repositories
│   │   └── JpaUserRepository.java
├── frameworks        # Frameworks & Drivers
│   └── Spring-specific configurations, database settings, etc.
```

---

## **Benefits of Clean Architecture**
1. **Modularity**: Easier to test and maintain.
2. **Framework Independence**: Core logic isn’t tied to a specific framework.
3. **Ease of Testing**: Entities and Use Cases can be tested in isolation.
4. **Separation of Concerns**: Each layer has a clear responsibility.

---

## **Conclusion**
By adhering to Clean Architecture principles, you can build systems that are:
- Resilient to changes in frameworks or external dependencies.
- Maintainable and extendable.
- Testable at all levels.
