## 빌트인 객체
- 자바스크립트는 객체 기반의 스크립트 언어이며, 자바스크립트를 이루고 있는 거의 "모든 것"이 객체이다.
- 자바스크립트 객체는 크게 3가지로 구분 가능하다.
  - 표준 빌트인 객체 : ECMAscript 사양에 정의된 객체
  - 호스트객체 : ECMAscript에 정의 되지 않은 javascript에서 제공하는 객체
  - 사용자 정의 객체 : 사용자가 직접 정의한 것  

### 표준 빌트인 객체
Object, String, Number, Boolean, Function, Array, Date, RegExp, Promise, Symbol, Map, WeakMap, Set, WeakSet, Math, Reflect, JSON, Error  

### 원시값과 래퍼 객체
String, Number와 같은 객체가 필요한 이유는 원시 타입도 객체로 지원 하기 위해서이가.
```javascript
const str = 'hello';
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```
이렇게 가능한 이유는 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해주기 떄문입니다. 이것을 래퍼 객체라고 부릅니다.  

### 전역 객체
- 전역 객체는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이다.
- parseInt, encodeURI와 같이 바로 사용할수 있는 .


## this
- this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수이다.

1. 객체 리터럴
~~~javascript
const circle = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};
~~~
2. 생성자 함수
~~~javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
  console.log(this); // Circle {}

  this.radius = radius;
  this.getDiameter = function () {
    // 2. this에 바인딩된 인스턴스를 통해 프로퍼티나 메서드를 참조할 수 있다.
    return 2 * this.radius;
  };
}
~~~

this 호출하는 예제
~~~javascript
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
~~~
하지만 staticmode가 적용 된 경우 일반 함수에서는 this가 undefinded가 바인딩 된다.

1. 일반 함수 호출 : 전역 객체가 바인딩 된다.static 모드인경우 undefinded , 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
2. 매서드 호출 : 메서드를 호출한 객체가 바인딩 된다.
~~~javascript
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee
~~~
매서드는 프로퍼티에 바인딩 되어 있기 때문에 메서드 내부에서 this를 사용하면 메서드를 호출한 객체에 바인딩 된다.
3. 생성자 함수 호출 : 생성자 함수 내부의 this에는 생성할 인스턴스가 바인딩 된다.
~~~javascript
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
      return 2 * this.radius;
    };
}

const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
~~~
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출 : apply, call, bind 메서드에 첫번째 인수로 전달한 객체에 바인딩 된다.

## 실행 컨텍스트
- 실행 컨텍스트는 자바스크립트의 동작원리를 담고 있는 핵심 개념이다.

### 소스코드의 타입
- 소스코드의 타입에 따라 실행 컨텍스트를 구분한다.
- 소스코드의 타입은 크게 4가지로 구분할 수 있다.
  - 전역 코드 : 전역에 존재하는 소스코드
  - 함수 코드 : 함수 내에 존재하는 소스코드
  - eval 코드 : 빌트인 전역 함수인 eval 함수에 인수로 전달되어 실행되는 소스코드
  - 모듈 코드 : 모듈 뱔로 독립적인 모듈 스코프를 생성한다.

### 소스코드의 평가와 실행
- 평가와 실행 두가지 과정으로 나뉜다. 평가에서 실행 컨텍스트를 생성하고 변수, 함수 등 선언 문만 먼저 실행한다. 실행 컨텍스트가 관리하는 스코프에 등록한다.
- 평가가 끝나면 순차 적으로 실행한다.

~~~javascript
var x;
x = 1;
~~~

평가 과정에서 x를 실행 컨텍스트의 스코프에 넣고 undefined를 할당한다. 그리고 실행 과정에서 1을 할당한다.  

- 실행 컨텍스트란?
  - 소스 코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.
  - 식별자(변수, 함수, 클래스 등의 이름)를 등록하고 관리하는 스코프와 코드 실행 순서 관리를 구현한 내부 메커니즘으로 이다.
  - 실행 컨텍스트 스택이 코드의 실행 순서를 관리한다.

### 렉시컬 환경
식별자와 식별자에 바인딩된 값들 즉 스코프와 식별자를 관리한다.
스코프를 구분하여 식별자를 등록하고 관리하는 저장소 역활이다.
렉시컬 스코프는 함수를 어디서 호출했냐가 아닌 어디서 정의 했냐 이다.


### 클로저
함수가 선언된 시점의 렉시컬 환경을 기억하여 함수가 실행될 때 자신의 렉시컬 환경 외부에서도 식별자를 참조할 수 있게 하는 기능이다.  

함수가 정의된 된 환경과 호출되는 환경에 따라 다른데 호출되는 위치와 상관 없이 가능하게 하기 위해 함수 자신의 내부 슬록에 자신을 정의해야 한다.  

~~~javascript
const x = 1;

function foo() {
  const x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1 
bar(); // 1
~~~


예제를 보자
~~~javascript
const x = 1;

function outer() {
  const x = 10;

  function inner() {
    console.log(x);
  }

  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer();
innerFunc(); // 10
~~~

중첩 함수를 사용하게 되면 수 x는 상위 스코프의 x를 바라 본다 또한 외부 함수 보다 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참고할수 있다 이를 클로저라고 한다.  

outer 함수의 실행 컨텍스트는 실행 컨텍스트 스탭에서 제거되지만 ouste 함수의 렉시컬 환경 까지는 소멸되지 않는다.

상위의 어떤 식별자도 참조 하지 않는 경우는 클로저가 아니다.  

* 결론적으로 클로저는 중첩 함수가 상위 스코프의 식별자를 참고 하고 있고 중첩 함수가 외부 함수 보다 오래 유지 되는 경우 이다.  

### 클로저의 활용
- 상태 유지
- 전역 변수의 사용 억제
- 정보의 은닉


~~~javascript
const counter = (function () {
  let num = 0;
  
    return {
      increase() {
        return ++num;
        },
        decrease() {
            return --num;
            }
    };
}());

console.log(counter.num); // undefined
console.log(counter.increase()); // 1
console.log(counter.decrease()); // 0
~~~

이런 방식으로 초기화랑 상태 유지에 사용할수 있다.  

고차 함수와 콜라보 한 예시이다.
~~~javascript
function makeCounter(predicate) {
  let num = 0;

  return function () {
    num = predicate(num);
    return num;
  };
}

function increase(n) {
  return ++n;
}

function decrease(n) {
  return --n;
}

const increaser = makeCounter(increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

const decreaser = makeCounter(decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
~~~
                    

## 클래스
- 클래스는 생성자 함수의 역활을 하는 함수다.
- 샤로운 객체 생성 매커니즘으로 보면 된다.

### 예시
~~~javascript
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log('Hello!');
  }
}

const me = new Person('Lee');

me.sayHi(); // Hi! My name is Lee
Person.sayHello(); // Hello!
~~~

클래스는 호이스팅 되지만 정의 이전에 호출 할수 없다 왜일까?  let const 키워드와 마찬가지로 선언문 이전에 일시적 사각지대에 빠지기 떄문에 호이스팅이 발생하지 않는것 처럼 동작한다.
