---
description: >-
  Template literal, for..of, Arrow Function, Rest, Spread,destructuring 등 새로 등장한
  문법을 학습한다.
---

# 문법과 어휘구조2\(ES6~new\)



* Template Literal
  * 있는 그대로 white space 를 적용 가능하다.

    ```javascript
      let template = ` 
          "" 도 쓸 수 있고, 
          '' 도 쓸수있다.
      `;
      console.log(template);
    ```

  * String Interpolation : 표현식으로 문자열 삽입이 가능하다.

    ```javascript
      let firstName = "Bo-Hun";
      let lastName = "Kim";
      let fullName = `${firstName} + ${lastName}`;
      console.log(fullName);
    ```

  * 중간의 계산식도 가능하지만, 최종 결과물은 문자열로 반환된다.

    ```javascript
      let exampleNumber1 = 1;
      let exampleNumber2 = 2;
      let sumOfExampleNumber = `${exampleNumber1 + exampleNumber2}`;
      console.log(sumOfExampleNumber);
    ```
* Arrow Function
  * 간단한 사용법 코드

    ```javascript
      let noParameter = () => { console.log("매개 변수가 없는경우");};
      let oneParameter = x => {console.log(x);};
      let multiParameter = (x,y) => { console.log(x+y)};
      let oneLineFunction = x => x*x;
      let returnObjectFunction = () => ({ x : 5 });
      let multiLineArrowFunction = () => {
          console.log("여러줄 선언이 된다.");
          console.log("여러줄 선언이 된다.");
          console.log("여러줄 선언이 된다.");
          console.log("여러줄 선언이 된다.");
          console.log("여러줄 선언이 된다.");
      }

      noParameter();
      oneParameter(5);
      multiParameter(1,3);
      console.log(oneLineFunction(5));
      console.log(returnObjectFunction());
      console.log(multiLineArrowFunction()());
    ```

  * ES5 방식보다 훨씬 간결해졌다.

    ```javascript
      // ES5
      var arr = [1, 2, 3];
      var pow = arr.map(function (x) { // x는 요소값
          return x * x;
      });

      console.log(pow); // [ 1, 4, 9 ]

      // ES6
      const arr = [1, 2, 3];
      const pow = arr.map(x => x * x);

      console.log(pow);
    ```

  * 기존 함수와의 차이점은 문법적인 것도 분명하나 사실은 this binding 부분에서 차이점이 있다.
    * 기존 함수는 호출방식에 따라 바인딩이 결정된다.
    * 일반 함수는 함수를 선언할 때 this 에 바인딩할 객체가 정적으로 결정되는 것이 아니고, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this 에 바인딩할 객체가 동적으로 결정된다고 하였다.
    * 화살표 함수는 함수를 선언할 때 this 에 바인딩할 객체가 정적으로 결정된다. 동적으로 결정되는 일반 함수와는 달리 화살표 함수의 this 언제나 상위 스코프의 this 를 가리킨다. 이를 Lexical this라 한다.
    * 화살표 함수는 call, apply, bind 메소드를 사용하여 this 를 변경할 수 없다.
  * Arrow Function 으로 메서드 등 선언하는 것은 자제하자. 전역 스코프를 가르키기 때문이다.
    * 메서드를 선언시 축약 표현으로 메서드를 사용하자.

      ```javascript
        let person = {
          name: 'Lee',
          sayHi: () => console.log(`Hi ${this.name}`)
        };            
        person.sayHi(); // undefined            

        person = {
          name: 'Lee',
          sayHi() { console.log(`Hi ${this.name}`); } // 축약 메서드 표현
        };

        person.sayHi(); // Hi Lee
      ```

    * 이벤트 할당도 동일한 문제를 가지고 있으니 일반 함수로 사용을 권장한다. 
    * 생성자 함수로 사용할 수 없다. 프로토타입을 가지고 있지 않기 때문이다.
