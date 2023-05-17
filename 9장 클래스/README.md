# 9장 클래스

1. 자바스크립트 객체에 대해 설명하면서 객체는 프로퍼티의 고유한 집합이며 다른 어떤 객체와도 같지 않다고 했다.

    - 하지만 일부 프로퍼티를 공유하는 객체 클래스를 만드는 것이 유용할 때도 많다.
    - 클래스의 인스턴스는 자신의 상태를 정의하는 자체 프로퍼티도 갖지만, 자신의 동작을 정의하는 메서드도 가진다.
        - 이런 메서드는 클래스에서 정의하며 모든 인스턴스에서 공유한다.

1. 자바스크립트 클래스는 프로토타입 기반 상속을 사용한다.
    - 두 객체가 같은 프로토타입에서 프로퍼티를 상속한다면 이들은 같은 클래스의 인스턴스라고 부른다.
    - 두 객체가 같은 프로토타입을 상속한다면 일반적으로 이들은 같은 생성자 함수나 팩토리 함수를 통해 생성되고 초기화 됐을 가능성이 높다.

## 9.1 클래스와 프로토타입

1. 자바스크립트에서 클래스는 같은 프로토타입 객체에서 프로퍼티를 상속하는 객체집합이다.

    - 따라서 프로토타입 객체가 클래스의 핵심 기능이다.
    - 프로토타입 객체를 정의하고 `Object.create()`로 프로토타입을 상속하는 객체를 생성한다면 자바스크립트 클래스를 정의한 것이다.
    - 보통 클래스 인스턴스는 초기화가 더 필요하므로 새 객체를 생성하고 초기화하는 함수를 정의하는 것이 일반적이다.

1. 다음은 일정 범위의 값을 나타내는 클래스의 프로토타입 객체를 정의하고, 클래스의 인스턴스를 생성하고 초기화하는 팩토리 함수이다.

    ```jsx
    // Range 객체를 반환하는 팩토리 함수이다.
    function range(from, to) {
        // Object.create()를 써서 아래에서 정의하는 프로토타입 객체를 상속하는
        // 객체를 생성한다. 프로토타입 객체는 이 함수의 프로퍼티로 저장되며
        // Range 객체에서 공유하는 메서드를 정의한다.
        let r = Object.create(range.methods);

        // Range 객체의 시작점과 끝점(상태)을 저장한다.
        // 이들은 이 객체에 고유한 프로퍼티이며 상속되지 않는다.
        r.from = from;
        r.to = to;
        // 마지막으로 새 객체를 반환한다.
        return r;
    }

    // 이 프로토타입 객체는 Range 객체가 상속하는 메서드를 정의한다.
    range.methods = {
        // x가 범위 안에 있으면 true, 아니면 false를 반환
        // 이 메서드는 숫자뿐만 아니라 텍스트, 날짜 범위에도 동작한다.
        includes(x) {
            return this.from <= x && x <= this.to;
        },

        // 클래스 인스턴스를 이터러블로 만드는 제너레이터 함수이다.
        // 이 기능은 숫자 범위에서만 동작한다.
        *[Symbol.iterator]() {
            for (let x = Math.ceil(this.from); x <= this.to; x++) yield x;
        },

        // 범위를 나타내는 문자열을 반환한다.
        toString() {
            return "(" + this.from + "..." + this.to + ")";
        },
    };

    // Range 객체의 사용 예제
    let r = range(1, 3); // Range 객체를 생성
    r.includes(2); // true 2는 범위안에 있다.
    r.toString(); // "(1...3)"
    [...r]; // [1,2,3] 이터레이터를 통해 배열로 변환한다.
    ```

    - 이 코드는 `Range` 객체를 생성하는 팩토리 함수 `range()`를 정의한다.
    - `range()` 함수는 `methods` 프로퍼티에 클래스를 정의하는 프로토타입 객체를 저장한다.
    - `range()` 함수는 `Range` 객체에 `from`과 `to` 프로퍼티를 정의한다.
        - 이들은 공유되지 않고 상속되지 않는 프로퍼티이며 각 `Range` 객체의 고유한 상태를 나타낸다.
    - 프로토타입의 메서드 중에 계산된 이름 `Symbol.iterator`를 사용하는 메서드가 있다
        - 이 메서드가 `Range` 객체의 이터레이터를 정의한다.
        - 메서드 이름 앞에 있는 `*`는 일반적인 함수가 아니라 제너레이터 함수라는 의미이다.
    - `range.methods`에 정의된 공유, 상속되는 메서드는 모두 `range()` 팩토리 함수에서 초기화한 `from`과 `to` 프로퍼티를 사용한다.
        - 메서드는 `this` 키워드를 써서 자신이 호출된 객체를 참조하는 방식으로 해당 프로퍼티에 접근한다.
        - `this`를 이렇게 사용하는 것은 모든 클래스 메서드의 기본적인 특징이다.

## 9.2 클래스와 생성자

