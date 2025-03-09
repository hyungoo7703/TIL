# Truthiness narrowing
> 참고 [https://www.typescriptlang.org/docs/handbook/2/narrowing.html](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

Truthiness narrowing은 JavaScript의 truthy와 falsy 값의 특성을 이용하여 TypeScript에서 타입을 좁히는 기법

### Truthy와 Falsy 값
+ Falsy 값: 0, NaN, "" (빈 문자열), 0n (BigInt 0), null, undefined
+ Truthy 값: Falsy가 아닌 모든 값

### 주요특징
1. 조건문에서의 활용
```ts
function getUsersOnlineMessage(numUsersOnline: number) {
  if (numUsersOnline) {
    return `There are ${numUsersOnline} online now!`;
  }
  return "Nobody's here. :(";
}
```
☞ numUsersOnline이 0이면 falsy로 평가되어 두 번째 return문이 실행된다.

2. null과 undefined 체크
```ts
function printAll(strs: string | string[] | null) {
  if (strs && typeof strs === "object") {
    for (const s of strs) {
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  }
}
```
☞ strs && typeof strs === "object"는 strs가 null이 아니고 객체일 때만 true이다.

3. 부정 연산자(!)를 이용한 narrowing
```ts
function multiplyAll(
  values: number[] | undefined,
  factor: number
): number[] | undefined {
  if (!values) {
    return values;
  } else {
    return values.map((x) => x * factor);
  }
}
```
☞ !values는 values가 undefined일 때 true이다.

### 주의사항
+ 빈 문자열 처리
  + 빈 문자열("")은 falsy이므로, 문자열 처리 시 주의가 필요하다.
+ 과도한 사용 주의
  + Truthiness 체크만으로는 모든 경우를 완벽하게 처리할 수 없다.
+ 명시적 타입 체크와 병행
  + 가능한 경우, typeof나 instanceof와 같은 명시적 타입 체크와 함께 사용하는 것이 좋다.
