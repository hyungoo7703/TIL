# WeakHashMap

## WeakHashMap이 필요한 경우?

'이펙티브 자바 아이템7: 다 쓴 객체 참조를 해제하라'를 공부하다 이런 말이 나왔다.

> 캐시 역시 메모리 누수를 일으키는 주범이다. <br>
> ...중략... <br>
> 엔트리가 살아 있는 캐시가 필요한 상황이라면, <b>WeakHashMap</b>을 사용해 캐시를 만들자 <br>
> 다 쓴 엔트리는 그 즉시 자동으로 제거될 것이다. 단 WeakHashMap은 이러한 상황에서만 유용하다.

우선 엔트리가 살아 있는 캐시? 라는 말이 무엇인지 잘 인지가 되지 않았다. <br>
그렇기에 먼저 엔트리, 더 나아가 겉핥기 식으로 알고 있는 캐시에 대해서도 알아 보았다. <br>
그리고 왜 엔트리가 살아 있는 캐시가 필요한 상황에는 WeakHashMap으로 구현하는게 좋은지 알아 보았다.

### 1. 엔트리(Entry)

캐시에서 엔트리(entry)란, 키와 값으로 구성된 하나의 데이터를 의미한다. <br>
캐시에서 엔트리가 "살아 있다"는 것은 해당 엔트리가 유효한 정보를 포함하고 있으며, 언제나 접근할 수 있음을 의미한다. <br>

### 2. 캐시(Cache)

<b>데이터나 값을 임시로 저장하는 장소</b>이다. <br>
데이터에 더 빠르게 엑세스하기 위해, 시스템 성능을 향상시키기 위해 사용되나, <br>
잘못된 설계, 올바르게 구현되지 않으면 잘못된 결과를 제공할 수도 있다. <br>

#### 캐시의 예시
+ 웹 브라우저 캐시 <br><br>
웹 브라우저는 사용자가 이전에 방문한 웹 페이지의 내용을 캐시에 저장. <br>
이를 통해, 같은 웹 페이지에 대한 반복적인 요청이 있을 때 서버에 요청을 보내지 않고 로컬 캐시에서 바로 가져올 수 있다. <br><br>
+ CPU 캐시 <br><br>
CPU 캐시는 CPU 내부에 위치하여 빠른 액세스를 제공한다. <br>
CPU는 자주 사용되는 명령어나 데이터를 캐시에 저장하여 더 빠르게 액세스할 수 있다. <br><br>
+ 데이터베이스 캐시 <br><br>
데이터베이스 시스템에서는 쿼리 결과를 캐시에 저장하여, 동일한 쿼리가 반복적으로 실행될 때마다 시스템 성능을 향상시킨다. <br>
이를 통해, 더 많은 요청을 더 빠르게 처리할 수 있습니다. <br><br>
+ 소프트웨어 캐시 <br><br>
소프트웨어에서는 자주 사용되는 데이터나 값을 메모리에 저장하여 빠르게 액세스할 수 있다. <br> 
예를 들어, 자주 사용되는 파일이나 이미지를 캐시에 저장하여 반복적으로 로드하지 않고 빠르게 로드할 수 있다. <br><br>

### 3. 가장 간단한 구현으로한 캐시, 또 이것의 문제 (by HashMap)

캐시 라이브러리를 사용하지 않을때, <br>
Java에서 캐시를 구현하는 방법은 여러 가지가 있지만, 가장 간단하게는 HashMap을 사용하여 구현할 수 있다.

```java
import java.util.HashMap;
import java.util.Map;

public class Cache {
    private static final Map<String, Object> cache = new HashMap<>();

    public static Object getCache(String key) { // 캐시값을 가져온다
        return cache.get(key);
    }

    public static void setCache(String key, Object value) { // 캐시를 저장한다.
        cache.put(key, value);
    }
}
```

이는 정말 간단한 구현이지만, <br>
캐시를 사용하지 않을때에도 캐시에서 제거되지 않고 계속 유지되는 문제가 발생한다. <br>
이를 방지하기 위해서는 캐시에 저장된 데이터의 유효기간을 설정하고, <br>
일정 기간이 지나면 자동으로 캐시에서 제거되도록 하는 로직이 추가되어야 한다 <br><br>
<b>그렇기에 로직을 추가해도 되지만, WeakHashMap으로 구현하면 좀더 간단해진다.</b> <br>
WeakHashMap은 HashMap과 유사하지만, <br>
그 키에 대한 참조가 GC(Garbage Collection)될 때 해당 항목을 자동으로 제거하는 기능을 제공한다. <br>
이는 키가 더 이상 사용되지 않을 때 캐시에서 해당 값을 자동으로 제거하여 메모리를 절약할 수 있도록 해준다.

### 4. 엔트리가 살아 있지 않은 경우는 왜 사용이 권장되지 않는가?

"엔트리가 죽은 캐시"는 캐시에서 저장된 데이터가 만료되어 더 이상 유효하지 않은 정보를 담고 있음을 의미한다. <br>
WeakHashMap에서 키는 WeakReference(약한 참조)로 저장되기 때문에, <br>
해당 키 객체가 가비지 컬렉터에 의해 제거되면 자동으로 WeakHashMap에서도 제거된다. <br>
이 때문에 WeakHashMap은 캐시에서 엔트리가 살아 있는지 여부를 판단하지 않고, <br>
키 객체가 살아 있는지 여부만 판단한다. <br>
<b>즉 객체를 참조하는 변수나 필드를 null로 만들어 참조가 존재하지 않는 상황을 만들면 제거되기에 적절하지 않다고 할 수 있다.</b>

-----

## WeakHashMap을 통해 캐시 구현 해보기

```java
import java.lang.ref.WeakReference;
import java.util.Map;
import java.util.WeakHashMap;

public class Cache {
    private static final Map<String, WeakReference<Object>> cache = new WeakHashMap<>(); // value가 약한참조로 저장

    public static Object getCache(String key) { 
        WeakReference<Object> reference = cache.get(key);
        return reference != null ? reference.get() : null; // 만약 해당 WeakReference 객체가 GC에 의해 수거되어 null 값을 반환한다면, 유효하지 않은 캐시 값을 가진 key가 사용됨
    }

    public static void setCache(String key, Object value) {
        cache.put(key, new WeakReference<>(value));
    }
}
```

