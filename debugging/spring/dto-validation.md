# DTO Validation

1. 외부에서 내부로 전달
2. 내부에서 계층간 전달
3. 내부에서 외부로 전달

전달되는 DTO가 유효한 형식을 갖추고 있는지를 확인해야 할 필요가 있다. DTO를 사용할 때의 이점 중 하나가 이런 유효성 체크 책임을 분리할 수 있다.

implementation 'org.springframework.boot:spring-boot-starter-validation' dependency를 추가해주면 유효성 검사를 할 수 있게된다. 객체에 @NotNull과 같은 제한을 가해주고 파라미터로 받기전 @Valid를 해준다. 형식이 맞지 않는다면 MethodArgumentNotValidException에러를 발생시키는데 @ExceptionHandler로 에러 처리를 해주면 올바르게 예외처리가 가능하다.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>



참고 블로그: [https://tecoble.techcourse.co.kr/post/2020-09-20-validation-in-spring-boot/](https://tecoble.techcourse.co.kr/post/2020-09-20-validation-in-spring-boot/)
