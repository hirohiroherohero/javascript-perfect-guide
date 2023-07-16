# 12장 이터레이터와 제너레이터

1. 제너레이터의 일차적인 설계 목적은 이터레이터를 쉽게 생성하는 것이다.

## 12.1 이터레이터의 동작 방법

1. 자바스크립트의 순회를 이해하기 위해서는 세 가지를 이해해야 한다.

   - 이터러블 객체
     - 배열, 세트, 맵 같은 순회할 수 있는 타입의 객체이다.
     - 이터레이터 객체를 반환하는 특별한 이터레이터 메서드를 가진 객체는 모두 이터러블 객체이다.
   - 순회를 수행하는 이터레이터 객체 자체
     - 순회 결과 객체를 반환하는 `next()` 메서드가 있는 객체는 모두 이터레이터 객체이다.
   - 각 순회 단계의 결과를 담은 순회 결과 객체
     - `value`와 `done` 프로퍼티가 있는 객체가 순회 결과 객체이다.

1. 순회방법

   - 이터러블 객체를 순회할 때는 먼저 이터레이터 메서드를 호출해 이터레이터 객체를 얻는다.
   - 그리고 이터레이터 객체의 next() 메서드를 반복적으로 호출하며 반환 값의 done 프로퍼티가 true일 때 까지 반복한다.

1. 난해한 부분은 이터러블 객체의 이터레이터 메서드는 일반적인 이름을 사용하는 것이 아닌 `Symbol.iterator`를 이름으로 사용한다는 것이다.
1. 내장된 이터러블 데이터 타입의 이터레이터 객체는 그 자체가 이터러블이다.

   - 즉, 자기 자신을 반환하는 `Symbol.iterator` 메서드를 갖는다.

   ```tsx
   const list = [1, 2, 3, 4, 5];
   const iter = list[Symbol.iterator]();
   const head = iter.next(); // 1
   const tail = [...iter]; // 2,3,4,5
   ```

## 12.2 이터러블 객체 만들기

1. 이터러블 객체는 아주 편리하므로, 어떤 형태로든 순회할 수 있는 데이터 타입이라면 이터러블로 만드는것을 고려해봐야한다.

1. 클래스

   - 클래스를 이터러블로 만들기 위해서는 반드시 이름이 `Symbol.iterator` 인 메서드를 만들어야 한다.
     - 이 메서드는 반드시 `next()` 메서드가 있는 이터레이터 객체를 반환해야 한다.
   - `next()` 메서드는 반드시 순회 결과 객체를 반환해야 하며 순회 결과 객체에는 `value` 프로퍼티와 불 `done` 프로퍼티 중 하나는 반드시 존재해야 한다.

   ```tsx
   class Range {
     // ...
     [Symbol.iterator]() {
       return {
         // next() 메서드가 이터레이터 객체의 핵심이다.
         // 반드시 순회 결과 객체를 반환해야 한다.
         next() {
           return { value: "value", done: true };
         },

         // 편의를 위해 이터레이터 자체를 이터러블로 만든다.
         [Symbol.iterator]() {
           return this;
         },
       };
     }
     //...
   }
   ```

1. 자바스크립트 배열의 `map()`과 `filter()` 메서드를 이터러블 기반으로 고치기

   ```tsx
   // f()를 소스 이터러블의 각 값에 호출한 결과를 순회하는 이터러블 객체를 반환한다.
   function map(iterable, f) {
     const iterator = iterable[Symbol.iterator]();

     return {
       // 이 객체는 이터레이터인 동시에 이터러블이다.
       [Symbol.iterator]() {
         return this;
       },
       next() {
         const v = iterator.next();
         if (v.done) {
           return v;
         } else {
           return { value: f(v.value) };
         }
       },
     };
   }

   // 지정된 이터러블을 필터링 한느 이터러블 객체를 반환한다.
   // 판별 함수가 true를 반환하는 요소만 순회한다.
   function filter(iterable, predicate) {
     const iterator = iterable[Symbol.iterator]();

     return {
       // 이 객체는 이터레이터인 동시에 이터러블이다.
       [Symbol.iterator]() {
         return this;
       },
       next() {
         for (;;) {
           const v = iterator.next();
           if (v.done || predicate(v.value)) {
             return v;
           }
         }
       },
     };
   }
   ```

