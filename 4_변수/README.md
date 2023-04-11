# 변수

변수 이름을 식별자라고 한다. 식별할수 있는 고유의 이름이다.  

~~~javascript
const result = 1;
~~~

result라는 이름은 식별자이다. 식별자는 값이 아니라 메모리 주소를 기억하고 있다. 그래서 해당 메모리 주소에 1이라는 값이 저장되는 것이다.  


## 변수 선언
변수를 사용하려면 선언이 필요하다. var, let , const 가 있다.  
ES6이전에는 var를 사용하였고 지금은 let, const를 사용한다. 

var는 블록 레벨 스코프를 지원하지 않고 함수 레벨 스코프를 지원한다. 그래서 전역 변수 처럼 쓰이기 때문에 부작용이 많았다.  
let, const는 블록 레벨 스코프를 지원한다. let const만 사용하자.  

### var
~~~javascript
var a;
~~~
var는 선언과 초기화 단계가 동시에 진행된다 . undefined가 할당된다.

~~~javascript
console.log(score); // undefined
var score; // 변수 선언문
~~~

예상하기로는 RefereceError가 발생할 것 같지만 undefined가 출력된다.  
var , let, const , function, class 모두 호이스팅이 일어난다. 모두 런타임 이전 단계에서 먼저 실행 되기 때문이다. 
이러한 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 호이스팅이라고 한다.

변수의 선언은 런타임 이전에 실행되지만 할당은 런타임에 실행된다.
~~~javascript
console.log(score); // undefined
var score; // 변수 선언문
score = 80; // 변수 할당문
console.log(score); // 80
~~~

### let vs const
let은 재할당이 가능하고 const는 재할당이 불가능하다.
