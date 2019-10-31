# Generator

### 제너레이터란?

* 직접 구현하는 것 보다 이터레이션 프로토콜을 준수하여 이터러블을 생성하는 함수이다.
* 비동기 처리에 유용하게 쓰인다고 한다.

  ```javascript
  // 무한 이터러블을 생성하는 제너레이터 함수
  function* infinityFunctionByGenerator() {
      let i = 0;
      while (true) { yield ++i; }
  }

  for (const n of infinityFunctionByGenerator()) {
      if (n > 5) break;
      console.log(n);
  }
  ```

* 함수 코드 블록의 실행을 일시적으로 중지했다가, 원하는 시점에서 재시작을 할 수 있는 특별한 함수이다.

  ```javascript
  //각 호출 지점별로 간단하게 제네레이터를 정의해본다.
  function* counter() {
      console.log('첫번째 호출');
      yield 1;
      console.log('두번째 호출');
      yield 2;
      console.log('세번째 호출');
  }
  // 일반 함수로 호출하면 iterable 
  const generatorObj = counter();

  // next() 호출시 iterator
  console.log(generatorObj.next());
  console.log(generatorObj.next());
  console.log(generatorObj.next());
  ```

* 고로 제너레이터는 iterable 이며, iterator 이다.

### 제너레이터 함수의 정의

* function\* 키워드를 선언하여 사용하고, 하나 이상의 yield 문을 포함한다.
* 다음 코드는 제너레이터로 선언하는 다양한 함수선언문, 함수표현식, 메서드, 클래스 내부의 메서드 정의 방법이다.

  ```javascript
  // 제너레이터 함수 선언문
  function* genDecFunc() {
    yield 1;
  }

  let generatorObj = genDecFunc();

  // 제너레이터 함수 표현식
  const genExpFunc = function* () {
    yield 1;
  };

  generatorObj = genExpFunc();

  // 제너레이터 메소드
  const obj = {
    * generatorObjMethod() {
      yield 1;
    }
  };

  generatorObj = obj.generatorObjMethod();

  // 제너레이터 클래스 메소드
  class MyClass {
    * generatorClsMethod() {
      yield 1;
    }
  }

  const myClass = new MyClass();
  generatorObj = myClass.generatorClsMethod();
  ```

