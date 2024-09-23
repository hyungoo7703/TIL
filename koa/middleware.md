# Koa 프레임워크 미들웨어란?
> Koa 미들웨어는 요청(request)과 응답(response) 사이클 도중에 실행되는 함수

+ app.use() 메서드를 통해 애플리케이션에 등록
+ 요청 객체(ctx.request), 응답 객체(ctx.response) 그리고 애플리케이션의 나머지 부분을 처리할 next() 함수에 접근
+ 요청과 응답을 가로채서 로직을 실행하고 next() 함수를 호출하여 다음 미들웨어로 처리를 넘기거나, 요청-응답 사이클을 종료할 수 있음

### 간단한 사용방법
```js
app.use(async (ctx, next) => {
  console.log('첫번째 미들웨어 시작');
  await next();
  console.log('첫번째 미들웨어 종료'); 
});

app.use(async (ctx, next) => {
  console.log('두번째 미들웨어 시작');
  await next();
  console.log('두번째 미들웨어 종료');
});
```

<b> Koa는 미들웨어 함수들을 순차적으로 실행하는 것이 특징. 
<br> 미들웨어는 등록된 순서대로 실행되며, 마지막 미들웨어까지 실행된 후 역순으로 실행이 종료 </b>

위 사용방법 코드를 실행할 경우 아래와 같은 순서로 진행된다.
1. 첫번째 미들웨어 시작
2. 두번째 미들웨어 시작
3. 두번째 미들웨어 종료
4. 첫번째 미들웨어 종료

### 미들웨어 함수의 구조
Koa 미들웨어 함수는 <b>두 가지 파라미터</b>를 받는다:
+ ctx - 웹 요청과 응답에 대한 정보를 담고있는 Context 객체
  >  Context 객체의 예시: ctx.request(요청객체), ctx.response(응답객체)
+ next - 다음 미들웨어 함수를 호출하는 함수. Promise를 반환함.
<br>
<b>미들웨어는 일반적으로 비동기(async)로 작성하는 것이 특징</b>, next()를 호출하여 다음 미들웨어로 처리를 넘긴다.
<br><br>

```js
//요청 처리 완료 시간과 시작 시간의 차이를 계산하여 ms 변수에 저장하는 예시
app.use(async (ctx, next) => {
  const start = Date.now();
  await next();
  const ms = Date.now() - start;
  console.log(`${ctx.method} ${ctx.url} - ${ms}ms`);
});
```

### 활용

+ 라우팅 - URL 경로에 따라 요청을 처리하는 라우터 미들웨어 (koa-router)
+ 로깅 - 요청과 응답을 로깅하는 미들웨어 (koa-logger)
+ 파싱 - 요청 바디를 파싱하는 미들웨어 (koa-bodyparser)
+ 에러 처리 - 에러를 캐치하고 처리하는 미들웨어
+ 정적 파일 제공 - 이미지나 CSS 파일 등의 정적 에셋을 제공하는 미들웨어 (koa-static)
+ 세션 관리 - 세션을 관리하고 인증을 처리하는 미들웨어 (koa-session, koa-jwt)
+ CORS 처리 - CORS를 처리하는 미들웨어 (@koa/cors)

### 실무 적용 사례

+ 에러 처리(404, 500 처리)
```js
//koa 404, 500 처리
app.use(async(ctx, next) => {
  try { // try/catch 블록으로 감싸져 있어 에러를 캐치할 수 있다.(기본 에러처리 바탕)
    await next()
  } catch (err) {
    ctx.status = err.status || 500
    if (ctx.status === 404) { // 에러 상태 코드(err.status)가 404인 경우와 그 외의 경우를 구분하여 처리
      ctx.body = {result: 'fail', message: 'Not Found'}
    } else {
      ctx.body = {result: 'fail', message: `Internal Server Error: ${err.message}`}
    }
  }

  if (ctx.status === 404) {
    ctx.body = {result: 'fail', message: 'Not Found'}
  }
})
```

+ 세션 관리(페이지 뷰 카운터)
```js
const session = require('koa-session');

// 세션 설정
const CONFIG = {
  key: 'koa.sess', // 쿠키 키 (default is koa.sess)
  maxAge: 86400000, // 세션 만료 시간 (ms)
  autoCommit: true, // 자동 커밋 여부
  overwrite: true, // 기존 세션 덮어쓰기 여부
  httpOnly: true, // httpOnly 설정 (default is true)
  signed: true, // 서명된 쿠키 사용 여부
  rolling: false, // 세션 갱신 여부
  renew: false, // 세션 갱신 시 만료 시간 초기화 여부
};

// 세션 미들웨어 적용(app.use(session(CONFIG, app))으로 세션 미들웨어를 적용)
app.use(session(CONFIG, app));

// 세션 테스트 라우트
app.use(ctx => {
  let n = ctx.session.views || 0;
  ctx.session.views = ++n;
  ctx.body = `${n} views`;
});
```

+ CORS 처리
```js
const cors = require('@koa/cors');

// CORS 설정
const corsOptions = {
  origin: '',// 허용할 도메인(실무에선 채워져 있음.)
  allowMethods: ['GET', 'POST', 'PUT', 'DELETE'], // 허용할 HTTP 메서드
  allowHeaders: ['Content-Type', 'Authorization'], // 허용할 헤더
  exposeHeaders: ['x-row-count'], //클라이언트 측 JavaScript 코드에서 접근할 수 있는 헤더를 지정
  credentials: true // 쿠키 허용 여부
};

// CORS 미들웨어 적용
app.use(cors(corsOptions));
```
