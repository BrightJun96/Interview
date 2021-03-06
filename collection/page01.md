# Interview

## 브라우저 렌더링 과정

0. 임의의 주소를 입력한다.

1. 브라우저는 HTML,CSS,자바스크립트, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 요청하고 서버로부터 응답을 받는다.

2. 브라우저의 렌더링 엔진은 서버로부터 응답된 HTML과 CSS를 파싱하여 DOM과 CCSOM을 형성하여 이들을 결합한 뒤 렌더 트리를 생성한다.

3. 브라우저의 자바스크립트 엔진은 서버로부터 응답된 자바스크립트를 파싱하여 AST(Abstract Syntax Tree)를 생성하고 바이트코드로 변환하여 실행한다.

4. 렌더 트리를 기반으로 HTML 요소의 레이아웃을 계산하는 플로우와 이를 화면에 그리는 페인팅이 완성됨으로써 렌더링이 완료된다.

**정리**

브라우저는 서버로부터 HTML,CSS 파일을 응답받아 파싱하여 DOM과 CCSOM을 형성합니다.

이들은 렌더링을 위하여 렌더트리로 결합되고 레이아웃을 계산하는 플로우 작업과 HTML요소들을 화면에 그리는 페인트작업을 완료함으로써 렌더링이 됩니다.

플로우 : HTML 요소의 레이아웃을 계산하는 작업
페인트 : HTML 요소를 화면에 그리는 작업

(플로우 작업과 페인트 작업이 반복해서 실행될 수 있는데 이를 리플로우,리페인트라 하며 다음과 같은 경우 실행됩니다.

1. 자바스크립트에 의한 노드추가,삭제
2. 브라우저 창의 리사이징
3. HTML 요소의 레이아웃을 변경을 발생시키는 스타일 변경
   )

## Script 태그를 **head** 태그보다 **body** 태그에 넣는 이유는 무엇이고 태그 삽입 **순서에 상관없이** 태그를 삽입할 수 있는 방법은 무엇이 있을까요?

1. html은 렌더링 엔진에 의해 순차적으로 파싱되며 중간에 다른 파일을 로드하는 link,script 태그 등을 만나면 DOM생성을 중단합니다.

2. DOM의 생성된 후의 위치에 script 태그를 두는 것이 중요합니다.

head태그에 삽입할 경우 DOM트리가 완성되지 않은 상황이기 때문에 자바스크립트 코드 중 DOM API가 쓰일 경우 에러가 발생할 수 있습니다.

때문에 DOM 생성되기전인 head 태그보다 body태그 끝쪽에 위치시키는 것이 좋습니다.

3. 순서에 상관없이 태그를 삽입할 수 있는 방법은 script 태그의 어트리뷰트로 async나 defer을 부여해주면 됩니다.  
   이는 비동기적으로 script 태그를 실행시킬 수 있으므로 에러를 발생시키지 않습니다.

## 비동기 함수는 어떻게 동작하나?

- 비동기 처리 방식이란?
- 자바스크립트 엔진은 싱글 스레드
- 브라우저는 멀티 스레드
- 내부 처리 방식

비동기 처리 방식은 이전 태스크가 완료되지 않아도 다음 태스크가 진행될 수 있게 하는 방식이다.

하지만 자바스크립트 엔진은 한번에 한 가지 태스크를 처리할 수 있는 싱글 스레드이다.
때문에 진행중이던 태스크가 완료되어야 다음 태스크가 진행이 가능하다.

그래서 브라우저의 기능을 활용하면 된다.
브라우저는 한번에 여러 가지 태스크를 진행할 수 있는 멀티 스레드이기 때문에 비동기 처리 방식을 사용할 수 있다.

함수가 처리될 때 비동기 함수의 콜백함수는 콜스택이 진행됨과 동시에 브라우저에 의해 호출 스케쥴링되며  
이벤트 루프에 의해 태스크 큐로 옮겨지며 콜스택이 비게 되면 태스크 큐에서 콜스택으로 옮겨지게 된다.

이처럼 비동기 콜백함수를 호출 스케쥴링하고 태스크 큐로 옮기는 것은 자바스크립트 엔진이 아닌 브라우저의 역할이다.

