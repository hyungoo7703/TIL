# 클로저
자바스크립트에서의 강력한 기능으로, 함수가 자신이 생성될 때의 환경을 기억하는 근본적인 특성/메커니즘

> #### 환경 기억의 의미 <br>
1. 함수가 생성되는 시점의 모든 변수와 값들을 함수 내부에 저장 <br>
2. 함수가 나중에 실행되더라도 생성 시점의 변수들에 접근 가능 <br>
3. 마치 함수가 자신만의 '기억 창고'를 가지고 다니는 것과 같음

## 클로저의 기본 개념

#### [정의]
+ 함수가 만들어질 때 주변의 변수들을 <b>'사진 찍듯이'</b> 기억하는 것
+ 나중에 그 함수를 실행할 때도 <b>'찍어둔 사진'</b> 속의 변수들을 그대로 사용할 수 있음
+ <b>즉 클로저는 인스턴스가 생성될 때의 상태(환경)를 '닫아서(closing)' 저장하는 개념</b>

#### [예시]
```js
function makeGreeting(name) {
    // name 변수는 이 시점의 환경에 저장됨
    return function() {
        // 나중에 이 함수가 실행될 때도 name 값을 기억하고 있음
        return "안녕하세요, " + name + "님";
    }
}

const greetKim = makeGreeting("김철수");
const greetLee = makeGreeting("이영희");

// 각 함수는 자신이 생성될 때의 name 값을 기억함
console.log(greetKim());  // "안녕하세요, 김철수님"
console.log(greetLee());  // "안녕하세요, 이영희님"
```
> #### 이 예시에서의 클로저 동작방식 <br>
1. makeGreeting 함수가 실행될 때 name 매개변수는 함수의 렉시컬 환경(어휘적 환경)에 저장됨 <br>
2. 내부 함수가 생성될 때 이 환경에 대한 참조를 유지 <br>
3. return function() { <br>
    // 이 내부 함수는 자신만의 스코프 체인을 가짐 <br>
    // 여기서 name을 찾을 때 외부 함수의 렉시컬 환경을 참조 <br>
} 으로 클로저 형성

## 실제 응용예시
클로저를 통해 데이터 은닉, 상태 관리, 모듈화된 코드 작성 등이 가능하다. <br>
아래는 실제 응용예시 중 두 가지이다.

1. 이벤트 핸들링
```js
function setupHandler() {
    let count = 0; // 클로저에 의해 보존되는 변수
    document.getElementById('button').addEventListener('click', function() {
        console.log(++count);
    });
}
```
> #### 특징과 장점
+ count 변수는 외부에서 직접 접근 불가능
+ 버튼 클릭마다 count가 증가하면서도 변수를 안전하게 보호
+ 여러 버튼이 있어도 각각의 count가 독립적으로 관리됨

2. 팩토리 함수
```js
function personFactory(name, age) {
    return {
        getName: () => name, // name을 클로저로 캡슐화
        getAge: () => age, // age를 클로저로 캡슐화
        celebrateBirthday: () => age += 1 // age 수정 가능
    };
}
```
> #### 특징과 장점
+ name과 age가 외부에서 직접 접근 불가능
+ 객체의 메서드를 통해서만 데이터 접근 가능
+ 데이터 은닉화와 캡슐화 구현
+ 각 인스턴스마다 독립된 상태 유지

> #### 위 팩토리 함수의 사용예시
```js
const person = personFactory("Kim", 25);
console.log(person.getName());  // "Kim"
console.log(person.getAge());   // 25
person.celebrateBirthday();
console.log(person.getAge());   // 26
```
