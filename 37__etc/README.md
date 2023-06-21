##  SET MAP
###  1. set
- 중복을 허용하지 않는 데이터 집합
- 순서가 없다.
- 인덱스로 요소에 접근할 수 없다.

~~~javascript
const set = new Set();
set.add(1);
set.add(2);
set.add(3);

console.log(set); // Set { 1, 2, 3 }
console.log(set.size); // 3

console.log(set.has(2)); // true
set.delete(2);

set.clear();

~~~

### 2. map
- 키와 값의 쌍으로 이루어진 데이터 집합
- 순서가 없다.
- 인덱스로 요소에 접근할 수 없다.

~~~javascript
const map = new Map();
map.set('one', 1);

console.log(map); // Map { 'one' => 1 }
console.log(map.size); // 1
console.log(map.get('one')); // 1
~~~

## 브라우저 랜더링 과정
브라우저가 HTM , css , javascript를 해석하여 화면에 표시하는 과정을 브라우저 랜더링이라고 한다.  
브라우저 랜더링 과정은 크게 4가지로 나눌 수 있다.
1. 브라우저의 HTML, CSS, Javascript, 이미지, 폰트등 렌더링에 필요한 리소스를 요청하고 서버로 부터 응답 받는다.
2. 렌더링 엔진은 서버로 부터받은 파일을 파싱해서 DOM, CSSOM을 생성하고 이를 결합해서 렌더 트리를 생성한다.
3. 자바스크립트 엔진은 파싱하여 AST(abstract syntax tree)를 생성하고 바이트코드로 변환한다. 그리고 실행한다.
4. 렌터 트리 기반으로 HTML요소의 레이아웃을 계산 하고 브라우저 화면에 HTML요소를 그린다.

htts://www.hyundai.com으로 요청 한다고 했을때 암묵적으로 index.html을 요청한다.  
css, javascript등 네트워크를 보면 리소스 응답이 있는데 이는 HTML을 파싱하는 도중에 외부 리소스로드하는 경우에 파싱을 중단하고 리소스 파일을 서버로 요청한다.  

HTTP 1.1과 2.0의 차이는 커넥션당 하나의 요청과 응답만하는 1.1과 여러개의 요청과 응답을 하는 2.0의 차이가 있다.  

### HTML 파싱과 DOM 생성
HTML 파싱은 HTML문서를 파싱하여 DOM트리를 생성하는 과정이다.  
코드의 최소 단위인 토큰으로 분해하고 토큰을 객체로 변환하여 노드로 구성된 트리인 DOM트리를 생성한다.  
노드는 문서 노드, 요소 노드, 텍스트 노드 등등이 있고 이것들을 트리 자료구조로 구성한 것을 DOM이라고 한다.

### CSS 파싱과 CSSOM 생성
CSS 파싱은 CSS문서를 파싱하여 CSSOM을 생성하는 과정이다.  
HTML을 한줄씩 파실하여 DOM을 생성해나가다가 CSS를 로드하는 link 태그나 sytle태그를 만나면 CSS를 파싱한다.
이것을 CCSOM을 생성한다.

### 렌더 트리 생성
이것들을 모두 합치면 렌더 트리가 된다.
~~~javascript
HTML -> DOM Tree
                    -> Render Tree -> Layout -> Paint
CSS -> CSSOM Tree

javascript -> AST -> Bytecode -> Execution(DOM, CSSOM을 변경하는 DOM API 호출)
~~~
다음과 같은 조건에 레이아웃 계산과 페인팅이 제차 실행 된다. 이것을 리페인틴 리렌더링 이라고 한다.
1. 노드 추가 또는 삭제
2. 브라우저 창 리사이징에 의한 뷰포트 크리변경
3. HTML 요소의 레이아웃 변경

script 의 경우 body 위에 있는 경우 렌더링이 끝난 후에 실행되기 때문에 렌더링이 늦어진다.
그래서 async (비동기), defer(비동기 DOM 생성 완료된 후에 실행) 속성을 사용한다.

### DOM ETC
노드 탐색 parent , child, sibling등을 활용해서 담색한다.

data set Attribute를 활용해서 data-로 시작하는 속성을 추가할 수 있다.  
https://velog.io/@hwang-eunji/React-HTML-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%84%B8%ED%8A%B8dataset-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

~~~javascript
import React from 'react';

const App = () => {
  const onClick = (e) => {
    console.log(e.target.dataset);
  };

  return (
    <div>
      <button data-id="1" onClick={onClick}>
        1번
      </button>
      <button data-id="2" onClick={onClick}>
        2번
      </button>
      <button data-id="3" onClick={onClick}>
        3번
      </button>
    </div>
  );
};
~~~

## 이벤트
브라우저는 특정 사건이 발생하면 이벤트를 발생 시킨다.

### 이벤트 흐름
이벤트 흐름은 이벤트가 전파되는 방식을 말한다.
이벤트 흐름에는 캡처링 단계, 타깃 단계, 버블링 단계가 있다.
~~~javascript
<div>
  <button>버튼</button>
</div>
~~~
위와 같은 구조에서 버튼을 클릭하면 이벤트가 발생한다.
이벤트 흐름은 버튼 -> div -> document -> window 순으로 발생한다.
이벤트 흐름은 버블링 단계와 캡처링 단계로 구분된다.
버블링 단계는 이벤트가 발생한 요소에서 document까지 이벤트가 전파되는 단계이다.
캡처링 단계는 document에서 이벤트가 발생한 요소까지 이벤트가 전파되는 단계이다.

3가지 순서로 보통 된다 캡쳐링 -> 타겟 단계 -> 버블링 단계

~~~html
<!DOCTYPE html>
<html>
<body>
    <ul id="fruits">
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>    
    </ul>
<script>
    const fruits = document.querySelector('#fruits');
    const apple = document.querySelector('#apple');
    const banana = document.querySelector('#banana');

    fruits.addEventListener('click', e => {
        console.log('이벤트 단계: ', e.eventPhase); // 1: 캡쳐링 단계
    }, true);

    apple.addEventListener('click', e => {
        console.log('이벤트 단계: ', e.eventPhase); // 2: 타겟 단계
    }, true);

    fruits.addEventListener('click', e => {
        console.log('이벤트 단계: ', e.eventPhase); // 3: 버블링 단계
    }, false);
</script>
</body>
</html>
~~~


