# 호이스팅
호이스팅(Hoisting)은 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 현상이다. <br>
이로 인해 코드에서 변수나 함수가 선언되기 전에도 접근할 수 있게 된다.

## 기본개념
호이스팅은 사전적의미로 '끌어올리다'라는 뜻으로, <br>
자바스크립트 엔진이 코드를 실행하기 전에 변수와 함수 선언을 해당 스코프 최상단으로 끌어 올리는 것처럼 동작한다. <br>
이는 실제로 코드가 물리적으로 이동하는 것은 아니며 자바스크립트 엔진이 코드를 파싱하는 과정에서 발생하는 현상이다.

## 변수 호이스팅
> #### var 키워드
```js
console.log(name); // undefined
var name = "James";
console.log(name); // James
```
var로 선언한 변수는 호이스팅 시 undefined로 초기화 <br>
선언은 호이스팅되지만 할당은 호이스팅되지 않는다.

> #### let과 const 키워드
```js
console.log(name); // ReferenceError: Cannot access 'name' before initialization
let name = "James";
```
let과 const로 선언한 변수도 호이스팅되지만, TDZ(Temporal Dead Zone)에 의해 선언 전에 접근하면 참조 오류가 발생한다.

## 함수 호이스팅
> #### 함수 선언문
```js
sayHello(); // "Hello, world!"
function sayHello() {
  console.log("Hello, world!");
}
```
함수 선언문은 전체가 호이스팅되어 선언 전에도 호출할 수 있다.

> #### 함수 표현식
```js
sayHi(); // TypeError: sayHi is not a function
var sayHi = function() {
  console.log("Hi, world!");
};
```
함수 표현식은 변수 호이스팅 규칙을 따르므로, 위 예시에서는 var로 선언된 변수 이기에, undefined로 초기화되어 함수로 호출할 수 없게된다.

## 호이스팅 우선순위
1. 변수 선언이 함수 선언보다 먼저 호이스팅
2. 값이 할당되지 않은 변수의 경우, 같은 이름의 함수 선언이 변수를 덮어쓰게 된다.
3. 값이 할당된 변수의 경우, 변수가 함수 선언을 덮어쓴다.

## 실무적 관점에서의 호이스팅
1. 코드 가독성과 유지보수성
    + 호이스팅에 의존하는 코드는 가독성이 떨어지고 버그 발생 가능성이 높아짐을 기억
    + 변수와 함수는 사용하기 전에 선언하는 습관을 들이는 것이 당연히 좋다.

2. let과 const 사용 권장
    + var 대신 let과 const를 사용하면 TDZ로 인해 선언 전 접근 시 명확한 오류를 발생시켜 디버깅이 용이해진다.

3. 함수 표현식 활용
    + 함수 선언문보다 함수 표현식을 사용하면 호이스팅으로 인한 예상치 못한 동작을 방지할 수 있다는 점을 기억하면 좋다.