* for..of
  * for…of 문은 내부적으로 이터레이터의 next 메소드를 호출하여 이터러블을 순회하며 next 메소드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for…of 문의 변수에 할당한다. 그리고 이터레이터 리절트 객체의 done 프로퍼티 값이 false 이면 이터러블의 순회를 계속하고 true 이면 이터러블의 순회를 중단한다.

    ```javascript
      // 배열
      for (const item of ['a', 'b', 'c']) {
        console.log(item);
      }

      // 문자열
      for (const letter of 'abc') {
        console.log(letter);
      }

      // Map
      for (const [key, value] of new Map([['a', '1'], ['b', '2'], ['c', '3']])) {
        console.log(`key : ${key} value : ${value}`); // key : a value : 1 ...
      }

      // Set
      for (const val of new Set([1, 2, 3])) {
        console.log(val);
      }

      // 내부적으로 어떻게 동작하는지 for 로 표현하면 다음과 같다.
      // 이터러블
      const iterable = [1, 2, 3];

      // 이터레이터
      const iterator = iterable[Symbol.iterator]();

      for (;;) {
        // 이터레이터의 next 메소드를 호출하여 이터러블을 순회한다.
        const res = iterator.next();

        // next 메소드가 반환하는 이터레이터 리절트 객체의 done 프로퍼티가 true가 될 때까지 반복한다.
        if (res.done) break;

        console.log(res);
      }
    ```
* Destructuring
  * Array Destructuring
    * ES6의 배열 디스트럭처링은 배열의 각 요소를 배열로부터 추출하여 변수 리스트에 할당한다. 이때 추출/할당 기준은 배열의 인덱스이다.

      ```javascript
        const arr = [1, 2, 3];            
        /*
            배열의 인덱스를 기준으로 배열로부터 요소를 추출하여 변수에 할당
            변수 first, second, third 선언되고 arr(initializer(초기화자))가 Destructuring(비구조화, 파괴)되어 할당된다.
        */
        const [first, second, third] = arr;
        /*
            디스트럭처링을 사용할 때는 반드시 initializer(초기화자)를 할당해야 한다.
            const [first, second, third]; // SyntaxError: Missing initializer in destructuring declaration
        */            
        console.log(first, second, third); // 1 2 3
      ```
  * Object Destructuring
    * ES6의 객체 디스트럭처링은 객체의 각 프로퍼티를 객체로부터 추출하여 변수 리스트에 할당한다. 이때 할당 기준은 프로퍼티 이름\(키\)이다.

      ```javascript
        const obj = { firstName: 'Ungmo', lastName: 'Lee' };         

        const { lastName, firstName } = obj;

        console.log(firstName, lastName); // Ungmo Lee
      ```

    * 객체 디스트럭처링을 위해서는 할당 연산자 왼쪽에 객체 형태의 변수 리스트가 필요하다.

      ```javascript
        const { prop1: p1, prop2: p2 } = { prop1: 'a', prop2: 'b' };
        console.log(p1, p2); 

        // 동일한 표현이나 좀더 축약하면 다음과 같다.
        const { prop1, prop2 } = { prop1: 'a', prop2: 'b' };
        console.log({ prop1, prop2 }); 

        // default value 를 destructure 에 정할수 있다.
        const { prop1, prop2, prop3 = 'c' } = { prop1: 'a', prop2: 'b' };
        console.log({ prop1, prop2, prop3 });
      ```

    * Array.prototype.filter 메소드의 콜백 함수는 대상 배열\(todos\)을 순회하며 첫 번째 인자로 대상 배열의 요소를 받는다. 
    * 이때 필요한 프로퍼티\(completed 프로퍼티\)만을 추출할 수 있다.
    * 객체 디스트럭처링은 객체에서 프로퍼티 이름\(키\)으로 필요한 프로퍼티 값만을 추출할 수 있다. 아래의 코드를 살펴보자.

      ```javascript
        const todoList = [
          { id: 1, content: 'HTML', completed: true },
          { id: 2, content: 'CSS', completed: false },
          { id: 3, content: 'JS', completed: false }
        ];

        // java8 stream 에서 확인했던, 그 메서드와 기능상으로 매우 동일했다.
        const completedTodoList = todoList.filter(({ completed }) => completed);
        console.log(completedTodoList);
      ```

    * 중첩 객체의 경우는 아래와 같이 사용한다. 이런 식으로 접근이 가능하다는 것 정도로만 기억한다. \(남발하면 가독성 결여\)

      ```javascript
        const person = {
          name: 'Kim',
          address: {
            zipCode: '00000',
            city: 'Seoul'
          }
        };            
        const { address: { city } } = person;
        console.log(city);
      ```
