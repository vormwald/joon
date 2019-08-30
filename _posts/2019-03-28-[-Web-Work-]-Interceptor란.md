# Interceptor

xwork.xml : 해당 프로젝트의 xwork 설정. 프로젝트 전반적으로 사용되는 interceptor들을 정의할 수 있다.

xwork-(xxxxxx).xml : 각 파트별로 설정 세분화  
필요한 action을 정의하고 해당 action에 적용하고자 하는 interceptor들을 명시한다.

     <interceptor name="interceptorExample" class="com.test.interceptor.InterceptorExample"/> 

`interceptor를 정의하는 부분`  

위의 경우 InterceptorExample class를 interceptorExample라는 이름으로 정의하는 것이다.  
 
    <interceptor-ref name="interceptorExample"/>

`이미 정의된 interceptor를 사용하는 부분`  

reference, 즉 name에 해당하는 interceptor를 참조한다는 것  

     <interceptor-stack name="interceptorExampleStack">
            <interceptor-ref name="interceptorOne"/>
            <interceptor-ref name="interceptorTwo"/>
     </interceptor-stack>

interceptor를 여러 개 적용해야할 때 `interceptor-stack`를 통해 하나의 name으로 정의할 수 있다.  
interceptor-ref로 stack을 참조할 수도 있다.  
    
* * *

예시 ) xwork-mockexample.xml

    <?xml version="1.0" encoding="MS949"?>
    <!DOCTYPE xwork PUBLIC "-//OpenSymphony Group//XWork 1.1.1//EN"
            "http://www.opensymphony.com/xwork/xwork-1.1.1.dtd">

    <xwork>
        <package name="com.mockexample" namespace="/mockexample">
            <action name="mockEvent" class="com.mockexample.action.MockEventAction">
                <interceptor-ref name="eventBaseStack"/>
                <interceptor-ref name="authInterceptor"/>
                <interceptor-ref name="responseHandlerInterceptor"/>

                <result name="example">/event/example.jsp</result>
            </action>
        </package>
    </xwork>
    
 matchCountEvent action의 경우 result를 실행하기 전 interceptor들을 거친다.
 
 interceptor들이 여러 개일 경우 순서대로 실행한다.
 
 위 예시에서 eventBaseStack는 interceptor-stack이다.  
 이때도 해당 stack의 interceptor가 순서대로 실행된 후 authInterceptor를 시작한다.