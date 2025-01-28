# 14장: Spring Data

**스프링 데이터는 왜 필요한가?** \
스프링 앱 로직에서 영속성 계층과 연결될 때, jdbc로 관계형 dbms와 연결할수도 있고 nosql 구현체와 연결하기 위해서 다른 라이브러리를 선택해야 할 수도 있다. 그러나 스프링데이터를 사용하면 이 모든 것을 배워야 할 필요가 없다.

spring data는 Consistency layer의 구현을 다음과 같이 단순화 한다.

1. 다양한 영속성 기술에 대해 공통된 추상화 집합을 제공한다.
2. 사용자가 추상화만으로 영속성 작업을 구현할 수 있도록 지원한다. 작성할 코드가 줄어들고 앱 기능을 더 빠르게 구현할 수 있다.

스프링 데이터는 각각의 기술에 맞는 다양한 모듈을 제공하고 의존성으로 추가할 수 있다. 애플리케이션이 사용하는 영속성 기술에 관계없이, Spring Data는 애플리케이션의 영속성 기능을 정의하기 위해 확장할 수 있는 공통 인터페이스(계약)를 제공한다. ex) Repository -> CrudRepository -> PagingAndSortingRepository

**왜 모든 작업을 포함하는 하나의 인터페이스를 제공하지 않는 걸까?** \
여러 개의 계약을 구현하도록 하고, 하나의 "두꺼운(fat)" 계약을 제공하는 대신, Spring Data는 애플리케이션이 필요한 작업만 구현할 수 있도록 한다. 이를 \*\*인터페이스 분리 원칙(Interface Segregation Principle)\*\*이라고 부른다.

일부 Spring Data 모듈은 자신이 대표하는 기술에 특화된 계약을 제공한다. \
예를 들어, Spring Data JPA를 사용할 경우, JpaRepository 인터페이스를 직접 확장할 수 있다.

스프링 데이터 모듈 전체 목록은 [https://spring.io/projects/spring-data](https://spring.io/projects/spring-data)를 보자

모델에서 Primary key위에 @ID 주석을 넣는다. \
인터페이스에 함수를 정의하고 위에 @Query 주석으로 SQL문을 작성한다. 데이터를 변경하는 함수라면 @Modifiying을 추가해준다.

이렇게 생성한 인터페이스를 DI하려면 빈으로 만들어야 한다. \
하지만 스프링이 자동으로 동적 구현체를 만들어서 컨텍스트에 빈으로 넣어준다.