1. 생성자는 새로 생성된 객체를 초기화하도록 설계된 함수이다.

    - 생성자는 `new` 키워드를 사용해 호출한다.
    - `new`를 사용해 생성자를 호출하면 자동으로 새 객체가 생성되므로, 생성자 자체에서 할 일은 새 객체의 상태를 초기화하는 것뿐이다.
    - 생성자 호출에서 중요한 특징은 생성자의 `prototype` 프로퍼티가 새 객체의 프로토타입으로 사용된다는 것이다.

1. 프로토타입을 소개하면서 거의 모든 객체에 프로토타입이 있지만 `prototype` 프로퍼티를 가진 객체는 그중 일부라고 강조했다.

    - 정확히 말하면 `prototype` 프로퍼티를 가지는 것은 함수 객체이다.
    - 생성자 함수를 공유하는 개체는 모든 같은 객체를 상속하며, 따라서 같은 클래스 맴버이다.

1. 위의 `Range` 클래스를 팩토리 함수 대신 생성자 함수를 사용하여 만든 예제

    - ES6의 `class` 키워드를 지원하지 않는 자바스크립트 버전의 클래스…

    ```jsx
    // Range 객체를 초기화하는 생성자 함수이다.
    // 이 함수는 객체를 생성하거나 반환하지 않는다. 그저 초기화할 뿐이다.
    function Range(from, to) {
        // Range 객체의 시작점과 끝점(상태)을 저장한다.
        // 이들은 이 객체에 고유한 프로퍼티이며 상속되지 않는다.
        this.from = from;
        this.to = to;
    }

    // Range 객체는 모두 이 객체를 상속한다.
    // 코드가 동작하기 위해서는 프로퍼티 이름이 반드시 prototype이어야 한다.
    Range.prototype = {
        includes(x) {
            return this.from <= x && x <= this.to;
        },
        *[Symbol.iterator]() {
            for (let x = Math.ceil(this.from); x <= this.to; x++) yield x;
        },
        toString() {
            return "(" + this.from + "..." + this.to + ")";
        },
    };

    // Range 클래스의 사용 예제
    let r = new Range(1, 3);
    r.includes(2);
    r.toString();
    [...r];
    ```

    - 팩토리 함수와의 차이!!
        - 생성자 함수로 바뀌면서 `Range()`로 이름 변경
            - 생성자 함수는 어떤 의미로는 클래스를 정의한다고 할 수 있고, 클래스 이름은 관습적으로 대문자로 시작한다.
        - `new`
            - `Range()` 생성자를 new 키워드와 함께 호출했다.
                - 새 객체를 생성하기 위해 `Object.create()`를 호출하거나 다른 동작을 취할 필요는 없다.
        - 새 객체는 생성자를 호출하기 전에 자동으로 생성되며, `this` 값을 통해 접근할 수 있다.
            - `Range()` 생성자는 `this`를 초기화하기만 하면 된다.
        - 생성자는 새로 생성된 객체를 반환할 필요도 없다.
            - 생성자를 호출하면 자동으로 새 객체가 생성되고, 생성자를 그 객체의 메서드로 호출하며, 새 객체를 반환한다.
        - 프로토타입 객체의 이름
            - 팩토리 함수예제에서는 프로토타입에 `range.methods`라는 이름을 썼다.
                - 편리하고 알기 쉽지만 어떠한 강제성도 없다.
            - 생성자 함수예제에서는 프로토타입에 `Range.prototype`이라는 이름을 썼다.
                - 이 이름은 **규칙**이다.
                - `Range()` 생성자를 호출하면 자동으로 `Range.prototype`을 새 `Range` 객체의 프로토타입으로 사용한다.
        - 두 예제 모두 생성자나 메서드를 정의할 때 화살표 함수를 사용하지 않았다.
            - 화살표 함수는 prototype 프로퍼티가 없으므로 생성자로 사용할 수 없다.
            - 화살표 함수는 호출한 객체가 아니라 자신이 정의된 컨텍스트에서 this 키워드를 상속한다.
            - 메서드는 자신을 호출한 인스턴스를 `this`로 참조할 수 있다는 것이 특징이므로 메서드에 화살표 함수를 사용하는 것은 무의미하다.

### 9.2.1 생성자, 클래스 본질, instanceof

1. 프로토타입 객체는 클래스의 본질이다.

    - 두 객체가 같은 프로토타입 객체를 상속하지 않는다면 같은 클래스의 인스턴스가 아니다.
    - 생성자 함수는 그렇지 않다.
        - 서로 다른 생성자 함수의 `prototype` 프로퍼티가 같은 프로토타입 객체를 참조 할 수도 있다.
        - 그리고 두 생성자가 같은 클래스의 인스턴스를 초기화할 수 있다.

