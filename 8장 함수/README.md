1. 함수는 한 번 정의하면 몇 번이고 호출할 수 있는 자바스크립트 코드 블록이다.

    - 자바스크립트 함수는 매개변수화 된다.
        - 함수 정의에는 매개변수라고 불리는 식별자 리스트가 있는데, 이들은 함수 바디에서 로컬변수처럼 동작한다.
    - 함수를 호출할 때는 매개변수에 값을 전달하는데 이를 인자라고 한다.
        - 함수는 보통 인자를 사용해 반환 값을 도출하며, 이 값이 함수 호출 표현식의 값이 된다.
    - 매개변수 외에도 각 호출에는 호출 컨텍스트가 존재하며 이것이 `this` 키워드의 값이다.

1. 객체 프로퍼티로 할당된 함수를 객체의 메서드라고 부른다.
    - 객체를 통해 함수를 호출하면 그 객체가 호출 컨텍스트, 즉 함수의 `this` 값이다.
    - 객체를 새로 만들 목적으로 설계한 함수를 생성자라고 한다.
1. 자바스크립트 함수는 객체이며 프로그램에서 조작할 수 있다.

    - 자바스크립트는 함수를 변수에 할당하거나 다른 함수에 전달할 수 있다.
    - 함수는 객체이므로 프로퍼티를 정의할 수 있고 함수의 메서드를 호출하는 것도 가능하다.

1. 자바스크립트 함수는 다른 함수 안에서 정의할 수 있으며, 이렇게 정의된 함수는 자신의 정의된 스코프의 변수에 접근할 수 있다.
    - 이런 의미에서 자바스크립트함수는 클로저이다.
    - 클로저를 통해 중요하고 강력한 프로그래밍 기법을 사용할 수 있다.

## 8.1 함수정의

1. 자바스크립트 함수를 정의하는 가장 단순한 방법은 `function` 키워드이다.

    - 이 키워드 선언으로도, 표현식으로도 사용할 수 있다
    - ES6에서는 `function` 키워드 없이 함수를 정의하는 새로운 방법인 ‘화살표 함수’를 도입했다.
        - 이 문법은 아주 간결하며, 함수를 다른 함수에 인자로 전달할 때 특히 유용하다.

1. 객체 리터럴과 클래스 정의에는 메서드를 정의하는 단축 문법이 있다.

    - 이 단축 문법은 함수 정의 표현식을 사용하고 `name: value` 객체 리터럴 문법으로 그 표현식을 객체 프로퍼티에 할당하는 것과 동등하다.
    - 객체 리터럴에서 키워드 `get`과 `set`을 써서 프로퍼티 게터와 세터 메서드를 정의할 수도 있다.

1. `Function()` 생성자를 사용해 함수를 정의할 수도 있다.

### 8.1.1 함수 선언

1. 함수 선언은 `function` 키워드 뒤에 다음 세가지 구성요소를 쓴다.

    - 함수 이름이 될 식별자, 이름은 함수 선언에서 뺄 수 없는 부분이다.
        - 이 이름은 변수 이름으로 쓰이며, 새로 정의된 함수 객체가 이 변수에 할당된다.
    - 괄호로 감싸고 콤마로 구분한 0개 이상의 식별자 리스트, 이 식별자들은 함수 매개변수 이름이며 함수 바디 안에서 로컬 변수로 동작한다.
    - 중괄호로 감싼 0개 이상의 자바스크립트 문, 이 문이 함수 바디이며 함수를 호출 할 때마다 실행된다.

    ```jsx
    function add(x, y) {
        return x + y;
    }
    ```

1. 함수 선언에서 이해해야 할 중요한 점은 함수의 이름이 변수이며 그 값은 함수 자체라는 점이다.

    - 함수 선언문은 자신을 포함하는 스크립트, 함수, 또는 블록 맨 위로 끌어올려지므로 함수 선언문으로 정의한 함수는 정의하기 전에도 호출할 수 있다.
    - 블록 안에서 선언된 함수는 모두 그 블록 전체에 존재하며, 자바스크립트 인터프리터가 해당 블록의 코드를 실행하기 전에 정의된다고 봐도 된다.

1. ES6 스트릭트 모드에서는 블록 안에서 함수를 선언할 수 있다.
    - 이렇게 블록 안에서 정의된 함수는 해당 블록 안에서만 존재하며 블록 바깥에서는 볼 수 없다.

### 8.1.2 함수 표현식

1. 함수 표현식은 함수 선언과 거의 비슷하지만, 더 큰 표현식이나 문의 일부로서 존재하고 이름을 붙이지 않아도 된다는 점이 다르다.

    ```jsx
    const add = function (x, y) {
        return x + y;
    };
    ```

    - 함수 표현식은 변수를 선언하지 않는다.
        - 새로 정의한 함수 객체를 나중에 다시 참조해야 한다면, 프로그래머의 선택에 따라 변수 또는 상수에 할당한다.
    - 함수 표현식에 이름이 있으면, 로컬 함수 스코프에서 그 이름으로 함수 객체를 참조한다.
        - 즉, 함수 이름은 함수 안에서 로컬 변수가 된다.

1. 함수 선언으로 함수 f()를 정의하는 것과 표현식으로 함수를 생성하고 변수 f에 할당하는 것 사이에는 중요한 차이가 있다.
    - 선언 형태를 사용하면 함수 객체는 자신을 포함하는 코드가 실행되기 전에 존재하며 정의하기 전에 호출할 수 있다.
        - 표현식으로 정의된 함수는 이렇게 동작하지 않는다.
    - 함수를 정의하는 표현식이 실제로 평가되기 전에는 함수가 존재하지 않는다.
    - 또한 함수를 호출하려면 반드시 함수를 참조할 수 있어야 하는데, 표현식으로 정의된 함수는 변수에 할당하기 전에는 참조할 수 없다.
    - 따라서 표현식으로 정의된 함수는 정의하기 전에 호출할 수 없다.

### 8.1.3 화살표 함수

1. ES6 이후에는 화살표 함수라는 간결한 문법으로 함수를 정의할 수 있다.

    - 이 문법은 ‘⇒(화살표)’를 사용해 함수 매개변수와 함수 바디를 구분한다.
    - 화살표 함수는 문이 아니라 표현식이므로 `function` 키워드는 사용하지 않으며 함수 이름도 필요 없다.
    - 화살표 함수의 일반적인 형태는 괄호 안에 콤마로 구분한 매개변수 리스트를 쓰고, 그 뒤에 ⇒ 화살표와 중괄호로 감싼 함수 바디를 쓰는 형태이다.
    - 함수 바디가 `return`문 하나라면 `return` 키워드와 중괄호를 모두 생략하고 값을 반환하는 표현식 하나만으로 함수 바디를 구성할 수 있다.
    - 매개변수가 정확히 하나라면 매개변수를 감싼 괄호도 생략할수 있다.
        - 하지만 매개변수를 받지 않을 때는 반드시 빈 괄호를 써야 한다.
    - 또한 화살표 함수의 바디가 `return` 문 하나라고 해도, 반환할 표현식이 객체 리터럴이라면 객체 리터럴을 명시적으로 괄호 안에 써서 함수 바디의 중괄호와 객체 리터텉의 중괄호를 혼동하지 않게 해야 한다.

    ```jsx
    const add = (x, y) => {
        return x + y;
    };

    const add = (x, y) => x + y;

    const polynomial = (x) => x * x + 2 * x + 3;

    const f = (x) => {
        return { value: x };
    };

    const g = (x) => ({ value: x });
    ```

2. 화살표 함수는 문법이 간결하므로 함수를 다른 함수에 전달할 때 이상적이다.

    - `map(), filter(), reduce()` 같은 배열 메서들를 사용할 때

