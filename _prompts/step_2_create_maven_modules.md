# Create Maven Modules

The project must be structured as a multi-module Maven project with the following modules:

* application
* api
* common
* user
* author
* book

> Note: Do not create any classes until I explicitly ask.

---

## Parent POM Configuration

* All modules must be declared in the main `pom.xml` (parent POM).
* The parent POM must define **dependency management** (e.g., versions, BOM imports).
* The parent POM must not include actual dependencies used by modules; it should only manage them.
* Shared build configuration (plugins, compiler settings, etc.) must be defined in the parent POM.

---

## Module Responsibilities

### application

* Contains the main class (entry point).
* Responsible for bootstrapping and configuring the entire application.
* Acts as the **composition root**.
* Includes the actual dependencies required to run the application.

### api

* Contains REST controllers and request/response DTOs.
* Responsible for handling HTTP requests and responses.
* Must not contain business logic.

### common

* Contains shared utilities, exceptions, constants, and base classes.
* Can be used by all modules.

### user, author, book

* Contain domain logic for each feature.
* Include business rules, domain models, and service logic.
* Must not depend on the `api` module.

---

## Inter-module Dependency Rules

* `application` → can depend on all modules
* `api` → can depend on `user`, `author`, `book`, and `common`
* `user`, `author`, `book` → can depend only on `common`
* `common` → must not depend on any other module

### Forbidden Dependencies

* Domain modules must not depend on `api`
* `common` must remain independent
* Avoid circular dependencies between modules

---

## Internal Package Structure

### Example: user module

```
com.library.user
├── domain
│   ├── models         --> Core business objects (User, UserProfile, etc.).
│   ├── services       --> Business logic / orchestration. Operates on domain models, enforces rules, returns service-layer objects or DTOs.
│   ├── mappers        --> Converts persistence entities → service-layer objects. Example: UserMapper converts UserEntity → User.
│   ├── exceptions     --> Domain-specific exceptions (e.g., UserAlreadyExistsException).
│   └── validators     --> Business rules validations (optional).
│
├── facade
│   ├── dtos           --> DTOs for external consumption (used by API module). Example: UserResponseDto, UserCreateRequestDto.
│   ├── mappers        --> Converts service-layer objects → facade DTOs.
│
├── infrastructure
│   └── persistence
│       ├── entities      --> JPA/Hibernate entities (UserEntity, UserProfileEntity, etc.).
│       ├── repositories  --> Repository interfaces (e.g., UserRepository).
│       └── impl          --> Custom repository implementations if needed.
│   ├── cache             --> Optional: caching mechanisms.
│   ├── messaging         --> Optional: event producers/consumers.
│   └── external          --> Optional: integration with external systems.
```

---

### api module

```
com.library.api
├── controller   --> Contains REST controllers that handle HTTP requests and responses.
                  These classes call the appropriate services in the domain modules
                  or the application module to process requests.

├── dto          --> Contains Data Transfer Objects (DTOs) specifically for API communication.
                  These are often slightly different from domain DTOs, tailored for request
                  payloads or response formats. Example: UserResponseDto, UserCreateRequestDto.

├── mapper       --> Contains classes or interfaces that convert between domain objects
                  (or domain DTOs) and API DTOs. For example, mapping User entity to
                  UserResponseDto or vice versa.
```

---

### application module

```
com.library.application
├── config      --> Contains global or module-level Spring configuration classes.
                 Examples: security configuration, bean definitions, or shared module settings.
                 This is where you configure anything that other modules or the application 
                 itself may need at runtime.

├── bootstrap   --> Contains the main entry point of the application (the class with the main method)
                 and any classes that are responsible for starting up the Spring Boot application.
                 It orchestrates initialization but does not contain domain logic.
```

---

### common module

```
com.library.common
├── exception   --> Contains shared exception classes used across multiple modules.
                 Examples: CustomNotFoundException, InvalidInputException.
                 Helps standardize error handling across the project.

├── util        --> Contains utility or helper classes used by multiple modules.
                 Examples: DateUtils, StringUtils, ValidationUtils.
                 Should be framework-agnostic and contain no business logic.

├── config      --> Contains shared configuration classes that can be reused across modules.
                 Examples: common bean definitions, property loaders, shared converters.
```

---

## Key Architectural Principles

* Follow separation of concerns
* Keep domain logic independent from frameworks
* Use `application` as the composition root
* Prefer clear boundaries over convenience
* Design for scalability and maintainability


## Create Main Class

Under `com.library.application.bootstrap`, create a class that scans the 
main package and runs the Spring Boot application. This class must contain the `main` method.

# Pull Request Instructions

Each file is prefixed with `step_{number}` (e.g., `step_1`). For every step:

## Branch Naming
Create a new branch using the file name with the `prompts/` prefix. For example:

```
prompts/{current_file_name}
```


## Pull Request
Open a pull request for the branch, ensuring the following:

- The **title** clearly indicates the step number and the main change.  
  Example: `Step 1: Create multi-module Maven project`
- The **description** explains what was done in this step and why.
- Only the changes specific to this step are included in the pull request.