1. 생성자는 프로토타입만큼 본질적이지는 않지만 클래스에서 공개적인(`public`) 부분을 담당한다.

    - 가장 명백하게 드러나는 점은 생성자 함수의 이름이 대부분 클래스 이름을 따른다는 것이다.
        - 예를 들어 `Range` 객체를 생성할 때 `Range()` 생성자를 사용한다.
    - 또한 객체가 클래스의 맴버인지 확인할 때 생성자는 `instanceof` 연산자의 오른쪽 피연산자가 된다.
        ```jsx
        r instanceof Range; // true r은 Range.prototype을 상속한다.
        ```

1. `instanceof`
    - 이 연산자의 왼쪽 피연산자는 테스트할 객체이며 오른쪽 피연산자는 생성자 함수이다.
    - 상속 여부를 확인한다.
        - 직접 상속하지 않아도 상관 없다.
        - 생성자 함수를 상속하는 객체를 상속하는 객체의 상속하는 객체를 상속한다고 하더라도 표현식은 `true`로 평가된다.
    - 엄밀히 말해 `instanceof` 연산자는 실제로 생성자 함수를 통해 초기화 됐는지는 체크하지 않는다.
        - 단지 `prototype`을 상속하는지만 체크한다.
        - 예를 들어 함수 `Strange()`를 정의하고 이 함수의 프로토타입을 `Range.prototype`으로 정했다면 `instanceof`는 `new Strange()`로 생성된 객체를 `Range` 객체로 판단한다.
            ```jsx
            function Strange() {}
            Strange.prototype = Range.prototype;
            new Strange() instanceof Range; // true
            ```

### 9.2.2 생성자 프로퍼티

1. 화살표 함수, 제너레이터 함수, 비동기 함수를 포함해 일반적인 자바스크립트 함수는 모두 생성자로 사용될 수 있고, 생성자를 호출할 때는 `prototype` 프로퍼티가 필요하다.

    - 따라서 일반적인 자바스크립트 함수는 모두 자동으로 `prototype` 프로퍼티를 갖습니다.
    - 이 프로퍼티의 값은 열거 불가인 `constructor` 프로퍼티 단 하나이다.
        - `constructor` 프로퍼티의 값은 함수 객체이다.

    ```jsx
    let F = function () {}; // 함수 객체이다.
    let p = F.prototype; // F에 연결된 프로토타입 객체이다.
    let c = p.constructor; // 프로토타입에 연결된 함수이다.
    c === F; // true :모든 F에 대해 F.prototype.constructor === F
    ```

1. `constructor` 프로퍼티를 가진 미리 정의된 프로토타입 객체가 존재한다는 것은, 그 객체가 일반적으로 자신의 생성자를 참조하는 `constructor` 프로퍼티를 상속한다는 의미이다.

    - 생성자는 클래스의 공개된 부분을 담당하므로 이 생성자 프로퍼티가 객체에 클래스를 부여한다.

    ```jsx
    let o = new F();
    o.construct === F; // true
    ```

1. 생성자 함수, 프로토타입 객체, 프로토타입에서 생성자를 향하는 역참조, 생성자에서 생성된 인스턴스 사이의 관계를 나타내는 그림

    ![IMG_783701AEFEC9-1.jpeg](./img/1.jpeg)

## 9.3 class 키워드를 사용하는 클래스

1. 자바스크립트는 최초 버전부터 클래스를 지원했지만 ES6에서 `class` 키워드를 도입하면서 마침내 고유의 문법을 갖게 되었다.

    ```jsx
    class Range {
        constructor(from, to) {
            this.from = from;
            this.to = to;
        }
        includes(x) {
            return this.from <= x && x <= this.to;
        }
        *[Symbol.iterator]() {
            for (let x = Math.ceil(this.from); x <= this.to; x++) yield x;
        }
        toString() {
            return "(" + this.from + "..." + this.to + ")";
        }
    }

    let r = new Range(1, 3);
    r.includes(2);
    r.toString();
    [...r];
    ```

    - `class` 키워드로 클래스를 선언하며 그 뒤에 클래스 이름을 쓰고 중괄호로 감싼 클래스 바디를 쓴다
    - 클래스 바디에는 객체 리터럴 메서드 단축 프로퍼티를 사용해 메서드를 정의하며 function 키워드를 생략한다.
        - 하지만 객체 리터럴과 달리 메서드를 콤마로 구분하지는 않는다.
        - 클래스 바디는 피상적으로 보면 객체 리터럴과 비슷해 보이지만 같지는 않다.
            - 특히 클래스 바디는 이름-값 쌍으로 프로퍼티를 정의하는 것을 허용하지 않는다.
    - `constructor` 키워드는 클래스의 생성자 함수를 정의한다.
        - 하지만 정의된 함수에 실제로 `‘constructor'` 라는 이름을 쓰지는 않는다.
        - `class` 선언문은 새 변수 `Range`를 정의하고 `constructor` 함수의 값을 그 변수에 할당한다.
    - 클래스에 초기화가 전혀 필요하지 않다면 `constructor` 키워드와 그 바디를 생략할 수 있으며, 이럴 경우 빈 생성자가 함수가 묵시적으로 생성된다.

