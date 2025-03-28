# 기타 유용한 내장함수
실무, 코테 준비를 하며 유용한 내장함수가 있을때 마다 정리

## Math 객체(수학적 상수와 함수를 제공하는 내장 객체)
Math 객체는 생성자 함수가 아니므로 new 연산자로 인스턴스를 생성할 수 없다. 

### Math.abs(x)
숫자의 절대값을 반환
```js
console.log(Math.abs(-5)); // 5
console.log(Math.abs(5)); // 5
```

### Math.ceil(x)
주어진 숫자보다 크거나 같은 가장 작은 정수를 반환
```js
console.log(Math.ceil(4.3)); // 5
console.log(Math.ceil(-4.3)); // -4
```

### Math.floor(x)
주어진 숫자보다 작거나 같은 가장 큰 정수
```js
console.log(Math.floor(4.7)); // 4
console.log(Math.floor(-4.7)); // -5
```

### Math.round(x)
가장 가까운 정수로 반올림
```js
console.log(Math.round(4.4)); // 4
console.log(Math.round(4.5)); // 5
```

### Math.max(...values), Math.min(...values)
주어진 숫자 중 가장 큰 값, 가장 작은 값 반환
```js
console.log(Math.max(1, 3, 2)); // 3
console.log(Math.max(-1, -3, -2)); // -1
console.log(Math.min(1, 3, 2)); // 1
console.log(Math.min(-1, -3, -2)); // -3
```
#### [TIP]
코테에서 배열 내 숫자들에서 가장 큰 값, 가장 작은 값 반환은 심심치 않게 보인다. <br>
물론 최댓값, 최솟값 알고리즘을 써도 되지만, Math.max() 함수나 Math.min() 함수를 쓰면 간단하게 처리 가능하다. <br>
배열 자체를 매개변수로 넣을 수 없기에, 함수 호출에서의 스프레드 연산자를 사용해서 처리할 수 있다. <br>

<b>스프레드 연산자(Spread Operator)는 JavaScript ES6에서 도입된 문법으로, <br>
배열이나 객체의 요소를 펼쳐서(spread) 개별 요소로 확장하는 기능을 제공한다.</b> <br>
스프레드 연산자는 ... 기호를 사용하여 표현하며 함수 호출 시 인수를 펼쳐서 전달할 때도 쓸 수 있다. <br>
아래와 같은 예시로 쓸 수 있다.
```js
function sum(a, b, c) {
  return a + b + c;
}

const numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6
```
아래 이미지처럼 Math.max 함수도 마찬가지로 쓸수 있는데, 배열을 매게변수로 받을 수 없는 모습도 확인할 수 있다. <br><br>
![image](https://github.com/user-attachments/assets/8c2a6381-463d-45ec-8687-d602228ff1c9)

### Math.random()
0 이상 1 미만의 난수를 반환
```js
console.log(Math.random()); // 0과 1 사이의 난수
// 0에서 10 사이의 난수를 생성하려면:
console.log(Math.floor(Math.random() * 11));
```
