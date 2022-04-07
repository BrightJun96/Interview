## 1. Virtual Dom

## 2. LifeCycle Method

## 3. react를 사용하는 이유

## 4. 클래스형 컴포넌트와 함수형 컴포넌트의 차이

## redux를 사용하는 이유? Redux의 장단점

복잡한 상태를 체계적으로 관리하며 상태값을 전역적으로 사용할 수 있기 때문에 사용한다.
상태값이 여러개라면 이를 useState나 useReducer만으로 관리하기가 쉽지 않다.
하지만 redux를 사용하면 상태값을 컴포넌트별로 구분지어 체계적으로 관리하여 각 state값의 변화를 관찰하기 쉽다.

또한 컴포넌트 구조가 복잡하다면 상태값을 props로 여러번 전달해줘야하기 때문에 가독성,유지보수성에 좋지 않다.
redux는 이러한 상황일 때에 상태값을 필요한 부분에 가져다 쓰도록 store에 담아 전역적으로 관리할 수 있게 해준다.

하지만 상태가 복잡하지 않거나 컴포넌트 구조가 복잡하지 않다면 구성하는 것이 번거로울 수 있기 때문에 사용하지 않는 것이 낫다.

## 미들웨어는 무엇이고 대표적인 미들웨어인 redux saga와 redux thunk를 어떠한 상황에 적절히 사용하면 되는 것인가?

미들웨어는 action이 reducer로 가는 중간에서 action을 캐치하여 side effect와 같은 기능을 해당 action에 추가하여 reducer로 보낼 수 있는 기능이다.
때문에 대표적인 작업인 비동기 로직과 같은 side effect들을 reducer에서 사용할 수 없기때문에 미들웨어에서 실행해야한다.

대표적인 미들웨어로는 thunk와 saga가 있다.

redux-thunk는 특별한 셋업없이 간편하게 사용할 수 있다는 장점이 있으며 RTK에서 default로 지원이 되어 미들웨어 설정없이 사용할 수 있다.  
 다만 로직이 복잡해지면 가독성이 안 좋아지는 콜백헬을 발생시킬 수 있는 가능성 크고 유지보수가 어려워질 수 있다는 점이 있다.  
redux-saga는 thunk에 비해 셋업이 필요하다는 단점이 있다.  
 하지만 로직이 복잡해지고 프로젝트가 커져도 체계적으로 관리할 수 있다는 점이 있다.

단순한 비동기작업 등 미들웨어작업이라면 thunk를 쓰는 것이 낫다.  
saga를 사용하기 위해서는 기본적인 setup이 필요하고 간단한 로직임에도 굳이 불필요한 코드만 늘어날 수 있기 때문이다.  
하지만 프로젝트 규모가 크고 복잡한 로직이라면 saga로 체계적으로 관리해줘도 좋다.

RTK안에서 비동기 로직때문에 미들웨어를 사용하는 경우라면 redux-thunk를 사용하는것이 편하다.
RTK는 redux-thunk를 default로 지원하기 때문에 미들웨어 셋업없이 thunk를 선언하여 사용하면 된다.

**Ref**

- https://www.eternussolutions.com/2020/12/21/redux-thunk-redux-saga/
- https://shinejaram.tistory.com/76
- https://react.vlpt.us/redux-middleware/10-redux-saga.html

## react에서 state가 불변성을 유지해야하는 이유?

## Context API란?

## react 18버전 업데이트된 내용이 뭔지 알고 있는지?
