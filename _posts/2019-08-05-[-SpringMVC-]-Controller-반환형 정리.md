# Controller 반환형 정리  

Controller는 View에 의존적이지 않다. Controller는 반환형에 따라 결과를 생성할 View 이름만 지정할 뿐이다.  

이때 설정에서 지정한 기본 ViewResolver 타입에 따라 파일을 탐색하여 View 이름과 일치하는 뷰 템플릿을 찾는다.  

```
<!--spring-servlet.xml-->

<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/views/"/>
    <property name="suffix" value=".jsp" />
</bean>
```

아래 예시들은 InternalResourceViewResolver를 이용했다.  
(war 파일 내에 포함된 뷰 템플릿을 찾는다. 이때 뷰 템플릿의 경로는 <span style="color:#e11d21">prefix</span> + <span style="color:#207de5">View 이름</span> + <span style="color:#009800">suffix</span>으로 구성된다.)  

## 1. String형  

메소드에서 반환하는 String으로 View 이름을 정한다.  

```
@RequestMapping("/sinyutonori")
public String getUserInfo(Model model) {
    model.addAttribute("multiIdps", Idp.values());
    return "userGameInfo";
}
```

뷰 템플릿은 <span style="color:#e11d21">/WEB-INF/views/</span><span style="color:#207de5">userGameInfo</span><span style="color:#009800">.jsp</span>로 매핑된다.  

뷰로 전달하고자 하는 데이터가 존재할 경우 Model 객체에 담아서 반환한다.  

## 2. redirect  
반환하는 View 이름에 "redirect:" 접두어를 붙이면 지정한 페이지로 리다이렉트된다.  
* 해당 접두어는 UrlBasedViewResolver 클래스를 상속받은 ViewResolver를 사용할 경우 올바르게 처리된다.  

```
@RequestMapping("/userGameInfo")
public String getMemberInfo(@RequestParam(value = "url") String url) {
    return "redirect:" + url;
}
```

## 3. void형  

Spring은 View 이름을 지정하지 않아도 설정된 URI를 이용해(RequestToViewNameTranslator) View 이름을 결정한다.  

* Controller에서 View이름이나 객체를 제공하지 않았을 경우 요청 정보를 참고해서 자동으로 View 이름을 생성해주는 DI  
* (설정 파일에 해당 bean이 존재하지 않을 경우 기본적으로 DefaultRequestToViewNameTranslator 구현체 사용)  

```
@Controller
@RequestMapping("/casual/sinyutnori")
public class SinyutnoriController {
	@Autowired
	private SinyutnoriBO sinyutnoriBO;

	@RequestMapping("/userGameInfo")
	public void getUserInfo(@RequestParam(value = "memberId", defaultValue = "") String memberId, Model model) {
            model.addAttribute("multiIdps", Idp.values());
            return;
	}
}
```
<span style="color:#e11d21">/WEB-INF/views/</span><span style="color:#207de5">casual/sinyutnori/userGameInfo</span><span style="color:#009800">.jsp</span> 뷰 템플릿으로 매핑된다.

## 4. ModelAndView형  

```
@Controller
@RequestMapping("/casual")
public class SinyutnoriController {
	@Autowired
	private SinyutnoriBO sinyutnoriBO;

	@RequestMapping("/sinyutnori")
	public ModelAndView getUserInfo() {
	    return new ModelAndView("userGameInfo").addObject("multildps", Idp.values());
	}
}
```
ModelAndView 객체에 View 이름과 데이터를 직접 담아(addObject) 반환한다.  
View 이름은 String형에서와 같이 <span style="color:#e11d21">/WEB-INF/views/</span><span style="color:#207de5">userGameInfo</span><span style="color:#009800">.jsp</span> 뷰 템플릿에 매핑된다.  

## 5. POJO  
이 경우도 View 이름을 지정해줄 수 없기에 void와 같이 RequestToViewNameTranslator를 이용해 결정한다.  
```
@Controller
@RequestMapping("/casual/sinyutnori")
public class SinyutnoriController {
	@Autowired
	private SinyutnoriBO sinyutnoriBO;

	@RequestMapping("/userGameInfo")
	public MemberInfo getUserInfo(@RequestParam(value = "memberId", defaultValue = "") String memberId) {
            return MemberApiUtil.getMemberInfoModel(memberId);
	}
}
```
<span style="color:#e11d21">/WEB-INF/views/</span><span style="color:#207de5">casual/sinyutnori/userGameInfo</span><span style="color:#009800">.jsp</span>

## 6. @ResponseBody  
반환 데이터 타입에 따라 자동으로 HttpMessageConverter를 이용하여 데이터를 변환하고 해당 내용을 HTTP body에 매핑한다.  
따라서 이 어노테이션을 이용할 경우 View 이름을 지정하지 않는다.  

```
@Controller
@ResponseBody
public MemberInfo getMemberInfo(@RequestParam(value = "memberId", defaultValue = "") String memberId) throws MemberApiException {
    return MemberApiUtil.getMemberInfoModel(memberId);
}
```

* 기본적으로 converter는 RequestHeader의 Accept에 따라 선택된다.  
* Spring 4.0부터 @RestController (@Controller + @ResponseBody)를 붙일 경우 객체 반환 시 자동으로 JSON 방식으로 파싱된다.  




<br><br>참고
https://kimddochi.tistory.com/86  
https://ismydream.tistory.com/140  
http://wonwoo.ml/index.php/post/2007  
https://blog.naver.com/kalmia888/185985460  
https://devbox.tistory.com/entry/Spring-ViewResolver-설정  