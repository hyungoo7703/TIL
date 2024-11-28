# Optional
null 값으로 인한 NullPointerException을 방지하고 더 안전한 코드를 작성할 수 있게 해주는 래퍼(Wrapper) 클래스

## Optional의 주요 특징
+ 값의 존재 여부를 명시적으로 표현할 수 있다.
+ null 체크를 직접하지 않아도 되며, 불필요한 방어 로직을 줄일 수 있다.
+ 메서드의 리턴 타입으로 '결과 없음'을 명확히 표현할 수 있다.

## Optional 객체 생성 방법

### 1. Optional.of()
```java
Optional<String> optional = Optional.of("Hello");  // null이 아닌 값으로 생성
```
이 메서드는 <b>절대로 null이 아닌 값</b>을 다룰 때 사용한다. <br>
만약 null을 넣으면 즉시 NullPointerException이 발생한다. <br>
이는 의도적인 설계로, 확실히 null이 아닌 값을 다룰 때 사용해야 함을 명시적으로 나타낸다.

### 2. Optional.ofNullable()
```java
Optional<String> optional = Optional.ofNullable(value);  // null 가능한 값으로 생성
```
이 메서드는 <b>값이 null일 수도 있는 상황</b>에서 사용한다. <br>
입력값이 null이면 빈 Optional을 반환하고, null이 아니면 해당 값을 담은 Optional을 반환한다.

### 3. Optional.empty()
```java
Optional<String> optional = Optional.empty();  // 빈 Optional 생성
```
이 메서드는 의도적으로 빈 Optional을 생성할 때 사용한다.

> #### 의도적으로 빈 Optional?
+ 기본 반환값이 필요한 경우
+ 조건에 따라 값이 없음을 명시적으로 표현할 때
+ 메서드의 리턴 타입이 Optional이어야 하는 경우

## 자주 사용되는 Optional 메서드

### 1. 값 존재 확인 및 접근
```java
Optional<String> opt = Optional.of("Hello");
if (opt.isPresent()) {
    System.out.println(opt.get());
}
// 또는 더 간단하게
opt.ifPresent(str -> System.out.println(str));
```

### 2. 기본값 제공
```java
String result = optional.orElse("Default");  // 값이 없으면 기본값 반환
String result2 = optional.orElseGet(() -> "Generated Default");  // 값이 없을 때만 생성
```

### 3. 값 변환 및 필터링
```java
Optional<String> transformed = optional
    .map(String::toUpperCase)
    .filter(str -> str.length() > 5);
```

> #### Optional 에서의 map, filter 메서드의 유의점
+ **map 메서드**: Optional 내부의 값을 다른 값으로 변환, 변환된 결과는 다시 Optional로 감싸진다.
+ **filter 메서드**: 조건을 만족하면 값을 유지하고, 만족하지 않으면 빈 Optional을 반환한다.

이때 중요하게 기억할 점이, 항상 Optional 객체 반환을 한다는 것이다. (값이 있거나 없거나) <br>
Optional에 List가 들어가도 전체를 하나의 단위로 처리한다. <br>

[주의사항]
<b>List 내부 요소를 처리하려면 map 내에서 stream을 사용해야 한다.</b>

##### Optional List, Map 동작
```java
// List 전체를 대상으로 동작
Optional<List<String>> optionalList = Optional.of(Arrays.asList("John", "Jane", "Bob"));

// 모든 요소를 대문자로 변환
Optional<List<String>> upperList = optionalList.map(list -> 
    list.stream()
        .map(String::toUpperCase)
        .collect(Collectors.toList())
);
// 결과: Optional[["JOHN", "JANE", "BOB"]]

// List의 크기 구하기
Optional<Integer> size = optionalList.map(List::size);
// 결과: Optional[3]
```

##### Optional List, Filter 동작
```java
Optional<List<String>> optionalList = Optional.of(Arrays.asList("John", "Jane", "Bob"));

// List의 크기가 2보다 큰 경우만 필터링
Optional<List<String>> filtered = optionalList.filter(list -> list.size() > 2);
// 결과: Optional[["John", "Jane", "Bob"]]

// 모든 요소가 J로 시작하는 경우만 필터링
Optional<List<String>> startsWithJ = optionalList.filter(list -> 
    list.stream().allMatch(str -> str.startsWith("J"))
);
// 결과: Optional.empty (Bob이 J로 시작하지 않으므로)
```

### 4. 체이닝 예제
```java
Optional<User> user = Optional.ofNullable(getUser());
String result = user
    .map(User::getAddress)
    .map(Address::getCity)
    .orElse("Unknown City");
```

## 실용적인 사용 예시

### 1. 객체의 중첩 속성 접근
```java
// Optional 사용 전
if (computer != null && computer.getSoundcard() != null && computer.getSoundcard().getUSB() != null) {
    String version = computer.getSoundcard().getUSB().getVersion();
}

// Optional 사용 후
String version = Optional.ofNullable(computer)
    .flatMap(Computer::getSoundcard)
    .flatMap(Soundcard::getUSB)
    .map(USB::getVersion)
    .orElse("UNKNOWN")[5];
```

### 2. 예외 처리
```java
String result = Optional.ofNullable(value)
    .orElseThrow(() -> new IllegalStateException("Value must not be null"));
```
