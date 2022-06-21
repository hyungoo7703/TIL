# 스프링으로 구축한 REST 서비스에서 필드에러 처리

REST 서비스에서 에러 핸들링을 할때, response body에 에러 메시지를 응답해 주어야 한다. <br>
이와 같은 이치로 REST 서비스에서 Bean Validation을 적용할 때는 적절한 처리를 통해 보여주어야 한다. <br>

우선 TIL에 올렸던 [스프링 이용하여 REST 서비스 구축](https://github.com/hyungoo7703/TIL/blob/main/withSpring/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-REST-%EC%84%9C%EB%B9%84%EC%8A%A4-%EA%B5%AC%EC%B6%95.md)를 참고하여 <br>
적용해보기 쉽게 소스를 가져와 일부 변경했다. 

```java
/* Entity 수정*/
@Entity
@Data
class Example {

    @Id @GeneratedValue // 기본 키임을 나타내기 위한 어노테이션
    private Long id;

    /* validation 의존성 추가 후 사용하는 JSR-303 Bean Validation 방식의 검증 */
    @NotNull // 빈값 검증
    @Pattern(regexp="^[a-zA-Z가-힣]*$" , message = "이름은 한글, 또는 영어만 입력 가능합니다.") // 정규표현식을 이용한 검증로직 추가
    private String name;

}
```

[정규표현식(Regular Expression)](https://github.com/hyungoo7703/TIL/blob/main/etc/patternMatching.md)에 대한 정리는 [TIL](https://github.com/hyungoo7703/TIL)에 잘 정리해 두었다.

```java
/*newExample 수정*/
@PostMapping("/examples") 
public ResponseEntity<?> newExample(@Valid @RequestBody Example newExample) { // <?>:모든 제네릭 객체 타입을 받을 수 있는 와일드 카드
    try {	
		return ResponseEntity.status(HttpStatus.NO_CONTENT).body(exampleRepository.save(newExample));
	} catch (Exception e) {
		return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(e); 
    }
}
```
<br>

Json 형태로 필드 에러 내용을 응답해주기 위해 JsonComponent를 이용해야 한다. <br>
간단한 구글링을 통해 JsonComponent 관련하여 잘 정리 되어있는 사이트를 참고 하였다.
>참고: [https://www.baeldung.com/spring-boot-jsoncomponent](https://www.baeldung.com/spring-boot-jsoncomponent)

<br>

한줄로 정리해보면, **@JsonComponent와 JsonSerializer의 serialize 메소드를 오버 라이딩**해서 구현 가능하다.

```java
@PostMapping("/examples") 
public ResponseEntity<?> newExample(@Valid @RequestBody Example newExample, Errors errors) { // Example 객체의 에러를 Errors에 바인딩
    try {	
        if (errors.hasErrors()) { // 에러 존재시 
			return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(errors); // 클라이언트 오류를 호출하며, Json 형태로 담고 전달  
		}
		return ResponseEntity.status(HttpStatus.NO_CONTENT).body(exampleRepository.save(newExample));
	} catch (Exception e) {
		return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(e); 
    }
}
```

```java
/* ErrorsSerializer 클래스 추가 */
@JsonComponent // ObjectMapper에 Serializer를 등록해 주어야하는데 @JsonComponent를 사용하면 손쉽게 등록이 가능하다.
public class ErrorsSerializer extends JsonSerializer<Errors> { // JsonSerializer<Errors>: JSON으로 직렬화하는 클래스

	@Override
	public void serialize(Errors errors, JsonGenerator jsonGenerator, SerializerProvider serializerProvider) throws IOException { // errors를 받아 json으로 변환해준다.
		jsonGenerator.writeStartArray(); // 에러가 여러개 일 수 있기에 배열 형태로 감싸준다.
		errors.getFieldErrors().forEach(e -> { // fieldErrors 루프
			try {
				jsonGenerator.writeStartObject();
				jsonGenerator.writeStringField("field", e.getField()); // 필드명
				jsonGenerator.writeStringField("objectName", e.getObjectName()); // 오브젝트명 (여기선 Example)
				jsonGenerator.writeStringField("code", e.getCode()); // 오류코드 (필드 오류의 경우 어노테이션명)
				jsonGenerator.writeStringField("defaultMessage", e.getDefaultMessage()); // 기본 오류 메시지
                Object rejectValue = e.getRejectedValue(); // 입력값
                if(rejectedValue != null) { // 입력값이 있는 상태에서 오류 발생 시
                    jsonGenerator.writeStringField("rejectedValue", rejectedValue.toString()); // 내가 입력한 메시지 보여주기 위함
                }
				jsonGenerator.writeEndObject(); 
			} catch (IOException e1) {
				e1.printStackTrace();
			}
		});
    }
```
정상적으로 에러메시지가 출력되는지 보기위해 포스트맨을 통해 실행 해 보았다. <br>
(간단한 출력을 위해 필드명, 오류메시지, 입력한 메시지만 활성화 하였다.)

우선 빈값을 입력해 보았다.

![api-validation](https://user-images.githubusercontent.com/93297109/152738616-657d6611-8e7c-4ccd-9013-45864e3bc15a.png)

입력 값이 없기에 필드명과 기본 오류메시지만 출력된다. 오류메시지는 처음에 따로 설정 안했기 때문이다. <br>
위 로직에서 아래와 같이 추가하면 설정 값이 나온다.

```java
    @NotNull(message = "이름을 입력해 주세요.") // 추가시 "이름을 입력 해 주세요." 가 출력된다.
    @Pattern(regexp="^[a-zA-Z가-힣]*$" , message = "이름은 한글, 또는 영어만 입력 가능합니다.") 
    private String name;
```

이번엔 잘못된 값을 넣어 보았다. (이름에 숫자)

![api-validation2](https://user-images.githubusercontent.com/93297109/152739262-b3b5ce33-3e05-45f1-b5d4-7e9a09fb48c4.png)

검증 로직이 제대로 동작하고, 또 입력 값까지 정상적으로 출력되는 것을 확인 할 수 있다.





