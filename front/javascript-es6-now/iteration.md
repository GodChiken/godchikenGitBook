---
description: 이터레이션 프로토콜에 대해서 공부해보자.
---

# Iteration



![iterable &#xACFC; iterator&#xC758; &#xAD00;&#xACC4;](https://user-images.githubusercontent.com/16012504/66713227-ec0a4a80-ede2-11e9-98c5-ee2f330a42e4.png)

* 이터레이션 프로토콜
  * 데이터 콜렉션을 순회하기 위한 약속된 규칙을 의미한다.
  * 위 규칙을 준수하는 객체는 for..of 로 순회하거나 spread 문법의 피연산자가 될 수 있다.
  * iterable, iterator 에 해당하는 규칙이 존재한다.
* iterable
  * 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다.
  * Symbol.iterator 메소드를 구현하거나, 프로토 타입 체인에 의해 상속한 객체를 의미한다.
  * 배열은 기본적으로 Symbol.iterator 메소드를 소유하므로, 이터러블 프로토콜을 준수한다.

    ```javascript
    const array = [1,2,3];console.log(Symbol.iterator in array);for (const item of array){  console.log(item);}/*   일반 객체는 이터레이션 프로토콜을 준수하지 않는다.  Symbol.iterator 를 직접 메소드를 커스텀구현하면 일반 객체도 가능은 하다.*/const objectLiteral = { a: 10, b: 20};console.log(Symbol.iterator in objectLiteral); // falsefor(const oL of objectLiteral){  console.log(p);}
    ```

  * 빌트인 이터러블
    * 크게 배열, 문자열, 콜렉션에 해당하는 객체라면 이미 내장이 되어있는 이터러블이 존재한다.
  * 이터레이션 프로토콜의 필요성
    * 빌트인 이터러블 그리고 Symbol.iterator 를 커스텀 구현한 이터러블은 데이터 공급자의 역활을 한다.
    * 각기 다양한 데이터 자원들이 독자적인 순회 방식을 가질경우 데이터 소비자는 그에따른 데이터의 순회 방식을 모두 지원해야한다.
    * 규칙을 정해서 데이터 소비자와 데이터 공급자간의 연결고리 역활을 하는 인터페이스라고 보면 된다.
    * ![image](https://user-images.githubusercontent.com/16012504/66713231-fb899380-ede2-11e9-8493-29bfb6d8e529.png)                
* iterator
  * iterator 를 구현하는 규칙은 다음과 같다.
    * next\(\) 소유
    * next\(\) 호출시, iterable 을 순회하며 value, done 프로퍼티를 가지는 iterator result 객체를 반환하는 것을 의미.
* 커스텀 이터러블의 구현 방법
  * 예시 코드를 보며 살펴보면 다음과 같다.

    ```javascript
      /*      Symbol.iterator 의 내부 변수는 외부로 전달이 불가능하다.      이 특성을 이용하여 다양한 구현이 가능하다.      Symbol.iterator 메소드는 next 메소드를 소유한 이터레이터를 반환해야 한다.      next 메소드는 이터레이터 리절트 객체를 반환하며 순회시 마다 호출이 된다.  */      const fibonacci = {          [Symbol.iterator]() {      let [pre, cur] = [0, 1];              const max = 10;              //return iterator        return {                  next() {          [pre, cur] = [cur, pre + cur];          return {            value: cur,            done: cur >= max          };        }      };    }  };     // 반복문 에서의 사용          for (const num of fibonacci) {          console.log(num);   }  // spread 문법  const arr = [...fibonacci];  console.log(arr); // [ 1, 2, 3, 5, 8 ]  // destructuring  const [first, second, ...rest] = fibonacci;  console.log(first, second, rest); // 1 2 [ 3, 5, 8 ]
    ```

  * 위 예제의 피보나치 이터러블에는 외부에서 값을 전달할 방법이 없다는 점이 있으나, 다음과 같이 하면 이터러블을 반환하는 구조를 취하면 특정한 값을 외부에 전달할 수 있다.

    ```javascript
      const fibonacciFunc = function (max) {    let [pre, cur] = [0, 1];    return {                  [Symbol.iterator]() {                      return {                          next() {            [pre, cur] = [cur, pre + cur];            return {              value: cur,              done: cur >= max            };          }        };      }    };  };  for (const num of fibonacciFunc(10)) {      console.log(num);  }
    ```

  * 이터레이터를 생성하려면 이터러블의 Symbol.iterator 메소드를 호출해야 한다. 이터러블이면서 이터레이터인 객체를 생성하면 Symbol.iterator 메소드를 호출하지 않아도 된다.

    ```javascript
      const fibonacciFunc = function (max) {    let [pre, cur] = [0, 1];    /*      Symbol.iterator 메소드와 next 메소드를 소유한 이터러블이면서 이터레이터인 객체를 반환      하려면 위 예지와 다르게 감싸는 구조가 아닌 별도의 프로퍼티로 구성하면 된다.     */    return {                  [Symbol.iterator]() {        return this;      },                  next() {        [pre, cur] = [cur, pre + cur];        return {          value: cur,          done: cur >= max        };      }    };  };          let iter = fibonacciFunc(10);  // 이터레이터로 활용되는 코드  console.log(iter.next()); // {value: 1, done: false}  console.log(iter.next()); // {value: 2, done: false}  console.log(iter.next()); // {value: 3, done: false}  console.log(iter.next()); // {value: 5, done: false}  console.log(iter.next()); // {value: 8, done: false}  console.log(iter.next()); // {value: 13, done: true}  iter = fibonacciFunc(10);  // 이터러블로 활용되는 코드  for (const num of iter) {    console.log(num);  }
    ```

  * 무한 이터러블, 지연평가\(Lazy evaluation\) 를 통한 활용법
    * 무한한 이터러블을 생성하는 함수를 정의해보는 것과 그 사용법을 알아보자.
    * 이터러블의 특성 중 지연평가를 이용하여 필요한 값만 취득하는 예제이다.
    * 방법은 간단하다. next\(\)의 done 프로퍼티를 생략하면 된다.
    * 지연평가는 데이터를 소비하거나, 디스트럭처링 할당이 되기 이전까지 데이터를 생성하지 않는다.

      ```javascript
        const fibonacciFunc = function () {    let [pre, cur] = [0, 1];    return {      [Symbol.iterator]() {        return this;      },      next() {        [pre, cur] = [cur, pre + cur];                          return { value: cur };      }    };  };              // 제약을 걸지 않으면 무한히 생성된다.  for (const num of fibonacciFunc()) {    if (num > 10000) break;    console.log(num);  }  // 원하는 값만 취득하는 방식  const [f1, f2, f3] = fibonacciFunc();  console.log(f1, f2, f3); // 1 2 3
      ```

