# gateway-and-clients
example showing a spring boot gateway, and 2 react clients


#

We want to access each client though these URLS:
* http://localhost:8080/client1
* http://localhost:8081/client2

When we run the application (on localhost), we use these ports:

* gateway: 8080
* client1: 5173
* client2: 5174

# Spring Boot Gateway

The gateway is a spring boot app, with this dependency:

```groovy
implementation 'org.springframework.cloud:spring-cloud-starter-gateway'
```

The configuration file will list each client. The clients are
run on different ports, and have a path to uniquely define the
URL to each.

```yaml
server:
  port: 8080

spring:
  cloud:
    gateway:
      routes:
        - id: client1
          uri: http://localhost:5173
          predicates:
            - Path=/client1/**
          filters:
            - StripPrefix=0
        - id: client2
          uri: http://localhost:5174
          predicates:
            - Path=/client2/**
          filters:
            - StripPrefix=0
```

# React Clients

The React clients are Vite projects, with react and typescript.
Update vite.config.ts, and add the base: 

```javascript
export default defineConfig({
    plugins: [react()],
    base: "/client1"
})

```