3. 다른 방법으로 정의된 함수는 자신만의 호출 컨텍스트를 정의하지만, 화살표 함수는 자신의 정의된 함수의 `this` 키워드 값을 상속한다는 결정적인 차이가 있다.
    - 이것은 화살표 함수에서 중요하고 아주 유용한 기능이다.
    - 화살표 함수는 `prototype` 프로퍼티가 없으므로 새로운 클래스의 생성자 함수로 사용할 수 없다.

### 8.1.4 중첩된 함수

1. 자바스크립트에서는 다른 함수 안에 함수를 중첩할 수 있다.

    ```jsx
    function hypotenuse(a, b) {
        function square(x) {
            return x * x;
        }
        return Math.sqrt(square(a) + square(b));
    }
    ```

    - 중첩된 함수에서 흥미로운 것은 변수 스코프 규칙이다.
        - 중첩된 함수는 자신을 포함하는 함수(들)의 매개변수와 변수에 접근할 수 있다.
        - 위 예제에서 내부 함수 `square()`는 외부 함수 `hypotenuse()`에서 정의된 매개변수 a와 b를 읽고 쓸 수 있다.

## 8.2 함수 호출

1. 함수 바디를 구성하는 자바스크립트 코드는 함수를 정의할 때가 아니라 호출할 때 실행된다.

    - 자바스크립트 함수는 다섯가지 방법으로 호출할 수 있다.
        - 함수로 호출
        - 메서드로 호출
        - 생성자로 호출
        - `call(), apply()` 메서드를 통해 간접적으로 호출
        - 자바스크립트 언어 기능을 통한 묵시적 호출

    ### 8.2.1 함수로 호출

    1. 함수는 호출 표현식을 통해 함수 또는 메서드로 호출된다.

        - 호출 표현식은 함수 객체로 평가되는 표현식 뒤에 콤마로 구분한 0개 이상의 인자 표현식 리스트를 괄호로 감싼 형태이다.
        - 함수 표현식이 프로퍼티 접근 표현식이라면 즉, 해당 함수가 객체 프로퍼티거나 배열 요소라면, 이 표현식은 메서드 호출 표현식이다.

        ```jsx
        printprops({ x: 1 });
        let total = distance(0, 0, 2, 1) + distance(2, 1, 3, 5);
        ㄹ;
        ```

        - 괄호 안에 들어 있는 각 인자 표현식을 호출 시점에서 평가한 값이 인자가 된다.
            - 함수 바디에서는 각 매개변수가 이에 대응하는 인자로 평가된다.

    2. 일반적인 함수 호출에서는 함수의 반환 값이 호출 표현식의 값이다.

        - `return`문을 만나지 않은 채 인터프리터가 함수의 끝에 도달하면 반환 값은 `undefined`이다.
        - 인터프리터가 `return`문을 실행해서 함수를 종료한다면 `return`문 다음에 있는 표현식의 값이 함수의 반환 값이며, `return`문에 값이 없다면 `undefined`가 함수의 반환값이다.

    3. 조건부 호출

        - ES2020에서는 함수 표현식과 여는 괄호 사이에 `?.`를 넣어서 함수가 `null`이나 `undefine`가 아닌 경우에만 호출하게 할 수 있다.
            - 즉 부수효과가 없다고 가정하면 표현식 `f?.(x)`는 다음 코드와 동등하다.
            ```jsx
            f !== null && f !== undefined ? f(x) : undefined;
            ```

    4. 일반 모드에서 함수의 호출 컨텍스트(`this`)는 전역 객체이다.

        - 스트릭트 모드의 호출 컨텍스트는 `undefined`이다.
            - 단 화살표 문법으로 정의한 함수는 항상 자신이 정의된 곳의 `this` 값을 상속한다.
        - 메서드가 아니라 함수로 호출되도록 설계된 함수는 일반적으로 `this` 키워드를 전혀 사용하지 않는다.

    5. 재귀함수와 스택
        - 트리기반 데이터 구조 같은 일부 알고리즘은 재귀 함수를 사용해 아주 명쾌하게 구현할 수 있다.
        - 하지만 재귀 함수를 만들 때는 메모리를 고려하는 것이 중요하다.
            - 함수 A가 함수 B를 호출하고 다시 함수 B가 함수 C를 호출하는 상황에서 자바스크립트 인터프리터는 세 함수의 실행 컨텍스트를 모두 추적해야 한다.
        - 실행컨텍스트를 스택이라고 생각하면 함수가 다른 함수를 호출하면 새 실행 컨텍스트를 스택에 추가하고, 실행을 마치면 그 실행 컨텍스트 객체를 스택에서 꺼낸다.
            - 콜 스택도 메모리를 사용한다.
        - 최신 하드웨어에서 일반적으로 재귀 함수가 자신을 수백 번 호출하는 정도는 문제가 되지 않는다.
            - 하지만 함수가 자기 자신을 수만 번 호출한다면 콜 스택 크기가 한도에도 도달했다는 에러가 난다.

    ### 8.2.2 메서드로 호출

    1. 메서드는 객체 프로퍼티로 저장된 자바스크립트 함수이다.

        ```jsx
        o.m(x, y);
        ```

        - 예제의 코드는 호출 표현식이며 함수 표현식 `o.m`과 인자 표현식 `x,y`가 들어 있다.
            - 함수 표현식이 프로퍼티 접근 표현식이므로 이 함수는 일반적인 함수가 아니라 메서드로 호출된다.

    1. 메서드 호출과 함수 호출의 차이

        - 메서드 호출과 함수 호출은 호출 컨텍스트가 다르다는 중요한 차이가 있다.
            - 프로퍼티 접근 표현식은 객체와 프로퍼티 이름으로 이루어진다.
        - 메서드 호출 표현식에서 객체는 호출 컨텍스트가 되고 함수 바디는 키워드 `this`를 통해 그 객체를 참조할 수 있다.

        ```jsx
        let calculator = {
            operand1: 1,
            operand2: 2,
            add() {
                // this 키워드는 포함하는 객체를 참조한다.
                this.result = this.operand1 + this.operand2;
            },
        };

        calculator.add();
        calculator.result; // 2
        ```

        - 메서드와 `this` 키워드는 객체 지향 프로그래밍 패러다임의 핵심이다.
            - 메서드로 사용되는 함수는 모두 자신을 호출하는 객체를 묵시적인 인자로 받는다.
            - 메서드는 일반적으로 그 객체에서 동작하며, 메서드 호출 문법은 함수가 객체에서 동작한다는 의미를 명쾌하게 전달한다.

    1. 메서드 체인

        - 메서드가 객체를 반환하면 그 반환 값에서 다시 메서드를 호출할 수 있다.
            - 메서드 호출을 ‘체인’으로 이어서 표현식 하나로 만들 수 있다.
        - 프로미스 기반 비동기 동작을 구성할 때 자주 사용된다.
            ```jsx
            doStepOne().then(doSteopTwo).then(doStepThree).catch(handleError);
            ```
        - 반환 값이 필요없는 메서드를 작성할 때는 메서드가 `this`를 반환하도록 해보자
            - API전체에서 일관되게 `this`를 반환한다면 메서드 체인이라는 프로그래밍 스타일을 따를 수 있다.

    1. `this`는 변수나 프로퍼티 이름이 아니라 키워드이다.

        - `this` 키워드는 변수의 스코프 규칙을 따르지 않는다.
            - 화살표 함수의 예외를 제외하면 중첩된 함수는 포함하는 함수의 `this`값을 상속하지 않는다.
                - 중첩된 함수를 메서드로 호출하면 그 `this` 값은 호출한 객체이다.
            - (화살표 함수가 아닌) 중첩된 함수를 함수로 호출하면 그 this 값은 일반 모드에서는 전역 객체이고 스트릭트 모드에서는 `undefined`이다.

        ```jsx
        let o = {
            m: function () {
                let self = this;
                this === o; // true
                f();

                function f() {
                    this === o; // false
                    self === o; // true
                }
            },
        };
        o.m();
        ```

        - 중첩된 함수 `f()`에서 `this` 키워드는 객체 `o`와 같지 않다.
            - 이것은 자바스크립트 언어의 결함으로 지적받는 부분이고, 반드시 인지하고 있어야 한다.
        - ES6 이후에는 중첩된 함수 f를 화살표 함수로 변환해 `this` 값을 상속하게 하는 방법이 가능하다.
            ```jsx
            const f = () => {
                this === o; // true
            };
            ```
        - 중첩된 함수의 `bind()` 메서드를 호출해 지정된 객체에서 묵시적으로 호출되는 새 함수를 정의하는 방법도 있다.
            ```jsx
            const f = function () {
                this === o;
            }.bind(this);
            ```

    ### 8.2.3 생성자로 호출

    1. 함수나 메서드를 호출할 때 앞에 키워드 new를 붙이면 생성자로 호출된다.

        - 생성자 호출은 인자 처리, 호출 컨텍스트, 반환 값등에서 일반적인 함수나 메서드 호출과는 다르다.
        - 생성자를 호출할 때 괄호 안에 인자 리스트가 있으면 이 인자 표현식을 평가하여 함수나 메서드 호출과 같은 방법으로 함수에 전달한다.

        ```jsx
        o = new Object();
        ```

    1. 생성자를 호출하면 생성자의 `prototype` 프로퍼티에서 지정된 객체를 상속하는 빈 객체를 새로 생성한다.

        - 생성자 함수는 객체를 초기화할 의도로 만들어졌으며, 이렇게 새로 생성된 객체가 호출 컨텍스트로 사용되므로 생성자 함수는 새 객체를 `this`키워드로 참조할 수 있다.
            - 생성자 호출이 메서드 호출처럼 보이더라도 호출 컨텍스트는 새 객체라는 점을 기억해야한다.
            - 즉 표현식 `new o.m()`에서 `o`는 호출 컨텍스트로 사용되지 않는다.

    1. 생성자 함수는 일반적으로 `return` 키워드를 사용하지 않는다.
        - 생성자 함수는 일반적으로 새 객체를 초기화하며 함수 바디의 끝에 도달하면 종료한다.
            - 이 경우 새 객체가 생성자 호출 표현식의 값이다.
        - 하지만 생성자가 명시적으로 `return`문을 사용해 객체를 반환한다면 그 객체가 호출 표현식의 값이 된다.
        - 생성자가 `return`문을 값 없이 사용하거나 기본 값을 반환한다면 반환 값을 무시하고 새 객체를 호출 표현식의 값으로 사용한다.

    ### 8.2.4 간접적 호출

    1. 자바스크립트 함수는 객체이며 다른 자바스크립트 객체와 마찬가지로 메서드가 있다.
        - 이 메서드 중 `call()`과 `apply()`는 함수는 간접적으로 호출한다.
        - 두 메서드 모두 호출 시점에 `this` 값을 직접 명시할 수 있으므로, 함수를 어떤 객체의 메서드로도 호출할 수 있다.
            - `call()` 메서드는 인자 리스트를 받고, `apply()` 메서드는 인자로 배열을 받는다.

    ### 8.2.5 묵시점 함수 호출

    1. 자바스크립트에는 함수 호출처럼 보이지 않지만 함수를 호출하는 기능이 여럿 존재한다.

        - 이 기능은 묵시적으로 호출한 함수에서 버그, 부작용, 성능 문제가 발생한다면 단순히 코드를 들여다보는 것으로는 언제 호출되는지 명확히 알시 어려우므로 일반적인 함수에 비해 해결하기가 훨씬 어렵다.

    1. 묵시적인 함수 호출을 일으키는 언어 기능
        - 객체에 게터나 세터가 있다면 프로퍼티 값에 접근할 때 이 메서드가 호출될 수 있다.
        - 문자열을 받는 컨텍스트에 객체를 사용하면 `toString()` 메서드가 호출되고 객체를 숫자 컨텍스트에 사용하면 `valueOf()` 메서드가 호출된다.
        - 이터러블 객체의 요소를 순회할 때 여러 가지 메서드가 호출될 수 있다.
        - 태그된 템플릿 리터럴도 함수 호출을 일으킬 수 있다.
        - 프록시 객체는 완전히 함수에 의해 제어된다.
            - 이런 객체에는 어떤 동작을 취하든 항상 함수가 호출된다.

