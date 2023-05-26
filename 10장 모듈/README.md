1. 모듈화 프로그래밍의 목표는 큰 프로그램을 코드 모듈로 분리해서 모듈 개발자가 예측하지 못한 상황에서도 코드 전체가 정확히 실행되도록 하는 것이다.
   - 실용적인 관점에서 `모듈화`는 프로그램의 세부 사항을 캡슐화하고 전역 네임 스페이스를 깔끔하게 유지해서 모듈이 다른 모듈의 변수, 함수, 클래스를 수정하는 사고를 막는것을 말한다.

## 10.2 노드 모듈

1. 노드 모듈은 `require()` 함수를 통해 다른 모듈을 가져오고, `Exports` 객체의 프로퍼티를 수정하거나 `module.exports` 객체 자체를 바꾸는 방법으로 공개 API를 내보낸다.

### 10.2.1 노드 내보내기

1. 노드의 전역 객체 `exports`는 항상 정의되어 있다.

   - 여러가지 값을 내보내는 노드 모듈을 만들 때 다음과 같이 이 객체의 프로퍼티로 할당하면 된다.

   ```jsx
   const sum = (x, y) => x + y;
   const square = (x) => x * y;

   exports.mean = (data) => data.reduce(sum) / data.length;
   exports.stddev = function (d) {
     let m = exports.mean(d);
     return Math.sqrt(
       d
         .map((x) => x - m)
         .map(square)
         .reduce(sum) /
         (d.length - 1)
     );
   };
   ```

   - 함수와 클래스로 구성된 객체를 내보내지 않고 함수나 클래스 하나만 내보낼때는 내보낼 값을 `module.exports`에 할당한다.

   ```jsx
   module.exports = class Bitset extends AbstractWritableSet {
     // 클래스 바디
   };
   ```

   - `module.exports`의 기본 값은 `exports`가 참조하는 것과 같은 객체이다.
   - 통계 모듈 같은 모듈에서는 함수를 하나씩 내보내기 보다는 다음과 같이 모듈 마지막에서 객체 하나로 내보내는 경우도 많다.

   ```jsx
   const sum = (x, y) => x + y;
   const square = (x) => x * y;

   const mean = (data) => data.reduce(sum) / data.length;
   const stddev = (d) => {
     let m = mean(d);
     return Math.sqrt(
       d
         .map((x) => x - m)
         .map(square)
         .reduce(sum) /
         (d.length - 1)
     );
   };

   // 공개할 것만 내보낸다.
   module.exports = { mean, stddev };
   ```

### 10.2.2 노드 가져오기

1. 노드 모듈은 `require()` 함수를 호출해 다른 모듈을 가져온다.

   - 이 함수의 인자는 가져올 모듈 이름이며 반환 값은 모듈이 내보내는 값이다.
   - 노드에 내장된 시스템 모듈이나 패키지 매니저로 설치한 모듈을 가져올 때는 `/`를 쓰지 않고 모듈 이름만 쓴다.
     - `/`를 쓰면 파일 시스템 경로로 바뀐다.

   ```jsx
   // 노드에 내장된 모듈.
   const fs = require("fs");
   const http = require("http");
   ```

## 10.3 ES6 모듈

1. ES6에서 `import`와 `export` 키워드를 자바스크립트에 추가하면서 마침내 언어 코어에서 모듈을 지원하기 시작했다.

   - ES6의 모듈성은 노드의 모듈성과 같은 개념이다.
   - 각 파일이 하나의 모듈이며 파일에서 정의한 상수, 변수, 함수, 클래스는 명시적으로 내보내지 않는 한 해당 모듈에서만 사용된다.
   - 모듈에서 값을 내보내면 다른 모듈에서 명시적으로 가져와 사용할 수 있다.
   - ES6 모듈의 문법은 노드 모듈과 내보내기/가져오기 문법에 차이가 있고 웹 브라우저에서 모듈을 정의하는 방법도 다르다.

1. ES6 모듈이 일반적인 자바스크립트의 ‘스크립트’와 중요한 차이가 있다.
   - 가장 명백한 차이는 모듈성 자체이다.
     - 일반적인 스크립트에서는 최상위에서 선언한 변수, 함수, 클래스는 모두 모든 스크립트가 공유하는 전역 컨텍스트에 들어간다.
     - 모듈에서는 각 파일에 비공개 컨텍스트가 있으며 `import`와 `export` 문을 사용할 수 있다.