1. 이터러블 객체와 이터레이터의 핵심 특징 중 하나는 이들이 본질적으로 lazy 하다는 것이다.

   - 다음 값을 얻기 위해 계산이 필요하다면 그 값이 실제로 필요할때까지 계산을 늦출 수 있다.
   - 예를 들어 아주 긴 문자열이 있고 이 문자열을 공백으로 구분된 단어로 토큰화 한다고 가정.
     - 단순히 split() 메서드를 사용할 수도 있지만 그렇게 하면 첫번째 단어만 사용하면 되는 경우에도 문자열 전체를 처리할 때 까지 기다려야 한다.
     - 또한 반환된 배열과 배열 내 문자열에 메모리를 아주 많이 할당해야 한다.
   - 다음은 문자열을 메모리에 한 번에 담지 않고 느긋하게 순회하는 함수다.

   ```tsx
   function words(s) {
     const r = /\s+|&/g;
     r.lastIndex = s.match(/[^ ]/).index;

     // 이터러블인 이터레이터 객체를 반환한다.
     return {
       // 이터러블이 된다.
       [Symbol.iterator]() {
         return this;
       },

       // 이터레이터가 된다.
       next() {
         const start = r.lastIndex;
         if (start < s.length) {
           const match = r.exec(s);
           if (match) {
             return { value: start.substring(start, match.index) };
           }
         }
         return { done: true };
       },
     };
   }
   ```

   ### 12.2.1 이터레이터 ‘종료’: return 메서드

   1. 이터레이터를 분해 할당과 함께 사용하면 `next()` 메서드는 지정된 변수 각각의 값을 얻을 수 있을 만큼만 호출된다.

      - 이터레이터가 반환할 수 있는 값이 훨씬 많더라도 필요 이상으로 호출되는 일은 없다.

   1. 가상의 파일 이터레이터가 끝까지 실행되는 일이 절대 없더라도 파일을 닫을 수 있는 방법이 있어야한다.
      - 이 때문에 이터레이터 객체에 `next()` 메서드 외에 `return()` 메서드가 사용되기도 한다.
      - `next()`가 `done` 프로퍼티가 `true`인 순회 결과를 반환하기 전에 순회를 마쳐야 한다면 인터프리터는 인터레이터 객체에 `return()`메서드가 있는지 확인한다.
      - `return()` 메서드가 존재한다면 인터프리터는 인자 없이 `return()` 메서드를 호풀해서 파일을 닫고 메모리를 반환하는 등의 정리 작업을 하게 된다.
      - `return()` 메서드는 반드시 순회 결과 객체를 반환해야 한다.

## 12.3 제너레이터

1. 제너레이터는 ES6의 강력한 새 문법을 사용한 일종의 이터레이터이다.

   - 제너레이터는 순회할 값이 데이터 구조의 요소가 아니라 계산결과 일때 특히 유용하다.

1. 제너레이터를 만들기 위해서는 반드시 먼저 제너레이터 함수를 정의해야 한다.

   - 제너레이터 함수는 문법적으로는 일반적인 자바스크립트 함수와 비슷하지만 `function` 대신 `function*` 키워드를 사용한다.

