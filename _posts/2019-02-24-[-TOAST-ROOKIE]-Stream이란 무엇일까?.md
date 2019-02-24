메일 하나 쓰기도 벅찼던 서비스는 어느새 다른 TF와 메일을 주고 받는 기능을 구현 해야하는 단계에 이르렀다.  
이번에 나는 다른 서비스에게 메일을 쓰고 받을 수 있는 기능 구현을 맡았다.

메일을 쓸 때 받는 사람들의 도메인이 location에 존재하는지 파악하고 메일을 보낸다.

하지만 한번 메일을 보낼 때 최대 40명에게까지 동보 메일이 가능하기에 모든 사람의 도메인을 location에서 확인하는 것은 별로 좋은 방법이 아니라고 생각했다.  
그래서 메일 한 통에 받는 사람들 중 도메인이 같은 사람들을 묶고 도메인 별로 한번씩만 location에서 확인하는 형식으로 구현했다.

구현은 했지만....도메인별로 유저를 모으고 도메인을 확인하는 과정에서 **거의 대부분 for문을 사용했다.**  
for문을 좀 더 나이스(?)하게 바꿀 수 있는 방법이 없을까? 하는 고민이 생겼다.

그때 마침 같은 TF 팀원 중 한 명이 Stream을 사용해서 리팩토링 했던 코드가 떠올랐다.  
(이때 이전 코드도 아마 내가 수많은 for문으로 작성했던 코드였던 걸로 기억한다.)

또 Java 8의 기능들, Stream과 lambda 등을 활용하라 했던 조언도 생각났다.

그래서 코드를 Stream으로 리팩토링 해보자고 생각했다.

하지만 Java를 잘 몰라서 리팩토링 하기 전에 먼저 Stream의 개념에 대해 알아봐야했다.

# 스트림 Stream이란?

---

> 스트림은 '데이터의 흐름’입니다. 배열 또는 컬렉션 인스턴스에 함수 여러 개를 조합해서 원하는 결과를 필터링하고 가공된 결과를 얻을 수 있습니다. 또한 람다를 이용해서 코드의 양을 줄이고 간결하게 표현할 수 있습니다. 즉, 배열과 컬렉션을 함수형으로 처리할 수 있습니다. (1)

> Stream은 자바 8부터 추가된 기능으로 "컬렉션, 배열등의 저장 요소를 하나씩 참조하며 함수형 인터페이스(람다식)를 적용하며 반복적으로 처리할 수 있도록 해주는 기능"이다. (2)

기존에 for 문 안에서  if문 등 모두 구현하여 처리하던 작업을 이미 존재하는 함수를 사용하여 편하게 데이터를 가공할 수 있다는 뜻으로 이해했다.  
사실 Stream을 사용하기 전에 글을 읽을 때는 이해하기가 어려웠다.

그렇다면 Stream과 같이 언급되는 lambda는 무엇일까?

<br>

## 람다 lambda

---

> 람다 표현식(lambda expression)이란?
> 람다 표현식(lambda expression)이란 간단히 말해 메소드를 하나의 식으로 표현한 것입니다. (3)

> 자바에서는 클래스의 선언과 동시에 객체를 생성하므로, 단 하나의 객체만을 생성할 수 있는 클래스를 익명 클래스라고 합니다.
> 따라서 자바에서 람다 표현식은 익명 클래스와 같다고 할 수 있습니다. (4)

아 토비의 스프링 책에서 봤던 익명 클래스와 람다가 비슷하구나 하고 생각했다.  
또 작업 흐름에서 필요할 때 익명 내부 클래스를 이용하던 템플릿-콜백 패턴이 생각났다.

Stream에서 각 요소들에 대해 적절한 구현을 적용하고 싶을 때 람다를 이용할 수 있는 것 같다.

람다는 (매개변수) -> { 함수 몸체 } 형식으로 구현한다.

<br>

