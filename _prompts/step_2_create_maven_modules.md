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
│   ├── model
│   ├── service
│
├── facade
│   ├── dto
│
├── infrastructure
│   ├── persistence
│   ├── config
```

---

### api module

```
com.library.api
├── controller
├── dto
├── mapper
```

---

### application module

```
com.library.application
├── config
├── bootstrap
```

---

### common module

```
com.library.common
├── exception
├── util
├── config
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