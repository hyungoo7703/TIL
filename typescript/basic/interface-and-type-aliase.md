# 인터페이스와 타입 별칭
TypeScript에서 타입을 정의하는 두 가지 주요 방법인 인터페이스(Interface)와 타입 별칭(Type Alias)이 있다.

## 인터페이스(Interface)
인터페이스는 상호 간에 정의한 약속 혹은 규칙을 의미한다. 주로 객체의 구조를 정의하는 데 사용된다.

> #### 기본 문법
```ts
interface Person {
  name: string;
  age: number;
  sayHello(): void;
}

// 인터페이스 사용 예
const user: Person = {
  name: '홍길동',
  age: 30,
  sayHello() {
    console.log(`안녕하세요, ${this.name}입니다.`);
  }
};
```

## 타입 별칭(Type Alias)
타입 별칭은 특정 타입이나 인터페이스를 참조할 수 있는 타입 변수를 의미한다.

> #### 기본문법
```ts
// 기본 타입에 별칭 부여
type MyString = string;
const name: MyString = '홍길동';

// 객체 타입에 별칭 부여
type Person = {
  name: string;
  age: number;
  sayHello(): void;
};

// 유니온 타입에 별칭 부여
type ID = string | number;
const userId: ID = 123456;
```

## Interface vs Type: 주요 차이점

> #### 1. 확장 방식
**Interface** - extends 키워드로 확장
```ts
interface Animal {
  name: string;
}

interface Cat extends Animal {
  meow(): void;
}
```
**Type** - 인터섹션(&) 연산자로 확장과 유사한 효과
```ts
type Animal = {
  name: string;
};

type Cat = Animal & {
  meow(): void;
};
```

> #### 2. 선언 병합
**Interface** - 동일한 이름으로 여러 번 선언하면 자동으로 병합
```ts
interface User {
  name: string;
}

interface User {
  age: number;
}

// User는 { name: string; age: number; }
```
**Type** - 동일한 이름으로 재선언 불가능
```ts
type User = {
  name: string;
};

// 오류 발생!
type User = {
  age: number;
};
```

> #### 3. 적용 가능한 타입 범위
**Interface** - 주로 객체 구조 정의에 사용
+  클래스 구현(implements)에 활용 가능
+  객체 타입 정의에 최적화 

**Type** - 더 넓은 범위의 타입 정의 가능
+  객체 타입
+  원시 타입(string, number 등)
+  유니온 타입 (|)
+  인터섹션 타입 (&)
+  튜플, 배열 등

-----

## JavaScript 개발자가 TypeScript 사용 시 주의할 점?

> #### 1. any 타입은 신중하게 사용하기
```ts
// 😱 피해야 할 패턴
let data: any = getDataFromAPI();
data.nonExistentMethod(); // 런타임 오류 발생 가능

// 😀 권장되는 패턴
interface ApiResponse {
  items: string[];
  count: number;
}
let data: ApiResponse = getDataFromAPI();
```
TypeScript의 any 타입은 JavaScript와 같은 동적 타입을 사용할 수 있게 해주지만, 타입 안전성을 잃게 된다. <br>
꼭 필요한 경우가 아니라면 구체적인 타입을 지정하는 것이 좋다.

> #### 2. 명시적 타입 지정 습관화
변수와 함수 반환값에 타입을 명시적으로 지정하는 습관을 들이는 것이 좋다.
```ts
// 명시적 타입 지정
function fetchUser(id: string): Promise<User> {
  return api.getUser(id);
}

// 객체 구조도 인터페이스로 명확히 정의
interface User {
  id: string;
  name: string;
  email: string;
  active: boolean;
}
```
