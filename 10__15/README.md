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


