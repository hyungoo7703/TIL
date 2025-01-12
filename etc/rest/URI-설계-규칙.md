# URI 설계 규칙
URI 설계 규칙은 앞서 REST API에서 설명했던, 디자인 가이드보다 더 구체적이고 기술적인 규칙들을 포함해서 정리를 진행하려고 한다.

## 기본 규칙
> #### 슬래시(/) 사용
+ 계층 관계를 나타내는데 사용
+ URI 마지막에는 슬래시를 포함하지 않음

[잘못된/올바른 예시]
```text
❌ http://api.example.com/users/
✅ http://api.example.com/users
```

> #### 대소문자와 특수문자
+ URI는 소문자만 사용
+ 하이픈(-)을 사용하고 언더바(_)는 사용하지 않음
+ 긴 경로의 가독성을 위해 하이픈 사용

[잘못된/올바른 예시]
```text
❌ http://api.example.com/Users/userPosts
✅ http://api.example.com/users/user-posts
```

> #### 리소스 표현
+ 명사를 사용하고 동사는 피함
+ 복수형을 사용하여 일관성 유지
+ 리소스는 최소 하나의 URI로 식별되어야 함

## 기술적 규칙
> #### 파일 확장자
+ URI에 파일 확장자를 포함하지 않음
+ 대신 Content-Type 헤더 사용

[잘못된/올바른 예시]
```text
❌ http://api.example.com/users/123.json
✅ http://api.example.com/users/123
```

> #### URI 구조
+ URI = scheme "://" authority "/" path [ "?" query ] [ "#" fragment ]
+ 계층 구조를 명확히 표현
+ 예측 가능하고 일관된 구조 유지
