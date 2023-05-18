## 프로퍼티 접근법

~~~javascript
const person = {
  name: 'Lee',
  address: 'Seoul'
};

console.log(person.name); // Lee
console.log(person['name']); // Lee
~~~
마침표 표기법과 대괄호 표기법이 있다.  

## 원시값과 객체
원시 타입 : 변경이 불가능한 값 . 변수에 할당 되면 확보된 메모리 공간에 실제 값이 저장된다.    
객체 타입 : 변경이 가능한 값 . 변수에 할당 하면 확보된 메모리 공간에 값이 저장되는 것이 아니라 값이 저장된 메모리 공간의 주소가 저장된다.  

### 원시 값  
~~~javascript
const a = 1;
a = 2; // TypeError: Assignment to constant variable.

const o = {};
o.name = 'Lee'; // {name: 'Lee'}
~~~

## 문자열과 불변성
~~~javascript
var str = 'hello';
str = 'world'; // 'world'
~~~

첫번쨰 문이 실행 되면서 str은 hello라는 문자가 저장된 메모리 공간을 가리킨다. 두번째 열에서는 str이 가리키는 메모리 공간의 값을 변경하는 것이 아니라 새로운 메모리 공간을 확보하고 world라는 문자를 저장한다. 그리고 str은 새로 확보된 메모리 공간을 가리킨다.  

### 유사 배열 객체
마치 배열처럼 인텍스로 프로퍼티에 접근할수 있고 length 를 갖는 개체이다.
~~~javascript

var str = 'hello';

// 원시 값인 문자열이 객체처럼 동작한다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO

console.log(str[0]); // h

str[0] = 'H';
console.log(str[0]); // h
// 읽기 전용이라서 변경 불가능하다 원시값 바꾸고 싶으면 재 할당을 해야 한다.
~~~
원시 값을 객체처럼 사용하면 원시 값을 감싸는 래퍼 객체로 자동 변환 된다.

## 얕은 복사와 깊은 복사
얕은 복사는 한 단계 까지만 복사하는 것을 말하고 깊은 복사는 중첮되어 있는 모든 복사를 말한다.
~~~javascript
const o = { x:{y:1}};

// 얖은 복사
//const c1 = Object.assign({}, o);
const c1 = {...o};
console.log(c1 === o); // false
console.log(c1.x === o.x); // true

// lodash 깊은 복사
const _ = require('lodash');
const c2 = _.cloneDeep(o);
console.log(c2 === o); // false
console.log(c2.x === o.x); // false
~~~
깊은 복사로 생성된 객체는를 별대로 만든 것으로 복사본을 만드는 것이다.  

원시값을 할당한 변수를 다른 변수로 할당하는 것은 깊은 복사, 객체를 할당한 변수를 다른 변수에 할당하는 것을 얖은 복사라고 하는 경우도 있다.  

~~~javascript
const v =1;
// 깊은 복사
const v2 = v;
console.log(v === v2); // true

// 얕은 복사

const o = { x:1};
const o2 = o;
console.log(o === o2); // true
~~~


## const (arrow function)  vs function
~~~javascript
import React from 'react';

const TestPage = () => {
  return(
    <div>
      TEST
    </div>
  )
}

export default TestPage;
~~~

~~~javascript
import React from 'react';

function TestPage() {
  return(
    <div>
      TEST
    </div>
  )
}

export default TestPage;
~~~

2개는 뭐가 다를까?

(1) const 
~~~javascript
const printThing = () => {
  console.log(`Random number : ${getRandom()}`)
}

printThing()

const getRandom = () => (
  ((Math.random() * 10000).toString().split('.')[0])
)
~~~
ReferenceError: Cannot access 'getRandom' before initialization 발생
ES6에서 let/const의 호이스팅이 되지 않는다는 특징 때문에 발생하는 문제다
호이스팅 관련한 부분을 참고하자면 이렇다

developer0809.tistory.com/23


(2) function
~~~javascript
printThing()

function printThing() {
  console.log(`Random number : ${getRandom()}`)
}

function getRandom() {
  return (
    ((Math.random() * 100).toString().split('.')[0])
  )
}
~~~
printThing() 함수가 호이스팅 돼서 랜덤 숫자가 잘 출력된다

## 함수 리터럴
함수도 함수의 리터럴을 생성할수 있다.

~~~javascript
var f = function add(x,y) {
  return x + y;
};
~~~
함수 리터럴 에서는 함수의 이름을 생략 할수 있지만 함수 선언문은 함수 이름을 생략할 수 없다.
f를 식별자라고 하고 이것을 호출한 것이다.
자바스크립트 함수는 일급 객체이다. 함수가 일급 객체라는 것은 함수의 값을 자유롭게 사용할수 있다는 의미이다.  

