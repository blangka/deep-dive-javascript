## 프로퍼티 어트리 뷰트
내부 슬롯과 내부 메소드는 자바스크립트의 내부 동작방식을 설명하는 의사코드이다.

모든 객체는 prototype이라는 내부 슬롯을 가진다 내부엔 있지만 접근할수 없는 ???  
프로토 타입 관련해서는 해당 링크가 참조 할만하다.  
https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42
프로토타입은 어떤 개체의 상위 부모 역확을 하는 객체이다. 하위 객체 에서 자신의 프로퍼티와 매서드를 상속한다. 프로토타입 체인은 프로토타입이 단방향으로 연결된 리스트 이다.

prototype은 __protorype__ 으로 접근가능하다.

```javascript
const obj = {};
console.log(obj.__proto__ === Object.prototype); // true
```

## 프로퍼티 어트리뷰트와 디스크립터 객체  
프로퍼티의 상태란 value 값 , 갱신 가능 여부 writable, 열거 가능 여부 enumerable, 재정의 가능 여부 configurable 이다.  
아래와 같이 해서 확인 가능하다. 확인 한 내용이 프로퍼티 디스크립터 이다.  
```javascript
const person = {
  name: 'Lee'
};

pesrson.age = 20;

console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: true, enumerable: true, configurable: true},
  age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```
중요하지 않지만 property 도 정의 할수도 있다. 
```javascript
const person = {};

Object.defineProperty(person, 'firstName', {
  value: 'Ungmo',
  writable: true,
  enumerable: true,
  configurable: true
});

Object.defineProperty(person, 'lastName', {
  value: 'Lee'
});


let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log('firstName', descriptor);
// firstName {value: "Ungmo", writable: true, enumerable: true, configurable: true}

descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}
```


프로퍼티는 객체 변경 방지도 할수있다.
```javascript
const person = {
  name: 'Lee'
};

// 객체 확장 금지 하였다.
Object.preventExtensions(person);

// 프로퍼티 추가는 가능하다.
person.age = 20; // strict mode에서는 에러

// 프로퍼티 정의에 의한 프로퍼티 추가도 금지된다.
Object.defineProperty(person, 'age', { value: 20 }); // TypeError: Cannot define property age, object is not extensible
```

## 생성자 함수
### 객체 리처럴에 의한 생성 방식은 문제가 있다.

프로퍼티가 동일하지만 매번 생성해줘야 한다.
~~~javascript
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle2.getDiameter()); // 20
~~~

이런 불편한 점때문에 생성자 함수를 통해 객체를 생성한다.

~~~javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
~~~

생성자 함수가 아닌 그냥 함수 호출 하면 어떻게 될까?
~~~javascript
// 일반 함수로서 호출
const circle3 = Circle(15); // 일반 함수로서 호출
console.log(circle3); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius); // 15
~~~
??? 왜 radius는 15가 나오는거지???
생성자는 인스턴스를 생성하고 this를 바인딩하지만 일반 함수로 호출한경우에는 this 가 global인 window에 바인딩 된다.  

new를 사용하게 되면 인스턴스 생성과 this 바인딩을 한다. 또한 함묵적으로 this 를 반환한다.
~~~javascript
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 암묵적으로 this를 반환한다.
    
  // return 100;
  // 명시적으로 원시값 반환 하면 원시 값 반환을 무시하고 암묵적으로 this 를 반환한다. 생성자 함수는 내부에서 return  문을 사용하지 말아라
  // return { radius };
  // 객체로 반환 하면 반환 문은 유효하다.  
}

// 인스턴스의 생성
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
~~~

### constructor , non-constructor
둘의 차이는 constructor는 일반 함수 또는 생성자 함수로서 호출할 수 있는 객체, non-constructor는 일반 함수로서 호출할 수 있는 객체를 말한다.  

