# 6장: 스프링 AOP

DI외 스프링이 지원하는 강력한 기능 중 Asepct가 있다. aspect는 프레임워크가 메서드 호출을 가로채고, 메서드 실행을 변경하는 방법이다. 이를 통해 실행중이 메서드의 일부 로직을 추출할 수 있다. Aspect는 신중히 사용치 않으면 앱의 유지보수가 어려워질 수 있다. 이런 접근 방식으로 Aspect-Oriented Programming(AOP)라고 한다.

Aspect를 배우는 이유는 AOP를 통해서 스프링이 제공하는 트랙젝션 처리나 보안설정 같은 중요한 기능을 이용할 수 있기 때문이다. 그 이전에 Aspect에 관해서 알아야 한다.

aspect는 프레임워크가 특정 메소드를 호출할 때 실행하는 로직의 일부이다. aspect를 설계 시 다음의 항목을 정의한다.

1. **어떤 코드를 실행할까?** 이 코드가 **aspect**다.
2. **해당 로직을 언제 실행할까?** 예를 들어서 메서드 호출 전? 후? 대신에? 이를 **advice**라고 한다.
3. **어떤 메소드**에 프레임워크가 aspect를 적용해야 하나? 이를 **pointcut**이라고 한다.

**join point**란? aspect가 실행되는 이벤트.. 스프링에서는 이 이벤트는 항상 메서드 호출이다.

**aspect target**이 되려면 우선 스프링에서 관리해야 한다. 따라서 bean이어야 한다.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption><p>figure 6.2</p></figcaption></figure>

객체가 aspect target으로 지정된다면 getBean() 메소드나 DI를 통해서 빈이 사용될때마다 spring은 실제 메서드 대신 aspect 로직을 호출하는 객체를 반환한다. \
이런 객체를 **프록시 객체**라고 한다. 또한 이런 접근 방식을 **Weaving**이라고 한다.
