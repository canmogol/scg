# scg
Spring Cloud Gateway - SpringBoot3

## Building
You can use maven to build the application.
```
mvn clean install
```

## Running
You can run the application as a Jar file. You pass the `application.yml` file location.

```
java -jar target/scg-0.0.1-SNAPSHOT.jar --spring.config.location=file:///path/to/configuration/application.yml
```

## Routes
You can use the `routes` endpoint to list the active routes.
```
curl http://localhost:9090/actuator/gateway/routes
```

You can use the `refresh` endpoint to reload the routes.
```
curl -X POST http://localhost:9090/actuator/refresh
```
