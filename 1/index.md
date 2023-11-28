# IDEA远程调试docker容器


{{< music auto="https://music.163.com/#/song?id=2030754234"  autoplay="true" fixed="true">}}

## 远程 Debug 配置

`IDEA`中添加`Remote JVM Debug`配置项，拷贝以下配置

```java
-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005
```



## 容器启动配置

### dockerfile配置

```java
FROM openjdk:8
ADD  ./myapp.jar ./
EXPOSE 8082
EXPOSE 5005
ENTRYPOINT ["java","-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005","-jar","myapp.jar","--spring.profiles.active=prod"]

```

### docker-compose配置

暴露debug端口

```java
  Server:
    image: server
    container_name: server
    ports:
      - "30010:8082"
      - "5005:5005"
```

### 配置debug调试

<img src="/images/1701158568740.png" >

### 
