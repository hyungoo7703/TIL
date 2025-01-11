# computed VS watch

## Computed
+ 기존 데이터를 기반으로 새로운 데이터 값을 생성하는 용도
+ 캐싱 기능이 있어 의존하는 데이터가 변경될 때만 재계산
+ 동기적으로 작동하며 순수 계산 로직에 적합
+ 반드시 값을 반환해야 함

## Watch
+ 특정 데이터의 변경을 감시하고 부수 효과(side effect)를 실행하는 용도
+ API 호출, 라우팅 변경 등 비동기 작업에 적합
+ 실제 데이터 변경이 일어날 때만 실행
+ 이전 값과 새로운 값에 접근 가능