# Next.js 간단 요약

<b>Next.js</b>는 React를 기반으로 한 풀스택 웹 개발 프레임워크이다.

## 주요 특징

- Next.js는 Vercel이 2016년에 만든 오픈소스 프레임워크이다.
- React의 기능을 확장하여 서버 사이드 렌더링(SSR)과 정적 사이트 생성(SSG) 기능 제공한다.
- 프론트엔드와 백엔드 개발을 하나의 프로젝트에서 가능하게 한다.
- 자동 코드 분할로 성능 최적화나 파일 기반 라우팅 시스템 같은 개발 편의기능을 제공한다.

## 프로젝트 생성
create-next-app을 통해 생성할 수 있다.

```bash
# 프로젝트 생성
npx create-next-app@latest my-app

# 개발 서버 실행
cd my-app
npm run dev
```
생성과 실행도 React와 매우 유사한데, React 기반의 프레임워크라서 그렇다. <br>
그렇기에 Next.js의 코드 작성 방식은 React와 비슷하지만, <br>
Next.js는 더 많은 기능을 기본적으로 제공하여 개발자가 별도의 설정 없이 바로 애플리케이션을 개발하기에 최적화 되어 있다.

## Next.js vs React 핵심 차이점

### 렌더링 방식
- **React**: 클라이언트 사이드 렌더링(CSR) 기반
- **Next.js**: 서버 사이드 렌더링(SSR)과 정적 사이트 생성(SSG) 지원

> #### CSR, SSR, SSG?
+ CSR (Client Side Rendering)
    + 브라우저가 빈 HTML을 받고 JavaScript를 다운로드하여 클라이언트에서 렌더링
    + 초기 로딩 후 페이지 전환이 빠르며, JavaScript가 실행되기 전까지는 콘텐츠를 볼 수 없는 특징이 있다.
+ SSR (Server Side Rendering)
    + 서버에서 HTML을 생성하여 클라이언트에 전달
    + 매 요청마다 서버에서 새로운 페이지를 생성하기 때문에 JavaScript 실행 전에도 콘텐츠를 볼 수 있다.
+ SSG (Static Site Generation)
    + 빌드 시점에 HTML을 생성하여 저장
    + 요청시 미리 생성된 HTML을 제공하고 CDN을 통한 캐싱이 가능한 특징이 있다.

### 라우팅 시스템
+ **React**: react-router-dom 같은 추가 라이브러리 필요
+ **Next.js**: 파일 기반 라우팅 시스템 내장

### 사용 케이스
1. React 선택 시 <br>
단순한 단일 페이지 애플리케이션(SPA)이거나, UI 컴포넌트 중심 개발일 경우 채택하면 유리하다. <br> 
또한 높은 수준의 커스터마이징이 필요한 경우에도 권장된다.

2. Next.js 선택 시 <br>
SEO가 중요한 프로젝트 이거나, 서버 사이드 렌더링이나 정적 웹사이트 생성이 필요한 경우에 채택하면 된다. <br>







