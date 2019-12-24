# Filter, Interceptor, AOP 비교

- 공통점 : 전체 기능에서 공통적으로 처리해야할 업무들을 분리해서 관리하는 것  
- 호출 순서 : Filter -> Interceptor -> aop -> aop -> Interceptor -> Filter  

![1](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019-12-24_filter/1.jpg?raw=true)


|  | Filter | Interceptor | AOP |
| --- | --- | --- | --- |
| 실행 위치 | Servlet(Dispatcher Servlet 밖) | Servlet(Dispatcher Servlet 안) | Method |
| 실행 순서 | 1 | 2 | 3 |
| 설정 파일 | web.xml | spring-servlet.xml |  |
| 실행 메소드 | init, doFilter, destory | preHandler, postHandler, afterCompletion | pointcut (@after, @before, @around 등) |

## 1. Filter  
- DispatcherServlet 이전에 실행  
- Servlet 요청과 응답을 거른 뒤 정제하는 역할  
- web.xml에서 정의한다.  
- 일반적으로 인코딩 변환 처리, XSS방어 등의 요청에 대한 처리로 사용  
- Spring과 무관한 자원에 대해 동작  

![2](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019-12-24_filter/2.jpg?raw=true)


## 2. Interceptor
- DispatcherServlet 이후에 실행  
- HttpServletRequest, HttpServletResponse와   
같이 Spring Context 내부에서 Controller에 관한 요청과 응답에 대해 처리  
- spring-servlet.xml에서 정의  

![3](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019-12-24_filter/3.jpg?raw=true)
![4](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019-12-24_filter/4.jpg?raw=true)


## 3. AOP
- AOP란, 프로그램 구조를 다른 방식으로 생각하게 함으로써 OOP를 보완한다  
- OOP에서 모듈화의 핵심단위는 클래스이지만 AOP에서 모듈화의 핵심단위는 관점(aspect)이다  
- 관점은 다양한 타입과 객체에 걸친 트랙잭션 관리같은 관심(concern)을 모듈화할 수 있게 한다  
    - crosscutting concerns: 횡단 관심사  
- 메소드 전후의 지점에서 자유롭게 설정 가능  
- 주소, 파라미터, 어노테이션 등 다양한 방법으로 대상 지정 가능  

![5](https://github.com/SeonheeKim/SeonheeKim.github.io/blob/master/content/images/2019-12-24_filter/5.jpg?raw=true)


* * *

<br>
<br>

참고  
https://goddaehee.tistory.com/154  
https://blog.naver.com/platinasnow/220035316135  
https://intro0517.tistory.com/150  
https://m.blog.naver.com/PostView.nhn?blogId=fortunerain&logNo=220964510870&proxyReferer=https://www.google.com/  
https://greatkim91.tistory.com/94  
https://devjms.tistory.com/70  
https://nhnent.dooray.com/project/posts/2250793129627747036  