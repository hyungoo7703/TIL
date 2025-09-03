# 빌더 패턴

자바 빌더 패턴(Java Builder Pattern)은 객체 생성 과정을 단순화하고 가독성을 높이기 위한 디자인 패턴 중 하나이다. <br>
순서를 단순하게 나타내면 다음과 같다.

1. 객체 생성을 위해 빌더 클래스를 만든다.
2. 빌더 클래스에 필요한 메서드를 정의한다. (이 메서드들은 빌더 객체를 반환한다.)
3. 반환된 빌더 객체는 다시 다른 속성을 설정하는 메서드를 호출하거나, 최종적으로 build() 메서드를 호출하여 실제 객체를 생성한다.

## 간단한 예시

다음과 같이 Person 클래스가 있다.

```java
public class Person {
    private final String name;
    private final int age;
    private final String address;

    public Person(String name, int age, String address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }

    // getter
    // setter X
}
```

위 순서와 같이, 빌더 클래스를 정의해야 한다.

```java
public class PersonBuilder { // 1. 빌더 클래스를 만든다.
    private String name;
    private int age;
    private String address;

    /* 
    2. 빌더 클래스에서 필요한 메서드들을 정의한다. 
    반환 값은 PersonBuilder로 빌더 객체를 반환한다.
    */

    public PersonBuilder setName(String name) {
        this.name = name;
        return this;
    }

    public PersonBuilder setAge(int age) {
        this.age = age;
        return this;
    }

    public PersonBuilder setAddress(String address) {
        this.address = address;
        return this;
    }

    public Person build() { // 3. 최종적으로 build() 메서드를 호출하여 실제 객체를 생성한다.
        return new Person(name, age, address);
    }
}
```

실제 객체를 생성시는 다음과 같이 할 수 있다.

```java
Person person = new PersonBuilder()
    .setName("John")
    .setAge(30)
    .setAddress("123 Main St.")
    .build();
```

## 심화 내용 (by 이펙티브 자바 아이템2 : 생성자의 매개변수가 많다면 빌더를 고려해라)
>참고: 이펙티브 자바

책에서 소개할 때, 빌더 패턴을 이렇게 이야기 했다. <br>
<b>점층적 생성자 패턴의 안정성과 자바빈즈 패턴의 가독성을 겸비한 빌더 패턴(Builder Pattern)</b>이라 소개했다. <br>
빌더 패턴을 사용하기 위한 순서를 다음과 같이 이야기 했는데, <br>

1. 필요한 객체를 직접 만드는 대신 필수 매개변수로만으로 생성자(혹은 정적 팩토리 메서드)를 호출해 빌더 객체를 얻는다.
2. 빌더 객체들이 제공하는 일종의 setter를 이용해 매개변수를 설정한다.
3. build 메서드를 호출해 우리에게 필요한 객체를 얻는다.

위의 예시와 매우 유사하다. 그렇기에 예시 코드를 생략하고, 다음 내용에 집중했다. <br>
바로 <b>계층적으로 설계된 클래스와 함께 쓰기 좋다는 것이다.</b> <br>
그 예시 코드는 아래와 같다. <br>

#### 피자의 다양한 종류를 표현하는 계층구조의 루트에 놓인 추상클래스

```java
public abstract class Pizza{ // 인스턴스를 생성 할 수 없는 Pizza 클래스
   /* field */
   // Topping 이라는 이름의 enum 타입을 선언하고, 그 안에 HAM, MUSHROOM, ONION, PEPPER, SAUSAGE 5개의 상수를 정의한 것
   public enum Topping { HAM, MUSHROOM, ONION, PEPPER, SAUSAGE } 
   final Set<Topping> toppings;

   /* Pizza.Builder 추상클래스 */
   abstract static class Builder<T extends Builder<T>> { // 재귀적 타입 한정을 이용하는 제네릭 타입
      EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class); //Topping을 어느것도 포함하지 않는 EnumSet 생성

      public T addTopping(Topping topping) {
         toppings.add(Objects.requireNonNull(topping));
         return self();
      }

      abstract Pizza build();

      // 하위 클래스는 이 메서드를 재정의 하여 "this(하위 클래스의 Builder)를 반환 하도록 해야한다."
      protected abstract T self();
   }

   /* Piza 생성자 */
   Pizza(Builder<?> builder) { // Builder 안 Topping을 Set에 clone
      toppings = builder.toppings.clone();
   }
}
```

#### 크기(size)를 매개변수를 필수로 받는 NyPizza

```java
public class NyPizza extends Pizza {
   public enum Size { SMALL, MEDIUM, LARGE } // NyPizza의 고유 열거 타입
   private final Size size;

   public static class Builder extends Pizza.Builder<Builder> {
      private final Size size;

      public Builder(Size size) { // 사이즈를 정하고, NyPizza의 빌더를 생성
         this.size = Objects.requireNonNull(size);
      }

      @Override public NyPizza build() { // NyPizza(Builder builder) 반환
         return new NyPizza(this);
      }

      @Override protected Builder self() { return this; } 
   }

   private NyPizza(Builder builder) {
      super(builder);
      size = builder.size;
   }
}
```

#### 소스를 넣을지 선택하는 매개변수를 필수로 받는 CalzonePizza

```java
public class CalzonePizza extends Pizza {
   private final boolean sauceInside;

   public static class Builder extends Pizza.Builder<Builder> {
      private boolean sauceInside = false; // sauceInside의 기본값

      public Builder sauceInside() {
         sauceInside = true;
         return this;
      }

      @Override public CalzonePizza build() { // CalzonePizza(Builder builder) 반환
         return new CalzonePizza(this);
      }

      @Override protected Builder self() { return this; }
   }

   private CalzonePizza(Builder builder) {
      super(builder);
      sauceInside = builder.sauceInside;
   }
}
```

#### 사용할 때

```java
    NyPizza nyPizza = new NyPizza.Builder(Size.SMALL)
                .addTopping(Topping.SAUSAGE)
                .addTopping(Topping.ONION)
                .build();

    CalzonePizza calzonePizza = new CalzonePizza.Builder()
                .addTopping(Topping.HAM)
                .sauceInside()
                .build();
```

-----

위처럼 계층적 설계의 클래스에서 사용하기 좋고, 가변인수 매개변수를 여러개 사용할 수 있고, 또 유연하지만 <br>
장점만 있는 것은 아니다. <br>
객체 생성에 앞서 빌더부터 만들어야 하는 것도 단점이라 할 수도 있고, <br> 
또한 코드가 결코 쉽지 않은 것이 사실이고, 즉 이말은 코드가 장황하다는 것이기 때문에 <br>
책에서도 매개변수가 4개 이상은 되어야 값어치를 한다고 어필하긴 했다.

