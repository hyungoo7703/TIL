> 참고: [https://docs.spring.io/spring-data](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#project)
# Spring Data

많은 관계형 및 비관계형 데이터 저장소를 사용하여 개발에 핵심 Spring 개념을 적용하기 위한 프로젝트 이다.

## Spring Data 장점

+ 객체를 통해 DB테이블을 간접적으로 다루기에 직관적인 조작이 가능해진다. 
+ 제공되는 메소드를 통해 조작하며 적합한 쿼리를 자동으로 등록해준다.
+ 페이지 매김 지원, 동적 쿼리 실행, 맞춤형 데이터 액세스 코드 통합 기능 등 다양한 기능 지원이 된다.

## Spring Boot 환경에서 Spring Data 사용하는 방법

Spring Data 관련된 프로젝트 들은 <br>
Spring Module을 뒷받침하는 핵심 개념 Spring Data Commons에 더해 <br>
JPA, MongoDB 같은 기술들을 결합하여 **JpaRepository, MongoRepository**와 같이 기술 별 추상화가 잘 구현되어 있다. <br>

> Spring initializr: [https://start.spring.io](https://start.spring.io/)  

우리는 위 Spring initializr를 통해 Spring Boot 기반으로 프로젝트 설정시 Spring Data와 관련된 종속성을 부여할 때 <br>
사용 할 기술의 Spring Data 모듈에 대한 의존성을 추가하면 된다. (ex: Spring Data JPA, Spring Data MongoDB) <br>

이미 생성된 프로젝트에 의존성을 부여 할 경우에는 (ex: Maven 프로젝트에 Spring Data JPA를 추가할 경우) <br>
pom.xml에 아래 코드를 추가해주면 된다.

```xml
  <dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-jpa</artifactId>
  </dependency>
```

하위 프로젝트에 대한 정리는 이곳을 참고하면 된다. 

+ [Spring Data JDBC](springData/springDataJdbc.md)

