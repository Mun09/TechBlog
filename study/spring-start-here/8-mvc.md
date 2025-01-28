# 8장: 스프링부트와 MVC로 웹 앱 만들기

이렇게오늘날 현대 웹 앱에서는 정적인 페이지를 거의 찾아보기 어렵다. "HTTP 응답을 브라우저에 전달하기 전에 페이지에 어떤 내용을 추가할지 결정할 방법이 있어야 하지 않을까?" 라는 의문이 든다. \
이번 장에서는 현대 웹 앱의 기능들을 구현해보록 한다.

예를 들어서 쇼핑몰 홈페이지에서는 사용자마다 카트에 담은 상품이 다를 것이다. \
즉 동일한 페이지에 대한 요청이 서로 다른 데이터를 응답으로 받는 것이다. 따라서 페이지가 동적 컨텐츠를 가지고 있다.

**서버->클라이언트 데이터 전송**&#x20;

thymeleaf 의존성을 추가하자. \
thymeleaf 탬플릿 엔진은 쉽게 컨트롤러에서 뷰로 쉽게 데이터를 보낼 수 있다. \
addAttribute()함수를 이용해서 데이터를 뷰로 보내고 마지막에 정적 파일을 반환한다. \
정적파일은 이제 resources/static이 아니라 resources/templates에 보관한다. \
html파일의 태그에는 th 접두사를 사용해서 데이터를 활용한다. \
mvc 흐름에서는 컨트롤러가 데이터를 view resolver에게 뷰이름과 함께 전달해준다.

클라이언트->서버 데이터 전송 mvc 흐름에서는 Dispatcher servlet이 컨트롤러에게 데이터를 전달하고 컨트롤러가 view resolver에게 전달한다.

1. key-value 쌍 \
   ex) URI에 쿼리 문자열로 추가하기 \
   적은 양의 데이터를 전송할 때 적합.
2. http 헤더 \
   uri에 나타나지 않지만 대용량의 데이터 전송에는 부적합.
3. 경로 변수 \
   요청 경로에 데이터를 포함하여서 전송한다. \
   필수적인 데이터를 전소할 때 사용. \
   ex) GET /products/{productId}
4. http 요청 본문 \
   대량의 데이터 전송에 적합. \
   문자열 또는 바이너리 데이터로 전송. \
   rest api 구현 시에 많이 사용한다. \
   ex) { "username": "jaeyoung", "password": "securepassword123" }\
   \
   2014년 이후 get method도 Body request가 가능해졌다.

**request parameter를 이용해서 데이터 보내기** \
@RequestParam을 사용한다.

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption><p>이렇게 하면</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption><p> 이렇게 사용</p></figcaption></figure>

**경로 변수 사용하기**\
경로 변수를 사용한다는 것은 다음과 같이 변한다는 것이다. \
/home?color=blue -> /home/blue \
이런 특성 때문에 경로변수는 두 개 이상의 값이라면 경로를 읽기 어려워지므로 피해야 한다.\
선택적인 값도 피해야 한다. 선택해야 한다면 요청 파라미터를 이용하자.

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

**body request 사용하기**\
보통 post요청으로 Body request가 들어오고 @RequestBody 로 값을 가져올 수 있다. \
아래 예시처럼 PaymentDetails같은 DTO를 사용해도 스프링이 자동으로 객체를 정의해준다.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>



이때까지 HTTP GET method를 사용했다. \
하지만 클라이언트의 의도에 따라서 method의 종류는 달라진다. \
GET: 클라이언트의 요청이 오직 데이터 요구 \
POST: 클라이언트의 요청이 서버에 추가될 데이터를 전송 \
PUT: 클라이언트의 요청이 서버의 데이터를 변경 \
PATCH: 클라이언트의 요청이 부분적으로 서버의 데이터를 변경 \
DELETE: 클라이언트의 요청이 서버의 데이터를 삭제\
\
단앱 디자인 목적에 반하는 HTTP 메서드를 쓰면 안된다. \
EX) GET 메서드를 써놓고 데이터를 변경하는 기능성을 추가하기