1. 다른 클래스를 상속하는 서브클래스를 정의할 때 class 키워드와 함께 extends 키워드를 사용한다.

    ```jsx
    // Span은 Range와 비슷하지만 from과 to가 아니라 start와 length로 초기화된다.
    class Span extends Range {
        constructor(start, length) {
            if (length >= 0) {
                super(start, start + length);
            } else {
                super(start + length, start);
            }
        }
    }
    ```

1. `class` 문법에서 바로 들어나지는 않지만 반드시 알아야 할 중요한 점!
    - `“use strict”` 지시자가 없다 해도 `class` 선언의 바디는 모두 묵시적으로 스트릭트 모드로 동작한다
        - 따라서 바디 안에서는 8진수 정수 리터럴이나 `with` 문을 사용할 수 없으며, 변수를 사용하기 전에 선언하지 않으면 문법 에러가 일어난다.
    - 함수 선언과 달리 클래스 선언은 끌어올려지지 않는다.
        - 클래스 선언이 어떤 의미로는 함수 선언과 비슷하기는 하지만 호이스팅 동작까지는 공유하지 않는다.
        - 클래스를 선언하기 전에 인스턴스를 만들 수는 없다.

### 9.3.1 정적 메서드

1. 클래스 바디의 메서드 선언 앞에 `static` 키워드를 붙여 정적 메서드를 정의할 수 있다.

    - 정적 메서드는 프로토타입 객체의 프로퍼티가 아니라 생성자 함수의 프로퍼티로 정의된다.

    ```jsx
    class Range {
        constructor(from, to) {
            this.from = from;
            this.to = to;
        }
        static parse(s) {
            let matches = s.match(/^\((\d+)\.\.\.(\d+)\)&/);
            if (!matches) throw new Error(`Cannot parse Range from "${s}"`);
            return new Range(parseInt(matches[1]), parseInt(matches[2]));
        }

        // ....
    }
    ```

    - 이 코드가 정의하는 메서드는 `Range.prototype.parse()`가 아니라 `Range.parse()`이므로 인스턴스를 통해서는 호출할 수 없고 반드시 생성자를 통해 호출해야 한다.
        ```jsx
        let r = Range.parse("(1...10)"); // 새 Range 객체를 반환한다.
        r.parse("(1...10)"); // TypeError: r.parse는 함수가 아니다.
        ```
    - 정적메서드는 클래스/생성자의 이름을 써서 호출하기 때문에 클래스 메서드라고 부른다.
        - 이는 클래스 인스턴스에서 호출하는 일반적인 인스턴스 메서드와 구별하기 위해서 이다.
    - 정적 메서드는 인스턴스가 아니라 생성자에서 호출하기 때문에 정적 메서드에서 `this` 키워드를 사용하는 경우는 거의 없다.

### 9.3.2 게터, 세터, 기타 메서드 형태

1. 객체 리터럴과 마찬가지로 `class` 바디 안에서도 게터와 세터 메서드를 정의할 수 있다.

### 9.3.3 공개, 비공개, 정적 필드

1. 클래스 인스턴스에서 필드를 정의하려면 반드시 생성자 함수 안에서 정의하거나 메서드를 통해 정의해야 한다.

    - 필드란 객체 지향 프로그래밍에서 프로퍼티를 부르는 표현
    - 또한 클래스에 정적 필드를 정의하려면 반드시 클래스 바디 외부에서, 클래스 정의가 끝난 이후 정의해야 한다.

1. 인스턴스 필드와 정적 필드를 공개/비공개 형태로 정의할 수 있게 허용하는 클래스 문법 확장을 표준화 하는 작업은 진행 중이다.

    - 공개 인스턴스 필드 문법은 이미 리액트 프레임워크와 바벨 트랜스파일러를 사용하는 자바스크립트 프로그래머 사이에서 널리 사용되고 있다.
    - 다음과 같이 세 가지 필드를 초기화 하는 생성자를 써서 클래스를 만든다고 하자.

    ```jsx
    class Buffer {
        constructor() {
            this.size = 0;
            this.capacity = 4096;
            this.buffer = new Uint8Array(this.capacity);
        }
    }
    ```

    - 표준화가 얼마 남지 않은 새로운 인스턴스 필드 문법을 써보자

    ```jsx
    class Buffer {
        size = 0;
        capacity = 4096;
        buffer = new Uint8Array(this.capacity);
    }
    ```

    - 설명
        - 필드 초기화 코드를 생성자에서 꺼내 클래스 바디에 직접 포함했다.
            - 물론 이 코드는 생성자의 일부로 실행된다.
        - 할당의 왼쪽에 붙였던 `this.`가 사라졌지만 이 필드를 참조하기 위해 반드시 `this`를 사용해야 하는 것은 마찬가지이며, 초기화 표현식 할당의 오른쪽에도 `this`를 사용해야 한다.

