# Servlet

### 서블릿이란

* 웹 어플리케이션 개발요 스펙과 API 제공
* 기존 요청당 프로세스를 만들어 사용하는 방식인 Common Gateway Interface를 개선한 방식
  * 빠르고 플랫폼에 독립적이고 보안과 이식성이 뛰어나다
* 우리가 직접 서블릿을 실행하는 것은 불가하며 서블릿 스펙을 준수하는 컨테이너들을 사용하여 서블릿의 라이프 사이클을 관리한다.
  * 세션, 네트워크 관리
  * MIME 기반 메세지 인코딩 디코딩
  * 생명주기 등

### 서블릿 생명주기

* 서블릿 컨테이너가 서블릿 인스턴스의 init()메서드를 통해서 초기화를 진행한다. 단 최초 요청시에만 이 과정을 진행한다.
* 초기화 이후부터 요청을 처리할수 있으며 각요청은 멀티 쓰레드로 처리되고 이때 서블릿 인스턴스의 service() 메서드가 호출된다.
  * Http 요청과 응답을 처리
  * 요청에 따라 doGet(), doPost()로 처리를 위임.
* 서블릿 컨테이너의 판단에 의해서 메모리 해제 시점에 destory() 호출

### 서블릿 구현

* maven\_arch\_type 아키텍쳐를 가지고 심플 웹 애플리케이션을 개발 시 servlet api가 미포함되는 경우도 있다. 의존성을 추가하여 해결하되 서블릿 컨테이너에 이미 들어있기때문에 scope를 provied로 제공한다.

### 서블릿 리스너

* 이벤트를 감지하고 특별한 작업을 해야하는 경우 사용
* 처리 이벤트 종류
  * 서블릿 컨텍스트 수준의 이벤트
  * 세션 수준의 이벤트
* 각 이벤트별로 라이프 사이클과 애트리뷰트 변경을 처리

### 서블릿 필터

* 서블릿 컨테이너와 서블릿 사이에 존재
* 요청과 응답을 전달하며 여러 개가 존재가 가능하고 체인형태로 존재한다.

### 서블릿에서 스프링 IoC 컨테이너를 사용고 싶은 경우 고찰

* IoC 컨테이너 즉, ApplicationContext를 **ContextLoaderListener**가 서블릿 애플리케이션의 생명주기에 맞춰서 Servlet WebApplicationContext에 바인딩하도록 설정하도록하고 소멸시 제거되도록 하는 역활을 한다.
* Listener가 ApplicationContext를 활용하도록 직접 만들어야한다. xml 및 java config를 활용해서 만들면된다.
  * 설정방식과 설정파일들을 필터들보다 우선순위가 높도록 설정해야한다.
  * ApplicationContext는 ServletContext 내 추가가 되며 해당 컨텍스트 내에서 설정된 빈들을 활용이 가능하다.

### 스프링이 제공하는 서블릿 구현체 DispatcherServlet

* 모든 요청의 입구인 front controller parttern을 준수하며 각 요청별로 처리를 담당하는 요소에 dispatch(분배) 및 invoke() 한다.

![](<../../../.gitbook/assets/스크린샷 2022-08-05 오후 6.33.26.png>)

* ApplicationContext는 ServletContext 내 추가하여 사용되는 경우 Root WebApplicationContext를 활용하게된다. 이와 같은 방식으로 구현해야하는 경우 공통적으로 처리하고 싶어도 어렵거나 필터 계층을 무리하게 사용하게 되어 결합도를 증가시킨다.
* DispatcherServlet 이러한점을 해결하기 위해 기 등록된 Root ApplicationContext를 상속받는 웹과 관련된 자원을 관리하기 위한 Servlet ApplicationContext를 생성한다. 이와같은 구조를 같은 경우는 Root ApplicationContext에서 활용되는 Service, Repository 등의 웹과 관련이 없는의 자원을 여러 서블릿에서 공유할수 있다. Servlet ApplicationContext는 Root와 스코프 자체가 다르기 때문에 공유되어지지 않는다.

![](<../../../.gitbook/assets/스크린샷 2022-08-05 오후 6.34.30.png>)

* 서블릿 설정을 통하여 우리는 특정 지정된 패턴 이하의 경로를 dispatch해줄 수 있다.
* 복잡한 방식이므로 Root, Servlet등의 ApplicationContext를 일일히 설정하여 계층구조를 만드는건 사실 어려운 일이다. Servlet만 등록해도 충분하다
* Servlet Container(Tomcat)이 활성화되고 Spring을 연동하는 방식이 Spring MVC
* 내장 Tomcat 서버가 활성화된 이후 서블릿을 코드로 regist하여 하는 방식이 Spring Boot

![](<../../../.gitbook/assets/스크린샷 2022-08-05 오후 6.36.11.png>)

### DispatcherServlet의 동작원리

* 동작순서
  * 요청을 분석 하여 HandlerMapping에 위임하고 해당 요청의 handler 검색
  * 해당 handler를 실행하는 HandlerAdapter 검색하여 자바의 리플렉션을 사용하여 요청을 처리
  * 이 과정에서 예외가 발생하는경우 예외 처리 핸들러에 요청처리를 위임한다.
  * 모델이나 뷰로 응답해야하는지 판단하여 응답 결과를 만든다.
    * 뷰의 경우 실제 뷰에 해당되는 내용을 ViewResolver가 처리한다.

![](<../../../.gitbook/assets/스크린샷 2022-08-05 오후 6.44.05.png>)

* 초기화를 위한 인터페이스
  * 좀더 세부적으로 DispatcherServlet이 활용하는 초기화에 사용되는 인터페이스 목록

![](<../../../.gitbook/assets/스크린샷 2022-08-05 오후 6.45.33.png>)

* DispatcherSerlvet의 기본 전략
  * DispatcherServlet.properties
* MultipartResolver
  * 파일 업로드 요청 처리에 필요한 인터페이스
    * HttpServletRequest를 MultipartHttpServletRequest로 변환해주어 요청이 담고 있는 File을 꺼낼 수 있는 API 제공.
  * LocaleResolver
    * 클라이언트의 위치(Locale) 정보를 파악하는 인터페이스
    * 기본 전략은 요청의 accept-language를 보고 판단.
  * ThemeResolver
    * 애플리케이션에 설정된 테마를 파악하고 변경할 수 있는 인터페이스
    * 참고 : [https://memorynotfound.com/spring-mvc-theme-switcher-example/](https://memorynotfound.com/spring-mvc-theme-switcher-example/)
  * HandlerMapping
    * 요청을 처리할 핸들러를 찾는 인터페이스
  * HandlerAdapter
    * HandlerMapping이 찾아낸 “핸들러”를 처리하는 인터페이스
    * 스프링 MVC 확장력의 핵심
  * HandlerExceptionResolver
    * 요청 처리 중에 발생한 에러 처리하는 인터페이스
  * RequestToViewNameTranslator
    * 핸들러에서 뷰 이름을 명시적으로 리턴하지 않은 경우, 요청을 기반으로 뷰 이름을 판단하는 인터페이스
  * ViewResolver
    * 뷰 이름(string)에 해당하는 뷰를 찾아내는 인터페이스
  * FlashMapManager
    * FlashMap 인스턴스를 가져오고 저장하는 인터페이스
    * FlashMap은 주로 리다이렉션을 사용할 때 요청 매개변수를 사용하지 않고 데이터를 전달하고 정리할 때 사용한다. (redirect:/events)
