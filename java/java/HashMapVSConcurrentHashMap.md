# HashMap VS ConcurrentHashMap

## HashMap

### 기본 구조
+ Key-Value 쌍으로 데이터를 저장하는 자료구조(Map 인터페이스를 구현한 Key-Value 구조)
+ 내부적으로 해시 함수를 사용해 데이터를 저장
+ 하나의 null 키와 여러 null 값을 허용
```java
HashMap<String, String> map = new HashMap<>();
map.put(null, "null key value");   // null 키 허용
map.put("key", null);              // null 값 허용
```

### 성능
+ 단일 스레드 환경에서 삽입, 검색에 O(1)의 시간복잡도를 제공
+ 동기화되지 않아 멀티스레드 환경에서는 안전하지 않다

## ConcurrentHashMap

### 기본구조
+ Key-Value 쌍으로 데이터를 저장하는 자료구조(Map 인터페이스를 구현한 Key-Value 구조)
+ null 키와 null 값을 허용하지 않는다

### 동시성 제어
+ 내부적으로 동기화되어 스레드로부터 안전
+ 세그먼트 단위로 락을 나누어 관리하여 동시성을 향상

### 성능
+ 읽기 작업은 동기화하지 않아 성능이 향상
+ 수정 작업만 동기화되어 처리
+ 버킷 단위의 락을 사용해 동시성을 최적화

## 주요 차이점
![image](https://github.com/user-attachments/assets/4458192a-1258-4d54-a0e7-65896a2b5562)

## 동시성 문제
**HashMap은 동기화 처리가 전혀 되어있지 않아 여러 스레드가 동시에 접근할 경우 데이터 정합성이 깨질 수 있다.** <br>
한 스레드가 데이터를 순회하는 도중 다른 스레드가 수정하면 ConcurrentModificationException이 발생
```java
// 단일 스레드 환경
HashMap<String, Integer> map = new HashMap<>();
map.put("A", 1);
map.put("B", 2);

// 동시성 문제 발생 가능
Thread t1 = new Thread(() -> map.put("C", 3));
Thread t2 = new Thread(() -> map.put("C", 4));
```
ConcurrentHashMap은 앞서 말했 듯, 맵을 여러 세그먼트로 분할하여 관리하기에 <br>
기본적으로 16개의 세그먼트로 분할되어 있어 동시에 16개의 쓰기 작업이 가능하다. 동시성을 보장한다는 것.
```java
// 멀티 스레드 환경
ConcurrentHashMap<String, Integer> concurrentMap = new ConcurrentHashMap<>();
concurrentMap.put("A", 1);
concurrentMap.put("B", 2);

// 동시성 보장
Thread t1 = new Thread(() -> concurrentMap.put("C", 3));
Thread t2 = new Thread(() -> concurrentMap.put("C", 4));
```
