---
description: 2장의 주요 내용을 요약한다.
---

# 2장: Spring Context와 Bean

**Spring Context란?**

스프링 프레임워크가 관리해야 하는 객체들이  있는 공간



**Bean이란?**

스프링 프레임워크에서 관리해야하는 객체 인스턴스들



**Context에 Bean을 추가하기**

3가지 방법이 존재한다.

1. @Bean annotation 사용하기

* @Configuration annotation을 사용해서 구성클래스를 만든다.
* 해당클래스에 원하는 method를 모두 집어넣는다.&#x20;
* Application Context에 만든구성클래스를 집어넣는다.&#x20;

메소드들의 이름 중복 가능(@Bean의 name으로 구별 or @Primary)

2. stereotype annotation 사용하기

수많은 stereotype annotation이 있지만 대표적으로 @Component가 있다.\
@ComponentScan(basePackages="route")을 통해서 Spring framework에게 context에 넣으라고 알려준다.

그럼 @Bean 을 써야 하냐 아니면 stereotype annotation을 사용해야 하냐?\
각각의 장단점이 존재한다.

@Bean을 사용하면?

인스턴스의 생성과 구성 모두 개발자에게 달렸다.\
한번에동일 타입의 인스턴스를 여러개 생성가능하다.\
사용자 정의 클래스 뿐아니라 기본 클래스(String, Integer)도 가능\
코드가 길어진다.

Stereotype Annotation을 사용하면?

Spring이 인스턴스를 생성한 후에야 개발자가 해당 인스턴스를 제어가능\
한번에중복 타입은 컨텍스트에 추가 불능 \
해당 어플리케이션의 타입만 가능\
코드가 보다 간결하다

3. Programmatically

특정 조건에서 몇몇의 메소드들을 컨텍스트에 추가하고 싶을때 보통 사용한다.

ApplicationContext의 method중 하나인 registerBean()을 사용한다. registerBean에는 4개의 parameter가 있다.

1. beanName
2. beanClass
3. supplier
4. BeanDefinitionCustomizer

두번째 parameter까지는 이해할 수 있지만 나머지 2개는 뭘까? 세번째 매개변수는 Supplier 인터페이스의 인스턴스를 받는다. Supplier는 매개변수를 받지 않고 값을 반환하는 함수이다.

supplier 기능을 통해서 인스터스를 동적으로 생성가능하다. 즉 lazy loading을 구현한다. 네번째 매개변수는 bean과 관련된 속성들을 정의한다. 기본 bean, 우선순위 등등.

