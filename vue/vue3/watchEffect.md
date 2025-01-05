# watchEffect
Vue3의 watchEffect는 반응형 데이터의 변화를 자동으로 추적하고 반응하는 기능이다.

## 기본 특징

### 자동 의존성 추적
+ 콜백 함수 내부에서 사용되는 모든 반응형 데이터를 자동으로 감지
+ 컴포넌트가 생성될 때 즉시 실행됨
+ 명시적인 감시 대상 지정이 필요없음

> #### 자동 의존성 추적 메커니즘
콜백 함수 내부에서 접근하는 모든 반응형 데이터(ref, reactive)를 자동으로 추적
```js
const name = ref('John')
const age = ref(25)
const address = ref('Seoul')

watchEffect(() => {
  // name과 age만 추적됨
  console.log(`${name.value}는 ${age.value}살입니다`)
  // address는 사용되지 않아 추적되지 않음
})
```
[기억해야할 사항] <br>
watchEffect는 이전 값을 저장하지 않아 메모리 사용이 효율적이다. <br>
**이전 값과 현재 값을 모두 추적하여 비교하려면 watch를 사용해야 한다.**

### 실행 시점 제어
+ flush 옵션으로 콜백 실행 시점 조정 가능
  + pre: DOM 업데이트 전 실행
  + post: DOM 업데이트 후 실행
```js
const count = ref(0)
const element = ref(null)

// DOM 업데이트 후 실행
watchEffect(() => {
  console.log(element.value.textContent) // DOM 접근 가능
}, {
  flush: 'post'
})

// DOM 업데이트 전 실행 (기본값)
watchEffect(() => {
  console.log(count.value) // 데이터 변경 감지
})

// 즉시 실행 (성능 주의)
watchEffect(() => {
  console.log(count.value) // 데이터 변경 즉시 실행
}, {
  flush: 'sync'
})
```

## 감시자 정리 (Cleanup) 기능
watchEffect는 이전 실행을 정리할 수 있는 기능도 있다.
```js
const id = ref(1)
const loading = ref(false)

watchEffect(async (onCleanup) => {
  loading.value = true
  
  // API 호출 함수
  const { data, cancel } = await fetchData(id.value)
  
  onCleanup(() => {
    cancel() // 진행 중인 요청 취소
    loading.value = false
  })
  
  // 데이터 처리
})
```
**시나리오 설명:**
1. id가 1일 때 API 요청 시작
2. 요청이 완료되기 전에 사용자가 id를 2로 변경
3. onCleanup이 실행되어 이전 요청(id=1)을 취소
4. 새로운 요청(id=2)이 시작

> #### 이런 취소 메커니즘이 필요한 이유?
**빠른 연속 요청의 문제점 예:**
+ 사용자가 빠르게 여러 번 검색어를 입력하는 경우
+ 페이지네이션에서 빠르게 여러 페이지를 클릭하는 경우
+ 드롭다운에서 연속적으로 다른 옵션을 선택하는 경우 <br>

**이런 상황에서 이전 요청을 취소하지 않으면:**
+ 불필요한 네트워크 트래픽 발생
+ 서버 부하 증가
+ 응답 순서가 보장되지 않아 오래된 데이터가 최신 데이터를 덮어쓸 수 있음

## 실무예시

### 1. 자동 저장 기능
```js
const message = ref('');
watchEffect(() => {
  saveToDatabase(message.value);
});
```

### 2. API 데이터 동기화
```js
const page = ref(1);
const items = ref([]);

watchEffect(async () => {
  const response = await fetch(`/api/items?page=${page.value}`);
  items.value = await response.json();
});
```

### 3. 폼 유효성 검증
```js
const email = ref('');
const isValid = ref(false);

watchEffect(() => {
  isValid.value = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.value);
});
```
