# ArrayList와 LinkedList 

## ArrayList 특징
+ 배열을 기반으로 한 자료구조로 연속된 메모리 공간에 데이터 저장
+ 초기 용량(capacity)은 10이며, 데이터가 추가되면 자동으로 크기가 조정됨
+ 인덱스를 통한 빠른 접근(random access) 가능
+ 중간 데이터 삽입/삭제 시 요소들을 이동시켜야 하므로 비효율적

## LinkedList 특징
+ 노드(객체)들이 참조로 연결된 자료구조
+ 각 노드는 데이터와 다음 노드의 주소를 가짐
+ 메모리 공간이 연속적이지 않음
+ 데이터 삽입/삭제 시 노드의 참조만 변경하면 되어 효율적

## 성능 비교
<table>
  <tr>
    <th>작업</th>
    <th>ArrayList</th>
    <th>LinkedList</th>
  </tr>
  <tr>
    <td>접근 속도</td>
    <td>O(1) - 빠름</td>
    <td>O(n) - 느림</td>
  </tr>
  <tr>
    <td>삽입/삭제</td>
    <td>O(n) - 느림</td>
    <td>O(1) - 빠름</td>
  </tr>
  <tr>
    <td>메모리 사용</td>
    <td>효율적</td>
    <td>참조 저장으로 추가 공간 필요</td>
  </tr>
</table>

## 사용법

### ArrayList 기본 사용
```java
// 선언
ArrayList<String> list = new ArrayList<>();
ArrayList<Integer> list2 = new ArrayList<>(20); // 초기 용량 지정

// 데이터 추가
list.add("값1");
list.add(1, "값2"); // 인덱스 지정 추가

// 데이터 접근
String value = list.get(0);
```

### LinkedList 기본 사용
```java
// 선언
LinkedList<String> list = new LinkedList<>();

// 데이터 추가
list.addFirst("첫번째"); // 맨 앞 추가
list.addLast("마지막");  // 맨 뒤 추가
list.add(1, "중간");    // 인덱스 지정 추가

// 데이터 삭제
list.removeFirst();
list.removeLast();
```
