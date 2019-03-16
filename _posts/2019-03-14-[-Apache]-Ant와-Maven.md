사수님께 프로젝트 빌드 툴로 Ant를 사용한다는 이야기를 들었다.  
아마 내가 Maven을 사용해서 잘 모를거라고 하셨는데...정말 처음 들어봤다.  
그래서 Ant와 Maven의 차이를 조사해보라는 임무(?)를 받았다.  
Maven도 잘 모르는데...!!  
<br>

## 1. Ant와 Maven의 차이점
*** 
> Apache Ant : Java 프로그래밍 언어에서 사용하는 자동화된 소프트웨어 빌드 도구, 빌드를 위한 환경구성에 xml 파일을 사용한다.
> Apache Maven : Java용 프로젝트 관리 도구. Dependency를 리스트 형태로 관리한다. Apache ant의 대안으로 만들어졌다.  
<br>

### - Ant의 특징

ant는 각 프로젝트에 대해 xml 기반으로 빌드 스크립트를 개발한다.  
따라서 **대상 프로젝트에 맞게 target과 dependency, 결과물의 위치 등을 지정 해줘야 하며 명확한 빌드 절차의 정의**가 필요하다.  
또한 **각각의 target에 대한 의존관계와 task를 정의**해 주어야 한다.

직접 정의가 가능하므로 **유연성이 높다.**

### - Ant의 단점

다만 프로젝트가 복잡해질 경우 각각의 빌드 과정을 이해하기가 어렵다.
또한 **xml과 remote repository를 가져올 수 없다.**
그리고 스크립트의 재사용이 어렵다는 단점이 있다.

### - Maven의 특징

반면 maven의 경우 **표준화된 프로젝트**를 제공한다.  
따라서 개발자가 프로젝트 빌드에 신경쓰지 않아도 된다.  
또한 **Dependency를 리스트 형태로 maven에서 관리**하여 xml이나 remote repository를 선언만으로 사용할 수 있다.  

### - Maven의 단점

하지만 라이브러리가 서로 종속할 경우 xml이 복잡해지고  
**프로젝트의 구조가 표준과 다를 때 maven을 적용하기 어렵다**는 단점이 있다.  
<br>

## 2. Ant + Ivy 사용 이유
***  
위에서 ant는 xml과 remote repository를 가져올 수 없다고 언급했다.  
이 단점을 극복하기 위해 도입한 것이 Apache ivy이다.  
<br>

> Apache Ivy : apache Ant 프로젝트의 서브 프로젝트,  
> Ivy is a tool for managing(recording, tracking, resolving and reporting) project dependencies.

<br>
ivy를 사용함으로써 maven에서와 같이 **dependency를 쉽게 관리할 수 있다.**

<br>
_ps. maven과 ivy 모두 nexus repository에서 artifact index 등을 이용하여 dependency를 다운로드하는 방식이다. 사내에 proxy로(?) nexus repository가 존재한다!_  

_ps. Make(스크립트:빌드 개념을 확립) - Ant(XML:범용성을 높임, 크로스 플랫폼 대응) - Maven(XML:빌드 스크립트 작성 효율을 높임, 규칙 기반) - Gradle(스크립트:유연성을 눂임, 스크립트 언어로 회귀)_

<br>
참고 : https://ko.wikipedia.org/wiki/아파치_앤트, https://ko.wikipedia.org/wiki/메이븐  
참고 : https://jj-one-a-week.blogspot.com/2017/05/ant-maven-gradle.html  
참고 : http://sjava.net/tag/apache-ivy/  