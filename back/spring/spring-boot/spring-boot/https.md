# 스프링 부트 내에 HTTPS 적용해보기

### HTTPS 프로토콜을 내장 서블릿 컨테이너에 어떻게 적용하는가?

* 우리는 embedded-tomcat -> embedded-undertow로 바꿔보는 실습을 해보았다.
* 그렇다면 우리는 이런 다양한 내장 서블릿 컨테이너에서 HTTPS 프로토콜을 적용하려면 어떻게 해야할까?
* 결국 SSL 인증서를 만들어야하고 keystore를 생성해야한다.
* 이것하나 실습해보자고 공인된 사이트에서 일일히 SSL을 발급받는 것이 아니라 직접 한번 구현해보자.

{% tabs %}
{% tab title="키스토어 생성 스크립트" %}
* 키스토어 생성 스크립트이며 자세한 것은 구글링해보자.

```
keytool -genkey -alias tomcat -storetype PKCS12 -keyalg RSA -keysize 2048 -keystore keystore.p12 -validity 4000
```

* 스크립트 실행분으로 특별한 것은 없다.

![](<../../../../.gitbook/assets/스크린샷 2022-08-16 오후 10.05.01.png>)
{% endtab %}

{% tab title="SSL property 입력" %}
* 키스토어 스크립트에 적힌 속성대로 적는다.
* application.propertie 수정

{% code overflow="wrap" lineNumbers="true" %}
```properties
# ssl 설정

# resource 폴더 내에 위치하는경우 "classpath:"를 기술한다.
server.ssl.key-store: classpath:keystore.p12
server.ssl.key-store-password: 123456
server.ssl.keyStoreType: PKCS12
server.ssl.keyAlias: tomcat
```
{% endcode %}
{% endtab %}

{% tab title="다음과 같은 결과를 봤다면 1차실습 성공" %}
![](<../../../../.gitbook/assets/스크린샷 2022-08-16 오후 10.28.27.png>)
{% endtab %}
{% endtabs %}

### HTTPS 적용 이후 HTTP 프로토콜을 쓰지 못하게 된 경우 해결해보자&#x20;

* https로 curl 요청을 날려보면 공인된 인증서가 아니기 때문에 오류가 나타나고 이를 무시하는 명령어로 실습한다.
* 실습 이후에 https를 적용하면 더 이상 http 프로토콜을 사용할 수 없다.&#x20;
* 왜냐하면  http connecter는 하나이기 때문이므로 둘다 쓰려면 참고예제로 코딩을해보자
* 하는 김에 http2 허용까지 진행한다.

{% tabs %}
{% tab title="HTTPS 요청하기" %}
![](<../../../../.gitbook/assets/스크린샷 2022-08-16 오후 10.35.32.png>)
{% endtab %}

{% tab title="HTTP Protocol용 커넥터 만들기" %}
{% code overflow="wrap" lineNumbers="true" %}
```java
import org.apache.catalina.connector.Connector;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory;
import org.springframework.boot.web.servlet.server.ServletWebServerFactory;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class GodchikenSpringApplication {

    @Bean
    public ServletWebServerFactory servletContainer() {
        TomcatServletWebServerFactory tomcat = new TomcatServletWebServerFactory();
        tomcat.addAdditionalTomcatConnectors(createStandardConnector());
        return tomcat;
    }

    private Connector createStandardConnector() {
        Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
        connector.setPort(8080);
        return connector;
    }

    public static void main(String[] args) {
        SpringApplication.run(GodchikenSpringApplication.class, args);
    }
}
```
{% endcode %}

* 출처 : [https://github.com/spring-projects/spring-boot/blob/v2.0.3.RELEASE/spring-boot-samples/spring-boot-sample-tomcat-multi-connectors/src/main/java/sample/tomcat/multiconnector/SampleTomcatTwoConnectorsApplication.java](https://github.com/spring-projects/spring-boot/blob/v2.0.3.RELEASE/spring-boot-samples/spring-boot-sample-tomcat-multi-connectors/src/main/java/sample/tomcat/multiconnector/SampleTomcatTwoConnectorsApplication.java)
{% endtab %}

{% tab title="HTTP2 허용하기" %}
* application.propertie 수정을 진행한다.

```properties
# ssl 설정

# resource 폴더 내에 위치하는경우 "classpath:"를 기술한다.
server.ssl.key-store: classpath:keystore.p12
server.ssl.key-store-password: 123456
server.ssl.keyStoreType: PKCS12
server.ssl.keyAlias: tomcat

server.http2.enabled=true
```
{% endtab %}

{% tab title="결과 확인" %}
* 다음과 같이 커맨드를 입력하여 확인한다.

![](<../../../../.gitbook/assets/스크린샷 2022-08-16 오후 10.55.15.png>)

* 로그에 다음과 같이 나오는지 확인한다.

![](<../../../../.gitbook/assets/스크린샷 2022-08-16 오후 11.01.03.png>)
{% endtab %}
{% endtabs %}

