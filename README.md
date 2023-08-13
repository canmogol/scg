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

## Tracing
The trace and span ID can be found in the log messages. Also, the trace ID is returned as a header, the name of the header can be set by `scg.trace.header` value, the defaul is `X-TraceId`.

```
-> % curl -v http://127.0.0.1:9090/spring-k8s
*   Trying 127.0.0.1:9090...
* Connected to 127.0.0.1 (127.0.0.1) port 9090 (#0)
> GET /spring-k8s HTTP/1.1
> Host: 127.0.0.1:9090
> User-Agent: curl/7.88.1
> Accept: */*
>
< HTTP/1.1 200 OK
< transfer-encoding: chunked
< Content-Type: application/json
< Date: Sun, 13 Aug 2023 21:29:17 GMT
< X-TraceId: 64d94b2d93a341d889ec842c18e04619
<
* Connection #0 to host 127.0.0.1 left intact
{"key1":"value1","key2":"value2"}%
```
