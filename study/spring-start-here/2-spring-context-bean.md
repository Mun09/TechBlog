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

