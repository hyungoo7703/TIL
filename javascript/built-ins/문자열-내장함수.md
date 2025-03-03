# 문자열(String) 내장함수
문자열을 조작하는 데 사용되는 메서드이다.

## 문자열 접근 메서드
### charAt(index)
지정된 인덱스에 있는 문자를 반환
```js
let str = "Hello";
console.log(str.charAt(0)); // "H"
console.log(str.charAt(1)); // "e"
```

### charCodeAt(index)
주어진 인덱스에 대한 UTF-16 코드 단위를 나타내는 0부터 65535 사이의 정수를 반환
```js
let str = "Hello";
console.log(str.charCodeAt(0)); // 72
console.log(str.charCodeAt(1)); // 101
```
UTF-16 코드 단위를 써먹는 일이 있을까 싶지만, 생각보다 유용하게 사용 할 수 있다. <br>
#### [실무 예시]
1. 문자열 암호화 및 해시 함수
암호화 알고리즘이나 해시 함수를 구현할 때 문자의 유니코드 값을 사용하는 경우가 많다.
```js
function simpleHash(str) {
  let hash = 0;
  for (let i = 0; i < str.length; i++) {
    hash = ((hash << 5) - hash) + str.charCodeAt(i);
    hash = hash & hash;
  }
  return hash;
}
```
2. 유효성 검사
특정 범위의 문자만 허용하는 등의 유효성 검사에 사용
```js
function isAlphanumeric(str) {
  for (let i = 0; i < str.length; i++) {
    const code = str.charCodeAt(i);
    if (!(code > 47 && code < 58) && // 숫자 (0-9)
        !(code > 64 && code < 91) && // 대문자 (A-Z)
        !(code > 96 && code < 123)) { // 소문자 (a-z)
      return false;
    }
  }
  return true;
}
```
물론 <b>'정규식'이라는 간단한 유효성 검사가 있긴하다.</b> <br>
위 isAlphanumeric() 함수 역시 정규식을 사용한다면, 단 한줄로 코드가 바뀌긴 한다.
```js
function isAlphanumeric(str) {
  return /^[a-zA-Z0-9]+$/.test(str);
}
```
대부분의 경우 정규식을 사용하는 것이 더 간결하고 가독성이 좋은게 사실이다. <br>
그러나 charCodeAt()을 사용하는 방식이 정규식보다 나은 경우도 있다. <br>
그것은 바로 <br>
<b>동적 패턴 생성시 이다.</b> <br>
특정 유니코드 범위의 문자만 허용하는 동적 패턴을 생성하는 경우를 생각해보자. <br>
(사용자가 입력한 시작 코드와 끝 코드 사이의 문자만 허용하는 패턴을 만들고자 함)
```js
function createDynamicPattern(startCode, endCode) {
  let pattern = '';
  for (let i = startCode; i <= endCode; i++) {
    pattern += String.fromCharCode(i);
  }
  return new RegExp(`^[${pattern}]+$`);
}

// 사용 예시
const greekLettersPattern = createDynamicPattern(945, 969); // 그리스 소문자 알파벳 (α-ω)
console.log(greekLettersPattern.test('αβγδε')); // true
console.log(greekLettersPattern.test('abcde')); // false
```
이 방식은 charCodeAt()의 역함수인 String.fromCharCode()를 사용하여 직접 문자를 생성 <br>
<b>특정 유니코드 범위의 문자를 다루는 데 매우 직관적이고 유연함을 알 수 있다.</b> <br>
순수하게 정규식만으로 이러한 동적 패턴을 생성하려면 벌써 머리가 아파온다. 😊

### codePointAt(index)
지정된 위치의 문자에 대한 코드 포인트(유니코드 문자를 나타내는 정수 값) 값을 반환
```js
let str = "😊";
console.log(str.codePointAt(0)); // 128522
```

## 문자열 추출 메서드
### slice(start, end)
slice() 메서드는 문자열의 일부를 추출하여 새로운 문자열을 반환(시작 인덱스는 포함되고 끝 인덱스는 제외)
```js
let str = "Hello, world!";
console.log(str.slice(0, 5)); // 인덱스 0부터 5 이전까지의 문자열을 추출 → "Hello"
console.log(str.slice(7)); // 인덱스 7부터 끝까지의 문자열을 추출 → "world!"
console.log(str.slice(-6)); // 음수 인덱스를 사용하여 뒤에서부터 6번째 문자부터 끝까지 → "world!"
```