**Add**  
리액트에서 상태값을 변경시킬 때 state를 사용하는 것은 비동기 프로그래밍이랑 연관이 있다.  
리액트는 html이 아닌 자바스크립트내에서 요소들을 생성해낸다.

이 때 버튼을 클릭하면 카운트가 올라가는 버튼이 있고 카운트를 보여줄 ui가 있다하자.

코드는 다음과 같다.

```js
import React from "react";

const App = () => {
  let number = 0;
  // onClick이라는 이벤트핸들러는 비동기적으로 작동할것이기 때문에
  // 모든 요소가 렌더링된 뒤에 number가 올라갈 것이다.
  // 올라간 number의 모습을 보려면 리렌더링을 해야하는데
  // 새로고침을 하면 number는 초기화가 된다.
  // setState는 비동기적으로 작동하는 것을 알수가 있다.
  // 비동기적으로 작동하지않으면 state값을 사용할수없다.

  // 클로저를 쓴다 하여도 함수내부의 변수여서 외부에서 접근할 수 없다.
  const increase = (function () {
    let count = 0;
    return function () {
      return count++;
    };
  })();
  return (
    <div>
      <h1>{number}</h1>
      <button
        onClick={() => {
          console.log(number);
          return number++;
        }}
      >
        Increase
      </button>
    </div>
  );
};

export default App;
```

이벤트 핸들러는 비동기적으로 작동한다.
때문에 위 컴포넌트가 다 실행된뒤에 onClick이벤트핸들러가 작동한다.

그런데 number 값은 이미 0으로 보여지고 있다.

값은 비동기적으로 number가 1로 바뀌었다해도 이는 적용될 수가 없다.

(새로고침이 필요하다 하지만 새로고침을 하면 number은 다시 0으로 초기화)

때문에 number라는 값을 비동기적으로 변경해줄 비동기함수가 필요하다.  
이 함수가 바로 setState이다.  
setState는 비동기적으로 작동함과 동시에 리렌더링을 해주어 값을 바꾼뒤에 리렌더링을 해줘  
해당 값을 반영하게 한다.

이러한 원리로 어떠한 데이터값을 변경하기 위해 리액트에서는 setState를 사용하는 것이다.

(비동기적으로 작동하지 않으면 데이터값에 반영이 되지 않을 수 있음.)

## 태스크 큐와 마이크로태스크 큐

태스크 큐는 비동기함수의 콜백함수 및 이벤트 핸들러가 이벤트 루프에 의해 보관되는 영역이다.
마이크로태스크큐는 프로미스 후속처리 메서드의 콜백함수가 이벤트 루프에 의해 보관되는 영역이다.
(프로미스 후속처리메서드도 비동기함수이다.)

두 가지를 비교하였을 때 **차이점은 실행우선순위가 다르다**는 점이다.

일반 비동기 함수와 프로미스의 후속 처리 메서드가 있다 하였을 때
어느 위치에 코드가 있던 프로미스의 후속처리 메서드가 우선적으로 처리된다.

콜스택이 비게 되면 이벤트 루프는 먼저 마이크로태스큐에서 함수를 꺼내 콜스택으로 옮겨 처리하고  
다음으로 태스크큐에서 옮겨 처리하기 때문이다.

## ajax의 개념,탄생동기

- 개념
- 탄생동기
- 장단점
- hmlhttprequest

ajax란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고,
서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식이다.

ajax등장이전 웹페이지에서는 페이지 전환시 작은 데이터 변경에도 불구하고 새롭게 html을 요청하여 응답받아야했다.  
이로 인해 페이지 전환시 깜빡이는 현상이 나타나게 되고 이는 느린 퍼포먼스를 보여줬다.

또한 필요한 부분을 업로딩할 때마다 새로운 html에 데이터를 수기로 작성해줘야하니 이 또한 비효율적이다.

하지만 ajax는 필요한 데이터를 비동기적으로 받아와 필요한 부분을 업데이트 할 수 있다.
따라서 필요한 부분만 부분적으로 업데이트되고, 변경될 필요가 없는 부분은 변경되지 않게 할 수 있다.

ajax는 XMLHttpRequest 객체를 기반으로 작동한다.  
XMLHttpRequest는 http 비동기 통신을 위한 프로퍼티 및 메서드를 제공한다.
이를 간편하게 사용할 수 있는 API로는 fetch 또는 라이브러리 ajax가 있다.
이는 XML,json,txt,html 등 다양한 데이터 포멧을 주고 받는다.

