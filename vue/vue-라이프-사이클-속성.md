> 참고: Do it Vue.js 입문(삽입이미지 출처도 같음.)

# Vue 라이프 사이클 속성

뷰로 화면 개발을 위해선 필수적으로 생성해야되는 기본 단위가 존재하는데, 이를 뷰 인스턴스(instance)라고한다. <br>
뷰에서는 인스턴스 상태에 따라 호출 할 수 있는 라이프 사이클(life cycle) 속성이 존재한다. <br>
<b>인스턴스의 생성, 변경, 소멸과 관련되어 총 8개의 속성이 있다.</b> (아래 그림참조)

![lifecycle dcbe29f6](https://user-images.githubusercontent.com/93297109/198866673-f4a3afea-b10c-4eee-aa79-71e428238c6c.png)

그림에서 나타내는 것은 화면단에 인스턴스가 부착된 후 소멸되기까지의 과정을 다이어그램으로 표시한 것이다.

### 1. beforCreate

+ 인스턴스가 생성되고 가장 처음으로 실행되는 라이프 사이클이다. 
+ data나 methods 속성이 아직 인스턴에 정의되어 있진 않다.
+ 화면 요소에도 접근 할 수 없는 상태이다.

### 2. created

+ created 단계에선 data 속성과 methods 속성이 정의되므로 속성 정의된 값에 접근하는 로직 실행이 가능하다.
+ 인스턴스가 화면 요소에 부착되기 전이기 때문에 아직 화면 요소에 접근은 불가능하다.

### 3. beforeMount

+ 화면요소에 render() 호출 직전 단계이다.

### 4. mounted

+ 화면요소에 인스턴스가 부착되고 나면 호출되는 라이프 사이클이다.
+ 화면요소에 접근이 가능하다.

### 5. beforeUpdate

+ 데이터가 변경이 되면, 가상 돔으로 화면을 다시 그리기 전 호출되는 라이프 사이클이다.

### 6. updated

+ 데이터가 변경이 되고 가상 돔으로 화면을 다시 그리고나면 실행이 되는 라이프 사이클이다.

### 7. beforeDestroy

+ 뷰 인스턴스가 파괴되기 직전에 호출되는 단계이다.
+ 이 단계 까지는 아직 인스턴스에 접근이 가능하다.

### 8. destroyed

+ 뷰 인스턴스가 완전히 파괴되고 나서 호출되는 라이프 사이클이다.

## (추가) Vue3 라이프사이클 훅
vue3에서는 개발자의 편의에 따라 Options API나 Composition API를 선택하여 사용하기에, 라이프사이클 훅도 약간 다르다.

### Options API에서의 라이프사이클 훅
<b>update까지는 vue2와 동일</b>
### [변경점]
1. beforeDestroy → beforeUnmount: Vue 인스턴스가 제거되기 직전에 호출
2. destroyed → unmounted: Vue 인스턴스가 제거된 후 호출, 이 훅에서는 Vue 인스턴스의 모든 디렉티브가 바인딩 해제되고 이벤트 리스너가 제거됨

### Composition API에서의 라이프사이클 훅
<b>Composition API에서는 setup() 메서드 내에서 라이프사이클 훅을 사용</b>
+ setup(): beforeCreate와 created 훅 대신 사용
+ onBeforeMount: beforeMount 훅 대신 사용
+ onMounted: mounted 훅 대신 사용
+ onBeforeUpdate: beforeUpdate 훅 대신 사용
+ onUpdated: updated 훅 대신 사용
+ onBeforeUnmount: beforeUnmount 훅 대신 사용
+ onUnmounted: unmounted 훅 대신 사용
