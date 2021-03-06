# HTTP 2주차 공식문제

> 기한은 04월 20일 화요일 자정까지!
>
> 일단 보지 않고 생각나는 대로 적어보고, 강의자료를 보면서 부족한 부분을 메꾸는 방식으로 작성하면 좋다.

<br>

### 1. HTTP 메서드 활용

	1. 클라이언트에서 서버로 데이터 전송을 하는 4가지 상황에 대해서 설명하고, 각각의 상황이 일어나는 대표 예시도 함께 설명하라.

- `정적 데이터 조회` : 쿼리 파라미터를 사용하지 않고, 단순히 GET 요청을 통해서 이미지나 정적 텍스트 문서를 조회하는 것을 말한다. 일반적으로 첫 메인 페이지(index화면)을 받아올 때 많이 사용한다.
- `동적 데이터 조회`: 쿼리 파라미터를 사용하여 원하는 조건에 해당하는 결과 문서를 조회하는 것을 말한다. 이 역시 GET 요청을 사용하고, url의 뒤에 쿼리스트링을 붙여서 원하는 조건을 파라미터로 보낸다. 주로 검색이나 정렬할 때 사용한다.
- `HTML FORM 데이터 전송` : HTML FORM은 GET과 POST만 지원하므로, 이 두 메서드를 이용한다. POST로 전송할 때는 GET과 달리, HTTP BODY에 메시지를 감추어서 전송한다. GET으로 전송할 때는 쿼리 스트링 형태로, url에 드러나게 메시지를 전송한다. 단순 텍스트 뿐만 아니라 이미지, 영상 등의 복합적인 파일들을 전송할 때는 multipart/form-data 라는 encoding type을 이용해서 보내야한다.
- `HTTP API 데이터 전송` : 보통 서버와 서버가 통신할 때 사용하며, 현재 REST API가 주를 이룬다. POST, PUT, PATCH 등의 다양한 HTTP 메서드를 지원하며, json 타입을 이용해서 주로 파일을 주고 받는다.

<br>

### 2. HTTP 상태코드

	1. 영구 리다이렉션이 무엇인지 간단히 설명하고, 영구 리다이렉션 코드인 301, 308의 차이점에 대해서 설명하라
	2. 1xx ~ 5xx 의 상태코드가 각각 어떤 처리 상태를 의미하는지 서술하고, 하나의 상태코드에 대해 아는만큼 설명하라 (ex. 401 Unauthorized 상태 코드는 언제 발생하고, 문제를 해결하는 방법은 무엇인지 등)
	3. 일시적 리다이렉션의 예시 중 PRG란 무엇인가? 왜 PRG를 사용하는가?

- `영구 리다이렉션`이란 특정 URI가 영구적으로 변경되었을 때 사용하는 방법이다. 301은 POST로 요청했을 경우, 리다이렉트 되면서 GET으로 바뀌며 본문이 제거된다. 즉 클라이언트는 본문을 다시 작성해야한다. 반면 308은 POST로 요청했을 경우, 리다이렉트 되면서 POST와 본문이 유지된다. 따라서 클라이언트는 본문을 재작성하지 않아도 된다.
- `상태코드 의미`
  - 1xx : 요청이 수신되어 처리중일 때 반환하는 상태코드, 요즘은 거의 쓰이지 않는다.
  - 2xx : 클라이언트의 요청이 정상처리 되었음을 뜻한다. 200 OK는 보통 GET 요청을 보냈을 때 정상적으로 응답하는 경우 보낸다. 201 Created는 보통 POST 요청을 보냈을 때 정상적으로 생성되었다는 뜻을 의미한다. 202 Accepted는 잘 사용하지 않는다. 204 No Content는 서버에 요청을 보냈을 때 정상적으로 수행하지만, 따로 응답을 반환하지는 않는 것을 의미한다.
  - 3xx : 클라이언트의 요청을 정상처리 하기 위해서 추가적인 조치가 필요함을 뜻한다. 301, 308은 영구 리다이렉션으로써, POST 요청을 보냈을 때 HTTP 메서드와 본문 내용이 바뀌느냐 바뀌지 않느냐의 차이점을 가진다. 302, 307, 303은 일시 리다이렉션으로써, POST 요청을 보냈을 때, HTTP 메서드와 본문 내용이 바뀌느냐 바뀌지 않느냐의 차이점을 가지고, 추가적으로 303 같은 경우는 무조건 GET으로 바뀐다.
  - 4xx : 클라이언트의 잘못된 요청에 의한 오류이다. 400, 404의 경우는 클라이언트의 잘못된 요청 혹은, 요청에 의한 리소스를 찾을 수 없는 경우 사용한다. 401, 403은 인증/인가에 대한 상태코드인데, 401은 인증에 관한 상태이고 403은 인가에 관한 상태코드이다.
  - 5xx : 서버에 문제가 발생했을 때 반환하는 오류이다. 500, 503이 있으며 어떤 오류가 났을 때 상태코드를 4xx로 할 것인지, 5xx로 할 것인지 결정하는 문제는 정말 중요하다.
