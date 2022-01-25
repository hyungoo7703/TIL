>참고: [https://spring.io/](https://spring.io/) // [https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.repositories](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.repositories)
# Spring Data JPA

Spring Data JPA는 Spring 에서 제공하는 서브 프로젝트로 JPA 기반 저장소를 쉽게 구현 할 수 있다.
  

## Spring Data JPA 의 장점

+ 객체를 통해 DB테이블을 간접적으로 다루기에 직관적인 조작이 가능해진다. 
+ 제공되는 메소드를 통해 조작하며 적합한 쿼리를 자동으로 등록해준다.
+ 페이지 매김 지원, 동적 쿼리 실행, 맞춤형 데이터 액세스 코드 통합 기능 등 다양한 기능 지원이 된다.

## Spring boot 환경에서 Spring Data JPA 사용하는 방법

>Spring initializr: [https://start.spring.io](https://start.spring.io/)

start.spring.io에서 Spring Boot 기반으로 프로젝트 설정시 의존관계에 Spring Data JPA를 추가하면 된다.

-----
## 핵심 개념

Spring Data 저장소 추상화의 목표는 다양한 영속성 저장소에 대한 데이터 액세스 계층을 구현 시 필요한 코드의 양을 크게 줄이는 것이다.  
추상화의 중심 인터페이스는 Repository 이다. 관리할 도메인 클래스와 그 클래스의 ID 유형을 유형 인수로 사용한다.

```java
public interface CrudRepository<Example, ID> extends Repository<Example, ID> { // ID는 타입으로 많이 쓴다. (ex Long)

  <S extends Example> S save(S entity);  // 주어진 엔티티를 저장한다.   
  Optional<Example> findById(ID primaryKey); // 지정된 ID로 식별되는 엔터티를 반환한다.
  Iterable<Example> findAll(); // 모든 엔터티를 반환한다.              
  long count(); // 엔터티 수를 반환한다.                       
  void delete(Example entity); // 지정된 엔터티를 삭제한다.              
  boolean existsById(ID primaryKey); // 주어진 ID를 가진 엔터티가 있는지 확인한다.
  // ... 등등

}
```

CrudRepository 인터페이스가 관리되는 엔티티 클래스에 대한 CRUD 기능이 제공되는 것이 핵심이다.  
우리는 사용 시 재정의하여 사용하거나, 주어진 대로 이용하는 것도 가능하다. <br>

또한 **JpaRepository 또는 MongoRepository**와 같은 기술별 추상화가 잘 구현되어 있다.

<br>

### 쿼리 메소드 3단계 프로세스

1. Repository 또는 하위 인터페이스 중 하나를 확장하는 인터페이스를 선언하고 처리해야 하는 도메인 클래스 및 ID 유형에 입력한다.

```java
  interface PersonRepository extends JpaRepository<Person, Long> {}
```

2. 인터페이스에서 쿼리 메소드를 선언

```java
  interface PersonRepository extends Repository<Person, Long> {
    List<Person> findByLastname(String lastname); // lastname으로 찾는 기능 추가
  }
```

3. 리포지토리 인스턴스 의존성 주입 후 사용 (상황에 따라 JavaConfig 또는 XML 에서 Spring 설정이 필요하기도 함)

```java
class SomeClient {

  private final PersonRepository repository;
  
  @Autowired // 생성자 주입을 권장
  SomeClient(PersonRepository repository) {
    this.repository = repository;
  }

  void doSomething() {
    List<Person> persons = repository.findByLastname("Matthews");
  }
}
```

### 쿼리 메소드 정의해서 사용

+ 메소드 이름에서 쿼리 생성

첫번째 부분은 **findBy 또는 existsBy**가 되며, 이는 주제를 정의한다. 그후 뒤에 오는 두번째 연결 부분은 서술어를 형성한다. <br>
즉 첫번째 By가 실제 기준 서술어의 시작을 나타내는 구분 기호라 할 수 있으며 **And 및 Or**로 연결할 수 있다.

```java
  interface PersonRepository extends Repository<Person, Long> {

  List<Person> findByFirstnameAndLastname(Firstname firstname, String lastname);
  List<Person> findByLastnameIgnoreCase(String lastname); // 개별 속성에 대해 대/소문자 무시 사용 (모든 속성에 대해 무시할 경우 AllIgnoreCase)

  // 쿼리 ORDER BY 활성화
  List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
  List<Person> findByLastnameOrderByFirstnameDesc(String lastname);

  /*
   속성 식에 대해 Between, LessThan, GreaterThan, Like와 같은 연산자도 지원된다. 
  */
}
```

+ 속성 표현식

사람에게 우편번호가 있는 주소가 있다고 가정할때

```java
  List<Person> findByAddressZipCode(ZipCode zipCode); // 이 경우 메소드는 x.address.zipCode 속성을 순회한다. (모호성에 따라 오류가 나올 수 있다.)
  List<Person> findByAddress_ZipCode(ZipCode zipCode); // 모호성을 해결하고 순회지점을 직접 수동으로 지정할 수 있다.
```

+ 특수한 매개변수의 처리

쿼리에서 매소드의 매개변수의 처리는 **Pageable(페이지 매김) 및 Sort(정렬)** 와 같은 특정 유형에 대한 동적 처리도 가능하다. 

```java
  Page<User> findByLastname(String lastname, Pageable pageable); // 페이징 동적 추가 (Page는 사용 가능한 요소와 페이지의 총 수를 알고 있기에 제공되는 메소드와 함께 사용)

  List<User> findByLastname(String lastname, Sort sort); // List 반환에 정렬만 필요한 경우 메소드에 org.springframework.data.domain.Sort 매개변수를 추가
  List<User> findByLastname(String lastname, Pageable pageable);
```

+ 결과를 제한하는 쿼리

선택한 숫자의 값을 first 또는 top에 추가하여 반환할 최대 결과 크기를 지정 할 수 있다. (숫자 생략 시 결과가 1로 가정)

```java
  /*
   특수한 매개변수의 처리에서 사용한 메소드에 각각 first 와 top을 추가
  */
  List<User> findFirst10ByLastname(String lastname, Sort sort); // 10
  List<User> findTopByLastname(String lastname, Pageable pageable); // 1
  }
```

+ 메소드에 Java Collection이나 lterable를 반환

Streamable을 쿼리 메소드 반환 유형으로 사용 (lterable 또는 모든 컬렉션 유형 대신 사용)

```java
  interface PersonRepository extends Repository<Person, Long> {
  Streamable<Person> findByFirstnameContaining(String firstname);
  Streamable<Person> findByLastnameContaining(String lastname);

  Page<Person> findByFirstnameOrLastnameContaining(String firstname, String lastname, Pageable pageable); // 응용 (페이징 처리와 검색기능 동시 구현)
  }

  Streamable<Person> result = repository.findByFirstnameContaining("av").and(repository.findByLastnameContaining("ea")); 
```

-----
## 기본 JPA 에서 제공되는 EnitityManager를 사용하여 유지하거나 병합하여 사용하기 

>[JPA]() 관련하여 정리한 부분은 이곳을 참조하면 된다. 

###  Entity 저장

위 내용에서 알아보았듯이 Entity 저장 시 사용하는 메소드는 save() 이다.

```java
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import org.springframework.stereotype.Repository;

@Repository // Spring에서 제공하는 기본타입 -> ComponentScan의 대상이 되는 어노테이션
public class ExampleRepository {

	@PersistenceContext // Spring Boot (spring-boot-starter-data-jpa) 위에서 동작시 EntityManager를 스프링 컨테이너에 주입
	private EntityManager em; // JPA 에서 Entity 관리를 위해 사용, EntityManager 생성코드는 위 @PersistenceContext로 인하여 필요가 없다.
	
	public void save(Example example) { 
		em.persist(example); 
  }
}
```

엔티티가 아직 영속되지 않은 경우 entityManager.persist() 메소드를,  그렇지 않다면 entityManager.merge() 메소드를 사용한다.