## 8.3 함수 매개변수

1. 자바스크립트 함수는 매개변수로 어떤 타입을 받는지 정의하지 않으며, 함수 호출 시점에서도 전달받은 값의 타입을 체크하지 않는다.
    - 전달받은 인자의 개수조차 체크하지 않는다.

### 8.3.1 선택 사항인 매개변수와 기본 값

1. 선언된 매개변수보다 적은 인자로 함수를 호출하면, 대응하는 인자가 없는 매개변수는 기본 값으로 정해지며 일반적으로 이 값은 `undefined`이다.

    ```jsx
    // 객체 o의 열거 가능한 프로퍼티를 배열 a에 추가하고 a를 반환한다.
    // a를 생략하면 새 배열을 생성해 반환한다.
    function getPropertyNames(o, a) {
        if (a === undefined) a = [];
        // a = a || []
        for (let property in o) a.push(property);
        return a;
    }

    let o = { x: 1 };
    let a = { y: 2, z: 3 };
    getPropertyNames(o); // ['x']
    getPropertyNames(o, a); // ['x','y','z']
    ```

    - 선택 사항인 인자를 받는 함수를 만들 때는 선택 사항인 인자를 인자 리스트 마지막에 써서 생략하기 쉽게 만들어야 한다.
        - 함수를 호출할 때 첫 번째 인자를 생략하고 두 번째만 전달하려면 첫 번째 인자에 명시적으로 `undefined`를 써야 한다.
    - ES6 이후에는 함수를 정의할 때 함수 매개변수의 기본 값을 정의할 수 있다.
        - 매개변수 이름 뒤에 등호를 쓰고, 그 매개변수가 생략됐을 때 사용할 기본 값을 쓰면 된다.
        ```jsx
        function getPropertyNames(o, a = []) {
            for (let property in o) a.push(property);
            return a;
        }
        ```
        - 매개변수 기본 값 표현식은 함수를 정의할 때가 아니라 호출할 때 평가된다.
            - 따라서 `getPropertyNames()` 함수를 인자 하나로 호출할 때마다 빈 배열을 새로 생성해서 전달한다.
        - 매개변수 여러 개를 받는 함수에서 앞의 매개변수의 값을 사용해 그 다음 매개변수의 기본 닶을 정의할수 있다.
            ```jsx
            const rectangle = (width, height = width * 2) => ({
                width,
                height,
            });
            rectangle(1); // {width: 1, height: 2}
            ```

    ### 8.3.2 나머지 매개변수와 가변 길이 인자 리스트

    1. 나머지 매개변수는 정의된 매개변수보다 더 많은 인자를 써서 함수를 호출할 수 있다.

        ```jsx
        function max(first = -Infinity, ...rest) {
            let maxValue = first;
            for (let n of rest) {
                if (n > maxValue) {
                    maxValue = n;
                }
            }

            return maxValue;
        }
        ```

        - 나머지 매개변수는 앞에 점 세 개를 붙이는데, 반드시 함수 선언에서 마지막으로 정의된 매개변수여야 한다.
            - 나머지 매개변수를 써서 함수를 호출하면 전달한 인자는 나머지가 아닌 매개변수에 먼저 할당된다.
            - 그리고 남은 나머지 인자가 배열에 저장되며 이 배열이 나머지 매개변수의 값이 된다.
        - 함수 바디 안에서 나머지 매개변수의 값은 항상 배열이다.
            - 배열이 비어 있더라도 나머지 매개변수는 절대 `undefined`가 되지 않는다.
            - 따라서 나머지 매개변수에 매개변수 기본 값을 지정할 필요도 없고 허용되지도 않는다.

    2. 앞의 예제처럼 인자 개수에 제한이 없는 함수를 가변함수라고 부른다.
        - 함수 정의에서 나머지 매개변수를 정의하는 … 을 분해 연산자와 혼동하면 안된다.
            - 분해 연산자는 함수 호출에서 사용할 수 있다.

    ### 8.3.3. Argument 객체

    1. 나머지 매개변수는 ES6에서 자바스크립트에 도입했다.

        - 그 전에는 `Arguments`객체를 써서 가변 함수를 만들었다.
        - 함수 바디 안에서 식별자 `arguments`는 해당 호출의 `Arguments` 객체를 참조한다.
            - `Arguments` 객체는 배열 비슷한 객체이며 함수에 전달된 인자 값을 이름이 아닌 숫자로 참조할수 있게 한다.
                ```jsx
                function max(x) {
                    let maxValue = -Infinity;
                    for (let i = 0; i < arguments.length; i++) {
                        if (arguments[i] > maxValue) maxValue = arguments[i];
                    }
                    return maxValue;
                }
                ```

    1. `Arguments` 객체는 자밧크르비트 초기 버전부터 있어지만 이상하게 동작하고 비효율적이며 최적화하기도 어렵다.
        - 여전히 `Arguments` 객체가 포함된 코드를 볼 수 있겠지만, 새로 작성하는 코드에는 사용하지 말자.
        - 오래된 코드를 리팩터링할 때 `arguments`를 사용하는 함수를 `…args` 나머지 매개변수로 대체할 수 있을때가 많다.

    ### 8.3.4 함수 호출과 분해 연산자

    1. 분해 연산자는 개별 값이 예상되는 컨텍스트에서 배열이나 문자열 같은 이렅러블 객체를 분해한다.

        - 함수 호출에서도 같은 방법으로 분해 연산자를 사용할 수 있다.

        ```jsx
        let numbers = [5, 2, 10, -1, 9, 100, 1];
        Math.min(...numbers); // -1
        ```

        - 분해 연산자는 평가를 통해 값을 얻을 수 없다는 점에서 진정한 연산자로는 볼 수 없다.
            - 분해 연산자는 배열 리터럴과 함수 호출에 사용할 수 있는 특별한 자바스크립트 문법이다.

    1. 함수 정의에서 …문법은 분해연산자의 정반대로 동작한다.

        - 함수 정의에서 사용한 …는 여러 개의 인자를 배열에 모은다.
            - 나머지 매개변수와 분해 연산자를 함께 쓰면 유용한 경우가 많다.

        ```jsx
        function timed(x) {
            return function (...args) {
                // 인자를 나머지 매개변수 배열에 모은다.
                console.log(`Entering function ${f.name}`);
                let startTime = Date.now();
                try {
                    // 인자를 모두 전달한다.
                    return f(...args); // 인자를 다시 분해한다.
                } finally {
                    // 반환하기 전에 소요된 시간을 출력한다
                    console.log(
                        `Exiting ${f.name} after ${Date.now() - startTime}ms`
                    );
                }
            };
        }

        // 1과 n 사이의 숫자의 합을 계산한다.
        function benchmark(n) {
            let sum = 0;
            for (let i = 1; i <= n; i++) sum += i;
            return sum;
        }

        timed(benchmark)(1000000);
        ```

    ### 8.3.5 함수 인자를 매개변수로 분해

    1. 함수를 호출할 때 전달한 인자는 함수 정의 시 선언된 매개변수에 할당된다.

        - 이 작업은 변수 할당과 비슷하다.
        - 따라서 함수에서도 분해 할당을 사용할 수 있다.

    1. 함수를 정의할 때 매개변수 이름을 대괄호 안에 쓰면, 대괄호 한쌍마다 배열값을 받는다고 정의하게 된다.

        - 호출과정에서 배열 인자는 개별 매개변수로 분해된다.

        ```jsx
        function vectorAdd([x1, y1], [x2, y2]) {
            return [x1 + x2, y1 + y2];
        }

        vectorAdd([1, 2], [3, 4]); // [4,6]
        ```

    1. 객체인자를 받는 함수를 정의할 때도 인자르 랍ㄷ은 객체를 매개변수로 분해할 수 있다.

    ```jsx
    function vectorMultiply({ x, y }, scalar) {
        return { x: x * scalar, y: y * scalar };
    }

    vectorMultiply({ x: 1, y: 2 }, 2); // {x:2,y:4}
    ```

    - 프로퍼티를 다른 이름의 매개변수로 분해해야 한다면 문법이 복잡해진다.
        ```jsx
        function vectorAdd({ x: x1, y: y1 }, { x: x2, y: y2 }) {
            return { x: x1 + x2, y: y1 + y2 };
        }

        vectorAdd({ x: 1, y: 2 }, { x: 3, y: 4 }); // {x:4,y:6}
        ```
        - `{ x: 1, y: 2 }` 같은 문법을 분해할 때는 매개변수 이름과 프로퍼티 이름을 잘 구분해야 한다.
            - 분해 할당과 분해 호출에서 선언할 변수 또는 매개변수는 객체 리터럴에서 값이 있는 곳에 있어야한다.
                - 따라서 프로퍼티 이름은 항상 콜론 왼쪽에있고, 매개변수나 변수 이름은 항상 오른쪽에 있다.

    1. 매개변수 분해와 매개변수 기본 값을 섞어 쓸 수도 있다.

        ```jsx
        function vectorMultiply({ x, y, z = 0 }, scalar) {
            return { x: x * scalar, y: y * scalar, z: z * scalar };
        }

        vectorMultiply({ x: 1, y: 2 }, 2); // {x:2,y:4,z:0}
        ```

    1. 배열을 분해하고 남은 값으로도 나머지 매개변수를 정의할 수 있다.

        - 대괄호 안에 있는 나머지 매개변수는 함수에서 정의한 나머지 매개변수와는 완전히 다르다.

        ```jsx
        function f([x, y, ...coords], ...rest) {
            return [x + y, ...rest, ...coords];
        }

        f([1, 2, 3, 4], 5, 6);
        ```

    1. ES2018부터는 객체를 분해할 때도 나머지 매개변수를 사용할 수 있다.

        - 나머지 매개변수의 값은 분해되지 않은 프로퍼티를 모두 담은 객체이다.
        - 객체의 나머지 매개변수는 역시 ES2018에서 도입한 기능인 객체 분해 연산자와 함께 사용할 때 유용하다.

        ```jsx
        function vectorMultiply({ x, y, z = 0, ...props }, scalar) {
            return { x: x * scalar, y: y * scalar, z: z * scalar, ...props };
        }

        vectorMultiply({ x: 1, y: 2, w: -1 }, 2);
        ```

    1. 객체와 배열 인자만 분해할 수 있는 것은 아니다.
        - 객체 배열, 배열 프로퍼티를 가진 객체, 객체 프로퍼티를 가진 객체 모두 분해할 수 있으며 그 깊이에 제한은 없다.
        - 함수 인자 분해가 복잡하다면 코드가 간결해서 얻는 장점보다 읽기 어려워서 생기는 단점이 더 클것이다.
            - 객체 프로퍼티나 배열 인덱스로 접근하는 편이 더 명확할 때도 있다.

    ### 8.3.6 인자 타입

    1. 자바스크립트 메서드는 매개변수 타입을 선언하지 않으며 값을 전달할 때도 타입을 체크하지 않는다.
        - 함수 인자에 뜻이 분명한 이름을 쓰고, 함수에 주석을 달아 두면 코드 자체가 문서가 될 수 있다.
    2. 자바스크립트는 필요할 때마다 타입을 변환한다.

        - 따라서 문자열 인자를 받는 함수를 만들고 그 함수를 다른 타입의 값으로 호출한다면 전달된 값은 함수가 그 값을 문자열로 사용하려고 하는 시점에 문자열로 변환된다.

    3. 하지만 항상 그런건 아니다.
        - 배열 인자를 받고 배열 메서드를 사용하는 함수 중 배열이 아닌 잘못된 타입을 받으면 제대로 동작할 수가 없다.
        - 이럴땐 예측 가능한 형태로 즉시 실패하는 편이 좋다.
            - `throw new Error`

