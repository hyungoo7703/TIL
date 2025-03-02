# querySelector 선택자 
> #### querySelector, querySelectorAll

CSS선택자를 사용하여 HTML의 요소를 찾을 때 <br>
JavaScript의 querySelector(), querySelectorAll() 메소드를 이용하여 찾을 수 있다. 
> 단, 같은 id 혹은 class의 경우 최상단 요소를 찾는다.

```javascript
const selectOne = document.querySelector('.class'); // 부합하는 첫번째 요소만
const selectAll = document.querySelectorAll('.class'); // 부합하는 모든 요소

console.log(selectAll);
```

코드로 사용 할 경우 위와 같은데

![화면 캡처 2023-01-05 105512](https://user-images.githubusercontent.com/93297109/210684288-5565797f-eb71-4df6-bfca-71f6b24a56fd.png)

이 중 selectAll을 콘솔에 찍어보면 NodeList로 반환하는 것을 알 수있다.

## NodeList
> 참고: [https://developer.mozilla.org/ko/docs/Web/API/NodeList](https://developer.mozilla.org/ko/docs/Web/API/NodeList)

NodeList는 배열과 비슷한 객체인데, (브라우저에서 제공하는 API) <br>
for문 또는 forEach문을 사용하여 반복할 수 있고, 배열로 변환도 가능하다.

+ 주요 메서드
  + item(): 항목의 인덱스를 반환, 인덱스가 범위 외의 경우일 땐 null 반환
  + entries(): iterator 를 반환하여 코드가 콜렉션에 포함된 모든 키/값 쌍을 순회할 수 있도록 한다. (이 경우 키는 0부터 시작하는 숫자이고, 값은 노드)
  + keys(): iterator 를 반환, 키
  + values(): iterator 를 반환, 값
  + forEach(): NodeList의 요소(element)마다 한 번씩, 인자로 전달 받은 함수를 실행하여 요소를 인수(argument)로 함수에 전달