## 함수 생성 시점과 함수 호이스팅

~~~javascript
// 함수 참조
console.dir(add); // f add(x,y) { return x + y; }
console.dir(sub); // undefined

// 함수 호출
console.log(add(2,5)); // 7
console.log(sub(2,5)); // TypeError: sub is not a function

// 함수 선언문
function add(x,y) {
  return x + y;
}

// 함수 표현식
const sub = function (x,y) {
  return x - y;
}

~~~

왜 이런 결과가 나오는가?  

2개의 생성 시점이 다르기 떄문이다. 함수 호이스팅과 변수 호이스팅의 차이이다.  
변수 할당문의 값은 할당문이 실행되는 시점 . 즉 런타임에 평가가 되기 떄문에 런타임때에는 함수 객체가 그전에는 변수 객체이다.  함수 표현식에 오기 전까지는 변수 호이스팅이 된다.    

## 함수 호출
~~~javascript
function add(x,y) {
  return x + y;
}
console.log(add(2,5)); // 7

console.log(add(2)); // NaN

console.log(add(2,5,10)); // 7
~~~

초과된 인수는 버려지는 것이 아니라 모든 인수는 암묵적으로 arguments 객체의 프로퍼티로 보관된다. 그래서 맨 아래 결과가 7이 나오는 것이다  

다음과 같이 변수의 할당과 동시에 초기화도 가능하다.  
~~~javascript
function add(a=0,b=0,c=0) {
  return a + b + c;
}

console.log(add(1,2,3)); // 6
console.log(add(1,2)); // 3
console.log(add(1)); // 1
console.log(add()); // 0
~~~

함수 반환문에서 return 은 생략이 가능하며 undefined이다.
~~~javascript
function foo() {
  return;
}

function foo() {
}

console.log(foo()); // undefined
~~~

## 옵저버 패턴
옵저버 패턴은 주체 객체의 상태가 변경되면 주체 객체는 자신에게 등록된 옵저버에게 통지한다. 이때 주체 객체는 옵저버가 갖고 있는 특정 메서드를 호출한다. 이때 주체 객체는 통지를 하는 것이지 옵저버에 대해 직접적으로 어떤 행동을 취하는 것은 아니다.  
https://cocococo.tistory.com/entry/%EC%98%B5%EC%A0%80%EB%B2%84Observer-%ED%8C%A8%ED%84%B4-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EA%B5%AC%ED%98%84-%EB%B0%A9%EB%B2%95Java-JavaScript
https://woong-jae.com/javascript/220322-observer-pattern  

## 콜백 함수 고차 함수
고차 함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다.
~~~javascript
// 외부에서 전달 받은 f를 n만큼 반복 호출한다.
function repeat(n,f) {
  for(var i = 0; i < n; i++) {
    f(i);
  }
}

var logAll = function (i) {
  console.log(i);
}

// repeat 은 고차 함수 이고 repeat은 콜백 함수 이다.
// 고차 함수는 매개 변수를 통해 전달받은 콜백 함수의 호출 시점을 결정해서 호출한다.
repeat(5,logAll); // 0 1 2 3 4

~~~

콜백함수를 사용한 대표적인 케이스로 이벤트 처리가 있다.
~~~javascript
document.getElementById('myButton').addEventListener('click',function() {
  console.log('button clicked!');
});
~~~

## 스코프

스코프는 식별자가 유효한 범위를 말한다.  
코드는 전역과 지역으로 구분 가능하다. 전역은 어디서든 참고 가능하고 지역 스코프는 함수 몸체 내부에 만들어진다.
~~~javascript
var x = 'global';
function foo() {
  var x = 'local';
  console.log(x); // local
}
// x , foo는 전역 스코프에 해당 되고
// foo 안에 있는 x는 지역 스코프에 해당 된다.
~~~ 

함수를 전역으로 정의 할수 있고 함수 몸체 내부에서 정의할수도 있다. 스코프가 함수의 중첩에 의해 계층적 구조를 갖는다는 것을 의미한다.
이것을 스코프 체인이라고 한다.  

코드 블록으로 지정된 영역을 블록 스코프 함수로 된것은 함수 스코프라고 한다.   
var를 선언된 변수는오로지 함수의 코드 블록만을 지역 스코프로 인정하지만 es6에 도입된 let, const 는 블록 레벨 스코프를 지원한다.  

## 렉시컬 스코프
~~~javascript
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
~~~

2가지 패턴이 있다.  
1. 함수를 어디서 호출 했는지에 따라 상위 스코프를 결정한다. (동적 스코프)
2. 함수를 어디서 정의 했는지에 따라 상위 스코프를 정의 한다.

