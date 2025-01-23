# 컴파일러 옵션
TypeScript 프로젝트에서 모듈 관련 컴파일러 옵션을 선택하는 방법은 중요하다. <br>
코드 실행 환경 최적화, 모듈 해석 방식 결정, 개발 생산성 향상 등 다양한 이유 때문인데, <br>
아래 참고 페이지에서 모듈 관련 컴파일러 옵션을 선택하는 방법을 알려주고 있다.
> 참고 [https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options.html](https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options.html)

### 요약

모듈 관련 컴파일러 선택 시 고려 사항
1. 프로젝트 유형에 따른 설정
    + 다양한 실행 환경(브라우저, Node.js, 웹 워커 등)에 맞는 최적의 모듈 시스템을 선택해야 한다.
    + 각 환경에서 글로벌 객체와 모듈이 원활하게 작동하도록 설정해야 한다.
2. 번들러 사용 시 설정
    + 번들러가 import/export 문을 제대로 이해하고 작동할 수 있도록 설정해야 한다.
    + 사용 중인 도구(예: 번들러나 Node.js)에 맞는 모듈 해석 방식을 선택해야 한다.
3. Node.js에서 컴파일 및 실행 시 설정
    + 다른 라이브러리나 프레임워크와 호환되도록 설정해야 한다.
    + CommonJS와 ES 모듈이 서로 잘 통합될 수 있게 관리해야 한다.
4. ts-node 또는 tsx 사용 시
    + 자동 완성과 타입 체크 등 IDE(통합 개발 환경) 기능을 최대한 활용하는 것을 권장한다.
    + 모듈을 임포트할 때 발생할 수 있는 오류를 컴파일 시점에 잡아내는 것이 좋다.
5. 브라우저용 ES 모듈 개발 시 (번들러 없이)
    + 번들러 없이 ES 모듈을 사용할 때 최적의 설정을 통해 빌드 과정을 효율적으로 만들어야 한다.
    + 불필요한 코드를 제거하고, 트리 쉐이킹을 통해 코드의 크기를 줄이는 것이 좋다.

## 모듈 시스템 예시

**ESModule 방식(ES6+ 방식)**
```ts
// math.ts
// math.ts - 내보내기 방식
// 1. Named Export (여러 개 내보내기)
export const add = (a: number, b: number): number => a + b;
export const subtract = (a: number, b: number): number => a - b;

// 2. Default Export (기본 내보내기)
const mathUtils = {
    add: (a: number, b: number): number => a + b,
    subtract: (a: number, b: number): number => a - b
};
export default mathUtils;

// main.ts - 가져오기 방식
// 1. Named Import
import { add, subtract } from './math';

// 2. Default Import
import mathUtils from './math';

// 3. 전체 Import
import * as MathModule from './math';
```

**CommonJS 방식(Node.js 전통 방식)**
```ts
// math.ts - 내보내기 방식
// 1. 개별 내보내기
exports.add = (a: number, b: number): number => a + b;
exports.subtract = (a: number, b: number): number => a - b;

// 2. 한번에 내보내기
const add = (a: number, b: number): number => a + b;
const subtract = (a: number, b: number): number => a - b;
module.exports = { add, subtract };

// main.ts - 가져오기 방식
// 1. 전체 가져오기
const math = require('./math');
math.add(5, 3);

// 2. 구조분해 할당
const { add, subtract } = require('./math');
```