- `PRG`란 `POST REDIRECT GET`을 의미하며, POST 요청을 정상 수행한 후, GET 요청으로 리다이렉션 되는 것을 의미한다. 예를 들어 어떤 물건에 대해서 주문을 했을 때, POST 요청을 통해 하게 되는데 만약 주문이 완료된 후 새로고침을 하게 되면 POST 요청이 다시 발생하여 중복 주문이 발생할 수 있다. 따라서 PRG를 이용해서 POST 요청으로 주문 완료가 되면, 주문 결과 페이지를 GET으로 리다이렉트하여 사용자가 새로고침을 아무리 누르더라도, POST가 재요청되지 않고 GET이 재요청되게 하여 중복 주문을 예방할 수 있다.

<br>

### 3. HTTP 헤더1 - 일반헤더

	1. 쿠키란 무엇인가? 쿠키를 사용하는 경우, 사용하지 않았을 때에 비해 어떤 장점을 가지고 있는가? 쿠키의 생명 주기란 무엇인가?
	2. HTTP 헤더 중 Host는 꼭 들어가야하는 필수 정보이다. Host란 어떤 정보이며 왜 필수 정보인지 예시를 들어 설명하라.

- HTTP의 Stateless한 특성에 의해, 클라이언트와 서버는 요청/응답을 주고 받으면 연결이 끊어지므로 서버는 클라이언트의 이전 상태에 대해 기억하지 못한다. (비 연결성) 하지만 웹사이트의 특성 상, 클라이언트의 상태를 유지해야하는 경우가 있는데, 이를 위해 쿠키를 사용한다. 상태 유지가 되어야 하는 정보에 대해 서버에서 쿠키를 만들어서 클라이언트에게 응답하면, 클라이언트는 해당 쿠키 정보를 요청 시 계속 보낸다. 서버는 쿠키 정보를 바탕으로, 사용자의 상태를 유지할 수 있다. 이러한 쿠키에는 생명 주기가 있는데, 바로 쿠키가 얼마나 유지되는지 그 시간에 대한 것이다. 보통 max-age 라는 필드를 이용해 만들게 되는데, 지정한 시간이 지나면 쿠키는 자동으로 지워진다.
- 가상호스팅에 의해서 하나의 서버는 여러개의 도메인을 거느릴 수 있다. (a라는 서버가 aaa.com, bbb.com, ccc.com 이라는 여러 도메인 보유 가능) 따라서, 클라이언트에서 이 서버에 요청을 보내면 과연 여러 도메인 중에 어떤 도메인에 요청을 보내는 것인지 기존의 방법으로는 알 수가 없다. Host 라는 필드를 헤더에 넣어주면, 이를 해결할 수 있는데, 원하는 도메인 정보를 Host 필드에 넣게 되면 서버는 클라이언트가 해당 도메인에 요청을 보내는 것이라고 구별할 수 있게 된다. 그래서 이 Host 정보는 반드시 들어가야하는 필수 정보이다.

<br>