## 8.4 값인 함수

1. 자바스크립트 함수는 문법 구조일 뿐만 아니라 값으로써 변수에 할당될 수 있고, 객체 프로퍼티나 배열 요소에 저장될수도 있으며, 다른 함수에 인자로 전달될 수도 있다.

    ```jsx
    function square(x) {
        return x * x;
    }

    let s = square;
    square(4); // 16
    s(4); // 16

    //객체 프로퍼티에 할당
    let o = {
        square: function (x) {
            return x * x;
        },
    };
    let y = o.square(16); // 256

    //배열 요소에 할당
    let a = [(x) => x * x, 20]; // 배열 리터럴
    a[0](a[1]); // 400
    ```

    ### 8.4.1 함수 프로퍼티 직접 정의

    1. 자바스크립트 함수는 기본 값이 아니라 특별한 객체이므로 함수 역시 프로퍼티를 가질 수 있다.

        - 함수를 언제 호출하든 일정한 ‘정적’ 변수가 필요하다면 그 변수를 함수 자체의 프로퍼티로 정의하는게 편리하다.

        ```jsx
        // 팩토리얼을 계산하고 그 결과를 함수 자체의 프로퍼티로 캐시한다.
        function factorial(n) {
            // 양의 정수만 사용
            if (Number.isInteger(n) && n > 0) {
                // 캐시된 결과가 없다면
                if (!(n in factorial)) {
                    // 계산 후 캐시에 저장
                    factorial[n] = n * factorial(n - 1);
                }
                return factorial[n];
            } else {
                return NaN;
            }
        }
        ```

