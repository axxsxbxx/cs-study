# HTTP 1주차 공식문제

> 기한은 04월 13일 화요일 자정까지!
>
> 일단 보지 않고 생각나는 대로 적어보고, 강의자료를 보면서 부족한 부분을 메꾸는 방식으로 작성하면 좋다.

<br>

### 1. 인터넷 네트워크

- IP 만으로는 한계를 가지는데, 어떤 한계를 가지는가?

  > IP는 패킷을 받을 서버가 패킷을 받을 수 있는 상태인지 아닌지 모르는 `비연결성`, 패킷이 소실 되거나 패킷이 전달되는 순서를 보장할 수 없는 `비신뢰성`이라는 한계점을 가진다. 그리고 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 여러개여서 일어나는 문제를 해결할 수 없다. 이러한 한계를 극복하기 위해서 TCP를 추가적으로 사용한다.

- TCP의 3-way handshake란 무엇인가? 그리고 3-way handshake를 통해서 IP의 어떤 한계점을 극복할 수 있는가?

  > 3-way handshake는 세 번의 과정을 거친 후에 데이터를 전송하는 방식이다. 처음에 클라이언트에서 서버로 SYN(접속요청)을 보내면, 해당 서버는 이를 ACK(요청 수락)하고 다시 클라이언트에게 SYN을 한다. 그 후 클라이언트 서버의 요청을 ACK하면 이때서야 비로소 데이터를 전송한다. 최근에는 마지막 단계인 클라이언트의 ACK 과정에서 데이터 전송을 함께 하기도 한다.
  >
  > 이를 사용하면 서버와 클라이언트가 서로 연결되어 있는지 확인한 후에 전송하므로, `비연결성` 을 극복해서 데이터 전달을 보증한다.

<br>

### 2. URL 요청 흐름

- URI는 무엇인가? URL과는 어떤 차이점이 있는가?

  > URI는 Uniform Resource Identifier로, 리소스를 통일된 방식으로 구분하는 식별자이다. URL는 URI의 분류 방법 중 하나이다. URL는 Uniform Resource Locator로, 리소스가 있는 위치를 지정함으로써 식별한다. 분류 방법 중에는 URN도 존재하지만 거의 사용하지 않기 때문에, 일반적으로 URI와 URL을 거의 비슷한 개념으로 받아들인다.

- `https://www.google.com:443/search?q=hello&hl=ko`을 검색했을 때, 웹 브라우저의 요청 흐름에 대해서 설명하라.
  
  - 이 부분은 외워서 푼다기 보다는, 강의자료를 보면서 따라 치더라도 큰 흐름을 다시 한번 상기시키는 목적이 크다.
  
  > 1. 웹 브라우저가 DNS 서버에서 `www.google.com` 의 IP 정보를 알아낸다.
  > 2. HTTP 요청 메시지를 생성한다.
  > 3. 요청 메시지를 SOCKET 라이브러리를 통해서 TCP/IP 계층으로 전달한다.
  > 4. IP와 PORT 정보를 활용해서 서버가 연결되어 있는지 확인한다.
  > 5. 이후 TCP/IP 계층을 거치면서 TCP/IP 패킷으로 메시지를 감싼다.
  > 6. 요청 패킷을 서버로 전송한다(인터넷 망의 여러 노드들을 거쳐서 전달된다.)
  > 7. 요청 패킷이 도착하면 TCP/IP 패킷을 없애고 메시지만 전달받는다.
  > 8. 요청 메시지를 기반으로 요청에 대한 결과 데이터를 포함한 응답 메시지를 생성한다.
  > 9. 요청 메시지를 보낸 과정과 동일한 과정을 거쳐서 응답 패킷을 생성하고 서버로 응답 메시지를 보낸다.
  > 10. 브라우저는 응답 패킷을 받으면 패킷을 없애고 담겨 있는 응답 데이터를 기반으로 HTML을 랜더링하여 사용자에게 보여준다.

<br>

### 3. HTTP 기본

- Stateless에 대해 Stateful과의 차이점을 위주로 설명하라. (이론적으로 설명해도 좋고, 예시를 들어 설명해도 좋다.)

  > Stateful은 정해진 서버를 계속 호출해야 하고, Stateless는 아무 서버나 호출해도 된다는 차이점이 있다. 그래서 서버에 장애가 생긴다면 Stateful은 문제가 발생하겠지만 Stateless는 해당 서버가 아닌 다른 서버로 연결하면 되기 때문에 큰 문제가 되지 않는다.
  >
  > 상품 구매 과정을 예로 들면, Stateful은 특정 점원과 이야기하면서 구매 품목과 가격을 정했기 때문에 점원이 변경되면 그 과정을 처음부터 다시 해야한다고 볼 수 있다. Stateless는 고객이 품목과 가격을 정해와서 정보를 다 가지고 있기 때문에 어떤 점원이 배정되어도 상관 없는 것이다.

- 비연결성에 대해 간단히 설명하고, 위에 나왔던 Stateless와 비연결성이 통신 시 어떤 장점을 가져다 주는지도 설명하라.

  > 비연결성이란 할 일을 다 한 후에 TCP/IP 연결을 종료시키는 것을 의미한다. 따라서 최소한의 자원만 유지하기 때문에 서버 자원을 효율적으로 사용할 수 있다.

<br>

### 4. HTTP 메서드

- PUT과 POST의 차이점은 무엇인가?

  > PUT은 클라이언트가 리소스를 식별한다. 클라이언트가 리소스의 위치를 알고 URI를 지정하는 것이다. POST는 이와는 달리 서버가 리소스의 위치를 지정하기 때문에 클라이언트는 리소스의 위치를 알지 못한다.

- 멱등이라는 개념에 대해 간략히 적어보고, 어떤 메서드가 멱등에 해당하는지 그 이유와 함께 간단하게 적어보아라.

  > 멱등은 몇 번을 호출해도 결과가 동일한 것을 의미한다.
  >
  > - GET : 몇 번을 조회해도 같은 결과가 조회된다.
  > - PUT : 결과를 대체하는 것이기 때문에, 여러번 해도 같은 결과가 나온다.
  > - DELETE : 계속 삭제를 요청해도, 삭제된 결과는 같다.

