---
description: 'let, const, BlockLevel Scope 공부'
---

# let, const, BlockLevel Scope



* let keyword
  * 기존 함수형 스코프 대신 등장.
  * 블록 단위로 \(중괄호\) 스코프를 결정하기위해 등장한 키워드
  * 이 키워드로 선언한 곳에서 가장 가까운 블록을 스코프로 지정한다.
  * 기존 ES5 에서 [명시적이지 않거나 블록 단위로 스코프를 하지 않는 경우에 일어나는 일들을 방지 할 수 있다.](https://github.com/GodChiken/StudyES5/blame/master/src/main/resources/markdown/scope/scope.md)
  * _**ES6에서 모든 선언\(var, let, const, function, function**_**, class\)을 호이스팅한다.**
  * var keyword 와의 차이점
    * window object 에 설정이 되지 않는다.
    * 중복 선언을 방지한다. \(Syntax error\)
    * 호이스팅이 이루어지지 않는다.
      * let 으로 선언 하기 전 변수를 참고하면 에러가 발생한다.
      * 스코프의 시작에서 변수의 선언까지 일시적 사각지대\(Temporal Dead Zone; TDZ; 스코프의 시작~초기화 지점 사이의 구간\) 에 빠지기 때문이다.
      * 변수는 선언 -&gt; 초기화 -&gt; 할당의 단계를 걸쳐 생성되며 var 키워드는 이 과정 중 선언,초기화 단계가 한번에 이루어져 에러가 발생하지 않는다.
      * let 은 선언과 초기화의 단계가 별도로 분리되어 진행된다. 즉 변수를 위한 메모리 공간을 확보는 변수 선언문이 수행될 때 이루어진다.
    * 전역으로 선언을 할지라도 전역 객체의 프로퍼티가 될 수 없다.
* block scope
  * 말 그대로 블록 단위로 스코프가 유지됨을 의미한다.
  * 클로저로 예를 살펴보자. [이유는 이곳에서 확인하자](https://github.com/GodChiken/StudyES5/blame/master/src/main/resources/markdown/scope/scope.md#L41-L61)

    ```javascript
      var myArray = [];  // 함수의 배열을 생성하는 for 루프의 i는 전역 변수다.  for (var i = 0; i < 3; i++) {    myArray.push(function () {       console.log(i);     });  }  // 배열에서 함수를 꺼내어 호출한다.  for (var j = 0; j < 3; j++) {    myArray[j]();  }  // 3이 3번 출력된다.
    ```

  * 위 코드를 0,1,2 순으로 올바르게 작동 시키려면 다음과 같다.

    ```javascript
      var myArray = [];  // 함수의 배열을 생성하는 for 루프의 i는 전역 변수다.  for (var i = 0; i < 3; i++) {    (function (index) {       myArray.push(function () { console.log(index); });    }(i));  }  // 배열에서 함수를 꺼내어 호출한다  for (var j = 0; j < 3; j++) {    myArray[j]();  }
    ```

  * let 키워드를 사용하면 이러한 클로저를 작성 안해도 동일한 효과를 가져갈 수 있다.

    ```javascript
      var myArray = [];  // i 는 루프의 코드 블록에서만 유효한 지역 변수이면서 자유 변수이다.  for (let i = 0; i < 3; i++) {    myArray.push(function () { console.log(i); });  }         for (var j = 0; j < 3; j++) {    console.dir(myArray[j]);    myArray[j]();  }
    ```
* const keyword
  * let 과 다르게 재할당을 금지할 때 사용하는 키워드이다.
  * 선언과 동시에 할당을 하여 상수처럼 사용한다.
  * 객체로도 사용이 가능하고 객체 자체의 재할당까진 금지하나 객체 내부의 프로퍼티까지 재할당을 관여하진 않는다.
* 활용 노하우
  * ES6를 사용한다면 var 키워드는 사용하지 않는다.
  * 재할당이 필요한 경우에 한정해 let 키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
  * 변경이 발생하지 않는\(재할당이 필요 없는 상수\) 원시 값과 객체에는 const 키워드를 사용한다. const 키워드는 재할당을 금지하므로 var, let 보다 안전하다.
  * 재할당이 필요한지 직관적으로 파악하긴 어렵기 때문에 const 로 선언하고 필요에 따라 let 으로 변경하는 방식의 개발을 해나가자

