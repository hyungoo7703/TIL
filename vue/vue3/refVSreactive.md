# ref vs reactive
ref와 reactive는 반응형 상태를 만드는 두 가지 주요 방식으로써 <br>
Composition API의 일부인 반응형(Reactivity) API이다.

## ref
+ 모든 타입의 단일 값을 반응형으로 만들 수 있다.
+ .value를 통해 값에 접근해야 한다.
```js
const count = ref(0)
console.log(count.value) // 0
count.value++ 
```

## reactive
+ 객체 타입만 사용 가능하며, 직접 속성에 접근 가능하다.
```js
const state = reactive({
  count: 0,
  name: 'John'
})
console.log(state.count) // 0
state.count++
```

## 주요 차이점

### 사용 케이스
+ ref: 원시 타입이나 단일 값을 다룰 때 적합
+ reactive: 복잡한 객체나 중첩된 데이터 구조를 다룰 때 적합

### 성능 특성
+ ref: 원시 타입에 대해 더 나은 성능 제공
+ reactive: 복잡한 객체에 대해 더 효율적

### 권장 사항
단순한 값은 ref를 사용하는게 좋으며, 관련된 데이터를 그룹화할 때는 reactive를 사용하는 것이 좋다. <br>
전체 객체를 교체해야 하는 경우 ref를 사용하는데, 만약 중첩된 반응성이 필요한 경우 reactive를 사용한다.
