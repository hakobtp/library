# Dependencies

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