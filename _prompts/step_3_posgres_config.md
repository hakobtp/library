# Docker and Database Configuration

## PostgreSQL Setup

* Add a PostgreSQL service using Docker.
* Configure the following environment variables:

    * `POSTGRES_DB=library`
    * `POSTGRES_USER=library_user`
    * `POSTGRES_PASSWORD=library_password`

---

## Docker Configuration

* Create a `Dockerfile` for the `application` module. Use the following example:
  ```dockerfile
  FROM eclipse-temurin:17.0.5_8-jre-focal as builder
  WORKDIR extracted
  ADD ./target/*.jar app.jar
  RUN java -Djarmode=layertools -jar app.jar extract
  
  FROM eclipse-temurin:17.0.5_8-jre-focal
  WORKDIR application
  COPY --from=builder extracted/dependencies/ ./
  COPY --from=builder extracted/spring-boot-loader/ ./
  COPY --from=builder extracted/snapshot-dependencies/ ./
  COPY --from=builder extracted/application/ ./
  EXPOSE 8080
  ENTRYPOINT ["java", "org.springframework.boot.loader.launch.JarLauncher"]
  ```

* Ensure that the application can connect to the PostgreSQL container 
   (e.g., by using the container name as the database host).

---

## Application Configuration

Create an `application.yml` file inside the `application` module and configure the database connection.

Please make sure `ddl-auto` is set to validate.

## Notes

* When running with Docker, replace `localhost` with the PostgreSQL container name (e.g., `postgres`).
* Ensure PostgreSQL is started before the application.
* Use environment variables for sensitive data in production.
