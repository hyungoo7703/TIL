## 문제인식

Spring Data JPA 공부하면서, Junit5 환경에서 테스트를 돌리던 도중 <br>
아래와 같은 에러가 발생하였다.

![error-junit5](https://user-images.githubusercontent.com/93297109/150894117-0bce1f06-d76c-4a42-937a-5010bc3d92ee.png) <br>

아무리 봐도 로직에서 오류는 없는 것 같아 상당히 많은 고민을 했는데, 작성 테스트 코드는 아래와 같다.

```java
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit.jupiter.SpringExtension;

@ExtendWith(SpringExtension.class) // 스프링 관련하여 테스트 할 것이라는 것을 알려줌 (명시 하지않아도 된다.)
@SpringBootTest
public class ExampleRepositoryTest {
	
	private final ExampleRepository exampleRepository; 
	
	@Autowired
	public ExampleRepositoryTest(ExampleRepository exampleRepository) {
		this.exampleRepository = exampleRepository;
	}

	@Test
	void testExample() {
		Example example = new Example();
		example.setName("TEST");
		
		Example saveExample = exampleRepository.save(example);
		Example findExample = exampleRepository.findById(saveExample.getId()).orElse(null);
		
		Assertions.assertThat(findExample.getId()).isEqualTo(saveExample.getId());
		Assertions.assertThat(findExample.getName()).isEqualTo(saveExample.getName());
	}

}
```

<br>

## 해결접근

에러 메시지로 구글링을 열심해 해 보았으나, 그 내용 그대로 실행 할 수 없는 SQL문 이라는 오류 이기에 <br>
대부분 **오타, Not Null 값에 null을 입력하는 등** 같은 내용들 이었다. <br>
그럼 내가 작성한 코드에서 DB 테이블하고 맞지 않는 부분이 있는 것이 무조건 있다는 생각에 다시 Example 클래스를 보았다. <br>

```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

import lombok.Data;

@Entity 
@Data
public class Example {

	@Id @GeneratedValue(strategy = GenerationType.IDENTITY) 
	private Long id;
	private String name;

}
```

되게 간단하게 설정 해서 아무런 의심 없이 넘겼었는데, 기본키의 영속성 관리에서 잘 모르고 사용하고 있었음을 인지했다. <br>
```java
@GeneratedValue(strategy = GenerationType.IDENTITY) // 잘못된 부분
```

<br>

## 결론

그동안 항상 MySQL 환경의 DB에서 실행하였기에 기본키 자동생성 어노테이션을 저렇게 사용 해왔었다. <br>
하지만 이번엔 인메모리 DB인 H2로 가볍게 테스트 하려다 인지하게 된 것이었다. <br>

**@GeneratedValue에서 IDENTITY의 의미는 DB에 위임하여 Auto_Increment을 사용하겠다는 의미였다. (MySQL사용)** <br>
즉, H2 환경에서 실행을 하려는 나는 자동 지정이 되는 기본값으로서 @GeneratedValue만 사용했어야 했던 것이다.

```java
@GeneratedValue // 수정
```
위와 같이 수정 후 Test를 실행해본 결과

![error-junit5-succes](https://user-images.githubusercontent.com/93297109/150895233-cc59df6b-7a72-4161-a99a-209e620c5588.png)

정상적으로 테스트가 성공함을 알 수 있었다.
