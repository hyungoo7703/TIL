# TIL (Today I Learned)
모든 내용을 다 정리 할 수 없으니, 내가 잘 사용 할 수 있도록 이해 및 정리

## JavaScript
+ 핵심 개념
	+ [비동기 프로그래밍](javascript/core/async-programming.md)
	+ [클로저](javascript/core/closure.md)
	+ [이벤트 루프](javascript/core/event-loop.md)
	+ [호이스팅](javascript/core/hoisting.md)
	+ [프로토타입](javascript/core/prototype.md)
	+ [this 바인딩](javascript/core/this-binding.md)
+ DOM 관련
	+ [이벤트 핸들링](javascript/dom/event-handling.md)
	+ [DOM 조작 메서드](javascript/dom/manipulation.md)
	+ [querySelector 선택자](javascript/dom/querySelector.md)
+ 내장 객체와 메서드
	+ [객체(Object) 내장함수](javascript/built-ins/object.md)
  	+ [문자열(String) 내장함수](javascript/built-ins/string.md)
	+ 배열 관련
		+ [기본 배열 메서드](javascript/built-ins/array/basics.md)
		+ [reduce](javascript/built-ins/array/reduce.md)
	+ [기타 유용한 내장함수](javascript/built-ins/others.md)
+ 데이터 시각화
	+ [Canvas API 심화](javascript/visualization/canvas.md)
	+ WebGL
+ 데이터 처리관점에서의 JavaScript
 	+ TypedArray와 ArrayBuffer
	+ Web Workers
	+ 다른 기술과의 연동
		+ WebAssembly 연동
		+ WebGL 연동
 
## JavaScript(algorithm)
> #### 정리 및 학습리포지토리 ☞ [algorithm-js](https://github.com/hyungoo7703/algorithm-js)

## TypeScript
+ 기초
	+ [JavaScript와의 차이점](typescript/basic/differences-from-javascript.md)
	+ 인터페이스와 타입 별칭
 	+ [타입 시스템](typescript/basic/type-system.md)
+ 고급 기능
	+ [Narrowing](typescript/advanced/narrowing.md)
		+ [Truthiness narrowing](typescript/advanced/narrowing/truthiness-narrowing.md)
		+ 타입 가드
	+ 제네릭
	+ 유틸리티 타입
+ 실무 활용
	+ [컴파일러 옵션](typescript/for-use/compiler-option.md)
	+ 타입 선언 파일

## Vue
+ 기본 구성
	+ [Vue 프로젝트 생성 및 구조](vue/vue-프로젝트-생성-및-구조.md)
	+ [Vue 라이프 사이클 속성](vue/vue-라이프-사이클-속성.md)
		+ [라이프사이클 훅 실무적 관점](vue/라이프사이클-훅-실무적-관점.md)
  	+ 반응성(Reactivity) 시스템
  		+ [nextTick](vue/nextTick.md)
  		+ [computed vs watch](vue/computedVSwatch.md)
  		+ Vue3
  	 		+ [ref vs reactive](vue/vue3/refVSreactive.md)
  	   		+ [watchEffect](vue/vue3/watchEffect.md)
		+ [반응성 함정과 해결방법](vue/반응성-함정과-해결방법.md)
+ 실무 최적화
	+ [팝업에서의 데이터 전송(실무적 관점)](vue/팝업에서의-데이터-전송.md)
	+ [컴포넌트의 효율적 관리](vue/컴포넌트의-효율적-관리.md)
	+ 상태 관리(Vuex/Pinia)
	+ 성능 최적화

## React

## Next.js
+ [Next.js 프레임워크란?](next/next-js-프레임워크란.md)

## Node.js
+ [Crypto 모듈](nodejs/Crypto-모듈.md)

## Koa
+ [Koa 프레임워크란?](koa/koa-프레임워크란.md)
+ [미들웨어(middleware)](koa/middleware.md)
+ 라우팅
+ 에러 처리
+ 상태 관리
+ 파일 업로드
+ [데이터베이스 연동](koa/데이터베이스-연동.md)
+ 세션/쿠키 관리
+ 보안 미들웨어
	+ CORS
	+ helmet
	+ rate limiting

## NestJS

## Java
+ Java
	+ [Java에서 스레드(Thread)다루기](java/java/Java에서-Thread다루기.md)
 	+ [빌더 패턴 파헤치기](java/java/빌더-패턴.md)
	+ [정규표현식(Regular Expression)](java/java/patternMatching.md) 
 	+ 컬렉션 프레임워크 
		+ [sort()](java/java/sort().md)
  		+ [List(ArrayList vs LinkedList)](java/java/list.md) 
  		+ [WeakHashMap](java/java/WeakHashMap.md)
  		+ [HashMap vs ConcurrentHashMap](java/java/HashMapVSConcurrentHashMap.md)
 	+ 자료구조 & 알고리즘
	  	+ 자료구조
   			+ [배열](csKnowledge/dataStructure/배열.md) 
			+ [스택 & 큐](csKnowledge/dataStructure/stack%26queue.md)
   			+ Tree 구조
			+ Hash 구조
		+ 기본 알고리즘
			+ [정렬](csKnowledge/algorithm/sorting.md)
   			+ BFS/DFS 
			+ [완전탐색](csKnowledge/algorithm/brute-force-search.md)
				+ [비트마스크](csKnowledge/algorithm/bruteForceSearch/bitmask.md)
				+ 재귀함수
					+ [순열, 조합](csKnowledge/algorithm/bruteForceSearch/recursiveFunction/permutaion%26combination.md)
			+ Greedy Algorithm
			+ dynamic programming(동적계획법)