1. 제너레이터 함수를 호출하면 실제를 함수 바디를 실행하지는 않고 제너레이터 객체를 반환한다.

   - 이 제너레이터 객체는 이터레이터이다.
   - 제너레이터 객체의 `next()` 메서드를 호출하면 제너레이터 함수의 바디가 처음 또는 현재 위치에서 실행되면 `yield`문을 만나면 멈춘다.
     - `yield`는 ES6에서 처음 등장했으면 return 문과 비슷하다.
   - 이터레이터에서 `next()`를 호출하면 `yield` 문의 값을 반환한다.

   ```jsx
   // 한 자리 소수를 전달하는 제너레이터 함수
   // 이 함수를 호출해도 코드를 실행하지는 않는다.
   function* oneDigitPrimes() {
     yield 2;
     yield 3;
     yield 5;
     yield 7;
   }

   // 대신 제너레이터 객체를 반환한다.
   const primes = oneDigitPrimes();

   // 제너레이터는 전달받은 값을 순회하는 이터레이터 객체이다.
   primes.next().value; // 2
   primes.next().value; // 3
   primes.next().value; // 5
   primes.next().value; // 7
   primes.next().done; // true

   // 제너레이터에는 Symbol.iterator 메서드가 있다.
   primes[Symbol.iterator](); // 소수

   // 제너레이터는 다른 이터러블 타입처럼 사용할 수 있다.
   const array = [...oneDigitPrimes()]; // [2,3,5,7]
   let sum = 0;
   for (let prime of oneDigitPrimes()) {
     sum += prime;
   }

   sum; // 17
   ```

   - 제너레이터함수는 표현식 형태로 정의할 수 있다.
   - 클래스와 객체 리터럴에서 메서드를 정의할때 단축 프로퍼티 표기법을 쓸 수 있다.
   - 화살표 함수 문법으로는 제너레이터를 정의할 수 없다.

   ### 12.3.2 yield\*와 재귀 제너레이터

   1. `yield*`

      - `yield`와 비슷하지만 값 하나를 전달하는 것이 아니라 이터러블 객체를 순회하면서 각각의 값을 전달한다.

      ```jsx
      function* oneDigitPrimes() {
        yield 2;
        yield 3;
        yield 5;
        yield 7;
      }

      function* sequence(...iterables) {
        for (const iterable of iterables) {
          yield* iterable;
        }
      }

      console.log([...sequence("abc", oneDigitPrimes())]); // ['a', 'b', 'c', 2, 3, 5, 7]
      ```

      - `yield*`을 사용해 재귀 제너레이터를 만들수 있고, 재귀적으로 정의된 트리구조에 비재귀적 순회를 수행할 수 있다.

## 12.4 고급 제너레이터 기능