* ES6 에서의 파라미터의 변화
  * 파라미터에 디폴트 값 선언이 가능하다.            

    ```javascript
          function defaultValueInParameter(x=0, y=1){
              return x+y;
          }
    ```

    * 또한 기본값을 정하여도 length, arguments 에 영향을 미치지 않는다    

  * Rest Parameter
    * 기본적인 형식은 다음과 같다.

      ```javascript
      // 일반적인 형태이며 가변인자를 배열 형태로 받는다.
      function restParameterFunction(...rest){
        console.log(Array.isArray(rest));
        console.log(rest);
        return rest.reduce((first, second) => first + second);
      }
      // 반드시 제일 마지막에 위치하지 않으면 Syntax Error 발생
      function parameterCombination(first,second, ...rest){
        console.log(Array.isArray(rest));
        console.log(rest);
      }
      ```
* Spread Syntax
  * 정의된 설명으론 _**'이터러블'**_ 인 대상을 개별 요소로 분할한다.
  * 모양은 Rest Syntax 와 동일하나 일반적인 객체는 type error 을 발생시킨다.

    ```javascript
      console.log(...[1, 2, 3]); // 1, 2, 3        
      console.log(...'Hello');  // H e l l o 
      console.log(...new Map([['a', '1'], ['b', '2']]));  // [ 'a', '1' ] [ 'b', '2' ]
      console.log(...new Set([1, 2, 3]));  // 1 2 3        
      console.log(...{ a: 1, b: 2 }); // 이터러블이 아닌 일반 객체는 Spread 문법의 대상이 될 수 없다.
    ```

  * 일반 함수에 argument 로 Spread 형태의 배열을 전달되는 경우 순차적으로 해당 함수의 파라미터에 할당한다.

    ```javascript
      const arr = [1, 2, 3];

      function foo(x, y, z) {
        console.log(x); 
        console.log(y); 
        console.log(z); 
      }         
      foo(...arr);
    ```
* Rest/Spread Property
  * 일반 객체에 자체해서는 분할과 병합이 불가하나, 프로퍼티는 가능하다.
  * 일반적으로는 다음과 같이 객체를 병합해 나갔다.

    ```javascript
      const target = { a: 1, b: 2 };
      const source = { b: 4, c: 5 };

      const returnedTarget = Object.assign(target, source);

      console.log(target); // Object { a: 1, b: 4, c: 5 }        
      console.log(returnedTarget); // Object { a: 1, b: 4, c: 5 }
    ```

  * 특별히 원본 객체를 재사용 하는 방식이 아니라면 상관이 없으나, 재사용이 이루어져야하는 경우에는 치명적인 오류가 발생하는거 같았다.
  * 다음과 같이 Spread Property 를 사용하는 경우에 위와 같은 사항을 방지할 수 있다. 또한 특별히 따로 묶어서 분할하고자하는 경우에는 Rest Property 를 사용하면 된다.

    ```javascript
      // Spread Property
      const target = { a: 1, b: 2 };
      const source = { b: 4, c: 5 };
      console.log(target);
      console.log(source);
      const merge = { ...target, ...source };
      console.log(merge);
      console.log(target);

      // Rest Property
      const {single, ...collection} = merge;

      // 객체의 병합
      const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
      console.log(merged); // { x: 1, y: 10, z: 3 }

      // 특정 프로퍼티 변경
      const changed = { ...{ x: 1, y: 2 }, y: 100 };        
      console.log(changed); // { x: 1, y: 100 }

      // 프로퍼티 추가
      const added = { ...{ x: 1, y: 2 }, z: 0 };        
      console.log(added); // { x: 1, y: 2, z: 0 }
    ```

