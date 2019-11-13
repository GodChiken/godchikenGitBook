---
description: ECMAScript6 에 등장하는 class keyword
---

# class Keyword



### class 란?

* 자바스크립트는 프로토타입 기반\(prototype-based\) 객체지향 언어다. 비록 다른 객체지향 언어들과의 차이점에 대한 논쟁이 있긴 하지만, 자바스크립트는 강력한 객체지향 프로그래밍 능력을 지니고 있다.
* 프로토타입 기반 프로그래밍은 클래스가 필요없는\(class-free\) 객체지향 프로그래밍 스타일로 프로토타입 체인과 클로저 등으로 객체 지향 언어의 상속, 캡슐화\(정보 은닉\) 등의 개념을 구현할 수 있다.

### 클래스 정의 \(Class Definition\)

* 클래스도 사실은 함수이나 기존 prototype 기반으로 코딩하던 것을 쉽게 풀어나가기 위한 슈가코드이다.
* ES6 클래스는 class 키워드를 사용하여 정의한다. 앞에서 살펴본 Person 생성자 함수를 클래스로 정의해 보자.
* 클래스 이름은 성성자 함수와 마찬가지로 파스칼 케이스를 사용하는 것이 일반적이다. 파스칼 케이스를 사용하지 않아도 에러가 발생하지는 않는다.

  ```javascript
  class Person {      
    constructor(name) {
      this._name = name;
    }    
    sayHi() {
      console.log(`Hi! ${this._name}`);
    }
  }
  const me = new Person('Lee');
  me.sayHi(); // Hi! Lee

  console.log(me instanceof Person); // true
  ```