1. 제너레이터터는 기본적으로 계산을 잠시 멈추고 중간 값을 전달한 다음 계산을 재개할 수 있다는 특징이 있다.

   ### 12.4.1 제너레이터 함수의 반환 값

   1. 위에서 살펴본 제너레이터 함수에는 `return` 문이 없었고, 있다 하더라도 일찍 종료하기 위한 것일 뿐 값을 반환할 목적으로 쓰이지 않았다.

      - 하지만 제너레이터 함수도 다른 함수와 마찬가지로 값을 반환할 수 있다.

   1. 순회가 어떻게 이루어지는가?

      - `next()` 함수의 반환 값은 `value` 프로퍼티나 `done` 프로퍼티가 있는 객체이다.
        - 일반적인 이터레이터와 제너레이터에서는 `value` 프로퍼티가 있다면 `done` 프로퍼티는 정의되지 않거나 값이 `false`이다.
        - 그리고 `done`이 `true`이면 `value`는 정의되지 않았다.
      - 값을 반환하는 제너레이터에서 `next()`를 마지막으로 호출했을 때 반환하는 객체에는 `value`와 `done`이 모두 존재한다.
        - `value` 프로퍼티는 제너레이터 함수의 반환 값을 담고 있고 `done` 프로퍼티가 `true`이므로 순회할 값이 더는 남아 있지 않다.
      - `for/of` 루프와 분해 연산자는 이 값을 무시하지만 `next()`를 명시적으로 호출해서 순회하는 코드를 직접 만들 수 있다.
        ```jsx
        function* oneAndDone() {
          yield 1;
          return "done";
        }

        console.log([...oneAndDone()]);

        const generator = oneAndDone();
        console.log(generator.next()); // { value: 1, done: false }
        console.log(generator.next()); // { value: 'done', done: true }
        console.log(generator.next()); // { value: undefined, done: true }
        ```

      ### 12.4.2 yield 표현식의 값

      1. `yield` 는 표현식이라서 값을 가질 수 있다.

         - 제너레이터의 `next()` 메서드를 호출하면 제너레이터 함수는 `yield` 표현식을 만날 때 까지 실행된다.
         - `yield` 키워드 다음에 있는 표현식을 평가한 값이 `next()`의 반환 값이다.
         - 이 시점에서 제너레이터 함수는 실행을 즉시 멈춘다.
         - 제너레이터의 `next()` 메서드를 다음에 호출할 때 `next()`에 전달된 인자는 멈췄던 `yield` 표현식의 값이 된다.
         - 제너레이터는 `yield`로 호출자에게 값을 반환하며 호출자는 `next()`를 통해 제너레이터에 값을 전달한다.
         - 제너레이터와 호출자는 값과 실행 권한을 주고받는 별도의 실행 스트림이다.

         ```jsx
         function* smallNumbers() {
           console.log("next()가 처음 호출되었으며 인자는 무시됩니다.");
           const y1 = yield 1; // y1 === 'b'
           console.log(`next()가 두 번째로 호출됐으며 인자는 ${y1}입니다.`);
           const y2 = yield 2; // y2 === 'c'
           console.log(`next()가 두 번째로 호출됐으며 인자는 ${y2}입니다.`);
           const y3 = yield 3; // y3 === 'd'
           console.log(`next()가 두 번째로 호출됐으며 인자는 ${y3}입니다.`);
           return 4;
         }

         const g = smallNumbers();
         console.log("제너레이터가 생성됐습니다. 아직 실행된 코드는 없습니다.");
         const n1 = g.next("a"); // n1.value = 1;
         console.log(`제너레이터가 전달한 값은 ${n1.value}입니다.`);
         const n2 = g.next("b"); // n2.value = 2;
         console.log(`제너레이터가 전달한 값은 ${n2.value}입니다.`);
         const n3 = g.next("c"); // n3.value = 3;
         console.log(`제너레이터가 전달한 값은 ${n3.value}입니다.`);
         const n4 = g.next("d"); // n4 === {value: 4, done: true}
         console.log(`제너레이터는 ${n4.value}를 넘기고 종료됐습니다.`);
         ```

      ### 12.4.3 **제너레이터의 return()과 throw() 메서드**

      1. `next()`뿐만 아니라 `return()`과 `throw()` 메서드를 호출해서 제너레이터의 실행 흐름을 제어할 수 있다.

      1. 제너레이터에서는 `try/finally` 문을 통해 제너레이터가 종료될 때(`finally` 블록에서) `return()`을 사용하여 필요한 정리 작업을 수행하게 만들 수 있다.

         - `throw()`도 마찬가지로 임의의 신호를 예외의 형태로 제너레이터에 보내 예외 처리를 할 수 있다.

      1. 제너레이터가 `yield*`를 통해 다른 이터러블 객체에 값을 전달하면 제너레이터의 `next()` 메서드를 호출할 때 이터러블 객체의 `next()` 메서드가 호출된다.
         - `return()`과 `throw()` 메서드도 마찬가지이다.
         - 제너레이터가 `return()`과 `throw()` 메서드가 정의된 이터러블 객체에 `yield*`를 사용하면, 제너레이터에서 `return()`이나 `throw()`를 호출할 때 이터레이터의 `return()`이나 `throw()` 메서드가 이어서 호출된다.
