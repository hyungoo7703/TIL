>참고: [https://highcode.tistory.com/6](https://highcode.tistory.com/6)
# 정규표현식 

정규표현식(Regular Expression)은 특정한 규칙을 가진 문자열의 집합을 표현할 때 사용하는 형식이다. <br>

## 정규식의 장점

+ String의 검색, 치환, 추출을 하기위한 패턴으로 사용 가능
+ 전화번호, 이메일과 같은 형식을 사용할 때 용이

## 정규식 검증기 만들기 (by Pattern, Matcher 클래스)

자바에서는 정규표현식 만들거나 검증할 때 java.util.regex 패키지 안에 있는 Pattern, Matcher 클래스를 사용한다.

```java
// Pattern클래스 matches()를 통한 검증
String regex = "^[0-9]*$"; // 정규식 예시 (숫자만 사용 가능)
String string = "1234" // 검증할 String 예시

boolean check = Pattern.matches(regex, check); // 정규표현식 검증, 일치하면 true

// Matcher클래스 matches()를 통한 검증
String regex = "^[0-9]*$"; 
String string = "1234";

Pattern pattern = Pattern.compile(regex); // 정규식 패턴화
Matcher matcher = pattern.matcher(string); // pattern.matcher() 통해 Matcher 객체를 받음

boolean check = matcher.matches(); // 정규표현식 검증, 일치하면 true
boolean check = matcher.find(); // 정규표현식 검증, 일치하면 true
```

물론 정규식 검증 어노테이션인 @Pattern 사용을 위주로 한다.

## 정규식 표현법

정규식 해석을 위한 정리를 하면 아래 표와 같다.

<table>
  <thead>
    <tr>
      <th>표현식</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>^</td>
      <td>문자열의 시작, []내에 ^선행 존재시에는 Not의미</td>
    </tr>
    <tr>
      <td>$</td>
      <td>문자열의 마지막</td>
    </tr>
    <tr>
      <td>.</td>
      <td>임의의 한 문자 (단, \ 는 넣을 수 없음)</td>
    </tr>
    <tr>
      <td>*</td>
      <td>0번 이상 반복 (바로 앞 문자가 매우 많을 수도 없을 수도 있음)</td>
    </tr>
    <tr>
      <td>+</td>
      <td>1번 이상 반복 (바로 앞 문자가 매우 많을 수도 1개만 있을 수도 있음)</td>
    </tr>
    <tr>
      <td>?</td>
      <td>바로 앞 문자가 있을 수도 없을 수도 있음</td>
    </tr>
    <tr>
      <td>[]</td>
      <td>문자의 집합, []내 문자가 2개면 둘중 하나를 의미, 또한 범위 지정 (두 문자 사이에는 -로 표현)</td>
    </tr>
    <tr>
      <td>{n}</td>
      <td>앞에 지정한 것이 n개</td>
    </tr>
    <tr>
      <td>{n,}</td>
      <td>앞에 지정한 것이 n개 이상</td>
    </tr>
    <tr>
      <td>{n,m}</td>
      <td>앞에 지정한 것이 n~m개</td>
    </tr>
    <tr>
      <td>()</td>
      <td>소괄호 안의 문자를 하나의 문자로 인식</td>
    </tr>
    <tr>
      <td>|</td>
      <td>or 연산을 수행</td>
    </tr>
  </tbody>
</table>

## 정규식 해석 예시와 자주 쓰이는 정규식

1. 정규식 해석 예시 (위 예시로 들었던)

```java
String regex = "^[0-9]*$"; // 정규식 예시 (숫자만 사용 가능)
/*
^ 문자열 시작
[0-9] 0부터 9 범위
* 마음대로 사용가능
$ 문자열 끝
*/
```

2. 자주 사용되는 정규식

```java
^[0-9]*$ // 숫자만
^[a-zA-Z]*$ // 영어만
^[가-힣]*$ // 한글만
^[a-zA-Z0-9]*$ // 영어, 숫자만
^[a-zA-Z가-힣]*$ // 영어, 한글만
^[가-힣0-9]*$ // 한글, 숫자만

^[a-zA-Z0-9]+@[a-zA-Z0-9]+$ // Email
^01([0|1|6|7|8|9])-?([0-9]{3,4})-?([0-9]{4})$ // 전화번호
```

### 추가 (Java replaceAll에서의 정규식)

Java에서 대상 문자열을 원하는 문자값으로 변환해주는 함수에는 replace(), replaceAll() 등이 있는데 <br>
그 중 replaceAll의 경우 인자 값에 정규표현식이 들어간다. <br>

```java
String str = "Hello!";
str = str.replaceAll("[^a-zA-Z]",""); //[바꾸고싶은 문자의 정규식] 형태
System.out.println(str); // Hello 출력
```