* 클래스는 클래스 선언문 이전에 참조할 수 없지만, 호이스팅은 발생한다. 
  * 클래스는 var 키워드로 선언한 변수처럼 호이스팅되지 않고 let, const 키워드로 선언한 변수처럼 호이스팅된다. 
  * 따라서 클래스 선언문 이전에 [TDZ](https://github.com/GodChiken/StudyES6toNew/blame/master/markdown/act-1/letAndConstAndBlockScope.md#L6-L15) 에 빠지기 때문에 호이스팅이 발생하지 않는 것처럼 동작한다.

    ```javascript
    const Foo = '';        
    {          
        console.log(Foo);         
        class Foo {}  // ReferenceError: Cannot access 'Foo' before initialization
    }
    ```

### 생성자를 통한 인스턴스 생성해보기

* new 연산자와 함께 호출한 메서드는 constructor\(생성자\)이다. 표현식이 아닌 선언식으로 정의한 클래스의 이름은 constructor 와 동일하다. 
* new 연산자를 사용하지 않고 constructor 를 호출하면 타입 에러\(TypeError\)가 발생한다. constructor 는 new 연산자 없이 호출할 수 없다.

  ```javascript
  class Foo {}

  const foo = Foo(); // TypeError: Class constructor Foo cannot be invoked without 'new'
  ```

### 생성자\(constructor\)

* 생성자 내에서만 클래스 필드를 정의할 수 있다.
* 자바스크립트의 생성자 함수에서 this 에 추가한 프로퍼티를 클래스 기반 객체지향 언어에서는 클래스 필드라고 부른다.

  ```javascript
  class Person {      
  constructor(name) {            
    this._name = name; // class field
  }
  }
  // 인자값을 전달하여 클래스 필드 값을 설정이 가능하다.
  const me = new Person('Lee');
  console.log(me); // Person {_name: "Lee"}
  ```

* constructor 는 생략하면 클래스에 constructor\(\) {}를 포함한 것과 동일하게 동작한다. 즉, 빈 객체를 생성한다. 따라서 인스턴스에 프로퍼티를 추가하려면 인스턴스를 생성한 이후, 프로퍼티를 동적으로 추가해야 한다.

### 클래스 필드

* 클래스 바디에는 메소드만 선언할 수 있다. 클래스 바디에 클래스 필드\(멤버 변수\)를 선언하면 문법 에러\(SyntaxError\)가 발생한다. 왜 할수 없지라고 조금 고민하게 했었으나 이 자료를 보고 이해했다.
* constructor 내부에서 선언한 클래스 필드는 _**클래스가 생성할 인스턴스를 가리키는 this 에 바인딩**_ 한다. 이로써 클래스 필드는 클래스가 생성할 인스턴스의 프로퍼티가 되며, 클래스의 인스턴스를 통해 클래스 외부에서 언제나 참조할 수 있다. 즉, 언제나 public이다.
* ES6의 클래스는 다른 객체지향 언어처럼 private, public, protected 키워드와 같은 접근 제한자\(access modifier\)를 지원하지 않는다.

  > 하지만 제안이 오가고 있다곤하는데 찾아봐야한다. TC39 프로세스의 stage 3\(candidate\)에 후보로 Private Field 제안이 있다고 한다.

### 접근자 프로퍼티

* `get` 키워드를 사용하여 클래스 필드값을 사용하여 반환값을 얻기 위해서 사용이 된다. 해당 키워드를 사용하여 메서드를 선언하는 경우, 클래스 필드처럼 사용할 수 있으며 프로퍼티처럼 참조가 가능하다.
* `set` 키워드를 사용해서 클래스 필드를 조작하는 용도이다. 프로퍼티처럼 값을 할당하는 방식으로 사용하면된다.

  ```javascript
  class Foo{
    constructor(arr = []){
        this._arr = arr;
    }
    get firstMethod(){
        return this._arr.length ? this._arr[0] : null;
    }
    set firstMethod(value){
        this._arr = value;
    }
  }
  const foo = new Foo([1,2]);
  foo.firstMethod(); // 함수 호출하듯 하면 안된다.
  foo.firstMethod; // getter, 결과값 1
  foo.firstMethod = [4,3,2,1]; // setter
  foo.firstMethod; // 결과값 4
  ```

### 정적 메서드 \(static method\)

![](../../.gitbook/assets/image%20%2827%29.png)

* `static` 키워드를 사용하며, 클래스의 이름으로 호출하므로 별도의 인스턴스를 생성할 필요가 없다.
* 또한 정적 메서드는 인스턴스에서 호출은 불가능하다.

  > 이말은 곧 `this`를 사용할 수 없다는 것을 의미한다. 정적 메서드의 용도는 전역적을 사용하는 유틸성 함수를 생성할 때 추로 사용한다.

  ```javascript
  class Foo{ constructor(props) { 
      this.props = props;
  }
  static staticMethod() { 
      console.log("인스턴스 선언 안해도 되앳!"); 
  } 
  prototypeMethod(){ 
      return this.props; } 
  } 

  Foo.staticMethod();
  ```

* ES5 방식으로 클래스를 표현해보고 구조를 비교하여보자

  ```javascript
  var Foo = (function (){
        function Foo(prop) {
            this.prop = prop;
        }
        Foo.staticMethod = function() {
            return "인스턴스 선언따위 안해도 되앳";          
        };

        Foo.prototype.prototypeMethod = function() {
            return this.prop;
        };
        return Foo;
  })();
  var foo = new Foo(123);
  console.log(Foo.prototype === foo.__proto__); //true
  console.log(Foo.prototype.constructor === Foo); //true 
  console.log(foo.prototypeMethod());
  console.log(Foo.staticMethod());
  console.log(foo.staticMethod()); // Uncaught TypeError
  ```

* 해당 객체의 `prototype property` 는 객체가 생성자로 사용될 때, 이 함수를 통해 생성된 객체의 부모역활을 하는 프로토타입을 가르킨다. Foo 는 생성자 함수이며 이 함수의 `prototype property` 가 가르키는 프로토타입 객체는 생성자 함수 Foo 를 통해서 생성되는 foo 인스턴스의 부모역활을 한다.       
* 따라서 생성자 함수의 메서드인가 프로토타입의 메서드인가 구분하면 왜 호출이 불가한지 판별할 수가 있다.

### `extends` 키워드를 활용한 다양한 상속 활용법

![](../../.gitbook/assets/image%20%2814%29.png)

* 타 객체 지향 언어와 거의 동일하게 상위 클래스의 구성물을 활용하고 오버로딩, 오버라이딩을 할수 있다는 점이 매력이 있다.
* 그러나 현재 객체지향 언어도 상속보다는 constructor injection 을 통하여 필요한 내용을 조립하는 방식을 취하고 있기 때문에 자바스크립트에서도 이렇게 활용이 가능한지 조만간에 실습을 할 예정이다.
* 자식 인스턴스는 프로토타입 체인에 의해 부모 클래스의 내용을 사용이 가능하다.
  * 위의 내용이 가능한 이유는 프로토타입 체인은 특정 객체의 프로퍼티나 메소드에 접근하려고 할 때 프로퍼티 또는 메소드가 없다면 \[\[Prototype\]\] 내부 슬롯이 가리키는 링크를 따라 자신의 부모 역할을 하는 프로토타입 객체의 프로퍼티나 메소드를 차례대로 검색한다. 그리고 검색에 성공하면 그 프로퍼티나 메소드를 사용한다.
* `super` 키워드를 활용하여 부모클래스를 참조하거나 부모클래스의 생성자를 호출하는 용도로 사용된다.
* 또한 `static` 메서드와 각 인스턴스의 `prototype`에 있는 메서드 또한 프로토타입에 의하여 상속이 가능하다.
  * 하지만 유의할 점은 같은 static 메서드 내에서만 가능하다.
  * 자식 클래스의 정적 메소드 내부에서도 super 키워드를 사용하여 부모 클래스의 정적 메소드를 호출할 수 있다. 이는 자식 클래스는 프로토타입 체인에 의해 부모 클래스의 정적 메소드를 참조할 수 있기 때문이다.
  * 하지만 자식 클래스의 일반 메소드\(프로토타입 메소드\) 내부에서는 super 키워드를 사용하여 부모 클래스의 정적 메소드를 호출할 수 없다. 이는 자식 클래스의 인스턴스는 프로토타입 체인에 의해 부모 클래스의 정적 메소드를 참조할 수 없기 때문이다.

