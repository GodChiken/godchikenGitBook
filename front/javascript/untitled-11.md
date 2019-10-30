---
description: 함수에대하여 공부한다.
---

# 함수

* 함수 정의하기
  * 한번 정의한 이후 여러번 재사용을 하는 코드 블록
  * 인자 값을 취할 수 있으며 함수 내에서 지역변수와 동급으로 취급된다.
  * 이 인자값 외에도 호출 컨텍스트\(invocation context\)가 포함되는데 this 키워드의 값이 그러하다.
  * 사실 함수의 타입이 'Function' 이긴 하나 엄연히 객체이다.
    * 이로 인해서 다른 변수에 함수를 할당이 가능하고 인자 값으로 전달, 프로퍼티를 지정이 가능하다.
    * 개인적으로는 이러한 사실 때문에 콜백 지옥이 생긴게 아닌가 싶다.
  * function 키워드를 이용해 정의된다. 이 키워드로 함수 정의 표현식, 함수 선언문 에서 사용된다.    
    * 함수 정의 표현식

      ```javascript
        var functionExpressions = function(){
            //..something
        }
      ```

      * 호이스팅에 영향을 받지 않는다.

    * 함수 선언식

      ```javascript
        function functionDeclarations(){
            //..something
        }
      ```

      * 호이스팅에 영향을 받는다.  

    * 사족이긴 하지만 모듈 패턴의 내부 함수에 '\_'로 네이밍을 했던건 이 책에서의 출처가 아닐까 싶다.
    * return 을 이용한 반환되는 코드가 존재하지 않으면 undefined 로 반환된다.     
* 함수 호출하기
  * 총 네가지 방법으로 호출된다.
    * 일반적인 함수 형태
      * 너무 노말하므로 생략
    * 메서드 형태
      * 객체 안에 선언된 함수는 연관 배열이나 배열의 인덱스로도 접근이 가능하다.
      * 메서드와 this 키워드가 자바스크립트로 실천하는 객체지향 프로그래밍의 핵심이라고 언급하나 솔직 별론거 같다.
      * 변수와 달리 this 키워드는 유효범위가 없고, 중첩함수에서 상속을 하지 않는다.
      * 중첩함수가 메서드 형태로 호출 시 : this 는 중첩함수를 호출한 객체
      * 중첩함수가 메서드 형태로 호출 시 : this 는 global object\(일반모드\), undefined\(엄격모드\)
      * 그래서 outer 함수의 호출 컨텍스트를 접근하고 한다면, 미리 상위에 변수로 this 키워드를 사용하여 할당하고 사용하자.
    * 생성자
      * 함수나 메서드 호출 앞에 new 키워드가 존재 시 생성자 호출이다.
      * 일반,메서드 형태와 다르게 매개변수, 호출컨텍스트, 반환값을 다루는 방식이 다르다.
      * 인자 값이 있는 경우 먼저 평가된 이후 전달된다.
      * 또한 \(\) 값이 생략됨이 허용된다.

        ```javascript
          var o = new Object();
          var o = new Object;
        ```

      * 호출 시 생성자의 prototype 프로퍼티를 상속받은 빈 객체가 생성된다.
      * 생성자 함수는 객체를 초기화 하고, 이 객체는 생성자 함수의 호출 컨텍스트로 사용된다.
      * 그렇기에 this로 참조가 가능하다.
      * 생성된 객체의 호출 컨텍스트를 사용할 뿐, 메서드가 속한 객체의 호출 컨텍스트를 사용하지 않는다.
      * 내부에 return 키워드를 사용하지 않는 것이 관례. 초기화 이후 코드 블럭의 내용이 끝에 이르면 암시적으로 객체를 반환한다.
      * return 을 사용하여 명시적으로 특정 객체를 반환 경우는 그 객체가 생성자 호출 표현식의 값이 된다.
      * 반환값 없거나 primitive type을 반환하면 무시되고 새로 생성된 객체가 값이 된다.     
    * call\(\), apply\(\) 를 통한 호출
      * 두 메서드 모두 호출 시 this 값을 명시적으로 지정할 수 있다. 
      * 이것은 모든 함수에서도 특정 객체의 메서드를 호출이 가능함을 의미한다.
      * 소속되지 않아도 가능하다.
      * call\(\)
        * 전달 인자를 할당하면, 호출할 함수에 사용한다. 
      * apply\(\)     
        * 배열을 전달인자로 사용한다.
