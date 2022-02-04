>참고: [https://v3.vuejs.org/guide/installation.html#release-notes](https://v3.vuejs.org/guide/installation.html#release-notes) 

# Vue 프로젝트 생성 및 구조

Vue.js를 프로젝트에 추가하는 방법을 공식 문서에서는 4가지로 제시한다. <br>
그러나 Vue로 애플리케이션을 구축 할 때 수많은 패키지가 필요하고, 적용 해야 하기 때문에 권장 되는 설치 방식으로 <br>
Node.js 기반의 npm의 이용을 권장한다. <br>

이에 더하여 **Vue는 공식 빠른 싱글 페이지 어플리케이션 빌드를 위해 CLI를 제공하는데**, <br>
Vue 3의 경우 npm에서 @vue/cli로 제공되는 Vue CLI v4.5를 사용해야 한다. <br>
우선 터미널을 열고 아래 명령어를 입력한다.

```js
npm install -g @vue/cli // 최신 버전의 @vue/cli를 전역적으로 설치

/* 설치 완료 후 */
vue create example // example = 프로젝트명
```

create 명령어를 이용해 vue 프로젝트를 생성하려 할때 처음 보게 되는 것은 아래 이미지와 같은 설치 옵션이다.

![2-4(1)](https://user-images.githubusercontent.com/93297109/152459872-3d13924c-c35d-4725-b833-2814ecdfb862.png)

Vue 3를 기반으로 작성 하기에 **Default (Vue 3) 와 Manually**를 주목 하면된다. <br>
우선 Default는 말 그대로 기본 값으로 자동 생성이 되고, Manually는 옵션을 선택하여 프로젝트 생성이 가능하다.

### Default로 생성한 프로젝트

![2-4(2)](https://user-images.githubusercontent.com/93297109/152460423-a57490c6-f29b-4960-9496-fb623cd1fbe0.png)

Default 옵션 선택 시 별다른 조작 없이, 위와 같은 구조로 자동으로 생성된다. 그 구조를 알아보면 다음과 같다.<br>

+ node_modules: npm으로 설치되는 라이브러리들이 모여있는 폴더.
+ public: 정적 자원이 모여있는 폴더 ex) index.html
+ src
    + assets: 공용 css 파일이나 이미지 파일 등을 모아놓는 폴더
    + components: Vue 컴포넌트 파일이 모여있는 폴더
    + App.vue: Root 컴포넌트
    + main.js: 우리가 사용할 Vue 인스턴스를 생성 (가장 먼저 실행이 되는 js파일, 다양한 설정, Import 등의 작업을 해당 파일에서 수행)
+ .gitgnore
+ babel.config.js
+ package-lock.json: 설치된 패키지의 의존성 정보를 관리한다.
+ package.json: 프로젝트에 필요한 라이브러리를 정의하고 관리한다.
+ README.md

### Manually로 생성한 프로젝트

우선 Manually select features를 선택하게되면 아래와 같이 특징적 옵션들을 선택 할 수 있게 나온다.

![2-4(3)](https://user-images.githubusercontent.com/93297109/152460941-d04ba8b0-a04a-4b11-acf3-4b4615e3e7d8.png)

+ **Choose Vue versoin**: 2.x, 3.x 버전선택
+ **Babel**: Javascript 컴파일러 - 최신 버전 문법을 브라우저에서 사용하기 위함
+ TypeScript: TypeScipt 사용여부
+ Progressive Web App (PWA) Support: 프로그래시브 웹앱
+ **Router**: Vue 라우터 추가
+ **Vuex**: Vuex 추가 (Vue 상태 관리 패턴 + 라이브러리)
+ CSS Pre-processors: CSS 전처리기
+ **Linter / Formatter**: ESLint Formatters
+ Unit Testing: 단위 테스트 플러그인
+ E2E Testing: 통합 테스트 플러그인

내가 선택한 옵션은 굵게 작성 했다. 이후 세부적인 옵션들을 선택 해야 한다. <br>
그 최종은 아래와 같다. (물론 상황에 따라 만드는 방식은 다르다.)

![2-4(4)](https://user-images.githubusercontent.com/93297109/152462253-cb98a502-13cc-45d0-bd7a-612a4a5c672a.png)

모든 선택이 끝나고 생성 된 프로젝트의 모습은 아래와 같다. (표시 영역: Router와 Vuex 선택으로 인한 구조 차이)

![2-4(5)](https://user-images.githubusercontent.com/93297109/152463080-2341b477-9402-4b4e-bb97-43e8194ee6da.jpg)




