# `reduce` 메서드

`reduce` 메서드는 배열의 각 요소를 순회하며 콜백 함수를 실행하고, 콜백 함수의 반환값을 누적하여 최종 결과를 반환하는 메서드
<br>
<b>주로 배열을 기반으로 새로운 값이나 객체를 생성할 때 사용된다.</b>

## `reduce` 메서드의 기본 구문

```javascript
arr.reduce(callback[, initialValue])
```
설명은 아래 링크를 참조하면된다. <br>
[[Node.js] javascript: Array.reduce() 사용 방법 정리](https://miiingo.tistory.com/365)

## 실무관점에서 본 reduce(약관 객체를 예시로)

```javascript
const termData = this.terms.reduce((acc, term) => {
  acc[term.isEssential ? `필수약관${term.seq}` : `선택약관${term.seq}`] = term.isChecked;
  return acc;
}, {});
```

이 코드는 this.terms 배열을 순회하면서 각 약관 항목(term)에 대해 콜백 함수를 실행하는데, 이때 두개의 인수를 받는다.

- acc: 최종적으로 반환될 객체, 초기값은 빈 객체({})로 설정
- term: 현재 처리 중인 약관 항목입니다.(forEach와 비슷하게 순회)

로직의 순서는 아래와 같다.
1. term.isEssential 값에 따라 키를 결정한다.
2. acc 객체에 위에서 결정된 키를 사용하여 term.isChecked 값을 할당
3. acc 객체를 반환

<br>
### 직관적으로 보면 아래와 같다.
예를 들어, this.terms가 다음과 같은 배열이라고 가정
```javascript
[
  { seq: 1, isEssential: true, isChecked: true },
  { seq: 2, isEssential: true, isChecked: false },
  { seq: 3, isEssential: false, isChecked: true }
]
```

<b>'reduce'의 결과</b>
```javascript
{
  필수약관1: true,
  필수약관2: false,
  선택약관3: true
}
```

<br>
배열을 기반으로 새로운 객체를 생성할 때 사용하면 좋을거 같다.
