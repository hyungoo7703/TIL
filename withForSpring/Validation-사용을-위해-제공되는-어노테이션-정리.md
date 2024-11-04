>참고: [https://prinha.tistory.com/entry/자바-스프링-어노테이션-annotation의-정의와-종류](https://prinha.tistory.com/entry/%EC%9E%90%EB%B0%94-%EC%8A%A4%ED%94%84%EB%A7%81-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98-annotation%EC%9D%98-%EC%A0%95%EC%9D%98%EC%99%80-%EC%A2%85%EB%A5%98)
# Validation 사용을 위해 제공되는 어노테이션 정리

Validation은 Hibernate 유효성 검사기를 사용한 Bean 유효성 검사인데 Java의 데이터 유효성 검사 표준 기술이다. <br>
의존성에 'spring-boot-starter-validation'만 추가하면 간단하게 사용할 수 있다. <br>

이 유효성 검사를 간단하게 사용하기 위해 제공되는 어노테이션을 정리하려고 한다.

## @Valid, @Validated
(@Valid는 Java 표준 유효성 검사 어노테이션이고, @Validated는 스프링 프레임워크에서 제공하는 어노테이션이다.)

객체 안에서, 들어오는 값 검증을 위해 사용하는 어노테이션 (검사를 진행 할 곳에 추가하면 된다.)

-----

## 문자열 유무 검증

**@NotBlank** 
  - 반드시 값이 존재해야 한다.
  - Null, ""(초기화 Stirng), " "(공백) 허용 X

**@NotEmpty**
  - 반드시 값이 존재해야 한다.
  - Null, ""(초기화 Stirng) 허용 X

**@NotNull**
  - 반드시 값이 존재해야 한다.
  - Null만 허용 X
 
**@Null**
  - Null 이다.

## 최대, 최소, 양수, 음수에 대한 검증

### String

- **@DecimalMax**(value = ): 지정된 최대 값보다 작거나 같아야 한다.
- **@DecimalMin**(value = ): 지정된 최소 값보다 크거나 같아야 한다.

### int

- **@Max**: 지정된 최대 값보다 작거나 같아야 한다.
- **@Min**: 지정된 최소 값보다 크거나 같아야 한다.
- **@Positive**: 양수
- **@PositiveOrZero**: 0이거나 양수
- **@Negative**: 음수
- **@NegativeOrZero**: 0이거나 음수

## 시간 값에 대한 검증
     
- **@Future**: Now 보다 미래의 날짜, 시간
- **@FutureOrPresent**: Now 거나 미래의 날짜, 시간
- **@Past**: Now 보다 과거의 날짜, 시간
- **@PastOrPresent**: Now 거나 과거의 날짜, 시간

## 이메일 검증

**@Email**: 이메일 형식만 가능 (@ 유무 검증)

## 자릿수 범위 검증

**@Digits**(integer = , fraction = ): 허용된 범위 내의 숫자 (최대 정수 자릿수, 최대 소수 자릿수)

## 논리 검증

- **@AssertTrue**: 항상 True 여부
- **@AssertFalse**: 항상 False 여부

## 크기(길이) 검증

**@Size**(min = , max = ): 길이가 지정된 경계 사이여야 한다.
> 문자열, 컬렉션, 배열, Map 등에 사용 가능

## 정규식 검증

**@Pattern**(regexp = ): 정규식을 만족하는가

## URL 검증

**@URL**: URL 형식만 허용

## UUID 검증

**@UUID**: UUID 형식만 허용
