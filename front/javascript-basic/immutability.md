---
description: 객체의 변경 불가성에 대해서 정리를 해본다.
---

# 객체의 변경불가성 \(immutability\)

* Object immutability \(객체의 변경불가성\)
  * 객체를 더 이상 변경불가능한 상태로 만드는 디자인 패턴을 의미한다.
  * 객체는 참조를 통해 접근이 가능한 특성을 지니기 때문에 언제든 객체에 변경이 일어날 가능성이 있다는 의미도 된다.
  * 위를 위해 ES5에서는 방어적 복사\(defencive copy\)와 옵저버 패턴으로 위 문제를 해결하곤한다.
  * 불변 객체로 만들게 되면 복제나 비교를 위한 작업이 단순화 되며, 성능 개선에 도움이된다. 하나 기존 객체가 변경이 가능한 데이터가 많을 경우 역효과니 주의한다.
  * ES6에서는 이를 불변 데이터 패턴\(immutable data pattern\) 으로 쉽게 해결하도록 제공한다고 한다.
* 변경이 가능한, 변경이 불가한 것은 무엇인가

  * 원시 타입은 변경이 불가능하다.
    * Boolean, null, undefined, Number, String, Symbol\(ES6\)
  * 이전에 학습했기에 원시 타입을 제외하곤 모두 객체로 취급된다는 걸 안다.
  * 자바스크립트에서 변경이 불가능하다는 의미는 메모리에 올라가 있는 데이터는 변경이 불가능하다는 것이고, 재할당은 가능하다.
  * 아래의 경우에 myName 에 user.name 을 참조 시켰고 이후 변경을 가미했다. 한번 해당 객체를 참조하는 것이 아니라, 원시타입 String 'kim' 을 참조하는 사실을 알도록 하자.

  ```javascript
      var user = {
          name: 'kim',
          address: {
              city: 'Seoul'
          }
      };

      var myName = user.name; // myName : kim

      user.name = 'Lee';
      console.log(myName); // kim

      myName = user.name;  // myName : Lee
      console.log(myName); // Lee
  ```

  * 아래의 경우에는 별도로 새로운 메모리에 할당 되는 것이 아니라 같은 곳을 바라보기 때문에 동시에 변경되고 의도된 코딩이 아니라면 막아야한다.

  ```javascript
      var user = {
          name: 'kim',
          address: {
              city: 'Seoul'
          }
      };

      var user2 = user; 
      user.name = 'Lee';

      console.log(user.name); // Lee
      console.log(user2.name); // Lee
  ```

* 불변 데이터 패턴\(immutable data pattern\)
  * 결론적으로는 ES6 에서 등장하는 freeze\(\), assign\(\) 과 같은 방법의 불변객체를 만드는 법을 제공한다.
  * 위 방법으로 하는 법도 있으나 시간적인 공수 및 내부 객체 프로퍼티에 할당된 객체들까지 자동으로 불변이 되는 것은 아니기 때문에 의도한게 아니라면 크리티컬한 코딩이 될 수 있다.
  * Facebook 에서는 이러한 점 때문에 immutable.js 를 제공한다곤 한다. 이건 언제 쓰이는지 알아보아야겠다.

