# 날짜와 시간 클래스

## Java 8 이전 Date와 Calendar 클래스

### Date 클래스
java.util 패키지에 포함되어 있으며, <br>
특정 시점의 날짜와 시간을 밀리초 단위로 표현하기 위해 사용되는 클래스이다.
```java
// Date 객체 생성
Date date = new Date(); // 현재 날짜와 시간
System.out.println("현재 날짜: " + date);
```
단점
+ 월이 0부터 시작(0:1월, 11:12월)하여 직관적이지 않음
+ 가변(mutable) 객체라서 스레드 안전하지 않음
+ 날짜 연산 기능이 부족

> #### 가변객체?
가변 객체는 생성 후에도 내부 상태가 변경될 수 있는 객체를 의미한다.

### Calendar 클래스
Date 클래스의 한계를 보완하기 위해 Java 1.1에서 도입 <br>
연, 월, 일, 시, 분 등의 날짜/시간 필드를 세부적으로 다룰 수 있다. <br>
추상 클래스로 설계되어 있어 직접 인스턴스화 할 수 없는 특징이 있다.
```java
// Calendar 객체 생성 - 직접 new Calendar()로 생성 불가
Calendar calendar = Calendar.getInstance(); // 팩토리 메서드 사용

// 날짜 설정
calendar.set(2024, Calendar.MARCH, 14); // 2024년 3월 14일
System.out.println("설정된 날짜: " + calendar.getTime());
```
단점
+ 날짜 계산이 복잡하고 오류가 발생하기 쉬움
+ 역시 가변 객체라서 스레드 안전성 문제가 있음

### Java 8에서 새로운 날짜/시간 API가 필요했던 이유
1. 불변 객체를 통한 스레드 안전성 확보가 필요했다.
2. 직관적이고 사용하기 쉬운 API 설계 요구되었다.
3. 날짜/시간 연산의 정확성과 편의성 개선 필요했으며, 다양한 시간대 처리 기능도 필요했다.
4. 특히 날짜/시간 계산과 포맷팅 작업에서 성능개선이 필요했다.

## Java 8 에서 추가된 날짜와 시간 클래스

**날짜 관련**
+ LocalDate: 날짜만 표현 (2024-03-14)
+ LocalTime: 시간만 표현 (10:15:30)
+ LocalDateTime: 날짜와 시간 모두 표현 (2024-03-14T10:15:30)

**시간대 처리**
+ ZonedDateTime: 타임존이 포함된 날짜/시간
+ OffsetDateTime: UTC로부터의 오프셋이 포함된 날짜/시간
+ Instant: 특정 시점의 타임스탬프

**기간 표현**
+ Duration: 시간 기반 기간 (PT1H30M - 1시간 30분)
+ Period: 날짜 기반 기간 (P1Y2M3D - 1년 2개월 3일)

## 이전의 문제점들을 다음과 같이 해결

### 1. 불변 객체와 스레드 안전성
LocalDateTime 사용 예시
```java
// 불변 객체이므로 스레드 안전
LocalDateTime now = LocalDateTime.now();
LocalDateTime future = now.plusDays(3); // 새로운 객체 생성
System.out.println(now);      // 원본 객체는 변경되지 않음
System.out.println(future);   // 새로운 객체에 연산 결과 저장
```

### 2. 직관적인 API 설계
날짜 생성과 필드 접근
```java
// 명확한 날짜 생성
LocalDate date = LocalDate.of(2024, 3, 14);
LocalTime time = LocalTime.of(12, 30, 0);

// 직관적인 필드 접근
int year = date.getYear();          // 2024
int month = date.getMonthValue();   // 3 (1부터 시작)
int day = date.getDayOfMonth();     // 14
```

### 3. 날짜/시간 연산과 시간대 처리
날짜 연산
```java
LocalDate today = LocalDate.now();
// 날짜 계산
LocalDate nextWeek = today.plusWeeks(1);
LocalDate lastMonth = today.minusMonths(1);

// 기간 계산
Period period = Period.between(today, nextWeek);
```

시간대 처리
```java
// 시간대 처리
ZonedDateTime seoulTime = ZonedDateTime.now(ZoneId.of("Asia/Seoul"));
ZonedDateTime nyTime = seoulTime.withZoneSameInstant(ZoneId.of("America/New_York"));

// 시차 계산
Duration timeDiff = Duration.between(seoulTime, nyTime);
```

### 4. 향상된 포맷팅
날짜/시간 포맷팅
```java
LocalDateTime dateTime = LocalDateTime.now();

// 미리 정의된 포맷터 사용
DateTimeFormatter isoFormatter = DateTimeFormatter.ISO_DATE_TIME;
String isoFormatted = dateTime.format(isoFormatter);

// 커스텀 포맷터 생성
DateTimeFormatter customFormatter = DateTimeFormatter.ofPattern("yyyy년 MM월 dd일 HH시 mm분");
String customFormatted = dateTime.format(customFormatter);

// 문자열을 날짜로 파싱
LocalDateTime parsed = LocalDateTime.parse("2024-03-14T10:15:30");
```