**reference**

- https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX/Getting_Started
- javascript deep dive
- https://titus94.tistory.com/4

## Rest API란?(Representational State Transfer)

Rest는 Http를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처이고  
Rest API는 Rest를 기반으로 구성한 API이다.

Rest API는 3가지 구성요소로 이루어지는데 자원,행위,표현으로 이루어진다.
자원은 URI로서 리소스를 얻어오고자하는 origin을 말하고  
행위는 해당 자원에 대한 http 요청 메서드(get,post,patch,delete 등)를 말한다.
표현은 행위를 할 때 서버에 같이 보낼 구체적 내용이며 주로 데이터의 추가,수정을 할 때 보낸다.

따라서 Rest API로 설계할 때에는 얻어오고자하는 리소스를 표현하기 위해 알맞은 URI 표현하고 해당 리소스에 대해 어떻게 처리할 것인지를 표현하기 위해 적절한 http 요청 메서드를 사용해야한다.

## GET과 POST가 어떻게 다르게 쓰이는지 말씀해주세요.

```

Get과 post는 Rest API중 대표적인 메서드로서
주로 GET은 서버에게 요청하여 특정 리소스를 취득하는데 사용하고
POST는 클라이언트측에서 서버에 없는 특정 리소스를 서버에게 보낼 때에 주로 사용합니다.

하지만 get은 리소스를 얻어올 때, post는 리소스를 보낼 때, 이렇게 정해진 것이 아니라 get,post 둘다 서버에 특정 데이터를 보낼 때 사용할 수 있습니다.

다만 , GET은 보내고자하는 데이터가 URL에 드러나 보안성이 취약하다는 점과
데이터양이 많으면 URL의 길이제한이 있어 보내기 힘듭니다.
하지만 POST는 데이터를 Body(payload)에 담아 보내 데이터가 보이지 않다는 점에서 보안성도 있으며 길이 제한이 없어 주로 데이터를 보낼 때에 post로 보내는 것입니다.

따라서 서버의 리소스를 조회할 때에는 GET, 서버에게 리소스를 보낼 때에는 POST HTTP 요청 메서드를 사용합니다.
```

## JSON이란?

클라이언트와 서버가 http 통신을 할 때에 주고 받는 텍스트 데이터 포멧형식이다.

주로 ajax로 비동기 통신할 때 header의 content-type으로 설정하는 텍스트 데이터 포멧이다.

클라이언트 측에서 json데이터로 통신할 때에 서버에서 보낼 때는 문자열화(JSON.stringfy)에서 보내고 서버측에서 클라이언트측에 보낼 때는 객체화(JSON.parse)하여 보낸다.

## 비동기 처리를 할 때 기존 콜백함수로 처리하는 것보다 Promise처리하는 것의 장점은?

- 기존의 비동기 응답값 처리 => 콜백함수
- 그로 인한 문제점
- 이를 해결할 promise

기존에 비동기 처리에 대한 응닶값을 활용하기 위해서는 콜백함수로 처리해야했다.

그러다보니 응답값을 활용하여 또 다른 비동기 처리를 해야할 상황이 중첩되다보면 콜백함수가 중첩되는 경우가 발생하는데 이를 콜백지옥이라고 한다.

이는 가독성이 안 좋을 뿐더라 유지보수도 힘들어지게 되는 단점이 있다.

이를 위해 등장한 기능이 promise이다. promise는 비동기처리에 대한 응답값을 then이라는 후속처리 메서드로 순차적으로 처리할 수 있으며 에러값도 catch를 통해 잡아낼 수 있어 위 문제점을 해결할 수 있는 장점을 가지고 있다.

**Ref**

- 딥다이브

## async/await에 관해 설명하시오.

async/await는 프로미스에 대한 응답값을 후속처리메서드를 활용하지 않고 async/await라는 키워드로 순차적으로 조작할 수 있는 기능이다.

여러개의 비동기 동작을 실행시킬 코드가 있다하였을 때 , 이에 순서를 정하여 해당 코드가 진행되기전까지 다음 비동기 코드가 동작하지 않도록 순서를 정해줄 수 있는 순차적인 장점이 있다.