+ Java 버전별 특징
	+ Java 8
		+ [람다 표현식](java/java8/람다-표현식.md)
		+ [스트림 API](java/java8/스트림-API.md)
  		+ [날짜와 시간 클래스](java/java8/날짜와-시간-클래스.md)
  		+ [Optional](java/java8/optional.md)
	+ Java 11
		+ [새롭게 추가된 메서드](java/java11/새롭게-추가된-메서드.md)
  	+ Java 17
  		+ Record
		+ Sealed Classes
		+ Pattern Matching

## Kotlin
+ 기본 문법과 특징
+ Java와의 상호운용성
	+ Java 코드와의 통합
	+ Java에서 Kotlin으로의 점진적 마이그레이션
	+ 기존 Java 라이브러리 활용
+ 코루틴(Coroutines)을 이용한 비동기 프로그래밍
+ 컬렉션 API 활용
+ 함수형 프로그래밍 기법

## Spring
+ Spring Project
	+ [Spring Boot](spring/springBoot.md)
	+ [Spring Data](spring/springData.md)
		+ [Spring Data JDBC](spring/springData/springDataJdbc.md)
  		+ Spring Data JPA
  	+ Spring Security
  	+ Spring Batch
+ Spring으로 구현하기
	+ 파일 업로드/다운로드 구현 
	+ [JDBC Template CRUD (Feat: 인메모리 DB, Schema 및 Data 초기설정)](withForSpring/JDBC-Template-CRUD(Feat-인메모리-DB-Schema-및-Data-초기설정).md)
	+ [Spring 이용하여 REST 서비스 구축](withForSpring/스프링-이용하여-REST-서비스-구축.md)
	+ API 버전 관리
	+ [Spring 으로 구축한 REST 서비스에서 필드에러 처리](withForSpring/스프링으로-구축한-REST-서비스에서-필드에러-처리.md)
 	+ Exception Handling 
+ Spring 활용
	+ @Async
	+ [@Scheduler](withForSpring/@Scheduler.md)
	+ @EventListener
	+ @Profile과 환경설정
	+ @ConfigurationProperties
 	+ [JPA Auditing](withForSpring/JPA-Auditing.md) 
	+ Spring Actuator
	+ Spring Test 전략
	+ Spring Bean Lifecycle 
	+ [Spring에서 제공하는 기본 Formatter 어노테이션](withForSpring/스프링에서-제공하는-기본-Formatter-어노테이션.md)
	+ [Validation 사용를 위해 제공되는 어노테이션 정리](withForSpring/Validation-사용을-위해-제공되는-어노테이션-정리.md)
 	+ Logging 설정
	+ Transaction 관리

## SQL
+ SQL 개념
	+ [윈도우 함수](sql/윈도우-함수.md)
	+ [TOP N 쿼리](sql/TOP-N-쿼리.md)
   	+ 서브쿼리vs조인 
+ 실행 계획
	+ 실행 계획 읽는 방법
	+ DBMS별 실행 계획 차이점
	+ 비용(Cost) 분석
	+ 실행 계획 최적화 전략
+ 성능 튜닝
	+ 인덱스 설계와 관리

## Git
+ [Git 명령어 정리](git/git.md)

## Linux
+ [Linux 명령어 정리](linux/linux.md)
	
## 자격증
+ SQL[개발자]
	+ [과목1](qualifications/sqld/subject1.md)
 	+ [과목2](qualifications/sqld/subject2.md)
+ 리눅스마스터 2급
	+ [리눅스 운영 및 관리](qualifications/linuxm2/리눅스-운영-및-관리.md)
 	+ [리눅스 활용](qualifications/linuxm2/리눅스-활용.md)
+ 정보처리기사

## ETC
+ [캐싱](etc/caching/caching.md)
	+ 웹 캐시
	+ CDN
+ [REST API](etc/rest/rest-API.md)
	+ [URI 설계 규칙](etc/rest/URI-design-rules.md)
+ [윈도우 포트 종료하는 방법](etc/closing-windows-ports.md)
+ [DDD(Domain-Driven Design)](etc/ddd.md)
+ [HTTP 상태 코드](etc/HTTP-status-codes.md)
+ [프로토콜](etc/protocol.md)
+ [SSL](etc/ssl.md)

## ERROR
+ [쿼리 안티패턴 문제](error/쿼리-안티패턴-문제.md)
+ [node_modules 설치오류](error/node_modules-설치오류.md)
+ [@GeneratedValue 관련 주의사항(JPA)](error/@GeneratedValue-관련-주의사항.md)
+ 커넥션 풀 관련 이슈
+ OOM(Out Of Memory) 문제 해결
+ 동시성 문제 해결
