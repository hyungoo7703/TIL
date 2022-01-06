>참고: [spring.io](https://spring.io/) // [docs.spring.io](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.repositories)
# Spring Data JPA

Spring Data JPA는 Spring 에서 제공하는 서브 프로젝트로 JPA 기반 저장소를 쉽게 구현 할 수 있다.
  

## Spring Data JPA 의 장점

+ 객체를 통해 DB테이블을 간접적으로 다루기에 직관적인 조작이 가능해진다. 

+ 제공되는 메서드를 통해 조작하며 적합한 쿼리를 자동으로 등록해준다.

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
우리는 사용 시 재정의하여 사용하거나, 주어진 대로 이용하는 것도 가능하다.

##  Entity 저장

Entity 저장 시 사용하는 메서드는 save() 이다.
기본 JPA 에서 제공되는 EnitityManager를 사용하여 유지하거나 병합하여 사용한다. 
>[JPA]() 관련하여 정리한 부분은 이곳을 참조하면 된다. 
```java
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import org.springframework.stereotype.Repository;

@Repository // Spring에서 제공하는 기본타입 -> ComponentScan의 대상이 되는 어노테이션
public class CrudRepository {

	@PersistenceContext // Spring Boot (spring-boot-starter-data-jpa) 위에서 동작시 EntityManager를 스프링 컨테이너에 주입
	private EntityManager em; // JPA 에서 Entity 관리를 위해 사용, EntityManager 생성코드는 위 @PersistenceContext로 인하여 필요가 없다.
	
	public void save(Example example) { 
		em.persist(example); // 공부 필요 내용
	}
}
```
엔티티가 아직 영속되지 않은 경우 entityManager.persist() 메서드를,  그렇지 않다면 entityManager.merge() 메서드를 사용한다.
