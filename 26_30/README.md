## ES6 추가 기능

### 화살표 함수
일반 함수는 constructor, prototype, arguments를 가지고 있다.  
하지만 화살표 함수는 아니다. 이유는 화살표 함수는 생성자 함수로 사용할 수 없기 때문이다. 즉 non-constructor이다.  
특히 this가 전역 객체를 가리키는 문제 해결하기 위한 용도로 사용된다.

암묵적으로 return 을 생략할수 있다. 그래도 명시적으로 해주는게 좋다.
~~~javascript
const create = (id, content) => ({ id, content });

const create = (id, content) => {
  return { id, content };
};
~~~

### Rest 파라미터
Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 받는다.
~~~javascript
function foo(...rest) {
  console.log(rest); // [1, 2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);
~~~

~~~javascript
function foo(param, ...rest) {
  console.log(param); // 1
  console.log(rest);  // [2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);
~~~

### 기본값 매핑
~~~javascript
function foo(x = 3) {
  console.log(x);
}

foo(); // 3
~~~
생략해도 기본값을 매핑할수 있다.

## 배열
자료구조의 배열은 동일한 크기의 메모리 공간에 빈틈없이 나열된 형태이다.  
하지만 자바스크립트 배열은 각각 메모리 공간은 동일한 크기를 갖지 않아도 되고 배열의 요소가 연속적으로 이어지지 않을수도 있는 희소 배열이다.  
인덱스로 배열 요소에 접근하는 것은 일반 배열보다 느리지만 삭제와 삽입의 경우에 일반 배열 보다 빠르다.  

* const arr = [1, , 3]; 이렇게 하면 희소배열이 된다. 이건 성능에도 안좋은 영향을 미치기 떄문에 같은 타입의 요소를 연속 적으로 위치시키는 것이 최선이다. 희소 배열을 생성하지 않도록 주의하자.  

### 유사배열 객체를 배열로 변
~~~javascript
Array.from({length: 2, 0: 'a', 1: 'b' }); // ['a', 'b']
Array.from('Hello'); // ['H', 'e', 'l', 'l', 'o']
~~~

length와 인덱스를 가진 객체를 유사배열 객체라고 한다.  

### 배열 요소 추가/삭제
~~~javascript
const arr = [1, 2, 3];

arr.push(4); // [1, 2, 3, 4]
arr.pop(); // [1, 2, 3]
arr.splice(1, 1); // [1, 3]

// 배열 원본을 직접 변경하지 않고 새로운 배열을 생성한다.
const result = arr.concat(4, 5); // [1, 3, 4, 5]
~~~

희소 배열 만들기 싫으면 push, pop, splice를 사용하자.

### 배열 얕은 복사 방법
~~~javascript
const arr = [1, 2, 3];

// 1. slice
const copy = arr.slice();

// 2. spread
const copy = [...arr];
~~~
깊은 복사는 lodash의 cloneDeep을 사용하자.  

### 배열 고차 함수
고차 함수랑 함수를 인자로 전달 받거나 함수를 반환 하는 함수를 말한다.  대표적인 예로 sort를 볼수 있다.
~~~javascript
const arr = [1, 2, 3];

// 1. sort
arr.sort((a, b) => a - b); // [1, 2, 3]

// 2. map
arr.map((item) => item * 2); // [2, 4, 6]

// 3. filter
arr.filter((item) => item % 2); // [1, 3]

// 4. reduce
arr.reduce((acc, cur) => acc + cur, 0); // 6
~~~

## Date
표준 빌트인 객체인 Date는 날짜 시간을 나타낸다. 
UTC 는 국제 표준시 이다. UTC는 GMT와 동일하다. KST는 UTC에 9시간을 더한 시간이다. 현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정된다.  

### Date 생성자 함수
~~~javascript
// 1. new Date()
new Date(); // 현재 날짜와 시간 반환 MON Jun 07 2021 22:30:00 GMT+0900 (대한민국 표준시)

// 2. new Date(milliseconds)
new Date(0); // 1970-01-01T00:00:00.000Z
/*
86400000ms = 24h
1s = 1,000ms
1m = 60s * 1,000ms = 60,000ms
1h = 60m * 60,000ms = 3,600,000ms
1d = 24h * 3,600,000ms = 86,400,000ms
*/

// 3. new Date(dateString)
new Date('May 26, 2021 10:00:00'); // 2021-05-26T01:00:00.000Z

// 4. new Date(year, month[, day, hour, minute, second, millisecond])
new Date(2021, 4); // 2021-05-01 00:00:00
new Date(2021, 4, 26); // 2021-05-26 00:00:00
new Date(2021, 4, 26, 10); // 2021-05-26 10:00:00
new Date(2021, 4, 26, 10, 30); // 2021-05-26 10:30:00
// 가독성이 좋은건 아니다. 그래서 moment.js를 많이 사용한다.
new Date('2021/5/26/10:00:00:00'); // 2021-05-26 10:00:00
~~~

### Date 메서드
~~~javascript
const date = new Date();

// 1. getFullYear()
date.getFullYear(); // 2021

// 2. getMonth()
date.getMonth(); // 5

// 3. getDate()
date.getDate(); // 7

// 4. getDay() 요일 일요일 0 월요일 1 ....
date.getDay(); // 1

// 5. getHours() 0~23
date.getHours(); // 22

// 이 밖에도 많지만 내용이 너무 많아서 생략한다.
~~~