* 함수 전달인자와 매개변수
  * 함수를 정의하거나 호출할때 전달인자, 매개변수, 타입에 대해 별도의 검사는 자바스크립트에서 이루어지지 않는다.
  * 위의 경우 어떻게되는지 알아보자
  * 생략 가능한 매개변수
    * 정의된 매개변수보다 적은 인자 개수으로 함수를 호출할 경우 undefined 가 설정된다.
    * 위 사실을 이용해서 동적인 코딩이 가능하다. 
  * 가변길이 전달인자 목록 : Argument 객체
    * 위와 반대로 더 많이 보냈을 경우 직접적으로 참조할 방법이 없다.
    * 하지만 Argument 객체를 사용하면 해결은 할 수 있다. 
      * 유사 배열 객체이다.
      * 배열이 아니다.
    * 이름이 아닌 인덱스를 통해서 전달인자를 얻을 수 있다.
    * 반대로 호출한 쪽에서 넘긴 인자 값에 쓸데 없는 것이 있는 경우 방어 코딩이 가능하다.

      ```javascript
        // 이런 식으로 호출했을 경우
        f(1,2,3,4,5,6,7,8);
        function f(i,j){
            if (argument.length > 2){
                console.log("쓸데없는 것을 넘기지 마시오");
            }
        }
      ```

    * 위와 같이 가변적인 전달인자를 받을 수 있는 함수를 명명하는 것은 여러가지가 있으나 'varargs' 만 기억하겠다.    
    * 자꾸 툭하면 엄격 모드를 이책에서 언급하나 할 생각이 없으므로 생략한다.
    * Argument 객체 - callee 프로퍼티
      * 현재 실행되고 있는 함수를 참조한다.
    * Argument 객체 - caller 프로퍼티
      * 비표준이나, 호출한 함수를 참조한다. 재귀적인 처리를 요할때 유용하다.
  * 객체의 프로퍼티를 전달인자로 사용하기
    * 어떤 함수에 기억하기 어려울 정도로 매개변수가 존재한다면, 이 함수의 사용자가 올바르게 기억하기 어렵다.
    * 라고 하며 코드를 제안하나 애초에 이런 코드는 안 짤것이다. 모듈 패턴을 쓰고말지..
    * 궁금하면 '자바스크립트 완벽가이드 p218~219' 를 참고한다.
  * 전달인자의 형식
    * 이것도 마찬가지 할 가치를 못느끼겠다.
* 값으로서의 함수
  * 모듈 패턴 쓰시오.
* 네임스페이스로서의 함수
  * 어이쿠 모듈 패턴을 쓰시게.
* 클로저 \(본 이슈에서 다루지 않고 별도로 뺀다\)
  * '자바스크립트 완벽가이드 p226'
* 함수 프로퍼티, 메서드, 생성자
  * 중요하다 싶은 것만 꼬집어서 정리한다.
  * Function\(\) 생성자
    * 동적으로 자바스크립트 함수를 생성하여 실행 시간에 컴파일되는 것을 가능하게 한다.
    * 호출될 때마다 함수 몸체를 분석하여 새로운 함수 객체를 생성한다. 이와 반대로 중첩된 함수나 함수 정의 표현식은 루프 내라 하더라도 재 컴파일 되지 않는다.
    * 함수 생성자가 생성하는 함수는 렉시컬 스코프를 사용하지 않는다. 언제나 최상위 레벨 함수로 컴파일 된다.
* 함수형 프로그래밍
  * 고차함수
    * 하나 이상의 함수를 인자로 받고 새 함수를 반환하는 것을 의미한다.
  * 함수의 파셜 어플리케이션
    * 쭉 설명하며 함수의 함수의 함수의 호출식으로 되어 있으나, ES6의 에로우 펑션이 이 역활을 할거같다.
  * 메모이제이션
    * 이전 결과를 캐시 해놓는 것을 메모이제이션이라 한다. ES6에서는 어찌 해결하는지 봐야겠다.
