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
< X-TraceId: 64d94c75c4389e93bf51f1b53c4c9098
<
* Connection #0 to host 127.0.0.1 left intact
{"key1":"value1","key2":"value2"}%
```

Logs on SCG,
```
2023-08-14T00:34:45.705+03:00 DEBUG [scg,64d94c75c4389e93bf51f1b53c4c9098,bf51f1b53c4c9098] 9617 --- [ctor-http-nio-3] o.s.c.g.h.RoutePredicateHandlerMapping   : Route matched: route_spring-k8s_io_root

2023-08-14T00:34:45.712+03:00 DEBUG [scg,64d94c75c4389e93bf51f1b53c4c9098,bf51f1b53c4c9098] 9617 --- [ctor-http-nio-3] o.s.c.g.h.RoutePredicateHandlerMapping   : Mapping [Exchange: GET http://127.0.0.1:9090/spring-k8s] to Route{id='route_spring-k8s_io_root', uri=http://localhost:8080, order=0, predicate=Paths: [/spring-k8s], match trailing slash: true, gatewayFilters=[[[RewritePath /spring-k8s = '/'], order = 1]], metadata={}}

2023-08-14T00:34:45.713+03:00 DEBUG [scg,64d94c75c4389e93bf51f1b53c4c9098,bf51f1b53c4c9098] 9617 --- [ctor-http-nio-3] o.s.c.g.h.RoutePredicateHandlerMapping   : [e33ed070-2] Mapped to org.springframework.cloud.gateway.handler.FilteringWebHandler@14617b56

2023-08-14T00:34:45.714+03:00 DEBUG [scg,64d94c75c4389e93bf51f1b53c4c9098,bf51f1b53c4c9098] 9617 --- [ctor-http-nio-3] o.s.c.g.handler.FilteringWebHandler      : Sorted gatewayFilterFactories: [[GatewayFilterAdapter{delegate=org.springframework.cloud.gateway.filter.RemoveCachedBodyFilter@51b51641}, order = -2147483648], [GatewayFilterAdapter{delegate=org.springframework.cloud.gateway.filter.AdaptCachedBodyGlobalFilter@1a89414e}, order = -2147482648], [GatewayFilterAdapter{delegate=org.springframework.cloud.gateway.filter.NettyWriteResponseFilter@18bb1b88}, order = -1], [GatewayFilterAdapter{delegate=org.springframework.cloud.gateway.filter.ForwardPathFilter@139089a4}, order = 0], [GatewayFilterAdapter{delegate=org.springframework.cloud.gateway.filter.GatewayMetricsFilter@ff4b223}, order = 0], [[RewritePath /spring-k8s = '/'], order = 1], [GatewayFilterAdapter{delegate=org.springframework.cloud.gateway.filter.RouteToRequestUrlFilter@362a6aa5}, order = 10000], [GatewayFilterAdapter{delegate=org.springframework.cloud.gateway.config.GatewayNoLoadBalancerClientAutoConfiguration$NoLoadBalancerClientFilter@61554b6b}, order = 10150], [GatewayFilterAdapter{delegate=org.springframework.cloud.gateway.filter.WebsocketRoutingFilter@6a6e9289}, order = 2147483646], [GatewayFilterAdapter{delegate=dev.canm.scg.TraceFilterFactory@100eeedc}, order = 2147483647], [GatewayFilterAdapter{delegate=org.springframework.cloud.gateway.filter.NettyRoutingFilter@56266bda}, order = 2147483647], [GatewayFilterAdapter{delegate=org.springframework.cloud.gateway.filter.ForwardRoutingFilter@53b7bf01}, order = 2147483647]]
```

Logs on downstream Spring-k8 service,
```
2023-08-14T00:34:45.733+03:00 DEBUG [spring-k8s,64d94c75c4389e93bf51f1b53c4c9098,9877257f94bd921e] 5340 --- [nio-8080-exec-3] o.s.web.servlet.DispatcherServlet        : GET "/", parameters={}

2023-08-14T00:34:45.737+03:00  INFO [spring-k8s,64d94c75c4389e93bf51f1b53c4c9098,9877257f94bd921e] 5340 --- [nio-8080-exec-3] c.canmogol.k8s.KeyValuePairsController   : getPairs() called, returning: {key1=value1, key2=value2}

2023-08-14T00:34:45.740+03:00 DEBUG [spring-k8s,64d94c75c4389e93bf51f1b53c4c9098,9877257f94bd921e] 5340 --- [nio-8080-exec-3] o.s.web.servlet.DispatcherServlet        : Completed 200 OK
```



