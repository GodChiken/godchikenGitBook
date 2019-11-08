---
description: 실행 컨텍스트에 대한 목차를 정하고 정리
---

# 실행 컨텍스트



* 실행 컨텍스트\(Execution Context\)
  * 자바스크립트의 동작원리를 담고 있는 핵심원리이다.
  * 실행 코드가 수행될 수 있도 구성된 환경을 의미한다.
  * 즉 자바스크립트 런타임에서 엔진이 만들어주는 런타임 유효 범위라고 할 수 있다.
* 실행 코드
  * 다음 세 종류로 나뉘어 초기화 과정이 이루 진다.
    * 글로벌 코드 : 코드 전역에서 수행되는 코드
    * eval 코드 : eval 함수로 수행되는 코드
    * function 코드 : 함수 내에 존재하는 코드
  * 이와 같이 실행에 필요한 정보를 형상화하고 구분하기 위해 자바스크립트 엔진은 실행 컨텍스트를 물리적 객체의 형태로 관리한다.
  * 자바스크립트 엔진이 코드를 실행하려면 다음과 같은 정보를 실행 컨텍스트에서 물리적인 객체 형태로 관리가 되어야한다.
    * 변수 : 전역변수, 지역변수, 매개변수, 객체의 프로퍼티
    * 함수 선언
    * 변수의 유효범위\(Scope\)
    * this
  * 물리적인 형태는 크게 다음과 같다.
    * Value Object : 변수, 함수 선언문, 아규먼트
    * Scope Chain : Variable Object 와 모든 부모의 스코프
    * this : 컨텍스트 오브젝트
  * 이러한 실행 코드를 하나씩 접근할떄 자바스크립트는 실행 컨텍스트를 생성한다.
    * FunctionDeclaration, WithStatement, TryStatement 의 Catch 절과 같은 몇가지 특정한 구문 구조와 연관되어 있다.
* 실행 컨텍스트의 구조
  * Lexical Environment, Variable Environment 는 Lexical Environment 타입을 가지고 ThisBinding 은 object 타입을 가진다.
  * Variable Environment 도 Lexical Environment 인데 어째서 나뉜건가?
  * Lexical Environment :

    > 코드의 정적인 렉시컬 중첩 구조를 기반으로 하는 함수, 변수 등을 열거한다.  
    > 스코프 내부의 식별자들이 어떻게 바인딩 되어있는지 기록하는 역활을 한다.  
    > 외부 렉시컬 환경을 참조하는 구조를 취한다. 논리적으로 중첩되는 구조를 모델링 하기 위함이다.  
    > 가령 함수 선언문 중첩됬을 경우 현재 실행한 것에 대한 렉시컬 환경을 가질 수 있다.  
    > 렉시컬 환경은 자바스크립트 엔진이 실행 컨텍스트를 어떻게 생성하는가에 대한 정의라고 할 수있다.  
    > 또한 직접적인 제어는 불가능하다.

    * Environment Record
      * 추상 클래스라고 생각하면 된다.
      * 크게 두가지 구체적인 서브클래스로 나뉠 수 있다.
        * Object Environment Record : 객체 환경 레코드

          * 해당되는 컨텍스트 바인딩 하는 역활만 한다.

          ```javascript
              ObjectEnvironmentRecord = {
                  bindObject: [object]
              }
          ```

        * Declarative environment Record \(선언적 환경 레코드\)

          * ES3에서는 없었으나 ES5 부터 생겼다고 한다.
          * 정확한건 아니나 성능상이나 자바스크립트의 메커니즘상 더 부합해서 일까라는 자료를 봤다.
          * 선언적 환경 레코드는 ECMAScript 언어 구문 앨리먼트의 효과를 정의하는데 사용되는데, 그 언어 구문 앨리먼트들이란 FunctionDeclarations, VariableDeclarations, Catch 이 있다. 이들은 식별자 바인딩을 ECMAScript 언어 값과 직접 연관 시킨다.
          * 다음의 코드와 같이 와 Object 안에 실제 값들이 저장되어 있는 형태를 취한다.

          ```javascript
              DeclarativeEnvironmentRecord = {
                  a: 33,
                  b: 'Hello World'
              }
          ```
      * 환경 레코드는 위 설명과 같이 목적에 의해 두개로 나뉘어진 \(구채적인\)서브 클래스로 이루어진 추상 클래스다.
      * 콘크리드 구조의 추상 메서드는 별도로 조사하진 않고 종류만 알아두고 넘어간다.
        * HasBinding\(N\)
        * CreateMutableBinding\(N, D\)
        * SetMutableBinding\(N,V, S\)
        * GetBindingValue\(N,S\)
        * DeleteBinding\(N\)
        * ImplicitThisValue\(\)
    * Outer Environment Reference
      * 현재 Context 를 기준으로 외부 Context 를 참조하는 공간이다. 만약 유효범위가 중첩되어 있을 때 상위 유효범위를 참조할 수 있어야 하는데 상위 유효범위를 참조하기 위해서 존재하는 property 이다.
      * 그렇기 때문에 record 가 아닌 reference 라는 이름이 붙여졌다.

  * Variable Environment : 실행 컨텍스트 내에서 렉시컬 환경의 일부일 뿐이며 본질적으로 현재 컨텍스트 내에서 선언 된 변수와 함수이다.
  * This Binding
    * 현재 실행중인 컨텍스트의 This 를 가르킨다.
