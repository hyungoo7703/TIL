> 참고: [https://whitepro.tistory.com/396](https://whitepro.tistory.com/396?category=985210) // [https://develop-writing.tistory.com/106](https://develop-writing.tistory.com/106)
# 스프링에서 제공하는 기본 Formatter 어노테이션 정리

우리가 자주 사용하는 웹 환경을 생각 해 볼 때, <br>
숫자와 날짜를 생성 된 데이터 형식 그대로 사용하거나, 입력하지는 않는다. 또한 우리나라에서 표현하는 숫자의 단위와 같이 Ex) 1,000 <br>
실제 입력되는 정보와 데이터베이스에 저장되는 데이터 정보의 타입이 딱딱 맞는 경우는 거의 없다. <br>
지역 정보에 따라 다르게 입력되는 변환 타입을 하나하나 구현 하기엔 쉽지 않다. <br>

이에 대한 해결로써 **포맷터(Formatter)를 직접 구현 하거나, 제공되는 기본 어노테이션으로 간단하게 처리 할 수 있다.**

## 직접 구현 

1. Formatter<T> 인터페이스를 상속받아 메소드를 Override 하여 구현한다.

```java
public class MyNumberFormatter implements Formatter<Number> { 
	
    /* String을 Number로 바꿔주는 메소드 */
	@Override
	public Number parse(String text, Locale locale) throws ParseException { // text: 들어오는 문자(타입 변환 전), locale: 지역 정보(지역에 따라 변환되는 형식이 다를 수 있다.) 
		
		// 1,000 -> 1000
		return NumberFormat.getInstance(locale).parse(text); // java에서 지원하는 NumberFormat 객체의 parse 메소드 이용
    }
        
	/* Number Object를 String로 바꿔주는 메소드 */
	@Override
	public String print(Number object, Locale locale) { 
		
		// 1000 -> 1,000
		return NumberFormat.getInstance(locale).format(object); // NumberFormat 객체의 format 메소드 이용
	}
	
}
```

**참고사항** 

```java
// parse 메소드에 text = "1,000", locale = Locale.KOREA 를 넣어주고 1,000 -> 1000 으로 바꿔주는 상황
Number formatNumber = formatter.parse("1,000", Locale.KOREA);
```

위 결과 값으로 반환된 Number의 formatNumber는 구체적인 구현이 필요하기에 Long으로 구현된다. <br>
실제로 임의로 테스트를 실패해보면 아래와 같은 결과로서 알려준다.

![2-28](https://user-images.githubusercontent.com/93297109/155935761-38632ac4-b579-4d65-8247-298a48b56166.png)

2. 만든 MyNumberFormatter 클래스를 스프링 Bean으로 등록하여 사용한다. (WebMvcConfigurer의 addFormatters)

```java
@Configuration
public class Webconfig implements WebMvcConfigurer{

	@Override
	public void addFormatters(FormatterRegistry registry) {
		registry.addFormatter(new MyNumberFormatter());
	}
	
}
```

## @NumberFormat, @DateTimeFormat

객체의 각각 필드 마다 포맷터 형식을 주는 방법의 한계를 극복하기 위해 나온 방식이다. <br>
날짜나 숫자 형식에 대한 고민을 덜어준다.

```java
public class Form {

    @NumberFormat(pattern = "###,###")
    private Integer number;
 
    @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    private LocalDateTime localDateTime;
}
```

우리는 pattern에 지정한 양식으로 변환되기에 편하게 이용하면 된다.