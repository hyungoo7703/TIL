# 배열
배열은 동일한 타입의 데이터를 연속된 메모리 공간에 저장하는 자료구조 <br>
각 데이터는 인덱스를 통해 접근할 수 있으며, 인덱스는 0부터 시작

## 특징
+ **고정 크기**: 한번 생성된 배열의 크기는 변경할 수 없다.
+ **동일 데이터 타입**: 하나의 배열에는 같은 데이터 타입만 저장할 수 있다.
+ **빠른 접근**: 인덱스를 통해 빠르게 데이터에 접근할 수 있다.

## 배열 선언 방법

### 1. 초기화와 함께 선언
```java
int[] numbers = {1, 2, 3, 4, 5};
```

### 2. 선언 후 초기화
```java
int[] numbers;
numbers = new int[] {1, 2, 3, 4, 5};
```

### 3. 크기만 지정하여 선언
```java
int[] numbers = new int[5];
```

## 배열 조작(요소 접근과 수정)
```java
int[] scores = new int[3];
scores[0] = 83;  // 첫 번째 요소에 값 저장
scores[1] = 90;  // 두 번째 요소에 값 저장
scores[2] = 75;  // 세 번째 요소에 값 저장[6]
```

## 배열 정렬
```java
int[] numbers = {5, 2, 8, 3, 1};
Arrays.sort(numbers); // 오름차순 정렬

String[] names = {"John", "Alice", "Bob"};
Arrays.sort(names, Collections.reverseOrder()); // 내림차순 정렬[1]
```