* 전역 코드의 실행
  * Global Code 를 살펴보면 다음과 같다.

    ```javascript
        Global Execution Context {
            LexicalEnvironment: GlobalEnv,
            VariableLexicalEnvironment: GlobalEnv,
            ThisBinding: window
        }
    ```

  * 전역 실행 컨텍스트의 Lexical Environment 와 VariableLexical Environment 는 같은 GlobaleEnv 를 참조하고 있다.
  * This Binding 은 window 를 참조하고 있는데 브라우저의 전역 객체가 Window 이기 때문이다.
  * 만약 브라우저가 아닌 다른 환경\(Node.js\)일 경우에는 Global 같은 객체가 들어갈 것이다.
  * 전역 실행 컨텍스트의 렉시컬 환경이 참조하는 GlobalEnv 는 다음과 같다.

    ```javascript
    GlobalEnv {
      ObjectLexicalRecord: {
        ObjectBinding: Window,
      },
      OuterEnvironmentReference: null,
    }
    ```

  * 위와 같이 되어 있는데 객체 렉시컬 환경의 ObjectBinding 프로퍼티가 Window 를 보고있는데 GlobalEC의 ThisBinding도 Window를 보고 있다.
  * ObjectBinding 에 미리 Window 를 넣어놓고 그 후에 Object Lexical Record 에 넣어주는 형태이다.
  * 전역에서 변수를 선언했을때 전역객체의 프로퍼티에서 변수를 찾는 일이 불가능하다.
  * Global Code 와 Function Code 를 나눠서 EC를 생성하는 이유중에 하나가 될 수 있을 것같다.
  * 전역에서 변수를 선언하면 그 변수는 글로벌 환경의 객체 렉시컬 환경 내부에 있는 ObjectBinding 에서 찾게되고
  * ThisBinding 역시 Window 를 바라보고 있기때문에 전역에서 선언한 변수가 Global Object 의 프로퍼티로 찾을 수 있게 되는 것이다.
  * Outer Environment Reference 는 Global 이므로 상위 EC가 없다. 그래서 null이 들어간다.