### 4. HTTP 헤더2 - 캐시와 조건부 요청

	1. 확실한 캐시 무효화를 하기 위해 사용하는 지시어 중에는 `no-cache` 와 `must-revalidate`가 있다. 이 둘의 차이점에 기반하여 각각의 기본 동작에 대해서 설명하라.
	2. 캐시 검증 헤더인 Last-Modified와 ETag의 차이를 설명하고, 브라우저를 통해 서버에 요청 시 각각 어떤 조건부 요청 헤더가 사용되는지 설명하라.
	3. 서버에서 기존 데이터를 변경하지 않은 경우, 클라이언트는 캐시를 재사용할 수 있는데, 이를 위해서는 검증 헤더와 조건부 요청이 필요하다. 처음에 캐시가 만들어지는 과정 부터, 검증 헤더와 조건부 요청을 통해서 클라이언트가 캐시를 재사용하는 과정까지 상세하게 작성하라.
- no-cache는 클라이언트가 데이터를 캐시해도 되지만, 항상 ORIGIN 서버에 검증받고 캐시하는 것을 의미한다. must-revalidate는 캐시 만료 후 최초 조회 시 반드시 ORIGIN 서버에 검증을 해야하는 것을 의미한다. 이 둘은 다음과 같은 큰 차이점을 가진다. 먼저 no-cache는 웹 브라우저가 프록시 캐시서버에 데이터를 요청할 경우, 프록시 서버는 ORIGIN 서버에 데이터를 요청하는데, 만약 순간적으로 프록시서버와 ORIGIN 서버가 단절되면, 프록시 서버는 예전 캐시 데이터라도 반환하려고 한다. 즉 Error를 발생시키는 것 보다는 예전의 데이터라도 클라이언트에게 제공하려고 하는 것이다. 반면 must-revalidate는 반드시 ORIGIN에 검증을 받아야하기 때문에, 프록시 서버와 ORIGIN 서버가 단절되면, Error를 반환한다. 예전 데이터를 제공하지 않는 것이다. 이는 돈이나 통장과 같은 중요한 데이터인 경우에 적용해야한다.
- Last-Modified와 ETag의 가장 큰 차이점은, 캐시의 변화를 어떻게 파악하냐는 것이다. Last-Modified는 날짜와 시간값을 이용해서, 클라이언트 캐시와 서버의 데이터의 수정 날짜/시간이 서로 다르면 수정되었다고 판단하여 캐시를 재 요청한다. 하지만 이는 실제 데이터가 변하지 않더라도 단순히 저장만 새로 했을 경우에도 날짜/시간은 갱신되기 때문에, 데이터가 동일함에도 불구하고 서버에 데이터를 재 요청한다는 단점이 생긴다. 따라서 ETag는 파일의 해시값을 이용해서, 데이터의 내용이 동일하다면 같은 해시값을 지니므로, 단순히 저장만 새로 했을 경우에도 서버에 데이터를 요청하지 않는다. Last-Modified의 경우에는 If-Modified-Since라는 조건부 요청을 사용하고, ETag는 If-None-Match를 사용한다.
- 클라이언트는 서버에 원하는 데이터를 요청한다. 서버는 Last-Modified 혹은 ETag로 검증헤더를 응답에 담아서 클라이언트에게 보낸다. 클라이언트는 해당 데이터와 검증헤더를 캐시로 저장한다. 그러면 클라이언트의 로컬에는 캐시가 저장되는데, 일정 시간이 지나서 캐시의 유효시간이 초과되었다고 가정해보자. 그럼 클라이언트는 서버에 데이터를 재요청해야 하는데, 이때 검증헤더에 있었던 데이터를 조건부 요청으로 서버에 보낸다. 서버에서는 날짜/시간 혹은 해시값을 이용해서 클라이언트가 보낸 조건부 요청이 True인지 False인지 검증한다. 즉 서버의 데이터와 클라이언트의 캐시가 다른가?를 판단한다. 만약 다르지 않다면 304 상태코드를 헤더에 넣고, 본문없이 보내면 클라이언트는 기존 캐시 값을 그대로 사용한다. 다르다면 200 상태코드와 함께 수정된 데이터를 같이 보내면된다.