어떻게 구분하는가?  
constructor : 함수 선언문, 함수 표현식, 클래스  
non constructor : 매서드(ES6 메서드 축약표현) , 화살표 함수

~~~javascript
// 함수 선언문
function foo() {}

// 함수 표현식
const bar = function () {};

// 일반 함수로서 호출
foo(); // foo는 constructor
bar(); // bar는 constructor

// 화살표 함수 정의
const baz = () => {};

new baz(); // TypeError: baz is not a constructor

// 메서드(ES6 메서드 축약 표현)
const obj = {
  x() {}
};

new obj.x(); // TypeError: obj.x is not a constructor
~~~

### 스코프 세이프 생성자 패턴
~~~javascript
// Scope-Safe Constructor Pattern
function Circle(radius) {
    if(!(this instanceof Circle)) {
        return new Circle(radius);
    }
    
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
}

// 인스턴스 생성. Circle 생성자 함수를 new 연산자 없이 호출하면
// 일반 함수로서 호출된다. 이 경우 this는 전역 객체 window를 가리킨다.
const circle = Circle(5);
console.log(circle.getDiameter()); // 10
~~~


## 일급 객체
다음과 같은 조건을 만족하는 객체를 일급 객체라고 한다.  
1. 무명의 리터럴로 생성할수 있다 => 런타임에 생성 가능
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

~~~javascript
// 1. 무명의 리터럴로 생성할 수 있다.
// 2. 변수나 자료구조에 저장할 수 있다.
const increase = function (num) {
  return ++num;
};

const decrease = function (num) {
  return --num;
};

// 2. 함수는 객체에 저장할수 있다.
const auxs = {increase, decrease};

// 3. 함수의 반환값으로 사용할 수 있다.
// 4. 함수의 매개변수에 전달할 수 있다.
function calc(func, num) {
  return func(num);
}

~~~


## 프로토타입

### 객체 지향 vs 프로토타입
객체 지향 프로그래밍은 클래스를 기반으로 하는 프로그래밍 패러다임이다.  
실체는 특징을 가지고 있다.  
사람이라면 이름, 주소, 성별등 다양한 속성을 갖는데 필요한 것만 가지고 있다.  
이런 실체를 추상화 하면 클래스가 된다.  

~~~javascript
const circle = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};

console.log(circle.getDiameter()); // 10
~~~

프로토타입 방법
~~~javascript
// 생성자 함수
function Circle(radius) {
   this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가한다.
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받는다.
// 즉, Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유한다.
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172

~~~

프로토타입이란 객체 지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다.  

ES6의 축약 표현은 constructor도 갖지 않고 prototype도 갖지 않는다.

### 프로토타입 체인
~~~javascript
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// hasOwnProperty 메서드는 Object.prototype의 메서드다.
// me 객체는 hasOwnProperty 메서드를 소유하지 않지만
// 아래의 코드는 가능하다. 이는 프로토타입 체인 덕분이다.
console.log(me.hasOwnProperty('name')); // true
~~~
    
## static mode

~~~javascript
function foo() {
    x = 10;
}
foo();

console.log(x); // 10 ?
~~~
10 이 나오는 현상은 암묵적 전역으로 x 프로퍼티는 마치 전역 변수 처럼 사용 될수 있다.  
이를 방지하기 위해 ES5 부터는 static mode 가 추가 되었다. 최적화 작업에 문제를 일으킬수 있는 코드에 대해 명시적인 에러를 일으킨다.  

static mode 가 발생 시키는 에러는 다음과 같다
1. 암묵적 전역 : 선언하지 않은 변수를 참조
2. delete 연산자로 변수, 함수, 매개변수를 삭제
3. 함수 매개변수에 전달된 인수 중복 : function foo(x,x)
4. with 문 사용

static mode 가 발생 시키는 변화
1. 일반 함수의 this는 undefined, 생성자 함수의 this는 인스턴스를 가리킨다.
2. arguments 객체에 callee 프로퍼티가 없다.
