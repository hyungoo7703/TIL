# 이벤트 루프
이벤트 루프는 JavaScript의 비동기 프로그래밍을 가능하게 하는 핵심 메커니즘이다. <br>
비동기 프로그래밍에 대한 정리는 이곳에서 확인하면 된다. ☞ [비동기 프로그래밍](./async-programming.md)

## 이벤트 루프의 정의
이벤트 루프는 JavaScript 엔진이 태스크를 처리하고 비동기 작업을 관리하는 방식을 말한다.
JavaScript는 단일 스레드 언어이지만, 이벤트 루프를 통해 비동기적으로 여러 작업을 처리할 수 있게 되는 것이다.

## 이벤트 루프의 구성 요소
> #### 전체적인 개관 이미지 <br>
![image](https://github.com/user-attachments/assets/6620ceab-8bef-4b53-be02-762626363efe)
> 참고: https://baeharam.netlify.app/posts/javascript/event-loop

### 콜 스택 (Call Stack):
+ 현재 실행 중인 함수들이 쌓이는 곳
+ LIFO(Last In, First Out) 구조로 동작
+ 하나의 콜 스택 = 하나의 쓰레드 = 한번에 하나의 작업

### Web APIs:
+ 브라우저에서 제공하는 비동기 작업을 처리하는 API들
+ 예: setTimeout, fetch, DOM 이벤트 등

### 콜백 큐 (Callback Queue):
+ Web API에서 처리된 비동기 작업의 콜백 함수들이 대기하는 곳
+ FIFO(First In, First Out) 구조로 동작

### 이벤트 루프:
+ 콜 스택이 비어있을 때 콜백 큐의 함수를 콜 스택으로 이동

## 이벤트 루프의 동작 과정
1. 코드가 실행되면 함수들이 콜 스택에 쌓인다.
2. 비동기 작업(예: setTimeout)을 만나면 Web API로 보내진다.
3. Web API에서 작업이 완료되면 해당 콜백 함수를 콜백 큐로 보낸다.
4. 이벤트 루프는 지속적으로 콜 스택과 콜백 큐를 감시한다.
5. 콜 스택이 비어있으면 이벤트 루프가 콜백 큐의 첫 번째 콜백을 콜 스택으로 이동시킨다.
6. 이 과정을 반복한다.

> #### 예시로 이해하기
[예시 소스]
```js
console.log('시작');

setTimeout(() => {
  console.log('타이머 완료');
}, 0);

console.log('종료');
```
[결과]
```
시작
종료
타이머 완료
```
**이 코드가 실행되는 과정**:
1. '시작'이 콘솔에 출력
2. setTimeout이 Web API로 보내진다.
3. '종료'가 콘솔에 출력
4. 이때 콜 스택이 비워지게 된다.
5. setTimeout의 콜백이 콜백 큐로 이동한다.
6. 이벤트 루프가 콜백을 콜 스택으로 이동시킨다.
7. '타이머 완료'가 콘솔에 출력