## 8.5 네임스페이스인 함수

1. 함수안에서 선언한 변수는 함수 바깥에서 보이지 않는다.

    - 따라서 전역 네임스페이스를 어지럽히지 않도록, 임시 네임스페이스 기능을 하는 함수를 정의하는 것이 유용할 때도 있다

1. 코드가 다양한 프로그램에서 정의하는 변수가 다른 프로그램의 변수와 충돌할지 확실히 알 수 없을때

    - 해결책은 코드를 함수에 넣고 호출하는 것이다.
        - 이렇게 하면 전역에서 사용됐을 변수를 함수의 로컬 변수로 만들 수 있다.
        ```jsx
        function chunkNamesapce() {
            // 코드가 여기 존재한다. 코드에서 정의한 변수는 모두 함수의 로컬 변수이므로
            // 전역 네임스페이스를 어지럽히는 일은 없다.
        }

        chunkNamesapce(); // 단, 이 함수 호출은 잊지 말아야 한다.
        ```
        - 이 코드는 전역에 `chunkNamesapce`라는 함수 이름 하나만 정의한다.
        - 프로퍼티 하나라도 전역에 남겨 두고 싶지 않다면 표현식 하나로 익명 함수를 정의하고 즉시 호출할 수 있다.
        ```jsx
        (function () {
            //코드가 여기 존재.
        }); // 함수리터럴을 종료하고 즉시 호출한다.
        ```
        - 표현식 하나에서 함수를 정의하고 호출하는 기법은 워낙 널리 사용되므로 ‘즉시 호출하는 함수 표현식(`IIFE`)’이라는 이름도 있다.
        - `function` 앞에 있는 여는 괄호가 없으면 자바스크립트 인터프리터가 `function` 키워드를 함수 선언으로 분석하기 때문에 반드시 필요하다.
            - 괄호가 있으면 인터프리터는 이를 함수 정의 표현식으로 정확히 인식한다.

1. 이렇게 함수를 네임스페이스로 사용하는 방법은 네임스페이스 안에 있는 변수를 사용해 하나 이상의 함수를 정의하고, 정의된 함수를 네임스페이스 함수의 반환 값으로 사용할 때 아주 유용하다.
    - 이런 함수를 클로저라고 부른다.

## 8.6 클로저

1. 대부분의 최신 프로그래밍 언어와 마찬가지로 자바스크립트 역시 렉시컬 스코프를 사용한다.

    - 렉시컬 스코프란 함수가 호출 시점의 스코프가 아니라 **자신의 정의된 시점의 변수 스코프를 사용**하여 실행된다는 뜻이다.
    - 렉시컬 스코프를 구현하기 위해서는 자바스크립트 함수 객체의 내부 상태에 함수의 코드뿐만 아니라 함수가 정의된 스코프에 대한 참조도 반드시 포함되어 있어야 한다.
    - 이렇게 함수 객체와 스코프를 조합한 것을 클로저라 부른다.

1. 엄밀히 말하면 자바스크립트 함수는 모두 클로저이지만, 대부분의 함수가 자신이 정의된 곳과 같은 스코프에서 호출되므로 보통은 클로저인지 아닌지를 따질 필요가 없다.

    - 클로저가 유용할 때는 함수가 정의된 곳과 다른 스코프에서 호출될때 뿐이다.
        - 가장 흔한 경우는 함수가 함수를 정의해 반환하는 경우이다.

1. 클로저를 이해하는 첫번째 단계는 중첩된 함수의 어휘적 스코프 규칙을 살펴보는 것이다.

    ```jsx
    let scope = "global scope";
    function checkScope() {
        let scope = "local scope";
        function f() {
            return scope;
        }
        return f();
    }

    checkScope(); // 'local scope'
    ```

    - `checkScope()` 함수는 로컬 변수를 선언하고, 그 변수의 값을 반환하는 함수를 정의해 호출한다.

    ```jsx
    let scope = "global scope";
    function checkScope() {
        let scope = "local scope";
        function f() {
            return scope;
        }
        return f;
    }

    let s = checkScope()(); // 'local scope'
    ```

    - 이 코드에서는 괄호 한 쌍이 `checkScope()` 내부에서 외부로 이동했다.
    - `checkScope()` 는 이제 중첩된 함수를 호출하고 그 결과를 반환하는 대신, 중첩된 함수 객체 자체를 반환한다.
    - 렉시컬 스코프의 기본 규칙을 생각해보자!
        - 자바스크립트 함수는 자신의 정의된 스코프에서 실행된다.
        - 중첩된 함수 `f()`는 변수 `scope`가 `“local scope”`였던 스코프에서 정의되었다.
        - 이 연결은 `f`를 어디에서 실행하든 상관 없이 계속 유지된다.
            - 따라서 `“local scope”` 를 반환한다.
    - 간단히 말해 이것이 클로저의 강력함이다.
        - 클로저는 자신을 정의한 외부 함수의 로컬변수와 매개변수를 그대로 캡쳐한다.

