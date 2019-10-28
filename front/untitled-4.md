---
description: 클로저에 대해 정리하기전 목차를 정하고 정리한다.
---

# 클로저

클로저\(closure\)란?

* 자바스크립트 고유의 개념은 아니다.
  * MDN 에서는 다음과 같이 정의한다.

    > 클로저는 함수와 그 함수가 선언됐을 때의 렉시컬 환경\(Lexical environment\)과의 조합이다.

  * 자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우, 외부 함수 밖에서 내부함수가 호출되더라도 외부함수의 지역 변수에 접근할 수 있는데 이러한 함수를 클로저\(Closure\)라고 부른다.

    > 클로저는 반환된 내부함수가 자신이 선언됐을 때의 환경\(Lexical environment\)인 스코프를 기억하여 자신이 선언됐을 때의 환경\(스코프\) 밖에서 호출되어도 그 환경\(스코프\)에 접근할 수 있는 함수 즉 클로저는 자신이 생성될 때의 환경\(Lexical environment\)을 기억하는 함수다.

  * 실행 컨텍스트의 관점에 설명하면, 내부함수가 유효한 상태에서 외부함수가 종료하여 외부함수의 실행 컨텍스트가 반환되어도, 외부함수 실행 컨텍스트 내의 활성 객체\(변수, 함수 선언 등의 정보를 가지고 있다\)는   

    내부함수에 의해 참조되는 한 유효하여 내부함수가 스코프 체인을 통해 참조할 수 있는 것을 의미한다.
* 클로저의 활용
  * 클로저는 전역변수의 사용을 억제하고 변경된 최신 정보의 사항을 유지하는 것을 목적으로 두고있다.             
  * 가령 전체적으로 공유를 해줘야 되는 전역변수가 있다고 가정하자. 이럴 경우 전역변수를 남발하거나 그 전역변수에 의존을 하는 함수가 많아 질수록 유지보수성은 급격히 하락한다.
  * 그래서 함수 내부에 지역변수를 활용하여 초기화를 하는 식의 방지를 추구할 수 있으나, 변경사항에 대한 대처는 불가하므로 원하는 로직의 수행이 불가능하다.

    ```markup
          <!DOCTYPE html>
          <html>
          <body>
            <p>지역 변수를 사용한 Counting</p>
            <button id="inclease">+</button>
            <p id="count">0</p>
            <script>
              var incleaseBtn = document.getElementById('inclease');
              var count = document.getElementById('count');

              function increase() {
                // 카운트 상태를 유지하기 위한 지역 변수
                var counter = 0;
                return ++counter;
              }

              incleaseBtn.onclick = function () {
                count.innerHTML = increase();
              };
            </script>
          </body>
          </html>
    ```

  * 이전에 정리했전 즉시실행함수\(IIFE\)\(참고 : \#4 이슈\)도 일종의 클로저이다.

    > 이 즉시실행함수를 클로저로 선언하여 사용히면 호이스팅에도 영향을 받지 않고 변경된 정보를 유지하는 모듈 패턴을 만드는 구성요소가 된다.

    ```javascript
      function Counter() { 
        // 카운트를 유지하기 위한 자유 변수 
        var counter = 0;    
        // 클로저
        this.increase = function () {
          return ++counter;
        };

        // 클로저
        this.decrease = function () {
          return --counter;
        };
      }

      var counter = new Counter();

      console.log(counter.increase()); // 1
      console.log(counter.decrease()); // 0        
    ```

    \`\`\`

  * 생성자 함수 Counter는 increase, decrease 메소드를 갖는 인스턴스를 생성한다. 
  * 이 메소드들은 모두 자신이 생성됐을 때의 렉시컬 환경인 생성자 함수 Counter의 스코프에 속한 변수 counter를 기억하는 클로저이며 렉시컬 환경을 공유한다. 
  * 생성자 함수가 함수가 생성한 객체의 메소드는 객체의 프로퍼티에만 접근할 수 있는 것이 아니며 자신이 기억하는 렉시컬 환경의 변수에도 접근할 수 있다.
  * 이때 생성자 함수 Counter의 변수 counter는 this에 바인딩된 프로퍼티가 아니라 변수다. 
  * counter가 this에 바인딩된 프로퍼티라면 생성자 함수 Counter가 생성한 인스턴스를 통해 외부에서 접근이 가능한 public 프로퍼티가 되지만 생성자 함수 Counter 내에서 선언된 변수 counter는 생성자 함수 Counter 외부에서 접근할 수 없다. 
  * 하지만 생성자 함수 Counter가 생성한 인스턴스의 메소드인 increase, decrease는 클로저이기 때문에 자신이 생성됐을 때의 렉시컬 환경인 생성자 함수 Counter의 변수 counter에 접근할 수 있다. 
  * 이러한 클로저의 특징을 사용해 클래스 기반 언어의 private 키워드를 흉내낼 수 있고 활용한 것이 모듈 패턴이다. 



![&#xD074;&#xB85C;&#xC800; &#xD655;&#xC778;&#xD574;&#xBCF4;&#xAE30;](https://user-images.githubusercontent.com/16012504/65041717-51a91980-d992-11e9-95a2-3139801a048c.png)



![&#xD074;&#xB85C;&#xC800; &#xC548;&#xC5D0; counter&#xAC00; &#xC0B4;&#xC544; &#xC788;&#xB294;&#xAC00;?](https://user-images.githubusercontent.com/16012504/65042964-f4fb2e00-d994-11e9-80bb-8e9e84e3133f.png)

            

