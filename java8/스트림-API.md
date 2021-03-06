> 참고: [http://www.tcpschool.com/java/java_stream_concept](http://www.tcpschool.com/java/java_stream_concept)
# 스트림 API

자바에서는 많은 양의 데이터를 저장하기 위해 배열이나 컬렉션을 사용한다. <br>
이 데이터의 접근을 위해선 반복문이나 반복자를 통해 접근해야 되는데, 이 때 매번 새로운 코드를 작성해야 한다. <br>

이는 코드의 가독성을 떨어지게 하고, 재사용도 힘들어 진다. <br>
이에 대한 해결책으로 스트림(Stream) API를 도입하게 되는데, 한마디로 **여러 자료의 처리에 대한 기능을 구현한 것**이다. <br>
자료의 추상화가 되어 있기에 우리는 자료에 따라 기능을 각각 새로 구현할 필요없이 공통된 방법으로 이용하면 된다.

## 스트림 API의 동작 흐름

스트림은 다음과 같이  3가지 단계에 걸쳐서 동작한다.

1. 스트림의 생성
2. 스트림의 중간 연산(스트림의 변환)
3. 스트림의 최종 연산(스트림의 사용)

## 자주 사용하는 스트림의 연산

#### 중간연산

+ filter() <br>
filter()는 중간에 넣고 그 조건이 참인 경우만 추출한다. 즉 더 작은 컬렉션을 만들어내는 연산이라 할 수 있다.

+ map() <br>
map()은 해당 스트림의 요소들을 주어진 함수에 인수로 전달하여, 그 반환값들로 이루어진 새로운 스트림을 반환한다.(데이터 변환)

#### 최종연산

+ forEach() <br>
forEach()는 요소를 하나씩 꺼내는 기능을 수행한다.

+ findFirst() <br>
findFirst()는 해당 스트림에서 첫 번째 요소를 참조하는 Optional 객체를 반환한다.

+ findAny() <br>
findAny()는 해당 스트림에서 첫 번째 요소를 참조하는 Optional 객체를 반환한다.(findAny() 메소드는 병렬 스트림일 때 사용함)

### 스트림 사용의 예시

중간연산 filter(), 최종연산 forEach()를 사용한 예시

```java
ArrayList<String> list = new ArrayList<>();

list.add("HTML");
list.add("CSS");
list.add("JAVA");
list.add("JAVASCRIPT");

/* 스트림의 사용: 내부 반복을 통해 길이가 1보다 긴 String 추출해 요소를 반환 */
list.stream().filter(s-> s.length() > 1).forEach(System.out::println); // 메소드 참조로 표현(s를 출력할 것이기 때문에)
```
