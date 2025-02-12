# 10장: REST 엔드포인트

REST 엔드포인트는 두 애플리케이션 간의 통신을 구현하는 간단한 방법이다. \
애플리케이션이 웹 프로토콜을 통해 서비스를 노출하는 방식이기 때문에 이러한  웹 서비스를엔드포인트를고 한다. \
웹 앱에 사용하는 매커니즘과 동일하지만 디스패쳐 서블릿이 뷰를 찾지 않도록 한다는 점이 유일한 차이점이다. \
즉 view resolver를 사용하지 않는다.

**rest 엔드포인트의 몇 가지** **문제점**

1. 컨트롤러 액션이 너무 오래 걸릴 경우 통신이 중단될 수 있다.
2. 한 번의 호출로 많은 데이터가 전송된다면 호출시간이 오버되어 통신이 중단될 수 있다.
3. 동시 호출이 너무 많이 들어오면 과부화로 실패할 수 있다.
4. http 호출은 네트워크로 이뤄지고 네트워크 문제로 호출이 실패할 가능성은 항상 존재한다.

이러한 원인들로 호출이 실패했을 때의 대처를 항상 고려해야 한다.

1. 호출 실패가 앱에 어떤 영향을 미칠 수 있는지 확인
2. 데이터가 영향을 받는가?
3. 데이터 불일치 문제?
4. 사용자에게 오류 표시 구현?

컨트롤러의 액션 메서드 위에 @ResponseBody 주석을 통해서 디스패처 서블릿에게 해당 액션이 뷰 대신 http 응답을 반환함을 알려준다. \
이런 간단한 전환을 통해서 MVC 컨트롤러를 REST 컨트롤러로 변경할 수 있다.



@ResponseBody를 일일이 쓰는 수고를 덜어주기 위해서 @Controller대신 @RestController를 사용하면 해당 컨트롤러의 모든 액션이 rest 엔드포인트임을 알려준다.



두가지 앱 사이에서 주고받아지는 데이터의 타입 or 객체타입을 DTO(data Transfer object)라고 한다. \
ex) Country { String name, int population } \
이때 스프링은 이 객체를 string으로 바꾸고 json으로 포멧해서 보낸다. \
여러 개의 객체들을 리스트 형식으로 보내도 상관없다. \
JSON을 사용하는 것은 rest 엔드포인트를 활용할때 가장 흔한 방식이다.



**커스텀 상태코드?** \
컨트롤러의 액션을 반환할때 ResponseEntity 클래스를 활용해서 커스텀 상태코드와 헤더를 작성할 수 있다.



**rest 엔드포인트의 예외처리?**

1. **try-catch 구문 사용**
2. **@RestControllerAdvice** 사용 \
   보통 예외 처리 코드가 반복적으로 사용되거나 책임을 분리해서 이해하기 쉽도록 만들기 위해서 이 방법을 사용한다. 특정 에러클래스가 발생할때의 advice를 정의한다.

