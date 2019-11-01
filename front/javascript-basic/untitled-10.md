---
description: this keyword에 대해 알아보자
---

# This Binding



* this binding
  * 함수의 호출방식에 의해서 해당 호출 시점에 어떤 객체를 this 에 바인딩할지 결정하는 것을 의미한다.
  * 함수의 호출방식은 다음과 같다.
    * 함수\(Function\) 호출
    * 메소드\(Method\) 호출
    * 생성자 함수
    * Function property 의 apply, call, bind 사용

      > 함수와 메서드의 차이? 함수와 메서드의 차이는 특정 객체에 종속적이냐 아니냐에 따라서 갈린다. 한마디로 보는 관점에서의 차이.
* 함수 호출
  * 사실 함수 호출도 따지고보면 메서드 호출이다.
  * 함수는 전역객체의 프로퍼티로 접근할 수 있기 때문이다.
  * 기본적으로 this 는 전역 객체에 바인딩 된다. \(Browser : window, Node.js : Global\)
  * inner function 및 callback function 는 외부 함수를 바라보지 않으며, 전역 객체에 바인딩 된다.

    ```javascript
      var globalVar = 'something';

      console.log(globalVar);
      console.log(window.ga);

      function foo(){
          console.log('invoke');
          function inFoo(){
              console.log(this);      //window
          }
      }

      window.foo();
    ```

  * 결론적으로는 함수 자체가 별도의 처리가 없으면 항상 this binding 은 전역객체를 바인딩 한다. [왜 그런가?](https://github.com/GodChiken/StudyES5/blame/master/src/main/resources/markdown/context/executionContext.md#L117-L122)
  * 그렇다면 어떤 함수이건 간에 호출시점에서 다른 객체에 Binding 된 this 를 활용하는 방법은 무엇이 있을까? [왜지?](https://github.com/GodChiken/StudyES5/blame/master/src/main/resources/markdown/function/function.md#L32-L35)
  * 위의 링크와 다른 예제로 this 를 명시적으로 바인딩 하는 apply, call, bind 등을 활용하는것도 하나의 방법이다.
* 메소드 호출
  * 함수가 객체의 프로퍼티 값이면 메소드로서 호출된다. 이때 메소드 내부의 this 는 해당 메소드를 소유한 객체, 즉 해당 메소드를 호출한 객체에 바인딩된다.
  * 프로토타입 객체도 메소드를 가질 수 있다. 프로토타입 객체 메소드 내부에서 사용된 this 도 일반 메소드 방식과 마찬가지로 해당 메소드를 호출한 객체에 바인딩된다.

    ```javascript
      function Person(name) {
        this.name = name;
      }

      Person.prototype.getName = function() {
        return this.name;
      }

      var me = new Person('Lee');
      console.log(me.getName());

      Person.prototype.name = 'Kim';
      console.log(Person.prototype.getName());
    ```
* 생성자 함수 호출
  * 뭐라고 선언하든지 간에 기존 함수 new 키워드를 붙여 호출하면 생성자 함수가 된다.
  * 규제가 되있는건 아니나 보통 생성자 함수는 첫 글자는 대문자로 수행한다.
  * 생성자 함수의 작동 방식
    1. 빈 객체 생성 및 this binding
       * 생성자 함수의 코드가 실행되기 전에 빈 객체가 생성된다. 
       * 이 빈 객체가 생성자 함수를 새로 생성하는 객체라고 하며 this 키워드는 이 객체를 binding 한다.
       * 생성된 빈 객체는 생성자 함수의 prototype property 가 가르키는 객체를 자신의 프로토타입 객체로 설정한다.
    2. this 를 통한 프로퍼티 생성
       * 생성된 빈 객체에 this 를 활용하여 동적으로 프로퍼티나 메서드를 생성이 가능하다. this 는 새로 생성된 객체를 가르키므로 this 를 통해 생성한 프로퍼티,메서드는 새로 생성된 객체에 추가된다. 
    3. 생성된 객체 반환
       * 반환문이 없는 경우
         * this 에 binding 되어있는 가장 최근에 생성된 객체가 반환된다.

           > 명시적으로 return this; 를 하여도 마찬가지다.
       * 반환문이 있는경우
         * 명시적인 this 반환 외의 다른 객체를 반환하는 경우 해당 객체를 반환한다.
         * 또한 생성자 함수로서 역활 수행이 불가하다.
  * 객체 리터럴 vs 생성자 함수
    * 객체 리터럴 방식과 생성자 함수 방식의 차이는 프로토타입 객체\(\[\[Prototype\]\]\)에 있다.
      * 객체 리터럴 방식의 경우, 생성된 객체의 프로토타입 객체는 Object.prototype 이다.
      * 생성자 함수 방식의 경우, 생성된 객체의 프로토타입 객체는 Person.prototype 이다.

        ```javascript
        // 객체 리터럴 방식
        var foo = {
        name: 'foo',
        gender: 'male'
        }

        console.dir(foo);

        // 생성자 함수 방식
        function Person(name, gender) {
        this.name = name;
        this.gender = gender;
        }

        var me  = new Person('Lee', 'male');
        console.dir(me);

        var you = new Person('Kim', 'female');
        console.dir(you);
        ```
* 생성자 함수에 new 키워드를 붙이지 않을 경우
  * 결론적으론 생성자 함수는 특별히 자바스크립트에서 제제하는 부분이 없고 의미상 목적을 가지고 첫 글자를 대문자로 써서 활용한다 하였따.
  * 그러나 객체 생성을 목적으로 대문자로 생성자 함수를 선언 했을지라도 'new' 키워드를 사용하지 않고 사용한다면 오류가 있다.
  * 왜냐하면 일반 함수와 생성자 함수의 this binding 방식이 엄연히 다르기 때문이다.
    * 일반 함수는 전역 객체에 바인딩 된다.
    * new 키워드를 사용하여 생성자 함수를 호출하면 암묵적으로 생성된 빈 객체가 바인딩 된다.

      ```javascript
        function Person(name) {              
          this.name = name; // new 없이 호출하는 경우, 함수의 프로퍼티는 전역객체에 추가된다.
        };

        // 일반 함수로서 호출되었기 때문에 객체를 암묵적으로 생성하여 반환하지 않는다.            
        var me = Person('Kim');

        console.log(me); // undefined
        console.log(window.name); // Kim, node.js인 경우에는 global.name
      ```
  * 이러한 실수를 방지하기 위하여 Scope-Safe Constructor 패턴이라는 것이 존재한다. \(라이브러리에 적극 사용되는 패턴\)
  * 대부분의 빌트인 생성자\(내장 함수\)는 new 연산자의 유무를 따져서 적합한 값을 반환한다.

    ```javascript
          function ScopeSafeConstructorPattern(arg) {
              // 생성자 함수가 new 연산자와 함께 호출되면 함수의 선두에서 빈객체를 생성하고 this에 바인딩한다.
              // this가 호출된 함수(arguments.callee, 본 예제의 경우 A)의 인스턴스가 아니면 new 연산자를 사용하지 않은 것이므로 이 경우 new와 함께 생성자 함수를 호출하여 인스턴스를 반환한다.
              // arguments.callee는 호출된 함수의 이름을 나타낸다. 이 예제의 경우 A로 표기하여도 문제없이 동작하지만 특정함수의 이름과 의존성을 없애기 위해서 arguments.callee를 사용하는 것이 좋다.

              if (!(this instanceof arguments.callee)) {
                  return new arguments.callee(arg);
              }

              // 프로퍼티 생성과 값의 할당
              this.value = arg ? arg : 0;
          }

          var a = new A(100);
          var b = A(10);

          console.log(a.value);
          console.log(b.value);
    ```
* apply, call, bind 호출
  * Function prototype 에 있는 메소드를 이용하여 명시적으로 this 를 바인딩 하는 방식이다.         
  * 정리를 하려고 했으나 특별히 실무적으로 사용할 것 같지 않아 마무리 한다.               

