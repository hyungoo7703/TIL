# Narrowing
> 참고 [https://www.typescriptlang.org/docs/handbook/2/narrowing.html](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

Narrowing은 TypeScript가 코드의 흐름을 분석하여 변수의 타입을 더 구체적으로 좁혀나가는 과정<br>
(이를 통해 TypeScript는 더 정확한 타입 체크와 자동 완성을 제공)

### padLeft 함수를 예시로 설명
```ts
function padLeft(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}
```
위 예시함수의 프로세스
1. padding 매개변수는 초기에 number | string 타입이다.
2. if (typeof padding === "number") 조건문은 타입 가드로 작동한다.
3. 조건문 내부에서 TypeScript는 padding이 number 타입임을 알게 된다.
4. 조건문 외부에서는 padding이 string 타입임을 추론한다.

### Narrowing의 이점
+ <b>타입 안정성</b>: 런타임 오류를 줄일 수 있다.
+ <b>코드 가독성</b>: 타입이 명확해져 코드 이해가 쉬워진다.
+ <b>IDE 지원</b>: 더 정확한 자동 완성과 타입 관련 제안을 받을 수 있다.

### 다양한 Narrowing 방법
+ <b>typeof 타입 가드</b>: typeof x === "number"
+ <b>instanceof 타입 가드</b>: x instanceof Date
+ <b>in 연산자</b>: "property" in object
+ <b>등식 검사</b>: x === "value"
+ <b>사용자 정의 타입 가드 함수</b>

### 의문점. JavaScript의 typeof 연산자를 쓰면 동일한거 아닌가?
위 padLeft 예시 함수를 JavaScript로 바꾸면 아래와 같다.
```js
function padLeft(padding, input) {
  if (typeof padding === "number") {
    return " ".repeat(padding) + input;
  }
  return padding + input;
}
```
언뜻보면 비슷해 보이지만, 자세히 분석해 보면 Narrowing의 장점이 두각된다.<br>
위에서 작성한 Narrowing의 이점에서 연결지을 수 있다.

+ <b>타입 안정성</b>
  + TypeScript 버전에서는 padding이 number | string 타입으로 명시되어 있어, 다른 타입의 인자가 전달되면 컴파일 시점에 오류를 발견할 수 있다.
    > TypeScript는 padding이 string일 때 + 연산자 사용이 안전함을 보장하는 점도 있다.
  + JavaScript 버전에서는 런타임에 예상치 못한 타입의 인자가 전달될 수 있다.
+ <b>코드 가독성</b>
  + TypeScript 버전은 타입 정보가 명시되어 있어 코드의 의도를 더 쉽게 이해할 수 있다.
+ <b>IDE 지원</b>
  + TypeScript에서는 if 문 내부에서 padding이 number 타입으로 좁혀진다. 이로 인해 IDE에서 number 타입에 맞는 메서드와 프로퍼티만 제안하게 된다.
  + JavaScript에서는 이러한 지원이 제한적이다.

<hr>

### 'typeof' 타입가드에 대한 보충 설명

typeof 연산자는 JavaScript에서 값의 타입을 문자열로 반환한다. TypeScript는 이를 활용하여 타입을 좁히는 타입 가드로 사용한다.

+ typeof가 반환하는 문자열 목록:
  + "string"
  + "number"
  + "bigint"
  + "boolean"
  + "symbol"
  + "undefined"
  + "object"
  + "function"

### typeof 타입가드의 사용 주의점: <b>null의 처리</b>
```ts
function printAll(strs: string | string[] | null) {
  if (typeof strs === "object") {
    for (const s of strs) {
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  } else {
    // do nothing
  }
}
```
JavaScript의 오래된 버그중 하나가 있는데, typeof null은 "object"를 반환하는 것이다. <br>
<b>TypeScript는 이 문제를 인식하고 있어, strs의 타입을 string[] | null로 좁힌다. 즉, null의 가능성을 여전히 고려한다는 의미이다.</b> <br>
따라서 for (const s of strs)에서 TypeScript는 strs가 null일 수 있다고 경고하는 것이다. <br>

### 개선 방법
이런 경우, null 체크를 명시적으로 추가하는 것이 좋다.
```js
function printAll(strs: string | string[] | null) {
  if (strs && typeof strs === "object") {
    for (const s of strs) {
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  } else {
    // do nothing
  }
}
```
