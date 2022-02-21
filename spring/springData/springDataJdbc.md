> 참고: [https://docs.spring.io/spring-data/jdbc/docs](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference) // [https://www.baeldung.com/spring-data-jdbc-intro] (https://www.baeldung.com/spring-data-jdbc-intro)
# Spring Data JDBC

Spring Data Jdbc는 Spring Data의 일부로서 JDBC 기반 저장소를 쉽게 구현할 수 있다.

## Spring Data JDBC 특징

+ Spring Data JPA만큼 복잡하지 않다. (캐시, 지연 로딩 등 JPA의 많은 기능을 제공하지 않는다.)
+ 자체 ORM이 있으며 매핑된 엔터티, 저장소, 쿼리 주석 및 JdbcTemplate과 같이 Spring Data JPA와 함께 사용되는 대부분의 기능을 제공
+ 스키마 생성을 지원하지 않기에 명시적으로 생성해 주어야 한다.

## 프로젝트에 Spring Data JDBD 추가하기

Spring Data JDBD의 사용을 위해서는, JDBC 의존성 스타터가 있는 Spring Boot 애플리케이션이 필요하다. <br>
이에 Spring initializr에서 Spring Boot 기반으로 프로젝트 생성시 Spring Data JDBC를 의존성으로 추가하거나, <br>
만약 이미 생성된 프로젝트에 의존성을 부여 할 경우에는 pom.xml에 아래 코드를 추가해주면 된다.

```xml
<dependency> 
    <groupId>org.springframework.boot</groupId> 
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency> 
```