bar 함수의 상위 스코프는 foo 함수이다. 하지만 bar 함수는 foo 함수 내부에서 정의된 것이 아니라 전역에서 정의된 것이다.  

렉시컬 스코프는 클로저와 깊은 관계가 있다.  

## 전역 변수의 사용을 억제 하는 방법
(1) 즉시 실행 함수 사용  
즉시 실행 함수로 감싸면 실행함수의 지역 변수가 된다. 하지만 실무에서 글쎄??? 그래서 라이브러리에서 주로 사용하는 방법이다.  
~~~javascript
(function () {
  var foo = 10; // 즉시 실행 함수의 지역 변수
  // ...
}());

console.log(foo); // ReferenceError: foo is not defined
~~~

(2) 네임스페이스 객체 사용
~~~javascript
var MYAPP = {}; // 전역 네임스페이스 객체

MYAPP.name = 'Lee';

console.log(MYAPP.name); // Lee
~~~

저녁에 네임 스페이스 역활을 담당하는 것을 만들고 사용하고 싶은 변수의 프로퍼티를 추가 하는 방식이다.  

(3) 모듈 패턴 사용
클로저를 기반으로 동작하고. 전역 변수의 억제는 물론 캡슐화까지 구현 할수 있다는 것이다.  
~~~javascript
var Counter = (function () {
  // private 변수
  var num = 0;

  // 외부로 공개할 데이터나 메서드를 프로퍼티로 추가한 객체를 반환한다.
  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    }
  };
}());

console.log(Counter.num); // undefined

console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
console.log(Counter.decrease()); // 0
~~~

실행 함수는 객체를 반환한다. 하지만 내부의 변수를 외부에서 접근하지 못하게 되기 떄문에 private 변수가 된다.  

(4) ES6 모듈 사용   
~~~javascript
// lib.js
export const pi = Math.PI;

export function square(x) {
  return x * x;
}

export class Person {
  constructor(name) {
    this.name = name;
  }
}

// app.js
import { pi, square, Person } from './lib';

console.log(pi); // 3.141592653589793
console.log(square(10)); // 100
console.log(new Person('Lee')); // Person { name: 'Lee' }
~~~

## 변수
### let
(1) 중복 선언 금지
~~~javascript
var foo = 123;
var foo = 456; // 에러가 발생하지 않는다.

let bar = 123;
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
~~~

(2) 블록 레벨 스코프
~~~javascript
let foo = 1; // 전역 변수

{
  let foo = 2; // 전역 변수
  let bar = 3; // 전역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined

~~~

let으로 선언 된것은 블록 레벨을 따른다.  
(3) 변수 호이스팅  
var는 선언 단계와 초기화 단계가 한번에 이루어진다. 하지만 let은 선언 단계와 초기화 단계가 분리되어 진행된다.
~~~javascript
console.log(foo); // ReferenceError: foo is not defined
let foo;
~~~
ㄴ 선언단계               - ReferenceError  
ㄴ 일시적 사각 지대(TDZ)    - ReferenceError  
let foo; ㄴ 초기화 단계 - foo === undefined  
foo = 1; ㄴ 할당 단계 - foo === 1  

var로 선언한 전역 변수, 전역함수, 선언하지 않은 변수는 암묵적으로 전역 객체인 window의 프로퍼티가 된다  
전역 객체의 프로퍼티를 참조 할때 window를 생략할수있다.  

let 키워드로 선언된 전역 변수는 전역 객체의 프로퍼티가 아니다. let 전역 변수는 보이지 않는 개념의 블록 전역 렛시컬 환경의 선언적 환경 레코드에 들어간다.  


### const
const는 상수를 선언하기 위해 사용한다. 기본적으로는 let과 동일하다.  
(1) 선언과 초기화, 재할당 금지
~~~javascript
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
~~~
(2) 상수
상수는 재할당이 금지된 변수를 말한다.
~~~javascript
const TAX_RATE = 0.1; // 선언과 동시에 할당

let preTaxPrice = 100; // 선언과 동시에 할당

// 세후 가격을 계산하는 함수
function calculateTotalPrice(preTaxPrice) {
  // const, let으로 선언된 변수는 블록 레벨 스코프를 갖는다.
  return preTaxPrice + (preTaxPrice * TAX_RATE);
}

let afterTaxPrice = calculateTotalPrice(preTaxPrice);

~~~
(3) const 키워드와 객체
const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경 할수 있다.
~~~javascript
const person = {
  name: 'Lee'
};

// 객체는 변경 가능한 값이다. 따라서 재할당 없이 변경이 가능하다.
person.name = 'Kim';

console.log(person); // {name: "Kim"}
~~~
