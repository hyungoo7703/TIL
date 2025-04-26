# Record
자바 17에서 정식 도입된 새로운 클래스 타입으로, 주로 불변(immutable) 데이터 객체를 간결하게 정의할 수 있도록 하기위해 추가되었다.

> #### 추가된 이유?
기존의 데이터 클래스(예: DTO, VO)를 만들 때 생성자, getter, equals, hashCode, toString 등의 코드를 반복적으로 작성해야 했다. <br>
**이를 자동화해 개발자의 생산성과 코드의 가독성을 높이기 위해서 도입되었다.** <br>
실질적으로 Record로 작성된 클래스는 "이 객체는 단순히 데이터를 담기 위한 용도"임을 명확하게 표현함이 가능하다.

## Record 선언 예시
```java
public record Person(String name, int age) { }
```
위 코드는 다음과 같은 클래스를 자동으로 생성한다:
+ final String name, final int age 필드
+ public Person(String name, int age) 생성자
+ name(), age() getter 메서드
+ equals(), hashCode(), toString() 메서드