**Detail**
promise then 후속처리메서드의 경우 여러 개가 중첩되있을 경우, 비동기적으로 순서가 보장되지 않을 수 있다.
하지만 async/await를 사용하면 순서를 보장해줄 수 있다.

## fetch와 axios의 차이

fetch는 비동기 http 통신을 위한 web api이고 xmlHttpRequest API보다 간편하게 사용할 수 있는 API이다.
axios는 xmlHttpRequest을 기반으로 한 라이브러리이다.

둘의 사용방식은 비슷한 axios가 추가적인 기능이 더 많다.
fetch는 Web API이기 때문에 브라우저에서만 실행이 가능하지만 axios는 브라우저뿐만이 아니라 node.js환경에서도 실행이 가능하다.
또한 여러 origin에 대한 API 요청할 상황이 있을 때 , 각 origin에 대한 요청이 복잡하고 번거로워질 수 있는데 axios를 사용할 때에는  
여러가지 인스턴스를 생성하여 체계적으로 각 오리진마다 헤더 설정 및 요청을 관리할 수 있다.

또한 fetch는 xmlHttpRequest보다 간편하게 사용할 수는 있지만 xmlHttpRequest에 있는 기능이 없다.  
하지만 axios는 xmlHttpRequest기반으로 해당 기능들을 사용할 수 있다는 장점이 있다.

전반적으로 axios가 fetch보다 많은 기능을 가지고 있지만 해당 기능들이 사용되지 않는데 굳이 axios라는 라이브러리를 설치해 사용할 필요는 없다.  
상황에 맞춰 고민해보고 어떤 도구를 사용할지 고민해보는 것이 좋다.

**Ref**

- https://velog.io/@kcj_dev96/fetch

## http와 https 차이

http는 웹으로 비추어보았을 때, 클라이언트와 서버가 통신할 수 있도록 하는 프로토콜이다.
프로토콜이란 통신을 위한 규약을 말한다.
http는 html와 같은 여러 가지 리소스등을 교환할 수 있게 해준다.

https는 http의 보안성이 보장된 버전이다.
https위에서 클라이언트와 서버는 안전하게 데이터를 교환할 수 있으며 보안성을 위해 TLS와 SSL을 사용한다.

**Reference**

- https://developer.mozilla.org/en-US/docs/Glossary/Protocol
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview
- https://developer.mozilla.org/en-US/docs/Glossary/https
- https://developer.mozilla.org/en-US/docs/Glossary/SSL

## CORS

CORS는 서로 다른 origin으로 네트워크 요청을 하여도 CORS 규칙을 지킨다면 리소스를 응답받을 수 있도록 하며 이에 대한 정책을 규정한 기능이다.

이 때 서버에서 허용 origin 설정(Access-Control-Allow-Origin)을 제대로 해주지 않았다면 CORS에러가 발생할 수 있다.

때문에 해당 에러를 해결하기 위해서는 서버에서 허용 origin을 알맞게 해주면 된다.(예)Access-Control-Allow-Origin: https://requestorigin.com)

다른 방법은 웹팩 개발 서버에서 proxy기능을 사용하는 방법이 있다.
proxy로 요청을 보내고자하는 서버를 입력해주면 나의 개발 서버로 요청을 보냈을 때 proxy가 대신하여 요청을 보내고자하는 서버에 요청을 보내고 응답을 받아 전달해준다.
(다만 proxy 설정 서버가 production모드인 경우 해결이 안될 수 있으니 로컬 서버로 설정할 경우에 사용하는 것이 좋다.
production 모드에서도 사용하고 싶다면 client orgin과 server origin이 같은 경우에 사용하는 것이 권장된다.)
)
**Reference**

- https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#the_http_request_headers

- https://evan-moon.github.io/2020/05/21/about-cors/#simple-request

- https://stackoverflow.com/questions/52877492/proxy-not-working-for-create-react-app-in-production

## 비동기 함수의 콜백함수가 비동기 로직을 처리할 수 있는 원리는 뭘까?

비동기 함수는 콜스택이 다 비워진 뒤에 실행이 되기때문에 비동기 로직을 처리하기 위해서는 비동기함수 안에서 해야한다.  
비동기 함수밖에서 비동기 로직은 처리될 수가 없다. 비동기함수는 동기함수보다 먼저 실행될 수 없기 때문이다.
