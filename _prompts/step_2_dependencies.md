# dependencies

1) Add a `.gitignore` file for a Java project, VS Code, and IntelliJ IDEA. 
2) Add Spring Boot dependencies for building a REST API, PostgreSQL, Hibernate, and Lombok.

### Configuration

* Spring Boot version: 4.0.4
* Java version: 17
* Base package: `com.library`

### Lombok Configuration

Add Lombok support for Spring annotations in `lombok.config`:

```
lombok.copyableAnnotations+=org.springframework.beans.factory.annotation.Value
lombok.copyableAnnotations+=org.springframework.context.annotation.Lazy
```