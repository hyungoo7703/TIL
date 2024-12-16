# TIL (Today I Learned)
모든 내용을 다 정리 할 수 없으니, 내가 잘 사용 할 수 있도록 이해 및 정리

## JavaScript
+ 핵심 개념
	+ [프로토타입](javascript/프로토타입.md)
	+ [클로저](javascript/클로저.md)
	+ 비동기 프로그래밍 
	+ [이벤트 루프](javascript/이벤트-루프.md)
+ DOM 조작
	+ [querySelector(All)()](javascript/querySelector(All).md)
 	+ 이벤트 핸들링 
+ 기본 내장함수
	+ [객체(Object) 내장함수](javascript/객체-내장함수.md)
  	+ [문자열(String) 내장함수](javascript/문자열-내장함수.md)
	+ [배열 내장함수](javascript/배열-내장함수.md)
		+ [reduce](javascript/array/reduce.md)
	+ [기타 유용한 내장함수](javascript/기타-유용한-내장함수.md)
+ 최신 JavaScript
	+ Promise/async-await
	+ ES6+ 신규 기능
	+ 모듈 시스템

## TypeScript
+ 기초
	+ [JavaScript와의 차이점](typescript/javascript와의-차이점.md)
 	+ [타입 시스템](typescript/타입-시스템.md)
+ 고급 기능
	+ [Narrowing](typescript/Narrowing.md)
		+ [Truthiness narrowing](typescript/truthiness-narrowing.md)
	+ 제네릭
	+ 유틸리티 타입
+ 실무 활용
	+ [컴파일러 옵션](typescript/컴파일러-옵션.md)
	+ 타입 선언 파일

## Vue
+ 기본 구성
	+ [Vue 프로젝트 생성 및 구조](vue/vue-프로젝트-생성-및-구조.md)
	+ [Vue 라이프 사이클 속성](vue/vue-라이프-사이클-속성.md)
		+ [라이프사이클 훅 실무적 관점](vue/라이프사이클-훅-실무적-관점.md)
  	+ 반응성(Reactivity) 시스템
  		+ nextTick
  		+ computed vs watch 
  		+ Vue3
  	 		+ ref vs reactive
  	   		+ watchEffect  
		+ [반응성 함정과 해결방법](vue/반응성-함정과-해결방법.md)
+ 실무 최적화
	+ [팝업에서의 데이터 전송(실무적 관점)](vue/팝업에서의-데이터-전송.md)
	+ [컴포넌트의 효율적 관리](vue/컴포넌트의-효율적-관리.md)
	+ 상태 관리(Vuex/Pinia)
	+ 성능 최적화

## Node.js
+ [Crypto 모듈](nodejs/Crypto-모듈.md)

## Koa
+ [Koa 프레임워크란?](koa/koa-프레임워크란.md)
+ [미들웨어(middleware)](koa/middleware.md)
+ 라우팅
+ 에러 처리
+ 상태 관리
+ 파일 업로드
+ 데이터베이스 연동
+ 세션/쿠키 관리
+ 보안 미들웨어
	+ CORS
	+ helmet
	+ rate limiting

## Java
+ Java
	+ [Java에서 스레드(Thread)다루기](java/java/Java에서-Thread다루기.md)
 	+ [빌더 패턴 파헤치기](java/java/빌더-패턴.md)
	+ [정규표현식(Regular Expression)](java/java/patternMatching.md) 
 	+ 컬렉션 프레임워크 
		+ [sort()](java/java/sort().md)
  		+ [WeakHashMap](java/java/WeakHashMap.md)  
  		+ HashMap vs ConcurrentHashMap
 	+ 자료구조 & 알고리즘
	  	+ 자료구조
   			+ [배열](csKnowledge/dataStructure/배열.md) 
			+ [스택 & 큐](csKnowledge/dataStructure/stack%26queue.md)
   			+ Tree 구조
			+ Hash 구조
		+ 알고리즘
			+ [정렬](csKnowledge/algorithm/sorting.md)
			+ [완전탐색](csKnowledge/algorithm/brute-force-search.md)
				+ [비트마스크](csKnowledge/algorithm/bruteForceSearch/bitmask.md)
				+ 재귀함수
					+ [순열, 조합](csKnowledge/algorithm/bruteForceSearch/recursiveFunction/permutaion%26combination.md) 
+ Java 버전별 특징
	+ Java 8
		+ [람다 표현식](java/java8/람다-표현식.md)
		+ [스트림 API](java/java8/스트림-API.md)
  		+ 날짜와 시간 API
  		+ [Optional](java/java8/optional.md)
	+ Java 11
		+ [새롭게 추가된 메서드](java/java11/새롭게-추가된-메서드.md)
  	+ Java 17
  		+ Record
		+ Sealed Classes
		+ Pattern Matching

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
+ [SSL](etc/ssl.md)
+ [프로토콜](etc/프로토콜.md)
+ HTTP 상태 코드
+ [REST API](etc/restApi.md)
	+ URI 설계 규칙 
+ [윈도우 포트 종료하는 방법](etc/윈도우-포트-종료하는-방법.md)

## ERROR
+ [쿼리 안티패턴 문제](error/쿼리-안티패턴-문제.md)
+ [node_modules 설치오류](error/node_modules-설치오류.md)
+ [@GeneratedValue 관련 주의사항(JPA)](error/@GeneratedValue-관련-주의사항.md)
+ 커넥션 풀 관련 이슈
+ OOM(Out Of Memory) 문제 해결
+ 동시성 문제 해결
