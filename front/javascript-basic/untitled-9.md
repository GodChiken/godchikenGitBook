# 이벤트 & 이벤트 핸들링



* 이벤트
  * 브라우저에서의 이벤트란 예를 들어 사용자가 버튼을 클릭했을 때, 웹페이지가 로드되었을 때와 같은 것인데 이것은 DOM 요소와 관련이 있다.
  * 이벤트가 발생하는 시점이나 순서를 사전에 인지할 수 없으므로 일반적인 제어 흐름과는 다른 접근 방식이 필요하다. 
  * 즉, 이벤트가 발생하면 누군가 이를 감지할 수 있어야 하며 그에 대응하는 처리를 호출해 주어야 한다.
  * 브라우저는 이벤트를 감지할 수 있으며 이벤트 발생 시에는 통지해 준다. 이 과정을 통해 사용자와 웹페이지는 상호작용\(Interaction\)이 가능하게 된다.
* 이벤트 핸들러
  * 특정 이벤트가 발생 시 실행되는 함수를 총칭한다.

![&#xC774;&#xBCA4;&#xD2B8; &#xB8E8;&#xD504;&#xC640; &#xBE0C;&#xB77C;&#xC6B0;&#xC800; &#xD658;&#xACBD;](https://user-images.githubusercontent.com/16012504/65396587-a6a9bd00-dde2-11e9-9c70-74b866241dad.png)

* 이벤트 루프\(Event Loop\) 와 동시성\(Concurrency\)
  * 브라우저는 single-thread 에서 event-driven 방식으로 동작한다.
  * 하나의 작업은 하나의 스레드 안에서 이루어 지지만 자바스크립트는 Concurrency 를 지원하기 때문에 많은 작업이 동시에 처리되는 것처럼 느껴진다.
  * 자바스크립트의 엔진
    * 여러 종류의 자바스크립트 엔진이 존재하나 크게 다음 2가지 영역으로 나뉜다.
      * Call Stack \(호출스택\)
        * 작업이 요청되면 순차적으로 이곳에 쌓이게 되며 순차적으로 수행된다.
        * 오직 한개만 존재하고 사용하며, 처리하는 작업이 종료되기 전까지 다른 작업을 수행하지 않는다.
      * Heap
        * 동적으로 생성된 객체 인스턴스가 할당되는 영역이다.
    * 단순히 엔진은 Call Stack 을 활용하여 작업을 순차적으로 실행할 뿐이다.
  * 동시성을 가지고 실행하기 위해서는 구동되는 환경, 브라우저, Node.js 가 처리한다.
    * Event Queue\(혹은 Task Queue\)
      * 비동기 처리 함수의 콜백, 이벤트 핸들러, Timer 함수 등이 위치하는 영역이다.
      * Event Loop 에 의해서 Call Stack 이 비어졌을 때 순차적으로 할당되어 실행된다.
    * Event Loop\(이벤트 루프\)
      * Call Stack 내에서 작업이 있는지, Event Queue 에 작업이 있는지를 반복하며 실행하는 것을 이야기한다.             
  * 코드를 통해 어떻게 작동할 지 예측해보자

    ```javascript
          function func1() {
            console.log('func1');
            func2();
          }

          function func2() {
            setTimeout(function () {
              console.log('func2');
            }, 0);

            func3();
          }

          function func3() {
            console.log('func3');
          }

          func1();
    ```

    * 별도의 특별한 함수가 없다면 순서대로 실행이 되겠지만 그렇지가 않다.
    * 비동기에 관련된 이벤트가 설명될때 자주 언급되는 setTimer\(\) 이벤트는 다음과 같은 방식으로 실행된다.
      * 평범한 함수의 경우에는 순서대로 Call Stack 에 쌓인다.
      * 즉시 실행되지 않고, 타이머 객체의 실행 이벤트\(tick\)가 발생하면 Event Queue 에 적재된다.
      * _**\(핵심!!\) 그 이후에 Call Stack 이 전부 비워진 이후에 실행이 된다.**_
      * 

![&#xC774;&#xBCA4;&#xD2B8; &#xB8E8;&#xD504;\(Event Loop\)&#xC5D0; &#xC758;&#xD55C; setTimeout &#xCF5C;&#xBC31;&#xD568;&#xC218;&#xC758; &#xC2E4;&#xD589;](https://user-images.githubusercontent.com/16012504/65396616-086a2700-dde3-11e9-8ea5-e0674aedffb3.gif)

* 이벤트 핸들러 등록 방식
  * 인라인 이벤트 핸들러 방식 
    * DOM 요소에는 이벤트 핸들러에 관한 속성이 존재하지만 쓰지 않는 점이 좋다.
    * 관심사가 다르고 자바스크립트의 이벤트가 DOM 요소에 종속적으로 두면 각 관심사 별로 집중이 안되어 분리해서 사용하곤 있다.
  * 스크립트 코드 내에서 특정 DOM 의 프로퍼티에 해당 이벤트 핸들러 기술
    * 이벤트 핸들러 프로퍼티 방식은 이벤트에 하나의 이벤트 핸들러만을 바인딩할 수 있다
    * 여러 이벤트를 등록하기 위해서는 addEventListener\(IE 8 이하 : attachEvent\) 를 통해 등록해야한다.
    * eventTarget.addEventListener\('eventType', functionName \[, useCapture\]

      > useCapture 는 기본이 false 이며 버블링을 허용하게된다.



      ```javascript
      <script>
          var btn = document.querySelector('.btn');        
    
          btn.onclick = function () {
            alert('나는 아래 함수로 재선언 되기 때문에 등록이 안되');
          };

          // 두번째 바인딩된 이벤트 핸들러
          btn.onclick = function () {
            alert('윗놈의 자리를 내가 차지했다.');
          };

          btn.addEventListener('click', function () {
            alert('아우 먼저');
          });
            
          btn.addEventListener('click', function () {
            alert('형님 먼저');
          });
      </script> 
      ```



![&#xC774;&#xBCA4;&#xD2B8;&#xC758; &#xD750;&#xB984;](https://user-images.githubusercontent.com/16012504/65396651-6991fa80-dde3-11e9-9b88-2d6f80c60cae.png)

* 이벤트의 흐름
  * 버블링 \(Bubbling\)
    * 엘리먼트에서 이벤트가 감지 되었을 때, 해당 엘리먼트를 포함하고 있는 부모 엘리먼트를 통하여 최상위 까지 이벤트가 전달되는 것을 버블링이라고 한다.
  * 캡쳐링 \(Capturing\)
    * 캡처링은 window 부터 최초 이벤트가 발생한 자식 요소로 내려가는 과정을 말한다.
  * 주의할 것은 버블링과 캡처링은 둘 중에 하나만 발생하는 것이 아니라 캡처링부터 시작하여 버블링으로 종료한다는 것이다. 
  * 즉, 이벤트가 발생했을 때 캡처링과 버블링은 순차적으로 발생한다. IE8 버전에서 지원되지 않는다.
  * 버튼을 클릭했다고 했을 때 다음 코드의 결과를 예상해보자.

    ```markup
      <html>
      <head>
        <style>
          html, body { height: 100%; }
        </style>
      <body>
        <p>버블링과 캡처링 이벤트 <button>버튼</button></p>
        <script>
          const body = document.querySelector('body');
          const para = document.querySelector('p');
          const button = document.querySelector('button');

          // 버블링
          body.addEventListener('click', function () {
            console.log('Handler for body.');
          });

          // 캡처링
          para.addEventListener('click', function () {
            console.log('Handler for paragraph.');
          }, true);

          // 버블링
          button.addEventListener('click', function () {
            console.log('Handler for button.');
          });
        </script>
      </body>
      </html>
    ```

  * body, button 요소는 버블링 이벤트 흐름만을 캐치하고 p 요소는 캡처링 이벤트 흐름만을 캐치한다. 따라서 button 에서 이벤트가 발생하면 먼저 캡처링이 발생하므로 p 요소의 이벤트 핸들러가 동작하고 그후 버블링이 발생하여 button, body 요소의 이벤트 핸들러가 동작한다.
  * 누구나 코딩할때 위와같은 문제를 겪었을 것이고 신경쓰지 않고 해당 DOM 에만 집중하고 싶었을 것이다.
    * Jquery 를 사용하고 있다면 해당 이벤트 핸들러에 return 값으로 false  쥐어준다.
    * 바닐라 자바스크립트의 경우는 다음의 절차를 따른다.
      * 요소가 가지고 있는 기본 동작을 중단시키기 위한 메소드가 preventDefault\(\)를 이용한다.
      * 어느 한 요소를 이용하여 이벤트를 처리한 후 이벤트가 부모 요소로 이벤트가 전파되는 것을 중단시키기 위한 메소드 stopPropagation\(\)를 이용한다.
* 비동기 처리와 콜백 함수
  * 비동기 처리 예시 코드
    * ajax 통신

      ```javascript
            function getData() {
                var tableData;
                $.get('https://domain.com/products/1', function (response) {
                    tableData = response;
                });
                return tableData;
            }            
            console.log(getData());
      ```

    * setTimeout

      ```javascript
        console.log('Hello');

        setTimeout(function () {
            console.log('Bye');
        }, 3000);

        console.log('Hello Again');
      ```
  * 위 코드의 결과는 undefined 가 된다. 그 이유는 특정 로직의 실행이 끝날 때까지 기다려주지 않고 나머지 코드를 먼저 실행이 됬기 때문이다.
  * 해당 코드들을 해결하기위해 위와 같은 비동기 작업의 실행 이후 다음 실행해야 할 함수를 아규먼트로 쥐어주는데 이것이 바로 콜백 함수이다.
  * 허나 위와같은 구조를 남발하여 "**콜백의 콜백의 콜백**" 을 남발하는 것을 콜백 지옥이라 불른다. 위와같은 구조가 되버리면 가독성과 유지보수성이 떨어진다.
  * 하여 보통 분리를 하여 개선한다고 한다.
  * ES6 에서는 Promise, Async 등으로 이용하여 더 편하게 구현한다곤하나, 이 마저도 결국 좋은 방법은 아니라는 말이 있긴하다. 결국 이해하기 좋은 코드는 동기식 코드라는 말이 있었다. 무슨 말이었나 기억이 안났지만 [황성인](https://github.com/sunginHwang) 개발자의 도움을 얻었다.

![](../../.gitbook/assets/image%20%2818%29.png)

![](../../.gitbook/assets/image%20%285%29.png)

