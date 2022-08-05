---
description: 원리에 대해서 학습 정리
layout: editorial
---

# Spring Boot 원리

### 스프링 부트와 스프링 프레임워크의 차이&#x20;

* 어찌됬건, 프로덕션 레벨의 애플리케이션을 xml과 같은 static resource로 일일히 설정해줘야하는 불편함이 없는 점을 강조한다.
* 서블릿의 주객전도 되는 아키텍쳐
* 의존성 관리
  * 부트 프로젝트의 의존성 관리는 spring boot parent로 이루어진다. 각종 의존성과 리소스 파일을 활용한 자동설정, 플러그인 등등 이 이루어진다.
  * 사내 특별한 의존성 구조를 가져서 쓰기 싫은경우 부모 프로젝트에 선언하거나, dependencyManagement를 활용하면 되나 후자의 경우에는 의존성 관리 외의 자동으로 관리되는 이점을 활용할 수 없으므로 추천하지 않는다.
  * 기초적으로 버전에 관련된 프로퍼티는 parent에 명시되어 있으나 다른 버전을 쓰고 싶은 경우에는 해당 프로퍼티와 같은 이름을 가지는 프로퍼티를 선언해서 사용하면 된다.

### 자동설정에 대한 이해

![](<../../../.gitbook/assets/스크린샷 2022-08-05 오후 5.47.15.png>)

* @SpringBootApplication은 다음과 같은 3가지의 어노테이션으로 구성되어있다.
  * @SpringBootConfiguration
  * @ComponentScan
  * @EnableAutoConfiguration&#x20;
* @EnableAutoConfiguration 이 어떻게 활용되어 자동 설정이 이루어지는가

#### 빈의 등록과정을 2번 거친다

{% tabs %}
{% tab title="@ComponentScan " %}
* @Component 를 선언한 빈들을 DI 한다.
* BeanFactory에 의하여 스프링 부트 내 테스트를 지원하기 위한 TypeExcludeFilter는 DI하지 않는다.
* 자동 설정을 등록하기 위한 필터 역시 등록하지 않는다. &#x20;
{% endtab %}

{% tab title="@EnableAutoConfiguration" %}


![](<../../../.gitbook/assets/스크린샷 2022-08-05 오후 6.04.10.png>)



* spring.factories
  * org.springframework.boot.autoconfigure.EnableAutoConfiguration
* @Configuration
* @ConditionalOnXxxYyyZzz
{% endtab %}
{% endtabs %}

#### 자동설정은 어떻게 이루어지는지 하나의 클래스를 예로 살펴보자

{% tabs %}
{% tab title="SpringDataWebAutoConfiguration" %}


![](<../../../.gitbook/assets/스크린샷 2022-08-05 오후 6.13.18.png>)
{% endtab %}

{% tab title="특정 조건이 충족 되어야만 자동설정을 한다." %}


![](<../../../.gitbook/assets/스크린샷 2022-08-05 오후 6.14.51.png>)
{% endtab %}
{% endtabs %}
