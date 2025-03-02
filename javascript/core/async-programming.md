# 비동기 프로그래밍
JavaScript의 비동기 프로그래밍은 싱글 스레드 환경에서 효율적인 작업 처리를 가능하게 하는 핵심 개념이다.

## 동기? 비동기?

> #### 동기
동기(Synchronous) 방식은 코드가 순차적으로 실행되며, 이전 작업이 완료될 때까지 다음 작업을 수행하지 않는 방식

> #### 비동기
비동기(Asynchronous) 방식은 특정 작업의 완료를 기다리지 않고 다른 작업을 동시에 수행할 수 있는 방식

## 비동기 처리 방식

> #### 콜백 함수
+ 다른 함수의 인자로 전달되어 나중에 실행되는 함수
+ 콜백 중첩으로 인한 콜백 지옥이 발생할 수 있는 단점

> #### Promise
+ 비동기 작업의 미래 결과값을 나타내는 객체
+ pending(대기), fulfilled(이행), rejected(거부) 세 가지 상태를 가짐
+ .then()과 .catch()를 통해 결과를 처리

> #### Async/Await
+ Promise를 더 간단하고 동기적인 코드처럼 작성할 수 있게 해주는 문법
+ async 함수 내에서 await 키워드를 사용해 Promise의 결과를 기다림

## 비동기 처리의 장점

+ 오래 걸리는 작업을 기다리지 않고 다른 작업을 수행할 수 있다.
+ 웹 애플리케이션의 성능과 사용자 경험을 향상시킬 수 있다.
+ 네트워크 요청, 파일 입출력, 타이머 등 시간이 걸리는 작업을 효율적으로 처리할 수 있다.

## 비동기 처리의 예시와 실무적 관점

### 상황1. 사용자 정보 조회 및 주문 처리
온라인 쇼핑몰에서 사용자가 주문을 완료할 때, <br>
사용자 정보 확인 → 재고 확인 → 결제 처리 → 주문 완료 처리의 순차적 작업이 필요하다고 가정해보는 상황 <br>
(실제 처리는 이렇게 단순하지 않으나, 비동기 처리의 예시를 위해 표현)

> #### 콜백 함수 방식 (비권장)
```js
function processOrder(userId, productId) {
    checkUser(userId, function(user) {
        checkStock(productId, function(stock) {
            processPayment(user, function(payment) {
                completeOrder(payment, function(order) {
                    sendConfirmation(order, function(result) {
                        console.log("주문 완료");
                    });
                });
            });
        });
    });
}
```
**비권장 사유**:
+ 코드가 오른쪽으로 계속 들여쓰기되어 가독성이 떨어지고 에러 처리가 복잡
+ 각 단계별 에러 처리가 어렵고 유지보수가 힘들어짐

> #### Promise 방식 (비권장 → 개선)
```js
function processOrder(userId, productId) {
    checkUser(userId)
        .then(user => {
            return checkStock(productId).then(stock => {
                return { user, stock };
            });
        })
        .then(data => processPayment(data.user, data.stock))
        .then(payment => completeOrder(payment))
        .then(order => sendConfirmation(order));
}
```
**개선 사유**:
+ 장점: 체인 형태로 가독성이 개선되고 에러 처리가 단순화
+ 단점: 여전히 코드가 길어진다면 복잡

> #### Async/Await 방식 (권장)
```js
async function processOrder(userId, productId) {
    try {
        const user = await checkUser(userId);
        const stock = await checkStock(productId);
        const payment = await processPayment(user);
        const order = await completeOrder(payment);
        const result = await sendConfirmation(order);
        console.log("주문 완료");
    } catch (error) {
        console.error("주문 처리 실패:", error);
    }
}

/**
Async/Await는 자바스크립트에서 비동기 작업을 더 쉽게 처리하기 위한 문법

async: 함수를 비동기 함수로 선언하며, 항상 Promise를 반환
await: Promise가 처리될 때까지 코드 실행을 일시 중지하고 기다림
*/
```
**권장 사유**:
+ 동기식 코드처럼 읽기 쉽고 이해하기 쉬움
+ 에러 처리가 try-catch로 깔끔하게 처리

### 상황2. 다중 API 호출
여러 개의 API를 동시에 호출하여 데이터를 취합해야 하는 상황

> #### Promise.all 활용 (권장)
```js
async function getDashboardData() {
    try {
        const [
            userStats,
            salesData,
            inventoryStatus
        ] = await Promise.all([
            fetchUserStats(),
            fetchSalesData(),
            fetchInventoryStatus()
        ]);
        
        return {
            userStats,
            salesData,
            inventoryStatus
        };
    } catch (error) {
        console.error("데이터 조회 실패:", error);
    }
}

/**
Promise.all은 여러 개의 Promise를 병렬로 처리하고 모든 Promise가 완료될 때까지 기다리는 메서드
여러 개의 Promise를 배열로 받아서 하나의 Promise를 반환함을 기억(하나라도 실패하면 전체가 실패로 처리)
*/
```
**권장 사유**:
+ 여러 비동기 작업을 동시에 처리하여 성능 향상
+ API 호출이 서로 독립적일 때 유용

## 실무 권장사항

> #### 권장하는 경우
+ 복잡한 비동기 로직은 Async/Await 사용
+ 병렬 처리가 필요한 경우 Promise.all 활용
+ 간단한 이벤트 핸들러는 Promise 체인 사용

> #### 피해야 할 경우
+ 단순 콜백은 이벤트 리스너에만 사용
+ 중첩된 콜백 구조는 지양
+ Promise 체인이 3단계 이상 길어지면 Async/Await로 전환
