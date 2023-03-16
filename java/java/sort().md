# Sort()

Java에서 배열이나 컬렉션을 정렬할때, <br>
java.util 패키지의 Collections 클래스와 Arrays 클래스에서 제공하는 sort() 메서드를 사용할 수 있다. <br>

Heap sort와 같은 정렬 알고리즘을 사용하며 정렬해도 되나, <br>
sort() 메서드가 강력한 정렬 도구로써의 역할을 하기에 정리하고 잘 사용하려고 한다. <br>

## 1. Array

Arrays 클래스의 sort 메서드를 사용한다. (이때, 정렬은 유니코드를 기준으로 정렬되며 순서는 아래와 같다.)

+ 오름차순: 숫자 → 대문자 → 소문자 → 한글순
+ 내림차순: 한글 → 소문자 → 대문자 → 숫자순

### 사용 예

```java
  Integer[] arr = new Integer[5];
  Arrays.sort(arr); // 오름차순
  /* Collections.reverseOrder() String, Integer, Double 등과 같은 Object 타입에 사용가능 (기본 타입엔 박싱과정 필요) */
  Arrays.sort(arr, Collections.reverseOrder()); // 내림차순
```

> ***Arrays.sort() 메서드의 두 번째 인수?*** <br><br>
> Arrays.sort() 메서드는 두번째 인수로 Comparator 객체를 전달할 수 있다. <br>
> Collections.reverseOrder() 메서드는 Comparator 객체를 반환하는데 이 객체는 두 요소를 비교할 때 역순으로 비교한다.

## 2. List (ArrayList)

Collections 클래스의 sort 메서드를 사용한다. (Arrays 클래스의 sort와 마찬가지로, 정렬은 유니코드를 기준으로 정렬되며 순서는 아래와 같다.)

+ 오름차순: 숫자 → 대문자 → 소문자 → 한글순
+ 내림차순: 한글 → 소문자 → 대문자 → 숫자순

### 사용 예

```java
  List<String> list = new ArrayList<String>();
  Collections.sort(list); // 오름차순 정렬
  Collections.sort(list, Collections.reverseOrder()); // 내림차순 정렬
```

## 3. Set (HashSet)

Collections 클래스의 sort 메서드를 사용한다. <br>
Set 을 정렬하기 위해선, List 로 변환한 후 정렬한다.

### 사용 예

```java
  Set<String> set = new HashSet<String>();

  List<String> setToList = new ArrayList<String>(set);// list 변환
  Collections.sort(setToList);// 오름차순 정렬
  Collections.sort(setToList, Collections.reverseOrder());// 내림차순 정렬
```

## 4. Map (HashMap)

Collections 클래스의 sort 메서드를 사용한다. <br>
key를 기준으로 정렬하는 방법과 value를 기준으로 정렬하는 방법이 있는데, <br>
사실상 기준을 배열이나 리스트 형태로 바꾸고 정렬한다고 생각하면 된다.

### 사용 예

```java
  Map<String, Integer> map = new HashMap<String, Integer>();

  // key 를 기준으로 정렬 1
  Object[] sortByKeyArr = map.keySet().toArray();
  Arrays.sort(sortByKeyArr); // 오름차순
  Arrays.sort(sortByKeyArr, Collections.reverseOrder()); // 내림차순

  // key 를 기준으로 정렬 2
  List<String> sortByKeyList = new ArrayList<>(map.keySet()); // Map 의 Key 를 List 로
  Collections.sort(sortByKeyList); // 오름차순
  Collections.sort(sortByKeyList, Collections.reverseOrder()); // 내림차순

  // value 를 기준으로 정렬하기
  List<String> mapToList = new ArrayList<>(map.keySet()); // list 변환
  Collections.sort(mapToList, (key1, key2) -> map.get(key1).compareTo(map.get(key2))); // 오름차순
  Collections.sort(mapToList, (key1, key2) -> map.get(key2).compareTo(map.get(key1))); // 내림차순
```
