---
layout: post
title:  "IntelliJ에서 스프링 프로젝트 생성하기"
categories: spring
img: use-post/spring.jpg
toc: true
---

조금 헤메기는 했지만 IntelliJ를 사용하여 스프링 프로젝트를 생성해보았어요 :D

<br/><br/>

# 1. 스프링 프로젝트 생성.
***

`File > New > Project...` 차례대로 클릭하면 아래와 같은 창이 뜹니다.

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/newProject.jpg)

좌측의 메뉴에서 생성할 프로젝트를 선택하고 우측에서 추가할 기능을 선택합니다.

저는 일단 Spring MVC 와 Spring Web Services 두가지만 선택했어요.

선택하고 난 다음에 Next 버튼을 누르면 다음 화면으로 넘어가요 :D

<br/>

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/newProject2.jpg)

Project name 을 설정해줍니다. Project name 을 변경하면 Project location, More settings.. 에 있는 정보들이 자동으로 변경됩니다.

<br/>

입력한 후 Finish 를 누르면 프로젝트가 생성됩니다. 생성된 프로젝트의 구조는 아래와 같아요.

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/projectStructure.jpg)

<br/><br/>

# 2. 프로젝트 빌드 툴 추가
***

Spring 프로젝트를 선택하고 프로젝트를 생성하면 생성된 프로젝트에 Maven 이나 Gradle 과 같은 프로젝트 빌드 툴이 추가되지 않은 채 프로젝트가 생성됩니다.

하지만 그렇다고 해서 프로젝트 빌드 툴을 중간에 추가하지 못하는 것은 아니지요 :)

왼쪽 프로젝트에 우클릭을 하면 너무나도 정직한 이름의 메뉴가 있습니다. (바로 `Add Framework Support`)

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/addFrameworkSupport1.jpg)

<br/>

클릭해보면 아래와 같은 화면이 뜹니다. 저는 메이븐을 선택하고 OK 하였습니다. (이 메뉴에서 Gradle 은 찾을 수 없었습니다. 메이븐 혹은 그래들 프로젝트로 생성한 후 똑같은 방법으로 Spring MVC 나 Spring Web Services 를 추가할 수 있습니다.)

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/addMaven.jpg)

<br/>

그러면 프로젝트에 pom.xml 이 추가된 것을 확인할 수 있습니다 :D

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/pomxml.jpg)

<br/><br/>

# 3. 기본 xml 관리
***

메이븐까지 추가하고 나면 xml 파일은 총 4개가 됩니다. (`applicationContext.xml`, `dispatcher-servlet.xml`, `web.xml`, `pom.xml`)

<br/>

### 3-1. web.xml

자동 생성된 web.xml 파일의 내용은 아래와 같습니다.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>*.form</url-pattern>
    </servlet-mapping>
</web-app>
{% endhighlight %}

<br/>

servlet-mapping 의 url-pattern 을 `*.form` 에서 `/` 로 바꿔줍니다.

{% highlight xml %}
<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
{% endhighlight %}

<br/>


### 3-2. dispatcher-servlet.xml

자동 생성된 dispatcher-servlet.xml 의 내용은 아래와 같습니다.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
{% endhighlight %}

<br/>

아래의 내용을 추가해줍니다.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <mvc:annotation-driven/>
    <context:component-scan base-package="main.java.controller"/>

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
{% endhighlight %}

<br/>

그런데.. 자동 생성된 프로젝트에는 위에서 추가한 controller 나 views 디렉토리 등이 없습니다.

단지 src 와 WEB-INF 디렉토리만이 있을 뿐이죠. 없는 부분은 추가해주면 되죠 :D

<br/><br/>

# 4. controller 및 view
***
### 4-1. 컨트롤러 만들기

`src > main > java` 아래에 `controller` 패키지를 만들고 `BasicController` 클래스를 만들어주었습니다.

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/controller.jpg)

<br/>

컨트롤러가 정상적으로 컨트롤러 역할을 할 수 있게끔 어노테이션을 사용하여 코드를 작성합니다.

{% highlight java %}
@Controller
public class BasicController {
	@RequestMapping("/")
	public String index() {
		return "index";
	}
}
{% endhighlight %}