## Stream의 기본 흐름

---

<br>

Stream은 **생성, 가공, 결과 만들기** 세 단계로 이뤄져있다.

Stream에 대한 글들은 이미 많이 존재하기 때문에  나는 내가 적용했던 기능들에 대해 설명하고자 한다.   
당장 편하게 적용할 수 있었던(?) 함수들이었다.
<br>


        Map<String, List<User>> validServiceReceiverMap = getMailServiceList(writeData.getReceiverList());
        validServiceReceiverMap = createOtherServiceMap(validServiceReceiverMap)

        OtherServiceMail otherMail = mailSender.getOtherServiceMail(sender, writeData);

        List<String> otherWasList = validServiceReceiverMap.keySet()
                                                    .stream()
                                                    .collect(Collectors.toList());

        List<User> validReceiverList = validServiceReceiverMap.values()
                                                .stream()
                                                .flatMap(list -> list.stream())
                                                .collect(Collectors.toList());



<br>
위의 코드는 실제로 for문을 Stream으로 변경한 코드이다. 

기존 for문은 저장해놓지 않아서 해당 자료가 없지만 도메인 별로 유저 리스트가 존재하는 validServiceReceiverMap에서 도메인 리스트와 유저 리스트를 분리하는 작업이다.  
원래였다면 2차원 Map를 for문으로 돌면서 도메인 리스트를 추가하고 또 다시 유저 리스트를 돌면서 유저 리스트를 추가하는 형식이다.

<br>

**이 과정을 2단계로 나눠서 Stream으로 변경했다.**

**1. Map에서 ketSet(도메인 요소들)을 stream으로 만든 후 (생성) List 형식으로 변환(결과 만들기).**  
**2. Map에서 values(유저 리스트 요소들)을 stream으로 만든 후 (생성) 각 리스트 요소들을 하나로 모으고 (가공) List 형식으로 변환 (결과 만들기).**

<br>

코드를 정말 간결하게 바꿀 수 있었다.

.stream()을 통해서 각 자료형들을 Stream으로 생성하고 .flatMap()를 통해 각 요소들을 하나로 모았다.  
그리고 .collect()를 통해 최종 결과를 List로 만들었다.

**flatMap 함수와 같이 가공에 사용되는 함수들은 하나의 스트림에서 .을 통해 여러 번 사용**할 수 있다.  
다만 **collect()는 최종 작업이어서 스트림의 마지막에** 사용해야 한다.  
collect를 사용한 후에 다시 .을 이용하여 함수를 사용하려해도 할 수 없다.

또 많이 헷갈렸던 부분인데 .forEach() 함수도 최종 작업 함수여서 스트림의 마지막 단계에서 적용할 수 있다.  
만약 중간 단계에서 forEach를 적용하고 싶다면 .peek() 함수를 사용하면 된다!

<br>

## 소감

---

기능 구현 코드를 작성하는 것보다 Stream을 적용하는 데 시간이 더 오래 걸렸다.  
아무래도 기본 개념도 몰랐어서 동작을 이해하는 게 힘들었다. 물론 아직도 제대로 이해하지 못했다.

사실 Stream으로 변경하고 싶은 부분이 하나 더 있는데 복잡해서 아직 해결하지 못했다.  
위에 언급했던 Map에서 유저별 리스트에 if문으로 filtering 하여 각각 다른 로직을 적용하는 부분인데...  
좀 더 고민을 해봐야할 것 같다.

그래도 Stream을 사용하면 정말 코드도 간결해지고 구현이 간편해진다는 것을 느꼈다.  
더 많이 사용해보고 익숙해질 수 있으면 좋겠다.

<br>
<br>

(1) 출처 : https://futurecreator.github.io/2018/08/26/java-8-streams/

(2) 출처 : https://jeong-pro.tistory.com/165

(3, 4) 출처 : http://tcpschool.com/java/java_lambda_concept
