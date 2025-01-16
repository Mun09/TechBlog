---
description: 7장 내용 요약
---

# 7장: 스프링부트와 MVC

**서블릿 컨테이너란?**&#x20;

서블릿 컨테이너는 http 메시지를 Java 어플리케이션이 이해할 수 있는 방식으로 변환하는 역할을 한다. 이를 통해서 Java 애플리케이션이 통신계층을 직접 구현하지 않아도 된다. \
ex) tomcat, jetty, jboss, payara, etc

**서블릿이란?**&#x20;

서블릿은 서블릿 컨테이너와 직접 상호작용하는 자바 객체다.

즉 서블릿 컨테이너는 다음의 것들을 처리하게 된다.

1. 클라이언트의 http 요청 수신.
2. 요청을 적절한 서블릿으로 전달.
3. 서블릿의 응답 데이터를 클라이언트에게 전송.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption><p>figure 7.6</p></figcaption></figure>



