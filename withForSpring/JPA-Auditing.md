## JPA Auditing

Spring Data JPA를 사용하는 환경에서, <br>
**JPA Auditing**는 DB에 생성시간, 수정시간 자동화를 위한 방법이다. <br>

이때 사용하는 날짜, 시간 형식에서 사용하는 데이터 타입은 <br> 
Java8 부터 제공되는 java.time 패키지 안 **LocalTime**, **LocalDate**, **LocalDateTime** 클래스이다. <br> 

+ LocalDate 클래스: 날짜를 표현
+ LocalTime 클래스: 시간을 표현

=> java.time 패키지에 포함된 클래스는 이 두 클래스를 확장한 것이 많아, 두 클래스의 이해가 중요

```java
// 사용 예
LocalDate today = LocalDate.now(); // 오늘 날짜를 출력
LocalTime present = LocalTime.now(); // 오늘 시간을 출력
```

+ LocalDateTime 클래스: 시간과 날짜 동시에 필요할 때 사용

```java
LocalDateTime todayPresent = LocalDateTime.now();
```

### JPA Auditing 사용법

1. Entity에 JPA Auditing 어노테이션들 추가

```java
@Entity
@EntityListeners(AuditingEntityListener.class) // Example 클래스에 Auditing 기능을 포함시킨다.
public class Example {

	@CreatedDate // 생성시간 자동저장
	private LocalDateTime dateCreated;
	@LastModifiedDate // 생성시간 자동저장 + 수정시 수정시간 자동저장
	private LocalDateTime dateUpdated;
}

```

2. JPA Auditing 어노테이션들 활성화 할 수 있도록 Application에 활성화 어노테이션 추가

```java
@EnableJpaAuditing // JPA Auditing 활성화
@SpringBootApplication
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```

위 방식대로 코드를 작성해보고, 기본적인 컨트롤러, 리파지토리 생성 후 포스트맨으로 간단히 테스트 해보았다. <br>

![jpaauditing-succes](https://user-images.githubusercontent.com/93297109/149711109-628891aa-36da-422b-aa33-f58c520ed845.png)

그 결과 생성시간 정상 생성되는 모습이었고, 후 수정시에도 자동 수정시간이 기록되었다.
