# 스프링 부트와 스프링 프레임워크의 차이

* 어찌됬건, 프로덕션 레벨의 애플리케이션을 xml과 같은 static resource로 일일히 설정해줘야하는 불편함이 없는 점을 강조한다.
* 서블릿의 주객전도 되는 아키텍쳐 [#dispatcherservlet](../../spring-mvc/servlet.md#dispatcherservlet "mention")
* 의존성 관리
  * 부트 프로젝트의 의존성 관리는 spring boot parent로 이루어진다. 각종 의존성과 리소스 파일을 활용한 자동설정, 플러그인 등등 이 이루어진다.
  * 사내 특별한 의존성 구조를 가져서 쓰기 싫은경우 부모 프로젝트에 선언하거나, dependencyManagement를 활용하면 되나 후자의 경우에는 의존성 관리 외의 자동으로 관리되는 이점을 활용할 수 없으므로 추천하지 않는다.
  * 기초적으로 버전에 관련된 프로퍼티는 parent에 명시되어 있으나 다른 버전을 쓰고 싶은 경우에는 해당 프로퍼티와 같은 이름을 가지는 프로퍼티를 선언해서 사용하면 된다.