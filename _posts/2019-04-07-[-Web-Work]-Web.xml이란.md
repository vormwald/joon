프로젝트를 이해하기 위해 살펴봤던 내용들을 정리한다.

# Web.xml

> web.xml : 웹 애플리케이션의 deployment descriptor(배포 설명자)  
> 각 애플리케이션의 환경을 설정하는 역할을 한다.  
> 서버가 처음 로딩될 때 읽어들이고, 해당 환경설정에 대해 tomcat에 적용하여 서버를 시작한다.  

***

## Web.xml tag
**web-app**

    <web-app xmlns="http://java.sun.com/xml/ns/j2ee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd"
             version="2.4">

`Servlet 스키마 정보`

xmlns로 XML Schemas for J2EE Deployment Descriptors 파일이 존재하는 위치를 선언

* * *
**context-param**

    <context-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>
                classpath*:common/applicationContext*.xml,classpath:applicationContext*.xml
            </param-value>
    </context-param>
    <context-param>
            <param-name>configurationLocation</param-name>
            <param-value>configuration.xml</param-value>
    </context-param>

> Context : 이름이나 객체를 바인딩하는 역할 담당
> 어떤 객체를 핸들링 하기 위한 접근 수단

`모든 Servlet과 filter에서 사용되는 설정 정의`

선언된 정보들로 초기화 시켜주는 일을 한다. (초기화 파라미터)



* * *
**listener**

    <listener>
            <listener-class>com.nhncorp.lucy.securitybeans.listener.DefencePolicyInitializer</listener-class>
    </listener>

`Servlet의 생성, 소멸, 수정을 알려주는 태그`

listener로 정의한 클래스들을 로드한다.

* * *
**filter**

    <filter>
            <filter-name>exampleServiceFilter</filter-name>
            <filter-class>com.web.filter.ExampleServiceFilter</filter-class>
    </filter>

`filter 정의`

위의 예시에서는 ExampleServiceFilter 클래스를 exampleServiceFilter라는 이름으로 정의한다

* * *
**filter-mapping**

    <filter-mapping>
            <filter-name>exampleServiceFilter</filter-name>
            <url-pattern>*.service</url-pattern>
            <dispatcher>REQUEST</dispatcher>
            <dispatcher>FORWARD</dispatcher>
    </filter-mapping>

`지정된 url pattern으로 들어온 모든 요청에 filter를 적용하여 가공한 후 웹 서버로 보낸다.`

위의 예시는 .service으로 끝나는 모든 url 요청에 대해 exampleServiceFilter로 정의된 filter를 적용한다는 뜻이다.  

이때 <dispatcher> forward가 존재하는데 일반 url 요청 아니라 forward()를 통해 들어올 경우도 filter를 적용한다는 의미이다.  
(ex. <jsp:forward ...>)  

dispatcher가 가질 수 있는 값은 REQUEST, INCLUDE, FORWARD, ERROR가 있다.  

* * *
**welcome-file-list**

    <welcome-file-list>
            <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

`최초로 보여주는 페이지 설정`

* * *
**servlet**

    <servlet>
            <servlet-name>errorHandler</servlet-name>
            <servlet-class>com.exception.ErrorHandlerServlet</servlet-class>
    </servlet>

> Servlet : Java에서 동적 웹 프로젝트를 개발할 때, 사용자의 요청과 응답을 처리해 주는 역할

`jsp/servlet에서 사용되는 servlet 이름을 등록, 초기화, servlet class 등록`

* * *
**servlet-mapping**

    <servlet-mapping>
            <servlet-name>errorHandler</servlet-name>
            <url-pattern>/servlet/errorHandler</url-pattern>
    </servlet-mapping>

`지정된 url 패턴으로 들어온 모든 요청에 대해 적용할 servlet 정의`

* * *
**error-page**

    <error-page>
            <exception-type>java.lang.Throwable</exception-type>
            <location>/servlet/errorHandler</location>
        </error-page>
    <error-page>
            <error-code>404</error-code>
            <location>/servlet/errorHandler</location>
    </error-page>

`오류가 발생했을 경우 location으로 연결`

위의 예시에서처럼 Throwable exception이 발생했거나 404 오류일 경우 location에서 지정한 errorHandler servlet으로 연결해서 처리한다.  

## 프로젝트별 web.xml 특징

***

### WebWork
- 클라이언트의 요청에 filter를 통해 알맞은 가공을 하고(filter chain 통과)  
- FilterDispatcher가 Action Mapper를 이용해 가공 결과에 매핑되는 action 클래스를 찾는다.  
- 이 과정을 거치기 위해서 web.xml에 filter들의 정의가 필요하다.  

### WebWork + SpringMVC
- 기존 WebWork 프로젝트의 filter, servlet 등 설정들과 SpringMVC의 DispatcherServlet이 web.xml에 함께 존재하는 형태  
    1. 기존 WebWork filter들에 매핑되는 url 요청이 들어올 경우 : 해당 filter를 거친 후 매핑되는 action을 검색한다. (Webwork flow)  
    2. 새로운 url 요청일 경우 : 최종 filter(이전의 filter들과 매핑되지 않을 것이므로) 이후 DispatcherServlet를 통해 처리  
    (이때 DelegatingFilterDispatcher를 최하위 Filter로 두고 servlet이 처리되도록 한다. 이후 처리 Servlet은 DispatcherServlet이 처리한다.)  

- WebWork 기본적인 FLOW 구조 : ActionContextCleanUp > ~~sitemesh filter~~ > DelegatingFilterDispatcher 순으로 호출 필요  

### SpringMVC
- 클라이언트 요청을 처리하기 위해 별도의 filter나 servlet을 web.xml에서 정의할 필요가 없다.  
- Spring의 DisaptcherServlet에서 모든 요청(url pattern "/")을 처리한다.  
(Controller Mapping 및 View 검색, 출력 등)

![dispatcherServlet](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/dispatcherServlet.png?raw=true)

<br>
<br>
참고 : https://rebeccajo.tistory.com/5
참고 : https://mkil.tistory.com/286
참고 : http://wiki.gurubee.net/display/DEVSTUDY/WebWork+architecture
참고 : https://m.blog.naver.com/PostView.nhn?blogId=myca11&logNo=80127282862&proxyReferer=https://www.google.com/
참고 : https://nhnent.dooray.com/project/posts/2038433059499774114
이미지 출처 : http://egloos.zum.com/springmvc/v/504151