1. 클로저는 함수 호출 시점의 로컬 변수를 캡처하므로 이 변수를 비공개 상태로 사용할 수 있다.

    - 다음 예제 코드를 통해 즉시 호출하는 함수 표현식에서 네임스페이스를 정의하고 클로저가 이 네임스페이스를 사용해 비공개 상태를 유지하도록 하자.

    ```jsx
    let uniqueInter = function () {
        let counter = 0;
        return function () {
            return counter++;
        };
    };

    // 즉시 호출하는 함수 표현식을 사용 했으므로 uniqueInter에 할당 되는 것은 내부함수이다.
    uniqueInter(); // 0
    uniqueInter(); // 1
    ```

    - 중첩된 함수는 자신의 스코프에 있는변수에 접근할 수 있으며 외부 함수에 정의된 `counter` 변수도 사용할 수 있다.
    - 일단 외부 함수가 종료되면 다름 코드에서는 `counter` 변수를 볼 수 없다.
        - 오직 내부 함수만이 `counter` 변수에 접근할 수 있다.

1. 두 개 이상의 중첩된 함수가 같은 외부 함수에서 정의되고, 같은 스코프를 공유해도 아무 문제 없다.

    ```jsx
    function counter() {
        let n = 0;
        return {
            count: function () {
                return n++;
            },
            reset: function () {
                n = 0;
            },
        };
    }

    let c = count(),
        d = count(); // 카운터 두개 생성
    c.count(); // => 0
    d.count(); // => 0 : c와 별도로 계산된다.
    c.reset();
    c.count(); // => 0
    d.count(); // => 1
    ```

    - 첫번째로 이해해야 할 것은 두 메서드가 같은 비공개 변수 `n`에 접근한다는 것.
    - 두번째는 `counter()`를 호출할 때마다 이전 호출과 독립된 새 스코프가 생성되며, 그 스코프 안에서 비공개 변수 역시 새로 생성된다는 것.
    - `conuter()`를 두 번 호출하면 카운터 객체가 두개 생기며 이들의 비공개 변수는 각각 다르다.
        - 카운터 객체 한쪽에서 `count()`나 `reset()`을 호출하더라도 다른 객체에는 영향을 주지 않는다.

1. 클로저 기법과 프로퍼티 게터와 세터를 조합할 수 있다.

    ```jsx
    function counter(n) {
        return {
            get count() {
                return n++;
            },
            set count(m) {
                if (m > n) n = m;
                else throw new Error("카운트는 더 큰 값으로만 바꿀 수 있다.");
            },
        };
    }

    let c = counter(1000);
    c.count; // 1000
    c.count; // 1001
    c.count = 2000;
    c.count; // 2000
    c.count = 2000; // Error
    ```

    - 로걸 변수를 선언하지 않고 매개변수 n에 프로퍼티 접근자 메서드에서 공유한 비공개 상태를 담았다.
        - 이렇게 하면 `counter()`의 호출자에서 비공개 변수의 초깃값을 정할 수 있다.

