# nextTick
nextTick은 Vue의 중요한 비동기 업데이트 메커니즘이다.

## 기본 개념
+ DOM 업데이트가 이루어지기를 기다렸다가 특정 코드를 실행하게 해주는 Vue의 기능
+ Vue의 반응형 시스템에서 데이터 변경 후 DOM이 실제로 업데이트되는 시점을 보장

## 작동 방식
1. Vue는 데이터 변경을 감지하면 가상 DOM을 업데이트
2. 실제 DOM 업데이트는 다음 "tick"까지 지연됨
3. nextTick은 이 DOM 업데이트가 완료된 후 콜백을 실행

## 사용이 필요한 경우
+ 데이터를 변경한 직후 변경된 DOM에 접근해야 할 때
+ computed나 watch의 변경이 DOM에 반영된 후 작업이 필요할 때
+ v-for로 렌더링된 목록이 업데이트된 후 DOM 조작이 필요할 때

## Vue2 사용법
```js
// 방법 1: 콜백 함수 사용
this.$nextTick(() => {
  // DOM 업데이트 완료 후 실행될 코드
})

// 방법 2: Promise 사용
async mounted() {
  await this.$nextTick()
  // DOM 업데이트 완료 후 실행될 코드
}
```

## Vue3 사용법
```js
import { nextTick } from 'vue'

// 방법 1: 콜백 함수 사용
nextTick(() => {
  // DOM 업데이트 완료 후 실행될 코드
})

// 방법 2: Promise 사용
await nextTick()
// DOM 업데이트 완료 후 실행될 코드
```

## 실무 사용예시

### 1. 데이터 변경 후 DOM 측정
```ts
const updateLayout = async () => {
  // 데이터 변경
  participants.value.push(newParticipant)
  
  // DOM 업데이트 대기
  await nextTick()
  
  // 업데이트된 DOM 요소의 높이 측정
  const height = participantsList.value?.offsetHeight
}
```

### 2. 폼 입력 후 포커스 설정
```ts
const addNewInput = async () => {
  // 새 입력 필드 추가
  inputFields.value.push({ id: Date.now(), value: '' })
  
  // DOM 업데이트 대기
  await nextTick()
  
  // 새로 추가된 입력 필드에 포커스
  const lastInput = document.querySelector('.input-field:last-child')
  lastInput?.focus()
}
```

### 3. 동적 컨텐츠 렌더링 후 스크롤
```ts
const appendMessage = async (message: string) => {
  // 메시지 추가
  messages.value.push(message)
  
  // DOM 업데이트 대기
  await nextTick()
  
  // 스크롤을 최하단으로 이동
  const chatContainer = document.querySelector('.chat-container')
  chatContainer?.scrollTo(0, chatContainer.scrollHeight)
}
```
