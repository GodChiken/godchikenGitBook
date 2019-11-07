---
description: 호이스팅에 대해 조사
---

# 호이스팅

* 호이스팅\(Hoisting\) 이란?
  * 특정 스코프 내에 위치하는 함수 혹은 변수의 선언문을 스코포의 최상단으로 위치시키는 것을 이야기함.
  * 선언문은 자바스크립트 엔진 구동시 최우선으로 해석된다.
  * 할당문은 런타임 과정에서 해석되기 때문에 호이스팅이 관여되지 못한다.
* 변수 호이스팅
  * 몇가지 예를 살펴보자.
  * ```javascript
     function isHoist(){     console.log(x);     var x = 100;     console.log(x); }
    ```
  * ```javascript
     function isHoist2(){     console.log(x);     var x;     x = 100;     console.log(x); }
    ```
  * 실행컨텍스트에서[ Declarative environment Record](https://github.com/GodChiken/StudyES5/blame/master/src/main/resources/markdown/context/executionContext.md#L177-L179) 이부분을 언급한 적이 있다.
  * 선언문 "var x" 는 최상단으로 끌어올려지기 때문에 런타임시 할당문이 실행되기 전까지는 "undefined" 상태라는 것을 유념하자. 호이스팅이 안되는 것이 아니다.
  * 또한 if 문과 같은 중괄로 감싸져 있다 할지라도 선언문은 항상 최상위로 위차하니 주의시킨다.
* 함수 호이스팅
  * 글로벌 스코프에 위치하는 함수는 모든 내용이 글로벌 스코프의 최상단으로 끌어 올려진다.
  * 함수 선언식은 호이스팅이 이루어 진다. 정상적인 출력이 이루어진다.  

    ```javascript
      hoy();  function isHoy(){      console.log("hoy");  }
    ```

  * 함수 표현식 호이스팅에 영향을 받지 않는다. 왜냐면 앞서 설명했던 대로 변수에 할당문은 런타임시 이루어지기 떄문에 구문 에러가 발생한다.  

    ```javascript
      hoy();  var isHoy = function(){      console.log("hoy");  }
    ```
* 변수, 함수의 호이스팅 순위
  * var 변수, 함수 선언식을 사용한 함수의 선언부가 유효범위의 최상단으로 호이스팅 된다는 것을 공부했다.
  * ```javascript
      function f(){}  var f;  console.log(f);  var g;  function g(){}  console.log(g);
    ```
  * 변수 선언이 최우선, 그 다음이 함수 선언, 그 다음이 변수 값 할당임을 명심하자.
* 참고자료
  * [https://stackoverflow.com/questions/28246589/order-of-hoisting-in-javascript](https://stackoverflow.com/questions/28246589/order-of-hoisting-in-javascript)
  * [https://scotch.io/tutorials/understanding-hoisting-in-javascript](https://scotch.io/tutorials/understanding-hoisting-in-javascript)
  * [https://yuddomack.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85Hoisting](https://yuddomack.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85Hoisting)
  * [http://chanlee.github.io/2013/12/10/javascript-variable-scope-and-hoisting/](http://chanlee.github.io/2013/12/10/javascript-variable-scope-and-hoisting/)

