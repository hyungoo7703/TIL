> 참고: [https://learnjs.vlpt.us/basics/09-array-functions.html](https://learnjs.vlpt.us/basics/09-array-functions.html)
# 배열 내장함수

자바 스크립트에서 배열 자료형을 다룰 때 알고 있으면 유용한 내장 함수를 정리하려고 한다.

## forEach

배열안 요소를 출력하기 위해 우리는 아래와 같이 반복문을 사용하곤 하는데, 

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];

// 기존 반복문 출력
for( let number of numbers ) {
    console.log(number); // 출력결과: [1, 2, 3, 4, 5, 6, 7, 8]
}
```

forEach 함수를 통해서도 구현 가능하다.

```javascript
// forEach 함수를 이용한 출력
numbers.forEach(number => {
    console.log(number); // 출력결과: [1, 2, 3, 4, 5, 6, 7, 8]
});
```

## map, filter
배열안의 원소를 변환 할 때 사용되는 함수로, 과정의 결과로 새로운 배열이 만들어진다. <br>

### map
<b>목적</b>: 배열의 각 요소를 변환하여 새로운 배열을 생성한다. <br>
<b>동작</b>: 배열의 모든 요소에 대해 주어진 함수를 호출하고, 그 결과로 새 배열을 만든다. <br>
<b>특징</b>: 원본 배열의 길이와 새 배열의 길이가 항상 같다. <br>
→ 원래 원소가 바뀔수 있다.

### filter
<b>목적</b>: 주어진 조건을 만족하는 요소만을 추출하여 새로운 배열을 생성한다. <br>
<b>동작</b>: 배열의 각 요소에 대해 주어진 조건 함수를 실행하고, 그 결과가 true인 요소만 새 배열에 포함시킨다. <br>
<b>특징</b>: 새 배열의 길이는 원본 배열의 길이보다 같거나 작다. <br>
→ 원래 원소는 바뀌지 않고 true, false로 걸러진다고 보면 된다.

```javascript
const fruits = ['apple', 'banana', 'cherry', 'date', 'elderberry'];

// map 예시: 모든 과일 이름을 대문자로 변환
const upperCaseFruits = fruits.map(fruit => fruit.toUpperCase());
console.log(upperCaseFruits);
// 출력결과: ['APPLE', 'BANANA', 'CHERRY', 'DATE', 'ELDERBERRY']

// filter 예시: 5글자 이상인 과일 이름만 선택
const longNameFruits = fruits.filter(fruit => fruit.length >= 5);
console.log(longNameFruits);
// 출력결과: ['apple', 'banana', 'cherry', 'elderberry']

// map과 filter 조합 예시: 5글자 이상인 과일 이름을 선택하고 대문자로 변환
const longUpperCaseFruits = fruits
  .filter(fruit => fruit.length >= 5)
  .map(fruit => fruit.toUpperCase());
console.log(longUpperCaseFruits);
// 출력결과: ['APPLE', 'BANANA', 'CHERRY', 'ELDERBERRY']
```

## indexOf, findIndex, find

1. **indexOf** <br>
배열에서 원하는 항목이 몇번째 원소인지 찾아준다.

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];

const index = numbers.indexOf(2);
console.log(numbersMap); // 출력결과: 1
```

2. **findIndex** <br>
배열안 원소가 객체일 경우는 indexOf로 찾을 수 없어 findIndex 함수를 이용해야 한다. <br>
객체의 검사하고자 하는 조건을 넣어서 검색하면 된다.

```javascript
const numbers = [
  {
    number: 1,
    text: '하나'
  },
  {
    number: 2,
    text: '둘'
  },
  {
    number: 3,
    text: '셋'
  }
];

const index = numbers.findIndex(numbers => numbers.number === 3);
console.log(index); // 출력결과: 2
```

3. **find** <br>
findIndex와 비슷한데, 찾아낸 객체를 몇번째 인지를 찾는 것이 아닌 찾은 값 자체를 반환한다.

## splice, slice

1. **splice** <br>
배열에서 특정 항목을 제거할때 사용하는 함수이다. <br>

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];

numbers.splice(1, 1); // splice(지우기 시작할 인덱스, 몇개를 지울 것인가)
console.log(numbers); // 출력결과: [1, 3, 4, 5, 6, 7, 8]
```

2. **slice** <br>
splice 함수와 비슷하지만, 기존의 배열은 건들이지 않는 것이 특징이다.

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];

const sliced = numbers.slice(0, 4); // splice(시작 인덱스, 끝 인덱스) : 시작부터 끝에서 잘라서 추출

console.log(sliced); // 출력결과: [1, 2, 3, 4]
console.log(numbers); // 출력결과: [1, 2, 3, 4, 5, 6, 7, 8]
```

## unshift

배열의 맨 앞에 새 원소를 추가해준다.

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];

numbers.unshift(5);
console.log(numbers); // 출력결과: [5, 1, 2, 3, 4, 5, 6, 7, 8]
```

## concat

여러개의 배열을 하나로 합쳐준다.

```javascript
const arr1 = [1, 2, 3, 4];
const arr2 = [5, 6, 7, 8];
const numbers = arr1.concat(arr2);

console.log(numbers); // 출력결과: [1, 2, 3, 4, 5, 6, 7, 8]
```

## some

배열 안의 어떤 요소라도 주어진 판별 함수를 통과하는지 테스트한다.

```javascript
const numbers = [1, 2, 3, 4, 5];
const hasEven = numbers.some(num => num % 2 === 0);

console.log(hasEven); // 출력결과: true
```

## every

배열 안의 모든 요소가 주어진 판별 함수를 통과하는지 테스트한다.

```javascript
const numbers = [2, 4, 6, 8, 10];
const allEven = numbers.every(num => num % 2 === 0);

console.log(allEven); // 출력결과: true
```
