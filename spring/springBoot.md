>참고: [spring.io](https://spring.io/)
# Spring Boot

Spring Framework 토대로 애플리케이션을 개발할 때 가속화 하기 위한 서브 프로젝트

## Spring Boot 장점

+  라이브러리 버전 관리, 의존성 관리 자동화 -> 의존성 추가 시 버전을 고려하지 않아도 된다.
+  내장 서버 Tomcat, JUnit 라이브러리를 통한 테스트 케이스 작성 등 편리한 기능 제공
+  매우 간편한 종속성 설정, 자동 수행 -> Spring Initializr 에서 매우 간편하게 프로젝트 생성이 가능하다.

## Spring Boot 기반 프로젝트 생성

Spring 에서는 Spring Initializr를 통해 프로젝트 생성 및 의존성 주입에서 엄청난 편의성을 제공해준다. <br>
아래 링크를 통해 들어가서, 쉽게 프로젝트 생성이 가능하다.

>Spring initializr: [https://start.spring.io](https://start.spring.io/)

## 내가 자주사용하는 Dependencies

+ Spring Boot DevTools <br>
빠른 애플리케이션 재시작, LiveReload 및 구성을 제공
+ Lombok <br>
반복되는 getter, setter 등의 메소드, 생성자 등의 상용구 코드를 줄여주는 라이브러리
+ Spring Web <br>
Spring MVC를 사용하여 RESTful을 포함한 웹 애플리케이션을 빌드, Apache Tomcat을 기본 내장 컨테이너로 사용
+ Spring Security

+ [Spring data JPA](springData/springDataJPA.md) <br>
Spring Data 및 Hibernate 그리고 Java Persistence API를 사용하여 SQL 저장소의 데이터 유지 목적
+ Validation <br>
Hibernate 유효성 검사기를 사용한 Bean 유효성 검사.
