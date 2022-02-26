> 참고: [https://docs.spring.io/spring-data/jdbc/docs](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference) // [https://www.baeldung.com/spring-data-jdbc-intro](https://www.baeldung.com/spring-data-jdbc-intro)
# Spring Data JDBC

Spring Data Jdbc는 Spring Data의 일부로서 JDBC 기반 저장소를 쉽게 구현할 수 있다.

## Spring Data JDBC 특징

+ Spring Data JPA만큼 복잡하지 않다. (캐시, 지연 로딩 등 JPA의 많은 기능을 제공하지 않는다.)
+ 자체 ORM이 있으며 매핑된 엔터티, 저장소, 쿼리 주석 및 JdbcTemplate과 같이 Spring Data JPA와 함께 사용되는 대부분의 기능을 제공
+ 스키마 생성을 지원하지 않기에 명시적으로 생성해 주어야 한다.

## 프로젝트에 Spring Data JDBC 추가하기

Spring Data JDBC의 사용을 위해서는, JDBC 의존성 스타터가 있는 Spring Boot 애플리케이션이 필요하다. <br>
이에 Spring initializr에서 Spring Boot 기반으로 프로젝트 생성시 Spring Data JDBC를 의존성으로 추가하거나, <br>
만약 이미 생성된 프로젝트에 의존성을 부여 할 경우에는 pom.xml에 아래 코드를 추가해주면 된다.

```xml
<dependency> 
    <groupId>org.springframework.boot</groupId> 
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency> 
```

## 핵심 개념

**핵심은 CrudRepository 인터페이스가 관리되는 엔티티 클래스에 대한 CRUD 기능이 제공되는 것이다.** <br>

Spring Data 저장소 추상화의 목표는 다양한 영속성 저장소에 대한 데이터 액세스 계층을 구현 시 필요한 코드의 양을 크게 줄이는 것이다. <br>
추상화의 중심 인터페이스는 Repository 이다. 관리할 도메인 클래스(T)와 그 클래스의 ID 유형을 유형 인수로 사용한다.

```java
public interface CrudRepository<T, ID> extends Repository<T, ID> { // ID는 타입으로 (ex Long)

  <S extends T> S save(S entity);  // 주어진 엔티티를 저장한다.   
  Optional<T> findById(ID primaryKey); // 지정된 ID로 식별되는 엔티티를 반환한다.
  Iterable<T> findAll(); // 모든 엔티티를 반환한다.              
  long count(); // 엔티티 수를 반환한다.                       
  void delete(T entity); // 지정된 엔티티를 삭제한다.              
  boolean existsById(ID primaryKey); // 주어진 ID를 가진 엔티티가 있는지 확인한다.
  // ... 등등

}
```

우리는 사용 시 재정의하여 사용하거나, 주어진 대로 이용하는 것도 가능하다. <br>

주요 모듈로서 사용되는 PagingAndSortingRepository 인터페이스는 <br>
엔티티에 대한 페이지 매김 액세스를 쉽게 하기 위한 메소드를 제공한다.

```java
public interface PagingAndSortingRepository<T, ID> extends CrudRepository<T, ID> {

  Iterable<T> findAll(Sort sort); // 정렬
  Page<T> findAll(Pageable pageable); // Pageable 제공

}
```

페이지 크기 20으로 User의 두 번째 페이지에 액세스하려면 다음과 같이 할 수 있게 된다.

```java
/* UserRepository가 CrudRepository에 상속 된 상황일 때  */
Page<User> users = UserRepository.findAll(PageRequest.of(1, 20)); // Spring Data의 페이지 시작은 0 부터 이므로 두번째 페이지는 1이다.
```

### 쿼리 메소드 3단계 프로세스 (기본 데이터에 대하여 쿼리 선언 가능)

**가정**: Person은 Long으로 ID값을 가지고 firstname과 lastname을 가지고 있다고 할 때,

1. 확장하는 인터페이스를 선언하고 처리해야 하는 도메인 클래스 및 ID 유형에 입력한다.

```java
  interface PersonRepository extends CrudRepository<Person, Long> {}
```

2. 인터페이스에서 쿼리 메소드를 선언

```java
  interface PersonRepository extends CrudRepository<Person, Long> {
    List<Person> findByLastname(String lastname); // lastname으로 찾는 기능 추가
  }
```

3. 리포지토리 인스턴스 의존성 주입 후 사용

```java
class SomeClient {

  @Autowired
  private final CrudPersonRepository crudRepository;
  
  void doSomething() {
    List<Person> persons = repository.findByLastname("Matthews");
  }
}
```

### 사용자에게 제공되는 어노테이션 (샘플 엔티티로 설명)

스키마를 직접 작성하는 Spring Data JDBC는 <br>
Auto-increment 관련 어노테이션이 주어지지 않기에, 사용을 원한다면 DB 테이블 생성 시 직접 설정이 필요하다. <br>

만약 자동생성 관련 설정을 하지 않거나, 다른 타입의 기본키를 사용 할 경우 <br>
CrudRepository에서 save시 식별자에 의한 문제가 발생할 수 있기에 <br>
Persistable 인터페이스의 getId(), isNew() 메소드를 구현하고, 식별을 새롭게 정의하는 것이 필요하다. 

아래는 예시로 작성해본 엔티티이다. 
```java
class Person {

  @Id // PK  
  private Long id;                                                
  private String firstname, lastname;                                 
  private LocalDate birthday;
  @Transient // 테이블 칼럼과 매핑 시키지 않겠다는 의미로 사용 (물론 현재 예시에서 age가 필요 없다는 것은 아님)
  private int age;

  /* JdbcAuditing이 제공되며 @EnableJdbcAuditing 추가해줌으로 사용 할 수 있다. */
  @CreatedDate // 날짜 자동 생성
  private LocalDateTime created;
  @LastModifiedDate // 날짜 자동 생성 (수정)
  private LocalDateTime updated;                                                    

  /* 
  Spring Data JDBC 에서의 매핑 관계는 
  단방향으로 존재하며(명확한 경계가 존재), One-To-One, One-To-Many, Many-To-Many만 표현 가능하다.
  One-To-One, One-To-Many의 관계는  @MappedCollection 어노테이션을 이용하며
  Many-To-Many 관계는 연결 테이블을 나타내는 엔티티를 추가해주고 경계상 상위에게 컬렉션으로 하위를 표현해주면 된다.
  */

  // One-To-Many 예시 
  @MappedCollection(keyColumn = "{경계상 하위의 외래키}", idColumn = "{경계상 하위의 ID}")
  private List<Example> eamples = new ArrayList<>(); // Example = 경계상 하위의 엔티티명을 넣어준다.

  // ... 아래 부분 생략
}
```

Persistable 인터페이스의 getId(), isNew() 메소드의 사용이나 Spring Data JDBC에서 Many-To-Many 관계의 표현 등은 <br>
추후 필요할 경우 정리하려고 한다.