### substring(start, end)
substring() 메서드는 slice()와 유사하지만 음수 인덱스를 지원하지 않는다. 시작 인덱스와 끝 인덱스를 바꿔도 동작한다. <br>
(시작 인덱스는 포함되고 끝 인덱스는 제외)
```js
let str = "Hello, world!";
console.log(str.substring(0, 5)); // "Hello"
console.log(str.substring(7)); // "world!"
console.log(str.substring(5, 0)); // 시작 인덱스가 끝 인덱스보다 크지만, substring()은 내부적으로 인덱스를 교환하여 동작 → "Hello"
```

### substr(start, length)
substr() 메서드는 시작 인덱스부터 지정된 길이만큼의 문자를 반환(길이를 생략하면 문자열 끝까지 추출)
```js
let str = "Hello, world!";
console.log(str.substr(0, 5)); // "Hello"
console.log(str.substr(7)); // "world!"
console.log(str.substr(-6)); // "world!"
```
slice 매서드와 매우 유사해 보이지만, 두 번째 매개변수는 추출할 부분 문자열의 길이인 점이 다르다. <br>
substr()은 ECMAScript에서 공식적으로 지원되지 않으며 향후 사용이 중단될 수 있기에 <br>
slice()를 사용하는 것이 ECMAScript 표준에 포함되어 있으며 권장되는 방법이다.

## 문자열 변환 메서드
### toLowerCase()
모든 문자를 소문자로 변환
```js
console.log("HELLO".toLowerCase()); // "hello"
```

### toUpperCase()
모든 문자를 대문자로 변환
```js
console.log("hello".toUpperCase()); // "HELLO"
```

### toString()
문자열을 반환(주로 다른 타입의 객체를 문자열로 변환할 때 사용)
```js
console.log((123).toString()); // "123"
console.log(["Hello", " world!"].toString()); // "Hello, world!"
```

## 문자열 조작 메서드
### concat(str1, str2, ..., strN)
하나 이상의 문자열을 현재 문자열에 결합 후 새로운 문자열을 반환
```js
let str = "Hello";
console.log(str.concat(" World", "!")); // "Hello World!"
console.log(str.concat(" ", "World", "!", " Goodbye", " World!")); // "Hello World! Goodbye World!"
```

### split(separator, limit)
문자열을 지정된 구분자(separator)로 나누어 배열로 반환 <br>
(limit 매개변수를 사용하여 반환되는 배열의 최대 크기를 지정할 수 있다.)
```js
let str = "apple,banana,cherry";
console.log(str.split(",")); // ["apple", "banana", "cherry"]
console.log(str.split(",", 2)); // ["apple", "banana"]
```

### replace(searchValue, replaceValue)
문자열에서 첫 번째로 일치하는 부분을 찾아 다른 문자열로 대체(searchValue는 문자열이나 정규식)
```js
let str = "Hello World";
console.log(str.replace("World", "JavaScript")); // "Hello JavaScript"
console.log(str.replace(/o/i, "0")); // "Hell0 World"
```

### replaceAll(regexp|substr, newSubstr|function)
문자열에서 일치하는 모든 부분을 찾아 다른 문자열로 대체(regexp|substr는 문자열이나 정규식)
```js
let str = "Hello Hello";
console.log(str.replaceAll("Hello", "Hi")); // "Hi Hi"
console.log(str.replaceAll(/l/g, "1")); // "He11o He11o"
```

### trim()
문자열 양 끝의 공백을 제거한 새로운 문자열을 반환
```js
let str = "   Hello World   ";
console.log(str.trim()); // "Hello World"
```

### trimStart()
문자열 시작 부분의 공백을 제거한 새로운 문자열을 반환
```js
let str = "   Hello World   ";
console.log(str.trimStart()); // "Hello World   "
```

### trimEnd()
문자열 끝 부분의 공백을 제거한 새로운 문자열을 반환
```js
let str = "   Hello World   ";
console.log(str.trimEnd()); // "   Hello World"
```

### padStart(targetLength, padString)
현재 문자열의 시작 부분을 다른 문자열로 채워, 주어진 길이(targetLength)를 만족하는 새로운 문자열을 반환
```js
let str = "5";
console.log(str.padStart(3, "0")); // "005"
console.log(str.padStart(3, "ab")); // "ab5"
```

### padEnd(targetLength, padString)
현재 문자열의 끝 부분을 다른 문자열로 채워, 주어진 길이(targetLength)를 만족하는 새로운 문자열을 반환
```js
let str = "5";
console.log(str.padEnd(3, "0")); // "500"
console.log(str.padEnd(3, "ab")); // "5ab"
```