### 10.3.1 ES6 내보내기

1. ES6 모듈에서 상수, 변수, 함수, 또는 클래스를 내보낼 때는 선언 앞에 `export` 키워드를 쓰면 된다.

   ```jsx
   export const PI = Math.PI;

   export function degressToRadians(d) {
     return (d * PI) / 180;
   }

   export class Circle {
     constructor(r) {
       this.r = r;
     }
     area() {
       return PI * this.r * this.r;
     }
   }
   ```

   - 평소처럼 `export` 문 없이 상수, 변수, 함수, 클래스를 정의하고 `export` 문을 하나만 써서 무엇을 내보낼지 정확히 선언하는 방법도 있다.
     ```jsx
     export { Circle, degressToRadians, PI };
     ```
     - 이 문법은 단축 프로퍼티 표기법을 사용한 객체 리터럴을 `export` 키워드 뒤에 붙인것처럼 보이지만, 여기서 쓴 중괄호가 객체 리터럴을 정의하지는 않는다.
     - 내보내기 문법에서 중괄호 안에 콤마로 구분된 식별자 리스트를 쓰도록 정했을 뿐이다.

1. 함수나 클래스 하나만 내보내는 모듈을 만드는 경우가 많은데 이럴 때는 `export default`를 사용한다.

   ```jsx
   export default class Bitset {
     // 클래스 바디
   }
   ```

   - `export`를 사용하는 일반 내보내기는 이름이 있는 선언에서만 사용할 수 있다.
   - `export default`를 사용하는 디폴트 내보내기는 익명 함수 표현식과 익명 클래스 표현식을 포함해 어떤 표현식이든 내보낼 수 있다.
     - 객체 리터럴도 내보내기 가능.
     - `export default`는 여러개 쓸 수 없다.

1. export 키워드는 자바스크립트 코드의 최상위 레벨에만 존재할 수 있다.
   - 클래스, 함수, 루프, 조건문 안에서 값을 내보낼 수는 없다.
     - 이는 ES6 모듈 시스템의 중요한 특징이며, 이 때문에 정적 분석이 가능하다.
   - 모듈은 항상 같은 값을 내보내며 내보낸 심벌은 모듈을 실제로 실행하기 전에 평가할 수 있다.

### 10.3.2 ES6 가져오기

1. 다른 모듈에서 내보낸 값을 import 키워드로 가져올 수 있다.

   - 디폴트 내보내기를 정의한 모듈에서 가져오는 것이 가장 단순하다.

   ```jsx
   import Bitset from "./bitset.js";
   ```

   - 지정된 모듈의 디폴트 내보내기 값이 현재 모듈의 식별자 값이다.
   - 가져온 값이 할당된 식별자는 const 키워드를 사용한 것처럼 상수로 선언된다.
   - 내보내기와 마찬가지로 가져오기 역시 모듈의 최상위 레벨에만 존재할 수 있으며 클래스, 함수, 루프, 조건문 안에는 존재할 수 없다.

1. 여러 값을 내보내는 모듈을 가져오기.

   ```jsx
   import { mean, stddev } from "./stats.js";
   ```

   - `default`를 사용하지 않는 내보내기에서는 내보내는 값에 이름이 있고, 가져오는 모듈에서는 그 이름으로 값을 참조한다.
   - 해당 모듈을 참조하는 `import` 문은 중괄호 안에 원하는 이름을 써서 원하는 값만 가져올 수 있다.

1. 여러가지를 내보내는 모듈에서 쉽게 전부 가져오기

   ```jsx
   import * as stats from "./stats.js";
   ```

   - 객체를 생성하고 stats라는 상수에 그 객체를 할당한다.

1. 모듈 지정자

   - `import` 문은 내보내기가 없는 모듈도 가져올 수 있다.

   ```jsx
   import "./analytics.js";
   ```

   - 처음 가져올 때 실행된다.
     - 이어지는 가져오기는 아무 일도 하지 않는다.
