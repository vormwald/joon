SpringMVC와 WebWork의 주요 차이점

# WebWork와 SpringMVC 

목차      
1. SpringMVC와 WebWork의 차이점  
2. SpringMVC  
3. WebWork  
4. 용어  

## SpringMVC와 WebWork 주요 차이점  
### Controller vs Action  
- Controller  
    - 싱글톤  
    - Spring Bean 으로 관리 된다  
        - Proxy 기반의 AOP 적용이 가능  
- Action  
    - 싱글톤이 아님 : 매 리퀘스트마다 인스턴스 생성  

## SpringMVC  
> Spring을 웹 기반에서 보다 편리하게 이용하기 위한 프레임워크  


![image.png](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/SpringMVC.png?raw=true)


## WebWork  
> architecture of WebWork was based on the MVC Framework, Command, and Dispatcher patterns and the principle of IoC  
> 
> Apache Struts : Java EE 웹 애플리케이션을 개발하기 위한 오픈소스 프레임워크.  
> Struts2는 사용자에게 요청을 받고 원활하게 작동할 수 있도록 환경을 꾸미고,   
> 어떤 액션을 호출할 지 결정한 후 액션을 실행한다.  
> Result를 통해 최종 응답 데이터를 사용자에게 반환하는 역할들을 제공한다.  

### Lifecycle  
1. request begins when the servlet container receives a new request  
2. passed through **filter chain(set of filters) and sent to the FilterDispatcher**  
3. forwards the request to the **Actionmapper**  
4-1. if the request requires an action : sends an **Actionmapping object** back to FilterDispatcher  
4-2. not requires an action : ActionMapper returns **a null object**  
5. FilterDispatcher forwards the ActionMapper object to the **ActionProxy for further action.**  
6. ActionProxy invokes the Configuration File manager to get the attributes of the action. which is stored in the xwork.xml file and creates an **ActionInvocation object.**  
7. the ActionInvocation object contains attributes like the action, invocation context, result result code, etc.  
8. **Action result code to look up the appropriate result**, which is usually a JSP page.(I think is it the view)  
9. also ActionInvocation returns the response as a HttpServletResponse.  

![image.png](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/WebWork.png?raw=true)

## 용어  
 (I think?)  
 
| SpringMVC | WebWork |
| --- | --- |
| Controller | Action |
| Model | ActionInvocation Object |
| View Resolver | Action Mapper |
| View | Result |


<br>

참고 : https://en.wikipedia.org/wiki/WebWork  
참고 : http://theeye.pe.kr/archives/260  
참고 : https://mer-bleu.tistory.com/24  
이미지 출처 : https://nhnent.dooray.com/project/2210798153185693714/2249870144271952624  
이미지 출처 : https://mer-bleu.tistory.com/24  
