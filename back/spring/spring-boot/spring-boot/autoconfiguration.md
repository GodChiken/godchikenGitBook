# AutoConfiguration에 대한 이해

![](<../../../../.gitbook/assets/스크린샷 2022-08-05 오후 5.47.15.png>)

* @SpringBootApplication은 다음과 같은 3가지의 어노테이션으로 구성되어있다.
  * @SpringBootConfiguration
  * @ComponentScan
  * @EnableAutoConfiguration&#x20;
* @EnableAutoConfiguration 이 어떻게 활용되어 자동 설정이 이루어지는가

### 빈의 등록과정은 순서대로 2번 거친다

{% tabs %}
{% tab title="@ComponentScan " %}
* @Component 를 선언한 빈들을 DI 한다.
* BeanFactory에 의하여 스프링 부트 내 테스트를 지원하기 위한 TypeExcludeFilter는 DI하지 않는다.
* 자동 설정을 등록하기 위한 필터 역시 등록하지 않는다. &#x20;
{% endtab %}

{% tab title="@EnableAutoConfiguration" %}


![](<../../../../.gitbook/assets/스크린샷 2022-08-05 오후 6.04.10.png>)



* spring.factories
  * org.springframework.boot.autoconfigure.EnableAutoConfiguration
* @Configuration
* @ConditionalOnXxxYyyZzz
{% endtab %}
{% endtabs %}

### 자동설정의 과정 하나의 클래스를 예로 살펴보자

{% tabs %}
{% tab title="SpringDataWebAutoConfiguration" %}


![](<../../../../.gitbook/assets/스크린샷 2022-08-05 오후 6.13.18.png>)
{% endtab %}

{% tab title="특정 조건이 충족 되어야만 자동설정을 한다." %}
```java
@Configuration(proxyBeanMethods = false)
@EnableSpringDataWebSupport
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ PageableHandlerMethodArgumentResolver.class, WebMvcConfigurer.class })
@ConditionalOnMissingBean(PageableHandlerMethodArgumentResolver.class)
@EnableConfigurationProperties(SpringDataWebProperties.class)
@AutoConfigureAfter(RepositoryRestMvcAutoConfiguration.class)
public class SpringDataWebAutoConfiguration {
    //someThing code..
}
```

* @ConditionalXXX 시리즈의 어노테이션을 활용하여 특정 조건에서만 자동지원을 하도록 체크한다.
{% endtab %}
{% endtabs %}

### 자동설정을 직접 진행해보자

{% tabs %}
{% tab title="자동설정에대한 예제구조" %}
![](<../../../../.gitbook/assets/스크린샷 2022-08-06 오후 3.16.37.png>)
{% endtab %}

{% tab title="@Configuration을 활용한 @Bean 등록 " %}
![](<../../../../.gitbook/assets/스크린샷 2022-08-06 오후 3.18.46.png>)
{% endtab %}

{% tab title="Untitled" %}


![](<../../../../.gitbook/assets/스크린샷 2022-08-06 오후 3.19.35.png>)
{% endtab %}

{% tab title="만들어진 자동 설정 활용법" %}
![](<../../../../.gitbook/assets/스크린샷 2022-08-06 오후 3.22.32.png>)

* mvn clean install 하여 로컬 메이븐 리포지토리에 배포한다.
* 이후 자동설정 프로젝트의 pom.xml 내의 스크린샷과 동일한 정보를 확인하여 활용하고자하는 타 프로젝트에 dendency 등록을 하면 활용이 가능하다.
* 특정 클래스나 관심사를 프로젝트 외부로 관리는 하고싶은데 Nexus와 같은 private repository를 아직 학습하지 못한경우 고려해볼만한 방법 중 하나가 아닐까 싶다.
{% endtab %}
{% endtabs %}

