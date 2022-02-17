> 참고: [https://docs.spring.io/spring-data](https://docs.spring.io/spring-data/commons/docs/current/reference/html/#project)
# Spring Data

많은 관계형 및 비관계형 데이터 저장소를 사용하여 개발에 핵심 Spring 개념을 적용하기 위한 프로젝트 이다.

## Spring Data의 장점

+ 객체를 통해 DB테이블을 간접적으로 다루기에 직관적인 조작이 가능해진다. 
+ 제공되는 메소드를 통해 조작하며 적합한 쿼리를 자동으로 등록해준다.
+ 페이지 매김 지원, 동적 쿼리 실행, 맞춤형 데이터 액세스 코드 통합 기능 등 다양한 기능 지원이 된다.

## Spring Boot 환경에서 Spring Data 사용하는 방법

Spring Data 관련된 프로젝트 들은 <br>
Spring Module을 뒷받침하는 핵심 개념 Spring Data Commons에 더해 <br>
JPA, MongoDB 같은 기술들을 결합하여 **JpaRepository, MongoRepository**와 같이 기술 별 추상화가 잘 구현되어 있다. <br>

> Spring initializr: [https://start.spring.io](https://start.spring.io/)  

우리는 위 start.spring.io에서 Spring Boot 기반으로 프로젝트 설정시 Spring Data와 관련된 종속성을 부여할 때 <br>
사용 할 기술의 Spring Data 모듈에 대한 의존성을 추가하면 된다. (ex: Spring Data JPA, Spring Data MongoDB) <br>

이미 생성된 프로젝트에 의존성을 부여 할 경우에는 <br>
Maven 프로젝트에 Spring Data JPA를 추가할 경우, pom.xml에 아래 코드를 추가해주면 된다.

```xml
<dependencies>
  <dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-jpa</artifactId>
  </dependency>
<dependencies>
```

하위 프로젝트에 대한 정리는 이곳을 참고하면 된다. 

+ [Spring Data JPA](springData/springDataJPA.md)
+ [Spring Data JDBC](springData/springDataJDBC.md)

## 핵심 개념

**핵심은 CrudRepository 인터페이스가 관리되는 엔티티 클래스에 대한 CRUD 기능이 제공되는 것이다.** <br>

Spring Data 저장소 추상화의 목표는 다양한 영속성 저장소에 대한 데이터 액세스 계층을 구현 시 필요한 코드의 양을 크게 줄이는 것이다. <br>
추상화의 중심 인터페이스는 Repository 이다. 관리할 도메인 클래스와 그 클래스의 ID 유형을 유형 인수로 사용한다.

```java
public interface CrudRepository<T, ID> extends Repository<T, ID> { // ID는 타입으로 많이 쓴다. (ex Long)

  <S extends T> S save(S entity);  // 주어진 엔티티를 저장한다.   
  Optional<T> findById(ID primaryKey); // 지정된 ID로 식별되는 엔터티를 반환한다.
  Iterable<T> findAll(); // 모든 엔터티를 반환한다.              
  long count(); // 엔터티 수를 반환한다.                       
  void delete(T entity); // 지정된 엔터티를 삭제한다.              
  boolean existsById(ID primaryKey); // 주어진 ID를 가진 엔터티가 있는지 확인한다.
  // ... 등등

}
```

우리는 사용 시 재정의하여 사용하거나, 주어진 대로 이용하는 것도 가능하다. <br>

주요 모듈로서 사용되는 <br>
PagingAndSortingRepository 인터페이스도 있으며 엔티티에 대한 페이지 매김 액세스를 쉽게 하기 위한 메소드를 제공한다.

```java
public interface PagingAndSortingRepository<T, ID> extends CrudRepository<T, ID> {

  Iterable<T> findAll(Sort sort);
  Page<T> findAll(Pageable pageable);
}
```

페이지 크기 20으로 User의 두 번째 페이지에 액세스하려면 다음과 같이 할 수 있게 된다.

```java
PagingAndSortingRepository<User, Long> repository = // 빈 접근
Page<User> users = repository.findAll(PageRequest.of(1, 20)); // 페이지 시작은 0 부터
```