* 함수 코드의 실행
  * 함수 코드의 실행 컨텍스트가 만들어 질때는 실행 컨텍스트 스택에 전역 실행 컨텍스트가 생성되어 있다.

    ```javascript
        function sum(x, y) {
            var result = x + y;
            var etc = function() {
                console.log('good');
            }
    
            function msg() {
                return result;
            }
        }
        sum(10, 20);
    ```

  * 자바스크립트 엔진이 sum\(10, 20\)을 만나면 함수에 대한 EC를 생성하게 된다.
  * 우선적으로 this 를 찾아서 바인딩 해주게 되는데 엔진이 this 를 바인딩하는 메커니즘은 함수 실행시에 좌항을 보는 것이다.
  * 만약 저 함수가 메서드로 실행된다면 메서드를 가지고 있는 객체가 this 에 바인딩 될 것이고 new 를 붙여서 실행한다면 해당 함수 자체가 this가 될 것이다.
  * 위 코드에서는 좌항에 아무것도 없기 때문에 저 EC의 ThisBInding 은 null 이 될 것이다.
  * EC의 ThisBinding 이 null 일 경우에는 Global EC를 참조하게 된다.
  * 브라우저에서는 Window 가 될 것이다.
  * 단, “use strict” 모드에서는 Global EC가 아닌 null 이 그대로 들어가게 된다.
  * sum 의 렉시컬 환경은

    ```javascript
      {
        DeclarativeEnvironmentRecord: { },
        OuterEnvironmentReference: null
      }          
    ```

  * 위와 같이 구성되는데 첫번째로 파라미터로 들어오는 x와 y가 저 곳에 들어가게 된다.

    ```javascript
        {
            DeclarativeEnvironmentRecord: {
                x: 10,
                y: 20
            },
            OuterEnvironmentReference: null
        }
    ```

  * 와 같은 형태가 될 것이고 그 다음으로는 함수 내부에 선언된 것들을 가져오는데, 처음으로는 함수선언을 먼저 가져온다.

    ```javascript
        {
            DeclarativeEnvironmentRecord: {
                x: 10,
                y: 20,
                msg: Function Reference
            },
            OuterEnvironmentReference: null
        }
    ```

  * 그리고 함수 내부에서 사용할 수 있는 arguments 를 세팅한다.

    ```javascript
    {
        DeclarativeEnvironmentRecord: {
            x: 10,
            y: 20,
            msg: Function Reference,
            arguments: Arguments Object,
        },
        OuterEnvironmentReference: null,
    };
    ```

  * 그리고 선언된 변수들을 가져오게 된다. 함수 내부에서 사용할 수 있는 arguments 를 세팅한다.

    ```javascript
    {
        DeclarativeEnvironmentRecord: {
            x: 10,
            y: 20,
            msg: Function Reference,
            arguments: Arguments Object,
            result: undefined,
            etc: undefined,
        },
        OuterEnvironmentReference: null,
    }
    ```

  * 이렇게 되는데 undefined 이고 msg 는 Function Reference 를 참조하는 이유는 함수 선언식과 함수 표현식에 대한 차이이다.
  * sum 의 EC의 LE의 DeclarativeEnvironmentRecord 의 세팅이 완료 되었는데 이게 바로 호이스팅의 실체이기 때문이다.
  * 런타임에서 실제적인 실행 전에 EC를 구성하는 단계에서 이런식으로 호이스팅이 일어나며 함수가 실행된다.
  * 이제 OuterEnvironmentReference 를 세팅해준다.
  * OuterEnvironmentReference 는 스팩을 보면 가까운 상위 EC를 참조한다고 되어있는데 현재의 경우에서는 상위 EC가 Global EC 이다.

    ```javascript
        {
            DeclarativeEnvironmentRecord: {
                x: 10,
                y: 20,
                msg: Function Reference,
                arguments: Arguments Object,
                result: undefined,
                etc: undefined,
            },
            OuterEnvironmentReference: GloabalEC,
        }
    ```

    * var result = x + y;를 만나서 계산한 후에 result 가 갱신된다.

      ```javascript
      {
          DeclarativeEnvironmentRecord: {
              x: 10,
              y: 20,
              msg: Function Reference,
              arguments: Arguments Object,
              result: 30,
              etc: undefined,
          },
          OuterEnvironmentReference: GloabalEC,
      }
      ```

    * var etc에 선언된 함수를 접근하여 다음과 같이 채워진다.

      ```javascript
          {
              DeclarativeEnvironmentRecord: {
                x: 10,
                y: 20,
                msg: Function Reference,
                arguments: Arguments Object,
                result: 30,
                etc: Function Reference,
              },
              OuterEnvironmentReference: GloabalEC,
          }
      ```

    * 함수를 실행할때 EC가 생성되는 과정을 보았다.
    * 만약 이제 상위 EC에 있는 값을 사용하려 할때는 어떻게 하는가 ? 에 대해 궁금해 할 수있는데 이때 사용되는것이 OuterEnvironmentReference 이다.
    * 이때 저 곳에 참조된 값을 사용하여서 상위 Scope로 찾아올라가게 된다.
    * 이 과정이 바로 Scope Chain 이다.
* 스코프 체인\(Scope Chain\)
  * 스코프 체인은 일종의 리스트로서 전역 객체와 중첩된 함수의 스코프의 레퍼런스를 차례로 저장하고 있다.
  * 스코프 체인은 해당 전역 또는 함수가 참조할 수 있는 변수, 함수 선언 등의 정보를 담고 있는 전역 객체\(Global Object\) 또는 활성 객체\(Active Object\)의 리스트를 가리킨다.
  * 현재 실행 컨텍스트의 활성 객체를 선두로 하여 순차적으로 상위 컨텍스트의 활성 객체를 가리키며 마지막 리스트는 전역 객체를 가리킨다.
  * 최상위 스코프는 null 이다. 최상위니까 더이상 갈 곳이 없으니까.
  * 엔진은 스코프 체인을 통해 렉시컬 스코프를 파악한다. 함수가 중첩 상태일 때 하위함수 내에서 상위함수의 스코프와 전역 스코프까지 참조할 수 있는데 이것는 스코프 체인을 검색을 통해 가능하다.
  * 함수가 중첩되어 있으면 중첩될 때마다 부모 함수의 스코프가 자식 함수의 스코프 체인에 포함된다.
  * 함수 실행중에 변수를 만나면 그 변수를 우선 현재 스코프에 해당되는 활성 객체에서 검색해보고, 만약 검색에 실패하면 스코프 체인에 담겨진 순서대로 그 검색을 이어가게 되는 것이다.
  * 예를 들어 함수 내의 코드에서 변수를 참조하면 엔진은 스코프 체인의 첫번째 리스트가 가리키는 AO에 접근하여 변수를 검색한다.
  * 만일 검색에 실패하면 다음 리스트가 가리키는 활성 객체를 타고 흐르다 최종적으로 전역 객체를 검색한다.
  * 이와 같이 순차적으로 스코프 체인에서 변수를 검색하는데 결국 검색에 실패하면 정의되지 않은 변수에 접근하는 것으로 판단하여 Reference 에러를 발생시킨다.
  * 스코프 체인은 함수의 감추인 프로퍼티인 "\[\[Scope\]\]"로 참조할 수 있다.
* 참고 자료
  * [https://stackoverflow.com/questions/500431/what-is-the-scope-of-variables-in-javascript](https://stackoverflow.com/questions/500431/what-is-the-scope-of-variables-in-javascript)

