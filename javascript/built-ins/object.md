# 객체(Object) 내장함수
Object는 JavaScript의 최상위 객체로써, 이를 다루는 내장함수들을 제공한다.

## Object 정적 메서드

### Object.create(proto)
지정된 프로토타입 객체와 속성을 갖는 새 객체를 만든다.
```js
const person = {
  sayHello() {
    console.log(`Hello, my name is ${this.name}`);
  }
};

const john = Object.create(person);
john.name = 'John';
john.sayHello(); // "Hello, my name is John"
```

### Object.keys(obj)
객체의 열거 가능한 속성 값들을 배열로 반환
```js
const obj = { a: 1, b: 2, c: 3 };
console.log(Object.keys(obj)); // ["a", "b", "c"]
```

### Object.values(obj)
객체의 열거 가능한 속성 값들을 배열로 반환
```js
const obj = { a: 1, b: 2, c: 3 };
console.log(Object.values(obj)); // [1, 2, 3]
```

### Object.entries(obj)
객체의 [key, value] 쌍을 배열로 반환
```js
const obj = { a: 1, b: 2, c: 3 };
console.log(Object.entries(obj)); // [["a", 1], ["b", 2], ["c", 3]]
```

## Object 인스턴스 메서드

### hasOwnProperty(name)
객체가 특정 <b>속성</b>을 직접 소유하고 있는지 확인하는 메서드
```js
const obj = { a: 1, b: 2 };

console.log(obj.hasOwnProperty('a')); // true
console.log(obj.hasOwnProperty('b')); // true
console.log(obj.hasOwnProperty('toString')); // false
```

#### 속성의 값의 소유를 확인하는 매서드는?
JavaScript에서는 객체의 값에 직접 접근할 수 있기에, Object 인스턴스 메서드로 존재하지 않는다. <br>
대신 앞서 확인했던, Object.values(obj)를 이용하면 쉽게 로직 구현이 가능하다.
```js
function hasValue(obj, value) {
  return Object.values(obj).includes(value); //속성의 값의 소유를 확인하는 로직
}

const example = { a: 1, b: 2, c: 3 };
console.log(hasValue(example, 2)); // true
console.log(hasValue(example, 4)); // false
```
