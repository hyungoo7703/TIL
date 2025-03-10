# 타입 시스템
TypeSciprt의 타입 시스템은 JavaScript에 정적 타입 시스템을 더한 독특한 점진적 타입 시스템(Gradual Type System)을 사용한다. <br>
이 시스템은 코드의 안정성과 가독성을 높이게 하는 여러 기능을 제공해준다.

## 타입 추론과 타입 명시
> #### 타입 추론(Type Inference)
TypeScript는 변수의 초기값을 기반으로 자동으로 타입을 추론하는데, 명시적인 타입 선언 없이도 타입 안전성을 확보할 수 있다.
```ts
let name = "John"; // 자동으로 string 타입으로 추론
name = 42; // 오류: '42' 타입은 'string' 타입에 할당할 수 없음
```

> #### 타입 명시(Type Annotation)
필요한 경우 변수나 함수 매개변수에 명시적으로 타입을 지정할 수 있다.
```ts
let rating: number = 5;
let blog: string = "OpenReplay";
let isActive: boolean = true;
```

## 고급 기능
> #### 유니온 타입(Union Types)
변수가 여러 타입 중 하나를 가질 수 있도록 허용하는 것이다.
```ts
function blog(id: number | string) {
  console.log("Blog ID: " + id);
}
```

> #### 타입 가드(Type Guards)
특정 스코프 내에서 값의 타입을 좁히는 방법이다.
```ts
function isNumber<T>(value: T): value is number {
  return typeof value === 'number';
}

let age: number | string = 30;
if (isNumber(age)) {
  console.log(`Age: ${age + 1}`); // number로 처리
}
```

> #### 조건부 타입(Conditional Types)
다른 타입에 따라 타입을 결정하는 방식이다.
```ts
type IsString<T> = T extends string ? string : never;
```