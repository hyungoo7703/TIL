# this 바인딩
this 바인딩은 함수가 호출되는 방식에 따라 동적으로 결정되는 중요한 개념이다. <br>
(함수 내부에서 this가 어떤 객체를 참조하는지 이해하는 것은 필수적이다.)

## 기본 this 바인딩 규칙
> #### 일반 함수 호출
```js
function showThis() {
  console.log(this);
}
showThis(); // window 또는 global
```
+ 일반 함수로 호출될 때는 this는 전역 객체를 가리킨다.
+ 브라우저에서는 window를 참조할 것이고, Node.js에서는 global 객체를 참조한다.

> #### 객체의 메서드로 호출
```js
const obj = {
  name: 'Alice',
  showThis: function() {
    console.log(this); // obj 객체
  }
};
obj.showThis();
```
+ 객체의 메서드로 호출될 때 this는 해당 메서드를 호출한 객체를 가리킨다.

> #### 생성자 함수 호출
```js
function Person(name) {
  this.name = name;
}
const alice = new Person('Alice');
console.log(alice.name); // 'Alice'
```
+ new 키워드와 함께 호출될 때 this는 새로 생성된 객체를 가리킨다.

## 명시적 this 바인딩
> #### call 메서드
```js
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}
const person = { name: 'Alice' };
greet.call(person, 'Hello'); // 'Hello, Alice'
```
+ 첫 번째 인자로 this 값을 설정하고, 나머지 인자들을 함수의 인자로 전달한다.

> #### apply 메서드
```js
function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}
const person = { name: 'Alice' };
greet.apply(person, ['Hello']); // 'Hello, Alice'
```
+ call과 유사하지만 두 번째 인자로 배열 형태의 인자를 전달한다.

> #### bind 메서드
```js
const person = { name: 'Alice' };
const greet = function() {
  console.log(`Hello, ${this.name}`);
}.bind(person);
greet(); // 'Hello, Alice'
```
+ this 값을 영구적으로 바인딩한 새로운 함수를 반환한다.

-----

## 내부 함수에서 this 문제가 발생할 수 있다?
아래 코드를 보면,  this가 전역 객체를 가리키는 문제가 발생할 수 있다.
```js
const obj = {
  value: 100,
  foo: function() {
    console.log("foo's this.value: ", this.value); // 100
    
    function bar() {
      console.log("bar's this.value: ", this.value); // undefined 또는 전역 객체의 value
    }
    bar();
  }
}
obj.foo();
```
이를 어떻게 해결 할 수 있을까?

> #### [해결방법]
1. 변수에 this 저장하기
```js
var obj = {
  value: 100,
  foo: function() {
    var that = this;
    function bar() {
      console.log(that.value); // 100
    }
    bar();
  }
};
```

2. **화살표 함수** <br>
화살표 함수는 자신만의 this를 생성하지 않고 상위 스코프의 this를 그대로 사용하기에 권장된다.