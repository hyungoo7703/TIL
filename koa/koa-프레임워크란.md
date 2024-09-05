# Koa 프레임워크 간단 요약

<b>Koa</b>는 Node.js를 위한 경량화되고 표현력 있는 웹 프레임워크이다.

## 주요 특징

- 경량화: koa는 핵심 기능만을 포함하고 있기 때문에 매우 가볍다. 필요한 기능을 미들웨어를 통해 추가할 수도 있다.
- 비동기 처리: async/await를 활용한 비동기 처리를 지원하기에 사용에 편리하다.
- 컨텍스트 객체: 각 요청에 대해 컨텍스트 객체를 생성한다. 이 객체는 request와 response 객체를 포함한다.
- 에러 처리: try-catch를 사용하여 중앙 집중식 에러 처리도 지원한다.

## 기본 사용 예시

```js
const Koa = require('koa');
const app = new Koa();

// 로깅 미들웨어
app.use(async (ctx, next) => {
  const start = Date.now();
  await next();
  const ms = Date.now() - start;
  console.log(`${ctx.method} ${ctx.url} - ${ms}ms`);
});

// 응답 미들웨어
app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```