1. 인스턴스 필드를 표준화하자는 제안에는 비공개 인스턴스 필드에 대한 표준도 포함되어 있다.

    - 앞에 예제에 사용한 인스턴스 필드 초기화 문법으로 이름이 `#`(일반적으로 자바스크립트 식별자에 쓸 수 없는 문자)로 시작하는 필드를 정의하면, 그 필드는 클래스 바디 안에서는 사용할 수 있지만 클래스 바디 외부에서는 볼 수 없고 접근할 수도 없으므로 자연스럽게 불변이 된다.

    ```jsx
    class Buffer {
        #size = 0;
        get size() {
            return this.#size;
        }
    }
    ```

1. `static` 키워드를 표준화하자는 내용 역시 제안되어 있다.
    - 공개 필드나 비공개 필드 선언 앞에 `static`을 추가하면 그 필드는 인스턴스 프로퍼티가 아니라 생성자 함수의 프로퍼티로 생성된다.

## 9.4 기존 클래스에 메서드 추가

1. 자바스크립트의 프로토타입 기반 상속 메커니즘은 동적이다.

    - 객체는 자신의 프로토타입에서 프로퍼티를 상속하며, 설령 객체를 생성한 후에 프로토타입의 프로퍼티가 바뀌더라도 상속 관계는 끊어지지 않는다.
    - 따라서 프로토타입 객체에 새 메서드를 추가하는 것만으로 자바스크립트 클래스를 확장할 수 있다.

1. 자바크립트에 내장된 클래스의 프로토타입 객체에도 같은 일을 할 수 있다.

    - 즉, 숫자와 문자열, 배열, 함수 등에 메서드를 추가할 수 있다.

1. 하지만 나중에 자바스크립트 새 버전에서 같은 이름의 메서드를 정의할 경우 혼란을 주거나 호환성 문제를 야기할 수 있으므로, 이렇게 내장 타입의 프로토타입에 메서드를 추가하는 건 일반적으로 좋은 방법은 아니다.
    - 특히 `Object.prototype`에 메서드를 추가하면 `for/in` 루프에 열거되기 때문에 더더욱 좋은 방법이 아니다.

## 9.5 서브클래스

1. 객체 지향 프로그래밍에서 클래스 B가 다른 클래스 A를 확장(`extend`)할 때 A는 슈퍼클래스, B는 서브클래스라고 부른다.
    - B의 인스턴스는 A의 메서드를 상속한다.
    - 클래스 B는 자신만의 메서드를 정의할 수 있고, 이 중 일부는 클래스 A에 있는 같은 이름의 메서드를 덮어 쓸 수 있다.
        - B의 메서드가 A의 메서드를 덮어 쓰는 경우, B에 존재하는 덮어 쓰는 메서드가 A에 존재하는 덮어 쓰인 메서드를 호출해야 할 때가 많다.
    - 마찬가지로 서브클래스 생성자 B()는 일반적으로 슈퍼클래스 생성자 A()를 호출해 그 인스턴스가 완전히 초기화 됐는지 확인한다.

### 9.5.1 서브클래스와 프로토타입

