>참고: [https://spring.io/guides/tutorials/rest/](https://spring.io/guides/tutorials/rest/) 

# 스프링 이용하여 REST 서비스 구축

>[REST API](../etc/restApi.md)에 대한 정리는 이곳을 참고하면 된다. 

RESTful API는 구현과 사용이 간편화 되어 웹에서 웹 서비스를 구축하기 위한 표준처럼 사용되고 있다. <br>
스프링을 이용하면 쉽게 RESTful 서비스를 구축 할 수 있는 장점이 있다. <br>
그 과정을 정리 해보려고 한다.

## 시작하기

기본적으로 Spring Boot 사용 기반에 
>Spring initializr: [https://start.spring.io](https://start.spring.io/)
에서 프로젝트 종속성을 가지고 시작한다. **추가 종속성: (Spring Web, Spring Data JPA, H2 Database)**

-----

## 구축과정

0. application.properties 설정
```
# DB 연결 설정 (DB명과, ID명에 각각 맞는 설정을 한다.)
spring.datasource.url=jdbc:h2:tcp://localhost/~/'DB명'
spring.datasource.username='ID명'
spring.datasource.password=
spring.datasource.driver-class-name=org.h2.Driver

# hibernate로 ddl 자동 생성 되도록 (단순한 테스트 용도로 create로) 
spring.jpa.hibernate.ddl-auto=create 
spring.jpa.properties.hibernate.show_sql=true

```

1. 시스템에서 사용할 Entity를 정의한다. (Lombok 사용을 기준으로 진행)

```java

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

import lombok.Data;

@Entity // 이 객체를 JPA 기반 데이터 저장소에 저장할 준비가 되도록 하는 JPA 어노테이션
@Data
class Example {

  @Id @GeneratedValue // 기본 키임을 나타내기 위한 어노테이션
  private Long id;
  private String name;

}

```

2. Spring Data JPA 저장소를 구현 한다. 

Spring Data JPA 저장소는 백엔드 데이터 저장소에 대한 레코드 생성, 읽기, 업데이트 및 삭제를 지원하는 메소드와의 인터페이스이다. <br>
세부적인 내용이나 자세한 설명은 [TIL](https://github.com/hyungoo7703/TIL)에 잘 정리해 두었다.
>[Spring Data JPA 정리 내용으로 이동](../spring/springData/springDataJPA.md) 

```java
import org.springframework.data.jpa.repository.JpaRepository;

interface ExampleRepository extends JpaRepository<Example, Long> { // JpaRepository 상속 <도메인 유형, ID 유형>

}

```

3. REST API Controller 구현

```java

import java.util.ArrayList;
import java.util.List;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;

@RestController // @ResponseBody가 추가된 컨트롤러 Json 형태로 객체 데이터를 반환하기 위해 사용
public class ExampleController {

	private final ExampleRepository exampleRepository;

	public ExampleController(ExampleRepository exampleRepository) {
		this.exampleRepository = exampleRepository;
	}

	// HttpStatus를 담기 위해 ResponseEntity<>를 사용해주면 좋다. 상태코드(Status), HttpHeader, HttpBody를 인자로 담을 수 있다.
	
	/* ResponseEntity<> 없이 사용
	 
        @GetMapping("/examples") 
	List<Example> all() { 
	  	return exampleRepository.findAll(); 
	}
	  
	*/
	
	@GetMapping("/example") // 전체 조회
	public ResponseEntity<List<Example>> all() {
	    try {
	      List<Example> examples = new ArrayList<Example>();
	      exampleRepository.findAll().forEach(examples::add); // examples에 담아 List 생성 -> 검색에 따른 로직 분리에 용이
	      if (examples.isEmpty()) {
	        return new ResponseEntity<>(HttpStatus.NO_CONTENT); 
	      }

	      return new ResponseEntity<>(examples, HttpStatus.OK); // body에 examples를 담고 상태코드 200 전송
	    } catch (Exception e) {
	      return new ResponseEntity<>(null, HttpStatus.INTERNAL_SERVER_ERROR);
	    }
	  }
	

	@ResponseStatus(HttpStatus.CREATED) // ResponseEntity 처럼 상태코드를 전송할 수 없어 제공되는 ResponseStatus 어노테이션을 Http 상태코드를 입력
	@PostMapping("/examples") // 새로운 Example 입력
	Example newExample(@RequestBody Example newExample) {
		return exampleRepository.save(newExample);
	}

	@GetMapping("/examples/{id}") // 아이디로 조회
	Example one(@PathVariable Long id) { 
		return exampleRepository.findById(id).orElse(null); // findById가 Optional값을 반환하기 때문에 없을경우 null 반환
	}

	@PutMapping("/examples/{id}") // 아이디로 값 업데이트
	Example replaceExample(@RequestBody Example newExample, @PathVariable Long id) {

		return exampleRepository.findById(id).map(example -> {
			example.setName(newExample.getName()); // 새 입력값을 받아서 새로 set
			return exampleRepository.save(example);
		}).orElseGet(() -> { // 없을경우 새로운 Example로 입력
			newExample.setId(id);
			return exampleRepository.save(newExample);
		});
	}

	@DeleteMapping("/examples/{id}") // 아이디로 삭제
	void deleteExample(@PathVariable Long id) {
		exampleRepository.deleteById(id);
	}
}

```

## 실행하기

위 과정을 다 마치고 우선 Spring Boot App을 실행하여(기본 8080포트사용) <br>
Controller에서 설정한대로 8080포트/examples로 접속하자 H2 데이터 베이스에서 테이블이 생성되었다.

![1-18(1)](https://user-images.githubusercontent.com/93297109/149893006-70acd281-83a6-4664-b726-0d47902c3d58.png)

정확한 데이터 전송이 되는지 확인하기 위해 포스트맨을 실행했고, <br>
![1-18(2)](https://user-images.githubusercontent.com/93297109/149893366-b307d301-e38e-4706-b336-ecb7eb26f87f.png)

실제 테이블에 데이터가 전송됨을 확인했다. <br>
![1-18(3)](https://user-images.githubusercontent.com/93297109/149893517-0837b31c-f4cb-4654-b2ed-320ba5c12636.png)
