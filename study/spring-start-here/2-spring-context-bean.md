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

2. Pragrammatically