1. [예제 9-2의 Range 클래스](https://www.notion.so/9-5cd8e3b40b2c4448b9468969f7ce4b2c)의 서브클래스인 `Span`을 만들어 보자!

    - `start`와 `span`으로 초기화
    - `Span` 클래스의 인스턴스는 `Range` 슈퍼클래스의 인스턴스이기도 하다.
    - `Span` 인스턴스는 `Span.prototype`에서 커스터마이징한 `toString()`메서드를 상속하지만, `Range`의 서브클래스이기도 하므로 `Range.prototype`에서 `includes()` 같은 메서드도 상속한다.

    ```jsx
    // 서브클래스에서 사용할 생성자 함수이다.
    function Span(start, span) {
        if (span >= 0) {
            this.from = start;
            this.to = start + span;
        } else {
            this.to = start;
            this.from = start + span;
        }
    }

    // Span 프로토타입은 Range 프로토타입을 상속한다.
    Span.prototype = Object.create(Range.prototype);

    // Range.prototype.constructor는 상속하지 않으므로 생성자 프로퍼티는 따로 정의한다.
    Span.prototype.constructor = Span;

    // Span은 toString() 메서드를 따로 정의하므로 Range의 toString()을 상속하지 않고 덮어쓴다.
    Span.prototype.toString = function () {
        return `(${this.from}... +${this.to - this.from})`;
    };
    ```

1. `Span.prototype = Object.create(Range.prototype)`

    - `Span()` 생성자로 생성된 객체를 `Span.prototype` 객체를 상속한다.
        - `Span.prototype`은 `Range.prototype`을 상속하므로 `Span` 객체는 `Span.prototype`과 `Range.prototype`을 모두 상속한다.
    - `Span()` 생성자는 `Range()` 생성자롸 마찬가지로 `from`과 `to`프로퍼티를 생성하므로 새 객체를 초기화하기 위해 `Range()` 생성자를 호출할 필요는 없다.

1. 서브클래스 메커니즘을 빈틈없이 만들기 위해서는 클래스에서 슈퍼클래스에 메서드와 생성자를 호출할 수 있게 허용해야 하지만, ES5 이전의 자바스크립트에는 이를 단순하게 처리할 방법이 없었다.
    - ES6에서는 `class` 문법에 `super` 키워드를 도입하여 이 문제를 해결했다.

### 9.5.2 extends와 super를 사용하는 서브클래스

1. ES6 이후에는 클래스 선언에 `extends` 절을 추가하기만 해도 서브클래스를 만들수 있으며 내장 클래스에도 이런 동작이 허용된다.

    ```jsx
    // 첫 번째와 마지막 요소에 게터를 추가하는 서브클래스
    class EZArray extends Array {
        get first() {
            return this[0];
        }
        get last() {
            return this[this.length - 1];
        }
    }

    let a = new EZArray();
    a instanceof EZArray; // true: a는 서브클래스의 인스턴스이다.
    a instanceof Array; // true: a는 동시에 슈퍼클래스의 인스턴스이다.
    a.push(1, 2, 3, 4); // a.length === 4: 상속된 메서드를 사용할 수 있다.
    a.first; // 1: 서브클래스에서 정의한 첫 번째 요소 게터이다.
    a.last; // 4: 서브클래스에서 정의한 마지막 요소 게터이다.
    a[1]; // 2: 일반적인 배열 접근 문법도 동작한다.
    Array.isArray(a); // true: 서브클래스 인스턴스도 배열이다.
    EZArray.isArray(a); // true: 서브클래스는 정적 메서드 역시 상속한다.
    ```

    - `EZArray` 서브클래스는 두 가지 단순한 게터 메서드만 정의한다.

        - `EZArray` 인스턴스는 일반적인 배열과 마찬가지로 동작하므로 `push(), pop(), length` 같은 메서드와 프로퍼티를 상속해 사용할 수 있다.
        - 이 서브클래스에는 `first`와 `last` 게터도 만들었다.
        - `Array.isArray` 같은 정적 메서드도 상속된다.

        ```jsx
        // EZArray.prototype이 Array.prototype을 상속하므로 인스턴스 메서드를 상속한다.
        Array.prototype.isPrototypeOf(EZArray.prototype); // true

        // EZArray가 Array를 상속하므로 EZArray는 배열의 정적 메서드와 프로퍼티 역시 상속한다.
        Array.isPrototypeOf(EZArray); // true
        ```

2. 좀 더 발전한 예제

    - 내장된 `Map` 클래스에 `TypedMap` 서브클래스를 만들어 `typeof`를 통해 키와 값이 지정된 타입인지 체크하는 기능을 추가.
    - `super` 키워드를 사용해 생성자와 슈퍼클래스 메서드를 호출.

    ```jsx
    class TypedMap extends Map {
        constructor(keyType, valueType, entries) {
            // entries가 지정됐으면 타입을 체크한다.
            if (entries) {
                for (let [k, v] of entries) {
                    if (typeof k !== keyType || typeof v !== valueType) {
                        throw new TypeError(
                            `Wrong type for entry [${k}, ${v}]`
                        );
                    }
                }
            }

            // 타입을 체크한 entries로 슈퍼클래스를 초기화한다.
            super(entries);

            // 타입을 저장하면서 서브클래스를 초기화한다.
            this.keyType = keyType;
            this.valueType = valueType;
        }

        // 맵에 추가되는 새 항목마다 타입을 체크하도록 set() 메서드를 재정의한다.
        set(key, value) {
            // 키나 값이 지정된 타입이 아니라면 에러를 일으킨다.
            if (this.keyType && typeof key !== this.keyType) {
                throw new TypeError("~");
            }
            if (this.valueType && typeof value !== this.valueType) {
                throw new TypeError("~");
            }

            // 타입이 정확하면 슈퍼클래스의 set() 메서드를 호출해서 맵에 항목을 추가한다.
            // 그리고 슈퍼클래스 메서드가 반환하는 것을 그대로 반환한다.
            return super.set(key, value);
        }
    }
    ```

    - `super`
        - 생성자가 `super` 키워드를 함수 이름인 것처럼 사용해 슈퍼클래스 생성자를 호출한다.
        - `Map()` 생성자는 `[key, value]` 배열로 구성된 이터러블 객체를 선택 사항인 인자로 받는다.
            - `TypedMap()` 생성자의 선택 사항인 세번째 인자는 `Map()` 생성자의 첫 번째 인자가 되고 `super(entries)`로 슈퍼클래서 생성자에 전달된다.
    - 생성자에서 `super()`를 사용할 때 알아야 할 중요한 규칙
        - `extends` 키워드로 클래스를 정의하면 클래스 생성자는 슈퍼클래스 생성자를 호출할 때 반드시 `super()`를 사용해야한다.
        - 서브클래스에 생성자를 정의하지 않으면 자동으로 생성된다.
            - 이렇게 묵시적으로 정의된 생성자는 전달된 값을 그대로 `super()`에 전달한다.
        - `super()`를 써서 슈퍼클래스 생성자를 호출하기 전에는 생성자 안에서 `this` 키워드를 사용하지 말아야 한다.
            - 이 규칙을 따르면 서브클래스보다 슈퍼클래스를 먼저 초기화해야 한다는 규칙도 지킬 수 있다.
        - `new` 키워드 없이 호출한 함수에서는 표현식 `new.target`의 값이 `undefined`이다.
            - 반면 생성자 함수에서 `new.target`은 호출된 생성자를 참조한다.
            - 서브클래스 생성자를 호출하고 `super()`로 슈퍼클래스 생성자를 호출하면, 슈퍼클래스 생성자는 `new.target`을 통해 서브클래스 생성자를 참조할 수 있다.
    - `set()`
        - `Map` 슈퍼클래스에는 맴에 새 항목을 추가하는 `set()`메서드가 있다.
            - `TypedMap`의 `set()` 메서드가 슈퍼클래스의 `set()` 메서드를 덮어 쓴다
        - 서브클래스의 `set()` 메서드는 맴에 키와 값을 추가하는 방법을 모르지만 슈퍼클래스의 `set()` 메서드가 할 수 있다.
            - 따라서 `super` 키워드를 다시 사용해 슈퍼클래스의 메서드를 호출한다.
        - 이 컨텍스트에서 `super`는 `this` 키워드처럼 슈퍼클래스를 참조하므로 슈퍼클래스에 정의된 메서드에 접근할 수 있다.
    - `this`에 접근해 새 객체를 초기화하기 전에 생성자에서 먼저 슈퍼클래스 생성자를 호출해야 하지만, 메서드를 덮어 쓸 때는 그런 규칙이 적용되지 않는다.
        - 슈퍼클래스 메서드를 덮어 쓰는 메서드가 슈퍼클래스 메서드를 호출할 필요는 없다.
        - 덮어 쓰는 메서드의 어디에서든 `super`를 사용해 덮어 쓰인 메서드나 기타 슈퍼클래스 메서드를 호출할 수 있다.

### 9.5.3 위임과 상속

1. 다른 클래스의 동작을 공유하는 클래스를 원한다면 서브클래스를 만들어 동작을 상속할 수도 있지만, 클래스에서 다른 클래스의 인스턴스를 만들고 그 인스턴스에 우리가 원하는 동작을 위임하는 것이 더 쉽고 유연한 방법일 때도 많다.

    - 다른 클래스의 래퍼를 만들거나 합성을 통해서도 새 클래스를 만들 수 있다.
    - 동작을 위임하는 방식을 ‘합성’이라 부르며 객체 지향 프로그래밍에는 ‘상속보다 합성을 우선하라’ 라는 격언이 자주 인용된다.

1. 예제 (`Histogram` 클래스)

    - 자바스크립트의 `Set`클래스와 비슷하지만, 세트에 어떤 값이 있는지 그치지 않고 각 값이 몇번 추가 됐는지도 추적하려고 함.
    - `count()` 메서드는 값과 그 값이 추가된 횟수를 연결해야 하므로 세트보다는 맵에 더 가까움.
        - 따라서 세트와 서브클래스를 만들기 보단 내장된 `Map` 객체에 실행을 위임하는 `API`를 가진 클래스를 만드는게 더 좋음.

    ```jsx
    /**
     * 세트와 비슷하지만 추가되는 값마다 몇 번 추가됐는지 추척하는 클래스이다.
     * 세트와 마찬가지로 add()와 remove() 메서드가 있고,
     * count() 메서드는 주어진 값이 몇 번 추가됐는지 반환한다.
     * 기본 이터레이터는 최소 한 번 이상 추가된 값을 전달(yield)한다.
     * [value, count] 쌍을 순회할 때는 entries()를 사용한다.
     */
    class Histogram {
        // 위임할 Map객체를 만든다.
        constructor() {
            this.map = new Map();
        }

        // 키가 추가된 횟수는 맵에 존재하며, 맵에 없으면 0이다.
        count(key) {
            return this.map.get(key) || 0;
        }

        // 세트 비슷한 메서드 has()는 count가 0이 아닌 값이면 true를 반환한다.
        has(key) {
            return this.count(key) > 0;
        }

        // 히스토그램 크기는 맵에 있는 항목 수와 같다.
        get size() {
            return this.map.size;
        }

        // 키를 추가하면 맵에서 count를 증가시킨다.
        add(key) {
            this.map.set(key, this.count(key) + 1);
        }

        // 키 삭제는 맵의 count가 0일 때만 키를 삭제한다.
        delete(key) {
            let count = this.count(key);
            if (count === 0) {
                this.map.delete(key);
            } else if (count > 1) {
                this.map.set(key, count - 1);
            }
        }

        // 히스토그램을 순회하면 저장된 키만 반환한다.
        [Symbol.iterator]() {
            return this.map.key();
        }

        // 나머지 이터레이터 메서드는 Map 객체에 위임한다.
        keys() {
            return this.map.keys();
        }
        values() {
            return this.map.values();
        }
        entries() {
            return this.map.entries();
        }
    }
    ```

    - `Histogram()` 생성자는 모두 `Map` 객체를 만든다.
        - 대부분의 메서드는 맵의 메서드에 동작을 위임하므로 간단히 구현할 수 있다.
    - 상속이 아니라 위임을 사용했으므로 `Histogram` 객체는 세트나 맵의 인스턴스가 아니다.

### 9.5.4 클래스 계층 구조와 추상 클래스

1. 서로 연관된 서브클래스 그룹에서 슈퍼클래스 구실을 하는 추상 클래스 예제를 만들자

    - 추상클래스란 완전히 구현되지 않은 클래스를 의미한다.
        - 추상 슈퍼클래스에서 정의하는 부분 구현을 서브클래스 전체가 상속하고 공유할 수 있다.
    - 서브클래스는 슈퍼클래스가 구현하지않고 정의한 추상 메서드를 구현하여 자신만의 고유한 동작을 정의할 수 있다.

    ```jsx
    class AbstractSet {
        has(x) {
            throw new Error("Abstract method");
        }
    }

    class NotSet extends AbstractSet {
        constructor(set) {
            super();
            this.set = set;
        }

        has(x) {
            return !this.set.has(x);
        }
        toString() {
            return `{ x| x ∉ ${this.set.toString()} }`;
        }
    }

    class RangeSet extends AbstractSet {
        constructor(from, to) {
            super();
            this.from = from;
            this.to = to;
        }

        has(x) {
            return x >= this.from && x <= this.to;
        }
        toString() {
            return `{ x| ${this.from} ≤ x ≤ ${this.to} }`;
        }
    }

    class AbstractEnumerableSet extends AbstractSet {
        get size() {
            throw new Error("Abstract method");
        }
        [Symbol.iterator]() {
            throw new Error("Abstract method");
        }

        isEmpty() {
            return this.size === 0;
        }
        toString() {
            return `{${Array.from(this).join(", ")}}`;
        }
        equals(set) {
            if (!(set instanceof AbstractEnumerableSet)) return false;

            if (this.size !== set.size) return false;

            for (let element of this) {
                if (!set.has(element)) return false;
            }

            return true;
        }
    }

    class SingletonSet extends AbstractEnumerableSet {
        constructor(member) {
            super();
            this.member = member;
        }

        has(x) {
            return x === this.member;
        }
        get size() {
            return 1;
        }
        *[Symbol.iterator]() {
            yield this.member;
        }
    }

    class AbstractWritableSet extends AbstractEnumerableSet {
        insert(x) {
            throw new Error("Abstract method");
        }
        remove(x) {
            throw new Error("Abstract method");
        }

        add(set) {
            for (let element of set) {
                this.insert(element);
            }
        }

        subtract(set) {
            for (let element of set) {
                this.remove(element);
            }
        }

        intersect(set) {
            for (let element of this) {
                if (!set.has(element)) {
                    this.remove(element);
                }
            }
        }
    }

    class BitSet extends AbstractWritableSet {
        constructor(max) {
            super();
            this.max = max;
            this.n = 0;
            this.numBytes = Math.floor(max / 8) + 1;
            this.data = new Uint8Array(this.numBytes);
        }

        _valid(x) {
            return Number.isInteger(x) && x >= 0 && x <= this.max;
        }

        _has(byte, bit) {
            return (this.data[byte] & BitSet.bits[bit]) !== 0;
        }

        has(x) {
            if (this._valid(x)) {
                let byte = Math.floor(x / 8);
                let bit = x % 8;
                return this._has(byte, bit);
            } else {
                return false;
            }
        }

        insert(x) {
            if (this._valid(x)) {
                let byte = Math.floor(x / 8);
                let bit = x % 8;
                if (!this._has(byte, bit)) {
                    this.data[byte] |= BitSet.bits[bit];
                    this.n++;
                }
            } else {
                throw new TypeError("Invalid set element: " + x);
            }
        }

        remove(x) {
            if (this._valid(x)) {
                let byte = Math.floor(x / 8);
                let bit = x % 8;
                if (this._has(byte, bit)) {
                    this.data[byte] &= BitSet.masks[bit];
                    this.n--;
                }
            } else {
                throw new TypeError("Invalid set element: " + x);
            }
        }

        get size() {
            return this.n;
        }

        *[Symbol.iterator]() {
            for (let i = 0; i <= this.max; i++) {
                if (this.has(i)) {
                    yield i;
                }
            }
        }
    }

    BitSet.bits = new Uint8Array([1, 2, 4, 8, 16, 32, 64, 128]);
    BitSet.masks = new Uint8Array([~1, ~2, ~4, ~8, ~16, ~32, ~64, ~128]);
    ```
