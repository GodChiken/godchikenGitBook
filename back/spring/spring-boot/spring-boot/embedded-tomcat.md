# 내장 톰켓(Embedded Tomcat)

### 톰캣을 직접 구현한다면?

![](<../../../../.gitbook/assets/스크린샷 2022-08-11 오후 8.56.13.png>)

* 우리는 톰켓의 작동하는 부분을 직접 구현하지 않아도 스프링 부트 덕에 편리하게 사용이 가능하다.
* 그렇다면 스프링 부트에서 어떻게 톰켓에 대한 자동설정을 진행하는 것인가 살펴보려한다.

### 그래서 직접한번 구현해보자

{% tabs %}
{% tab title="구현 코드" %}
```java
public class GodchikenSpringApplication {

	public static void main(String[] args) throws LifecycleException {
		Tomcat tomcat = new Tomcat();
		tomcat.setPort(8080);

		Context context = tomcat.addContext("/", "/");

		HttpServlet servlet = new HttpServlet() {
			@Override
			protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
				PrintWriter printWriter = resp.getWriter();
				printWriter.println("HTML Tag Here or Not");
			}
		};
		String servletName = "GodchikenServlet";

		tomcat.addServlet("/", servletName, servlet);

		context.addServletMappingDecoded("/hello", servletName);

		// tomcat 9 before
		/* tomcat.start();
		tomcat.getServer().await();*/

		// tomcat 9 after
		tomcat.getConnector();
		tomcat.start();
		tomcat.getServer().await();
	}

}
```


{% endtab %}

{% tab title="톰켓에 대한 자동설정은 어디에 있는가" %}
* 톰켓은 서블릿을 준수하여 구동되는 웹 어플리케이션 서버다.
* 자동설정을 보려면 아래와 같은 형식으로 접근하면 실제로 톰켓을 어떻게 구동하는가 확인이 가능하다.
* ServletWebServerFactoryAutoConfiguration -> EmbeddedTomcat -> TomcatServletWebServerFactory
{% endtab %}

{% tab title="왜 톰켓 코드와 DispatcherServlet과 별도로 존재하는가" %}
* 서블릿 컨테이너는 매번 달라질 수 있다.
* 즉 톰켓만 사용하는 것이 아니다.
* DispatcherServletAutoConfiguration은 DispatcherServlet을 만들어 서블릿 컨테이너에 등록하는 역활을 한다.
{% endtab %}
{% endtabs %}

### 다른 서블릿 컨테이너를 사용해보자

{% tabs %}
{% tab title="서블릿 컨테이너 교체" %}
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
	<exclusions>
		<exclusion>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
		</exclusion>
	</exclusions>
</dependency>

<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```

![](<../../../../.gitbook/assets/스크린샷 2022-08-11 오후 9.48.53.png>)

* 첫번째 그림과 같이 의존성을 수정하여 undertow를 활용하고 아래외 같이 로그를 확인한다.
{% endtab %}
{% endtabs %}

### Port를 애플리케이션 레벨에서 관리하는 법

```java
import org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext;
import org.springframework.boot.web.servlet.context.ServletWebServerInitializedEvent;
import org.springframework.context.ApplicationListener;
import org.springframework.stereotype.Component;

/**
 * 고정이나 랜덤하게 뜨는 Port를 애플리케이션 레벨에서 어떻게 활용할 것인가?
 * 레퍼런스의 추천되는 방식을 활용하여 구현해본다.
 * */

@Component
public class PortListener implements ApplicationListener<ServletWebServerInitializedEvent> {
    @Override
    public void onApplicationEvent(ServletWebServerInitializedEvent servletWebServerInitializedEvent) {
        ServletWebServerApplicationContext applicationContext = servletWebServerInitializedEvent.getApplicationContext();
        System.out.println(applicationContext.getWebServer().getPort());
    }
}
```
