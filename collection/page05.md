## 호이스팅과 소소코드의 실행과정

자바스크립트 엔진은 코드를 두 단계로 나누어 실행한다.  
첫 단계는 코드 평가단계를 실행하고 두 번째는 코드를 런타임으로 실행한다.

코드 평가단계에서는 선언형으로 선언된 식별자들을 평가하여 함수객체를 만들거나 변수의 경우 undefined로 초기화한다.  
그 다음에 실행단계에서 코드를 위에서부터 아래로 순차적으로 실행한다.  
이 때 처음에 평가단계에서 평가된 코드는 선언문이전에 코드가 있어도 실행이 된다.  
앞서 말했듯이 실행단계이전에 먼저 함수객체가 만들어졌기 때문이다.  
이렇게 선언문이전에 해당 함수를 참조할 수 있는 것을 코드가 위로 끌어올려진것 같다하여 호이스팅한다.

실행컨텍스트는 식별자들을 등록하여 스코프를 구분해 관리하고 해당 코드의 실행순서를 관리한다.  
식별자들을 등록하고 스코프를 구분하는 역할은 실행컨텍스트의 렉시컬환경이 하며  
코드의 실행순서관리는 실행컨텍스트 스택 일명 콜 스택에서 관리한다.

이처럼 실행컨텍스트는 코드들을 등록하여 실행순서를 관리해주는 역할을 한다.

## import export,webpack,babel

import export는 ES6에서 브라우저환경에서도 사용할 수 모듈 시스템 문법으로 대부분의 브라우저에서 지원이 되고 있다.  
하지만 몇몇 브라우저에서는 지원이 되지않기 때문에 웹팩과 같은 모듈 번들러가 필요하다.

웹팩을 통하여 자바스크립트의 하나의 파일 시작점으로부터 시작하여 여러 분리되어 있는 모듈을  
하나의 파일로 모아준다.

따라서 import export를 사용할 필요가 없기도 하며 또한 파일크기도 줄일 수 있다는 장점이 있다.

바벨은 자바스크립트 트랜스파일링 시스템으로 자바스크립트 문법의 변환을 하는데 사용된다.  
최신 문법은 몇몇 브라우저에서 적용이 안되는 경우가 있기에 이를 하위 문법으로 변환하여 작용할 수 있도록 해주는 역할을 한다.