1. 클로저 기법을 통해 비공개 상태를 공유하는 방법을 일반화 할수도 있다.

    ```jsx
    // 이 함수는 지정된 이름의 프로퍼티에 대한 프로떠티 접근자 메서드를 객체 o에 추가합니다.
    // 메서드 이름은 get<name>과 set<name>으로 지정됩니다. 판별 함수가 제공됐다면
    // 셰터 메서드는 인자톨 저장하기 전에 판별 함수틀 사용해 유효성올 테스트합니다.
    // 판별 함수가 false률 반환힌다면 셰터 메서드는 예외를 일으킵니다.

    // 이 함수의 독특한 점은 게터와 세터 메서드가 조작하는 프로퍼티 값이
    // 객체 o에 저장되지 않는다는 점입니다. 값은 이 함수의 로컬 변수에만 저장됩니다.
    // 게터와 셰터 메서드는 합수에 로컬로 정의됐으므로 로컬 번수에 접근힐 수 있습니다.
    // 따라서 값은 두 접근자 에서만 사용힐 수 있으며, 세터 메서드들 통하지 않고서는
    // 값을 수정하거나 저장할 수 없습니다.

    function addPrivateProperty(o, name, predicate) {
        let value; // 프로퍼티 값입니다.
        // 게터 메서드는 단순히 그 값을 반환합니다.
        o[`get${name}`] = function () {
            return value;
        };
        // 세터 메서드는 판별 합수의 판단에 따라 값을 저장하거나 예외를 일으킵니다.
        o[`set${name}`] = function (v) {
            if (predicate && !predicate(v)) {
                throw new TypeError(`set${name}: invalid value ${v}`);
            } else {
                value = v;
            }
        };
    }

    // 다융 코드는 addPrivateProperty() 메서드의 사용 방법 예시입니다.
    let o = {}; //빈객체입니다.

    // 프로퍼티 접근자 메서드 getName()과 setName()을 추가합니다. 오직 문자열 값만 허용합니다.
    addPrivateProperty(o, "Name", (x) => typeof x === "string");

    o.setName("Frank"); // 프로퍼티 값을 저장합니다.
    o.getName(); // => ”Frank”
    o.setName(0); // TypeError: 올바르지 않은 타입을 사용했습니다.
    ```

    - 하지만 클로저가 접근을 공유하면 안 되는 변수에 대한 접근까지 부주의하게 공유할 수도 있다는 점을 기억해야 한다.
        ```jsx
        // 이 함수는 항상v를 반환하는 함수를 반환합니다.
        function constfunc(v) {
            return () => v;
        }
        // 정적 함수 배열을 생성합니다.
        let funcs = [];
        for (var i = 0; i < 10; i++) funcs[i] = constfunc(i);

        // 인덱스 5의 함수는 5를 반환합니다.
        funcs[5](); // => 5
        ```
        - 이렇게 루프에서 클로저를 여러 개 생성하는 코드를 사용할 때, 흔히 루프를 클로저를 정의하는 함수 안으로 옮기는 실수를 하곤 한다.
            ```jsx
            // 0부터 9까지의 값을 반환하는 함수 배열을 반환
            function constfuncs() {
                let funcs = [];
                for (var i = 0; i < 10; i++) {
                    funcs[i] = () => i;
                }
                return funcs;
            }

            let funcs = constfuncs();
            funcs[5](); // => 10; 왜 5가 아닐까요?
            ```
            - 위 코드처럼 클로저 10개를 생성하지만, 외부 변수 `i`를 모든 클로저가 공유하게 되어 반환된 배열 안의 클로저들이 모두 같은 10을 반환하게 되는 결과를 낳으며, 의도와 전혀 다른 결과를 낳았다.
                - 위 문제는 변수를 `var`로 선언했기 때문이다.
                    - 변수 `i`는 루프 바디 안에만 존재 하는것이 아닌 함수 전체에 존재 하게 되어 클로저들이 이 변수를 공유하게 된것이다.
                - ES6에서 블록스코프를 도입하고, `let, const` 가 생김에 따라 `var i`를 `let` 또는 `const`로만 변경하게 되면 매 반복마다 독립적인 스코프를 생성하여 각 스코프는 각자의 `i` 를 참조하여 해결할 수 있다.

    ## 8.7 함수 프로퍼티, 메서드, 생성자

    1. 자바스크립트 함수는 특별한 종류의 자바스크립트 객체이자 값이다.

        - 함수는 객체이므로 다른 객체와 마찬가지로 프로퍼티와 메서드를 가질 수 있다.

        ### 8.7.1 length 프로퍼티

        1. 읽기 전용인 `length` 프로퍼티는 함수의 ‘항’, 즉 정의된 매개변수 개수이며 보통 함수가 예상하는 인자 개수이기도 하다.

        1. 함수에 나머지 매개변수가 있다면 그 매개변수는 이 `length` 프로퍼티에 포함되지 않는다.

        ### 8.7.2 name 프로퍼티

        1. 읽기 전용인 `name` 프로퍼티는 함수가 정의될 때 이름이 있었다면 그 이름, 또는 익명의 함수 표현식이라면 처음 생성됐을 때 할당된 변수나 프로퍼티 이름이다.

        ### 8.7.3 prototype 프로퍼티

        1. 화살표 함수를 제외하면 모든 함수에는 프로토타입 객체를 참조하는 `prototype` 프로퍼티가 있다.

            - 함수의 프로토타입 객체는 모두 다르다.

        1. 함수를 생성자로 사용하면 새로 생성된 객체는 프로토타입 객체에서 프로퍼티를 상속한다.

        ### 8.7.4 call()과 apply() 메서드

        1. `call(), apply()` 는 함수를 마치 다른 객체의 메서드인 것처럼 간접적으로 호출한다.

            - 첫번째 인자는 함수를 호출할 객체이다
                - 이 인자가 호출 컨텍스트이며 함수 바디 안에서 `this` 키워드의 값이다.

        1. 화살표 함수는 자신이 정의된 컨텍스트의 `this` 값을 상속한다.
            - 이 값은 `call(), apply()` 메서드로 덮어 쓸 수 없다.
                - 화살표 함수에서 이들 메서드를 호출하면 첫 번째 인자는 무시되는 것이나 마찬가지이다.
        1. `call()`을 사용할 때 호출 컨텍스트 다음에 오는 인자는 호출될 함수에 전달된다.
            - 이 인자는 화살표 함수에서도 무시되지 않는다.
            - 함수 `f()`에 숫자 두개를 전달하면서 `f()`가 `o`의 메서드인 것처럼 호출하려면?
                ```jsx
                f.call(o, 1, 2);
                ```
            - `apply()` 메서드는 함수에 전달할 인자가 배열로 제공된다.
                ```jsx
                f.apply(o, [1, 2]);
                ```

        ### 8.7.5 bind() 메서드

        1. `bind()` 의 주요 목적은 함수를 객체에 결합하는 것이다.

            - 함수 `f`에서 `bind()` 메서드를 호출하면서 객체 `o`를 전달하면 새 함수를 반환한다.
                - 새 함수를 함수로 호출하면 원래 함수 `f`가 `o`의 메서드로 호출된다.
                    - 새 함수에 전달한 인자는 모두 원래 함수에 전달된다.
                ```jsx
                function f(y) {
                    return this.x + y;
                }
                let o = { x: 1 };
                let g = f.bind(o);
                g(2); // 3
                let p = { x: 10, g };
                p.g(2); // 3 => g는 여전히 o에 결합되어 있다
                ```

        1. 화살표 함수는 자신이 정의된 환경에 this 값을 상속하며 이 값은 `bind()` 에서 덮어 쓸 수 없으므로, 위 코드는 함수 f()를 화살표 함수로 정의했다면 결합은 제대로 이루어지지 않았을 것이다.

            - `bind()` 를 호출하는 목적은 대개 화살표 함수가 아닌 함수를 화살표 함수처럼 사용하는 것이므로 화살표 함수에서 결합이 이루어지지 않는다는 것은 사실 문제가 되지 않는다.

        1. `bind()` 메서드는 단순히 함수를 객체에 결합하는 것으로 끝나지 않는다.

            - `bind()` 에 전달하는 인자 중 첫 번째를 제외한 나머지는 `this`값과 함께 결합된다.
                - 이러한 부분 적용은 화살표 함수에도 동작한다.
            - 부분 적용은 함수형 프로그래밍에서 널리 쓰이는 기법이며 커링이라고 부르기도 한다.

            ```jsx
            let sum = (x, y) => x + y;
            let succ = sum.bind(null, 1); // 1이 x에 결합됨
            succ(2); // 3

            function f(y, z) {
                return this.x + y + z;
            }
            let g = f.bind({ x: 1 }, 2); // this 와 y 를 결합한다
            g(3); // 6
            ```

        1. `bind()` 가 반환하는 함수의 `name` 프로퍼티는 `bind()` 를 호출한 함수의 `name` 앞에 `“bound”`를 붙인 값이다.

        ### 8.7.6 toString() 메서드

        1. 다른 자바스크립트 객체와 마찬가지로 함수에도 `toString()` 메서드가 있다.
            - `ECMAScript` 명세는 이 메서드가 함수 선언문의 문법을 지키는 문자열을 반환해야 한다고 규정한다.
            - 하지만 현실에서는 대부분의 환경에서 함수의 `toString()` 메서드가 소스 코드 전체를 반환한다.
            - 내장 함수는 일반적으로 `“[native code]”` 같은 문자열을 함수 바디로 반환한다.

        ### 8.7.7 Function() 생성자

        1. 함수는 객체이므로 `Function()` 생성자를 사용해 새 함수를 생성할 수 있다.

            ```jsx
            const f = new Function("x", "y", "return x*y");

            // 위 코드가 생성한 함수는 다음과 같이 익숙한 문법으로
            // 정의한 함수와 거의 동등하다.

            const f = function (x, y) {
                return x * y;
            };
            ```

            - `Function()` 생성자는 문자열 인자를 개수 제한 없이 받는다.
                - 마지막 인자는 함수 바디인 텍스트이다.
                    - 이 텍스트에는 자바스크립트 문을 제한 없이 넣을 수 있으며 각 문은 세미콜론으로 구분한다.
                - 다른 인자는 모두 함수가 받을 매개변수 이름이다.
                - 인자를 받지 않는 함수를 정의할 때는 생성자에 함수 바디 문자열만 전달하면 된다.
            - `Function()` 생성자에 전달하는 인자 중 함수 이름에 해당하는 것은 없다.
                - 함수 리터럴과 마찬가지로 `Function()` 생성자 역시 익명 함수를 생성한다.

        2. `Function()` 생성자에 대해 이해해야 할 중요한 포인트
            - `Function()` 생성자는 런타임에 자바스크립트 함수를 동적으로 생성하고 컴파일 할 수 있다.
            - `Function()` 생성자는 호출될 때 마다 함수 바디를 분석하고 새 함수 객체를 생성한다.
                - 루프나 자주 호출되는 함수 안에서 생성자를 호출하는 것은 효율적이지 않다.
                - 중첩된 함수와 함수 표현식은 루프 안에 있더라도 매번 재 컴파일 되지 않는다.
            - `Function()` 생성자가 만드는 함수는 렉시컬 스코프를 사용하지 않는다.
                - 생성자가 만드는 함수는 항상 최상위 함수로 컴파일된다.

    ## 8.8 함수형 프로그래밍

    1. 자바스크립트는 함수를 객체처럼 조작할 수 있으므로 함수형 프로그래밍 기법을 사용할수 있다.

        - 이어지는 예제는 자바스크립트의 함수의 강력함을 실감할수 있도록 만들어졌을뿐 좋은 프로그래밍 스타일 예제는 아니다.

        ### 8.8.1 함수로 배열 처리

        1. 숫자로 이루어진 배열이 있고 이 값들의 평균과 표준 편차를 구하자

            ```jsx
            // 함수형 프로그래밍 스타일을 사용하지 않을때
            let data = [1, 1, 3, 5, 5];

            // 평균 구하기
            let total = 0;
            for (let i = 0; i < data.length; i++) total += data[i];
            let mean = total / data.length; // mean === 3, 평균은 3이다

            // 표준 편차 구하기
            total = 0;
            for (let i = 0; i < data.length; i++) {
                let deviation = data[i] - mean;
                total += deviation * deviation;
            }
            let stddev = Math.sqrt(total / (data.length - 1)); //stddev === 2
            ```

            - 같은 계산을 배열 메서드 `map()`과 `reduce()`를 사용해 간결한 함수형 스타일로 바꿀 수 있다.

            ```jsx
            const sum = (x, y) => x + y;
            const square = (x) => x * x;

            // sum 과 square를 사용해 평균과 표준 편차를 계산한다
            let data = [1, 1, 3, 5, 5];
            let mean = data.reduce(sum);
            let deviations = data.map((x) => x - mean);
            let stddev = Math.sqrt(
                deviations.map(square).reduce(sum) / (data.length - 1)
            );
            stddev; // 2
            ```

            - 새로 만든 코드는 앞의 코드와 사뭇 다르지만 여전히 객체 메서드를 호출하고 있으므로 객체 지향 스타일도 남아있다
            - `map()`과 `reduce()` 메서드의 함수형 버전도 만들어 보자

            ```jsx
            const map = function (a, ...args) {
                return a.map(...args);
            };
            const reduce = function (a, ...args) {
                return a.reduce(...args);
            };

            const sum = (x, y) => x + y;
            const square = (x) => x * x;

            let data = [1, 1, 3, 5, 5];
            let mean = reduce(data, sum) / data.length;
            let deviations = map(data, (x) => x - mean);
            let stddev = Math.sqrt(
                reduce(map(deviations, square), sum) / (data.length - 1)
            );
            // stddev; // 2
            ```

        ### 8.8.2 고계 함수

        1. 고계 한수는 하나 이상의 함수를 인자로 받아 새 함수를 반환하는 함수이다.

            ```jsx
            function not(f) {
                return function (...args) {
                    // 새 함수를 반환
                    let result = f.apply(this, args); // 이 함수는 f를 호출하고
                    return !result; // 그 결과를 부정한다
                };
            }

            const even = (x) => x % 2 === 0;
            const odd = not(even)[(1, 1, 3, 5, 5)].every(odd); // true
            ```

            - `not()` 함수는 함수 인자를 받고 새 함수를 반환하는 고계함수이다.

        1. 다른 예제 `mapper()`

            - `mapper()` 함수는 함수 인자를 받고 그 함수를 사용해 배열을 다른 배열로 변환하는 새 함수를 반환한다.

            ```jsx
            const map = function (a, ...args) {
                return a.map(...args);
            };

            function mapper(f) {
                return (a) => map(a, f);
            }

            const increment = (x) => x + 1;
            const incrementAll = mapper(increment);
            incrementAll([1, 2, 3]); // [2,3,4]
            ```

        1. `f`와 `g` 두 함수를 받고 `f(g())`를 계산하는 새 함수를 반환

            ```jsx
            // f(g(...)) 를 계산하는 새 함수를 반환한다.
            // 반환되는 함수 h는 인자 전체를 g에 전달하고,
            // g의 반환 값을 f에 전달한 다음 f의 반환 값을 반환한다
            function compose(f, g) {
                return function (...args) {
                    return f.call(this, g.apply(this, args));
                };
            }
            const sum = (x, y) => x + y;
            const square = (x) => x * x;
            compose(square, sum)(2, 3); // 25
            ```

        ### 8.8.3 함수의 부분적용

        1. `bind()` 메서드는 왼쪽에 있는 인자를 부분적으로 적용한다.

            - 즉, `bind()`에 전달하는 인자는 원래 함수에 전달되는 인자 리스트의 시작 부분에 위치한다는 뜻이다.
            - 반대로 오른쪽에 있는 인자를 부분적으로 적용하는것도 가능하다.

            ```jsx
            // 이 함수의 인자는 왼쪽에 전달된다
            function partialLeft(f, ...outerArgs) {
                return function (...innerArgs) {
                    let args = [...outerArgs, ...innerArgs];
                    return f.apply(this, args);
                };
            }
            // 이 함수의 인자는 오른쪽에 전달된다
            function partialRight(f, ...outerArgs) {
                return function (...innerArgs) {
                    let args = [...innerArgs, ...outerArgs];
                    return f.apply(this, args);
                };
            }

            function partial(f, ...outerArgs) {
                return function (...innerArgs) {
                    let args = [...outerArgs];
                    let innerIndex = 0;
                    // 인자를 순회하며 정의되지 않은 값을 내부 인자로 채운다
                    for (let i = 0; i < args.length; i++) {
                        if (args[i] === undefined)
                            args[i] = innerArgs[innerIndex++];
                    }
                    // 남은 내부 인자를 이어 붙인다
                    args.push(...innerArgs.slice(innerIndex));
                    return f.apply(this, args);
                };
            }
            // 인자 세 개를 받는 함수
            const f = function (x, y, z) {
                return x * (y - z);
            };
            // 세 가지 부분 적용이 어떻게 다르게 동작하는지 보자
            partialLeft(f, 2)(3, 4); // -2, 첫번째 인자에 결합 2 * (3-4)
            partialRight(f, 2)(3, 4); // 6, 마지막 인자에 결합 3 * (4-2)
            partial(f, undefined, 2)(3, 4); // -6, 중간 인자에 결합 3 * (2-4)
            ```

            - 부분 적용 함수를 사용하면 이미 정의한 함수를 사용해서 더 흥미로운 함수를 쉽게 정의할 수 있다.

            ```jsx
            const sum = (x, y) => x + y;
            const increment = partialLeft(sum, 1);
            const cuberoot = partialRight(Math.pow, 1 / 3);
            cuberoot(increment(26)); // 3
            ```

            - 부분 적용과 고계 함수를 조합하면 더 흥미롭다.

            ```jsx
            function compose(f, g) {
                return function (...args) {
                    return f.call(this, g.apply(this, args));
                };
            }

            const not = partialLeft(compose, (x) => !x);
            const even = (x) => x % 2 === 0;
            const odd = not(even);
            const isNumber = not(isNaN);
            odd(3) && isNumber(2); // true
            ```

            - 합성과 부분 적용을 사용해 평균과 표준 편차를 완전한 함수형 스타일로 개선할 수도 있다.

            ```jsx
            const sum = (x, y) => x + y;
            const square = (x) => x * x;
            const map = function (a, ...args) {
                return a.map(...args);
            };
            const reduce = function (a, ...args) {
                return a.reduce(...args);
            };
            const product = (x, y) => x * y;
            const neg = partial(product, -1);
            const sqrt = partial(Math.pow, undefined, 0.5);
            const reciprocal = partial(Math.pow, undefined, neg(1));

            //이제 평균과 표준 편차를 계산하다
            let data = [1, 1, 3, 5, 5];
            let mean = product(reduce(data, sum), reciprocal(data.length));
            let stddev = sqrt(
                product(
                    reduce(
                        map(data, compose(square, partial(sum, neg(mean)))),
                        sum
                    ),
                    reciprocal(sum(data.length, neg(l)))
                )
            );
            [mean, stddev]; // [3,2]
            ```

        ### 8.8.4 메모제이션

        1. 함수를 인자로 받고 캐시를 활용하도록 수정해서 반환하는 고계함수

            ```jsx
            function memoize(f) {
                const cache = new Map();

                return function (...args) {
                    let key = args.length + args.join("+");
                    if (cache.has(key)) return cache.get(key);
                    else {
                        let result = f.apply(this, args);
                        cache.set(key, result);
                        return result;
                    }
                };
            }
            ```

            - `memoize()`함수는 캐시로 사용할 새 객채를 생성하고 이 객체를 로컬 변수에 할당하므로, 반환된 함수 외에는 이 객체를 볼수 없다.
            - 반환된 함수는 인자 배열을 문자열로 변환하고 그 문자열을 캐시 객체의 프로퍼티 이름으로 사용한다.
            - 값이 캐시에 존재하면 바로 반환하고 그렇지 않다면 인자를 넘기면서 지정된 함수를 호출해 값을 계산하고 캐시에 저장한 다음 반환한다.
