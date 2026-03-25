# Dependencies and Module Encapsulation

## Dependencies

* Add MapStruct dependency to the project.
* Add Spring Test dependencies.
* Add `org.springframework.modulith` for modular architecture support.
* Add ArchUnit

---

## Module Encapsulation Rules

### Domain Modules (`book`, `author`, `user`)

* Only the `facade` package must be exposed to other modules.
* The `domain` and `infrastructure` packages must remain internal.
* The `facade` package must have access to both `domain` and `infrastructure`.

---

### API Module

* The `api` module must be fully encapsulated.
* Other modules must not depend on or use code from the `api` module.
* Inside the `api` module, the `controller` package can access other internal packages (e.g., `dto`, `mapper`).

---

### Common Module

* The `common` module must be accessible to all modules.

---

### Application Module

* The `application` module acts as the composition root.
* It does not need strict encapsulation.
* It is responsible for configuring and running the application.

---

## Module Verification (Testing)

* Add unit tests to verify module boundaries using Spring Modulith.
* Ensure that:

    * Only allowed dependencies exist between modules
    * Internal packages are not accessed from outside
    * No forbidden dependencies are introduced