`BasicController` 에서 index 를 리턴하였는데요, `dispatcher-servlet.xml` 에 설정한 값에 따라 `/WEV-INF/views/index.jsp` 를 요청할 것입니다. 하지만 기본 생성된 구조에는 해당 폴더도 없고 `index.jsp` 가 알맞은 위치에 있는 것도 아니니 변경해주도록 합시다.

<br/>

### 4-2. view 디렉토리 만들기

기본적으로 web 디렉토리 이하로는 아래와 같은 구조를 가지고 있습니다.

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/beforeindex.jpg)

`WEB-INF` 디렉토리 아래에 `views` 디렉토리를 만들고 `index.jsp` 파일을 `views` 디렉토리 아래로 옮겨줍니다.

<br/>

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/afterindex.jpg)

<br/><br/>

# 5. server 설정
***

어느정도 기본 틀이 잡혔다면 서버를 설정해줍니다.

전 [톰캣 홈페이지](http://tomcat.apache.org/) 에서 apache-tomcat-8.5.23 버전을 다운받았습니다. (~~엄연히 말하자면 톰캣이 웹서버는 아니지만~~) 사용하기 편하다고 생각되는 경로에 다운받은 톰캣의 압축을 풀어줍니다.

`Run > Edit Configurations...` 이 경로의 메뉴에서 서버를 설정할 수 있습니다. 차례대로 클릭하면 아래와 같은 화면이 뜹니다.

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/tomcatserver01.jpg)

1. 먼저 좌측의 리스트에서 `Tomcat Server > Local` 을 선택합니다.
2. 이미 서버를 설정한 적이 있다면 `Application server` 의 리스트에 서버가 나타날 것이지만 처음 설정하는 것이라면 `Configure...` 을 눌러줍니다.
3. `Configure...` 창에서 `...` 을 클릭하여 설치한 톰캣 서버 디렉토리를 선택해줍니다.
![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/tomcatserver02.jpg)
4. After launch 의 선택을 해제해주고 (launch 후 선택한 브라우저로 페이지를 띄워주느냐에 대한 설정이기 때문에 필요하다면 굳이 해제하지 않으셔도 됩니다 :D)
5. HTTP port 설정을 해줍니다. (기본은 8080 이지만 다른 포트번호를 사용하고 싶으시다면 변경해주세요.)

그리고 OK 를 눌러주면 기본 설정은 끝났습니다.

(만약 설정 중 하단에 아래와 같은 메세지-Warning:No artifacts marked for deployment 가 나온다면 fix 를 눌러주세요. )

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/fix.jpg)

<br/><br/>

# 6. index.jsp 내용 수정
***

`index.jsp` 에 화면에 출력해 줄 내용을 추가합니다 :D

{% highlight html %}
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>spring</title>
  </head>
  <body>
    Hello world!
  </body>
</html>
{% endhighlight %}

<br/><br/>

# 7. server run
***

서버까지 추가되었으면 run 해봅시다. 톰캣을 run 하고 http://localhost:8080/ 에 접속해보니.. 404 오류가 발생하는군요. 혹시 서버 run 중 
{% highlight text %}
심각 [RMI TCP Connection(2)-127.0.0.1] org.apache.catalina.core.StandardContext.startInternal One or more listeners failed to start. Full details will be found in the appropriate container log file
심각 [RMI TCP Connection(2)-127.0.0.1] org.apache.catalina.core.StandardContext.startInternal Context [] startup failed due to previous errors
{% endhighlight %}
이러한 메세지가 발생한다면 추가적인 작업이 필요합니다.

<br/>

`File > Project Structure...` 메뉴로 들어가면 아래의 창이 뜹니다.

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/projectstructureAdd.jpg)

`Aviliable Elements` 영역에 있는 모든 항목을 더블클릭하여 추가한 후 OK 버튼을 눌러줍니다.

그리고 다시 서버를 run 한 후 http://localhost:8080/ 으로 접속하면 Hello wolrd 를 볼 수 있습니다 :D

![image]({{ site.url }}/upload/images/2017-10-31-new-spring-project/helloworld.jpg)

<br/><br/>

이렇게 IntelliJ 에서의 기초적인 Spring 프로젝트 생성은 완료하였습니다.

DB와의 연결을 추가하고 서비스를 만드는 등의 작업은 별도로 진행되어야 합니다 :D

각종 xml 파일에서 어떠한 것들을 설정하게 되는지는 다음에 자세히 알아보도록 하겠습니다 :D

<br/><br/>
