# @Scheduler

일정한 시간, 간격으로 로직을 실행 시키기 위해 사용되는 어노테이션으로 <br>
스프링에선 매우 간단하게 사용이 가능하다.

## 사용 방법

스프링 부트 환경에서 스케쥴링 사용을 위해 Application 실행 파일에 @EnableScheduling을 추가해준다.

```java
@SpringBootApplication
@EnableScheduling // 추가
public class App {

	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
	
}
```

이제 원하는 로직 위헤 @Scheduler의 추가로 정말 간단히 사용 할 수 있다.

```java
@Scheduled()
public void examMethod() {
	
}
```

## cron 표현식

스케쥴링의 사용 방법은 매우 간단하지만, 특정 시간마다 동작하기 위해서는 cron 표현식으로 시간대를 나타내어야 한다. <br>
스프링에서는 **6필드**로 동작하며 **("초 분 시 일 월 요일")** 형태이다. <br>

따라서 사용 시 아래와 같은 형태가 되며

```java
@Scheduled(cron = "0 * * * * *") // 언제든 00초에 실행
```

이 표현식에 대해 자세히 알아보면 다음과 같다.

+ 초 : 0-59
+ 분 : 0-59
+ 시 : 0-23
+ 일 : 1-31
+ 월 : 1-12, JAN-DEC
+ 요일 : 1-7, SUN-SAT

+ &#42; : 모든 값
+ ? : 특정 값 없음 (일과 요일만 사용 가능)
+ , : 값 구분에 사용
+ &#45; : 범위 지정
+ / : 앞부터 뒤마다
+ L : 지정할 수 있는 범위의 마지막 값
+ W : 월~금 (가장 가까운 시기를 의미하기도 함) 
+ &#35; : 몇 번째 무슨 요일

위 표현식 규칙에 따라 몇가지 예를 들어보면 다음과 같다.

```java
// 매일 오후 3시 10분 0초 실행
@Scheduled(cron = "0 10 15 * * *")

// 10분 마다 실행
@Scheduled(cron = "0 0/10 * * * *")

// 12월 언제든 0초에 실행
@Scheduled(cron = "0 * * * DEC *")
```

cron 표현식은 직관적이지도 않고, 작성에 혼선을 줄 수도 있기에 아래와 같은 cron 표현식을 만들어 주는 사이트의 도음을 받아도 된다.

> cron 생성 사이트 [http://www.cronmaker.com](http://www.cronmaker.com)
