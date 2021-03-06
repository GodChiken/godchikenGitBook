# Java - Interface

> 각 관련 개념의 예제는 다음의 경로에서 확인이 가능하다 [관련 예제 패키지](https://github.com/GodChiken/JavaTheory/tree/master/src/main/java/com/kbh/desk/theory/basic/polymorphism/interfaceTheory)

* 인터페이스의 역활
  * 객체의 사용 방법을 정의한 타입이다.
  * 객체의 교환성을 높혀준다. 이로 인해 다형성을 구현하는 중요한 역활을 한다.
  * 자바 8에서 람다식은 함수적 인터페이스의 구현객체를 생성하기 때문에 인터페이스의 중요성은 커졌다.
  * 개발 코드를 수정하지 않고, 사용하는 객체를 변경할 수 있도록 하는 역활을 한다.
* 인터페이스 선언
  * 물리적으로 일반적인 클래스와 동일한 소스파일과 동일한 형태로 컴파일 된다. \(.java, .class\)
  * 다음과 같이 선언이 가능하다

    > \[public\] interface 인터페이스명 {...}

  * 상수와 메소드만을 구성멤버로 가진다. 또한 직접 객체를 생성하는 것은 불가하기 때문에 생성자를 가질 수 없다.
  * 자바 8버전 부터 디폴트 메서드와 정적 메소드를 선언이 가능해졌다.

    ```java
    interface 인터페이스명 {
      //상수
      타입 상수명 = 값;
      //추상메서드
      타입 메소드명(매개변수,...);
      //디폴트메서드
      default 타입 메소드명(매개변수,...) {...}
      //정적 메소드
      static 타입 메소드명(매개변수) {...}
    }
    ```

    * 상수 필드
      * 인터페이스는 런타임 시 데이터를 저장할 수 있는 필드를 선언할 수 없다. 그러나 상수 필드는 선언이 가능하다.
      * 상수 필드는 인터페이스에 고정된 값으로 런타임 시 데이터 변경이 불가하다.
      * 상수 필드는 반드시 초기 값을 지정해야 한다.
      * 상수 필드는 다음과 같이 선언한다. 아래 해당되는 키워드를 생략하더라도 컴파일 과정에서 자동적으로 추가된다.

        > \[ public static final \] 타입 상수명 = 값;
        >
        > > 이때 상수명은 대문자와 스네이크 케이스 명명식으로 구성하는게 관례이다.
        > >
        > > ```java
        > > public interface RemoteControl {
        > >   public static final int MAX_VOLUME = 10;
        > >   public static final int MIN_VOLUME = 0;
        > > }
        > > ```
    * 추상 메소드
      * 객체가 가지고 있는 메소드를 설명하는 것으로 호출할 때 어떤 매개값과 리턴 타입을 가지고 있는지 설명한다.
      * 구현부는 구현 객체가 가지고 있다.
      * 인터페이스를 통하여 호출된 메소드는 최종적으로 구현 객체에서 실행되므로, 인터페이스에서는 추상 메서드로 선언한다. 아래 해당되는 키워드를 생략하더라도 컴파일 과정에서 자동적으로 추가된다.

        > \[public abstract\] 리턴타입 메소드명\(매개변수,...\);
        >
        > ```java
        > public interface RemoteControl {
        > public abstract void turnOn();
        > public abstract void turnOff();
        > public abstract void setVolume(int volume);
        > }
        > ```
    * 디폴트 메소드
      * 인터페이스에 선언되는 메소드지만, 구현 객체가 가지고 있는 인스턴스 메소드라고 생각해야한다.
      * 자바 8에서 기존 인터페이스를 확장하여 새로운 기능을 추가할 때 사용된다.
      * 기본적으로 접근 제한자 키워드를 생략하더라도 public 키워드가 컴파일시 자동으로 추가된다.

        > \[public\] default 리턴타입 메소드명\(매개변수,...\) {...};
        >
        > ```java
        > public interface RemoteControl {
        > public default void setMute(boolean mute){
        >   if(mute) {...}
        >   else {...}
        > }
        > }
        > ```
    * 정적 메소드
      * 디폴트 메서드와 다르게 객체가 없어도 인터페이스 만으로도 호출이 가능한 메서드이다.
      * 기본적으로 접근 제한자 키워드를 생략하더라도 public 키워드가 컴파일시 자동으로 추가된다.

        > \[public\] static 리턴타입 메소드명\(매개변수,...\) {...};
        >
        > ```java
        > public interface RemoteControl {
        > public static void changeBattery(){
        >   //...
        > }
        > }
        > ```
    * 각각의 요소가 컴파일시 자동으로 키워드가 붙는 이유는 다음 링크에 있다.
      * [왜 public과 public final 키워드를 제거하는게 맞나요?](https://stackoverflow.com/questions/17011374/are-public-and-public-final-redundant-for-interface-methods)



![&#xD0A4;&#xC6CC;&#xB4DC;&#xB97C; &#xC81C;&#xAC70;&#xD558;&#xC9C0; &#xC54A;&#xB294; &#xACBD;&#xC6B0; &#xB2E4;&#xC74C;&#xACFC; &#xAC19;&#xC740; &#xACBD;&#xACE0; &#xBA54;&#xC138;&#xC9C0;&#xAC00; &#xCD9C;&#xB825;&#xB41C;&#xB2E4;.](../../.gitbook/assets/interfacewarnning.png)



![&#xD574;&#xB2F9; &#xC778;&#xD130;&#xD398;&#xC774;&#xC2A4;&#xC5D0;&#xB294; &#xAC01; &#xC694;&#xC18C;&#xB9C8;&#xB2E4; &#xD0A4;&#xC6CC;&#xB4DC;&#xB97C; &#xC81C;&#xC678;&#xD558;&#xACE0; &#xCEF4;&#xD30C;&#xC77C; &#xD588;&#xC9C0;&#xB9CC;, &#xAE30;&#xBCF8;&#xC801;&#xC73C;&#xB85C; &#xBD99;&#xB294;&#xB2E4;&#xB294; &#xAC83;&#xC744; &#xD559;&#xC2B5;&#xD588;&#xB2E4;.](../../.gitbook/assets/interfacejavapresult.png)



* 인터페이스의 컴파일 결과물   
  * 해당 인터페이스에는 각 요소마다 키워드를 제외하고 컴파일 했지만, 기본적으로 붙는다는 것을 학습했다.
* 인터페이스 구현
  * 구현 클래스는 인터페이스를 구현하는 역활을 하는 클래스를 지칭한다.
  * 다음과 같은 형태를 지닌다.

    ```java
    public class 구현클래스명 implements 인터페이스명 {
    //...
    }
    ```

  * 기본적으로 인터페이스의 모든 메서드의 접근자는 public 이므로 더 접근범위가 낮은 키워드를 대체할 수 없다.
  * 인터페이스를 구현하는 클래스가 인터페이스 내부의 추상 메서드와 대응되는 메서드를 작성하지 않으면 캄파일러가 추상 클래스로 인식하기 때문에 이런 식으로 코딩하지 않겠지만, abstract 키워드를 붙여야한다.
* 인터페이스 사용
  * 인터페이스로 구현객체를 사용해야 하는경우 인터페이스 변수를 선언하고 구현 객체를 대입해야한다.
  * 인터페이스 변수는 참조 타입이기 때문에 구현 객체가 대입되는 경우 구현 객체의 참조값을 저장한다.
  * 개발 코드에서 인터페이스는 다음 요소로 선언될 수 있다.
    * 클래스의 필드
    * 생성자
    * 메소드의 매개변수
    * 생성자 또는 메서드의 로컬 변수

      ```java
      public class PersonalRemoteControl {
      RemoteControl remoteControl = new Television();

      PersonalRemoteControl(RemoteControl remoteControl){
        this.remoteControl = remoteControl;
      }

      void changeAudioControl(){
        RemoteControl rc = new Audio();
      }
      }
      ```
  * 추상 메서드의 사용
    * 구현 객체가 인터페이스 타입에 대입되면 인터페이스에 선언된 추상메서드를 개발 코드에서 호출이 가능해진다.

      ```java
      RemoteControl rc = new Television();
      rc.turnOn();
      rc.turnOff();
      ```
  * 디폴트 메서드의 사용
    * 인터페이스에서 선언되나 바로 사용이 불가하다.
    * 디폴트 메서드는 인터페이스의 모든 구현객체가 가지고 있는 기본 메서드이다.
    * 특정 구현객체는 디폴트 메서드의 내용이 적합하지 않아 오버라이딩 과정이 필요할 수 있다.
    * 왜 디폴트 메서드가 등장했는지 아직까지 의아하긴하다.
  * 정적 메서드의 사용
    * 다른 메서드와 다르게 인터페이스를 통해 바로 호출이 가능하다.
* 타입 변환과 다형성
  * 생성자 혹은 메서드의 파라미터를 통해 다양한 구현 객체를 주입하여 다형성을 추구할 수 있다.
  * 구현 객체가 인터페이스 타입으로 변환되는 것을 Promotion\(자동 타입 변환\) 이라고 명한다.

    > 인터페이스 변수 = 구현객체;
    >
    > > 위와 같이 인터페이스 변수에 구현객체가 대입 시 프로모션이 일어난다.

  * 필드의 다형성
    * 같은 인터페이스를 구현하는 클래스는 언제든 교체가 가능하다.
  * 매개변수의 다형성
    * 같은 인터페이스를 구현하는 클래스는 동일하게 파라미터로 주입이 가능하다.
* 인터페이스 상속
  * 인터페이스는 클래스와 다르게 다른 인터페이스를 다중 상속이 가능하다

    ```java
    public interface 하위인터페이스 extends A,B,C,.....
    ```
* 디폴트 메서드와 인터페이스 확장
  * 디폴트 메서드의 필요성
    * 기존 인터페이스를 확장해서 새로운 기능을 추가하기 위해 사용된다.
    * 기존 인터페이스의 이름과 추상메서드의 변경 없이 사용이 가능하다.
    * 이 말은 디폴트 메서드는 클래스에서 메소드를 구현할 필요 없이 사용이 가능하다로 이해한다.
  * 디플트 메서드의 사용법
    * 단순히 상속만 받는다.
    * 오버라이딩 해서 사용한다.
    * 디폴트 메소드를 추상 메서드로 재선언한다. 이 경우는 빈번하게 하위 구현 클래스에서 오버라이딩 하는 경우 고려할 만하다.
* 중첩 클래스와 인터페이스
  * 중첩 클래스
    * 특정 클래스가 여러 클래스가 관계를 맺는경우 독립적으로 활용하고, 그렇지 않을 경우에는 Nested 를 고려하는 것으로 공부하자.
    * 장점은 두 클래스의 멤버를 쉽게 접근이 가능하고, 불필요한 클래스의 관계를 감춤으로서 코드의 복잡성을 줄일 수 있다.
  * 중첩 인터페이스
    * 인터페이스를 해당 클래스와 긴밀한 관계를 유지하기 위해 고려된다.
    * 주로 UI 프로그래밍에서 자주 활용된다.

