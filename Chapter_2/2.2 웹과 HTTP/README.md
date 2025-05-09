# 2.2 웹과 HTTP

웹은 `온디맨드(on-demand) 방식`으로 **사용자가 원할 때 원하는 것을 수신한다.**

개인은 또한, 웹 상에서 많은 정보를 얻고 상호작용할 수 있다.

<br/>
<br/>
<br/>

# 2.2.1 HTTP 개요

웹 애플리케이션 계층 프로토콜인 HTTP는 웹의 중심이다.

RFC 1945, RFC 7230, RFC 7540에 정의되어 있다.

`HTTP`는 **메시지의 구조** 및 **클라이언트와 서버가 메시지를 어떻게 교환하는지**에 대해 정의하고 있다.

자세히 설명하기 전에 웹 전문 용어들을 알아보자.

<br/>
<br/>

## 웹 페이지(web page)

> 웹 페이지들은 객체(object)로 구성된다.

객체는 단순히 단일 URL로 지정할 수 있는 하나의 파일(HTML, JPEG 이미지, 자바스크립트 등)이다.

<br/>

대부분의 웹 페이지는 기본 HTML 파일과 여러 참조 객체로 구성된다.

예를 들어, 웹 페이지가 HTML 텍스트와 5개의 JPEG 이미지로 구성되어 있으면, 이 웹페이지는 6개의 객체를 갖는다.

<br/>

기본 HTML 파일은 페이지 내부의 다른 객체를 그 객체의 `URL`로 참조한다.

URL은 객체를 갖고 있는 서버의 호스트 이름과 객체의 경로 이름을 갖고 있다.

e.g.,

```
http://www.school.edu/picture.gif 
```

- 호스트의 이름 : `www.school.edu`
- 경로 이름 : `picture.gif`

<br/>
<br/>

## 웹 브라우저와 클라이언트

`웹 브라우저(Web browser)`는 HTTP의 클라이언트 측을 구현하기 때문에, 웹의 관점에서 브라우저와 `클라이언트(client)`라는 용어를 혼용하여 사용한다.

브라우저는 요구한 웹 페이지를 보여주고 여러가지 인터넷 항해와 구성 특성을 제공한다.

<br/>

`웹 서버(Web server)`는 URL로 각각을 지정할 수 있는 웹 객체를 갖고 있다.

일반적으로, 사용자가 웹 페이지를 요청할 때  
(1) 브라우저는 페이지 내부의 객체에 대한 HTTP 요청 메세지를 서버에게 보내고,  
(2) 서버는 요청을 수신하고 객체를 포함하는 HTTP 응답 메시지로 응답한다.

<p align="center"><img width="450" alt="웹서버와 브라우저" src="https://user-images.githubusercontent.com/76640167/210488592-53e2960a-2ec3-4ecf-8408-8d51a3ebb968.png">

<br/>
<br/>
<br/>

## HTTP와 TCP

> HTTP는 `TCP`를 전송 프로토콜로 사용한다.

<br/>

1. HTTP 클라이언트는 먼저 서버에 TCP 연결을 시작한다.


2. 연결이 이루어지면, 브라우저와 서버 프로세스는 각각의 소켓 인터페이스를 통해 TCP로 접속한다.


3. 클라이언트는 `HTTP 요청 메시지`를 소켓 인터페이스로 보내고, 소켓 인터페이스로부터 `HTTP 응답 메시지`를 받는다.  
   마찬가지로, HTTP 서버는 소켓 인터페이스로부터 요청 메시지를 받고 응답 메시지를 소켓 인터페이스로 보낸다.

<br/>

이렇게 TCP를 통해 메시지를 보내면 **TCP는 신뢰적인 데이터 전송 서비스를 제공하므로**  
**모든 HTTP 요청 메시지가 궁극적으로 서버에 도착한다.** (서버에서 보낸 메시지도 마찬가지다.)

HTTP는 TCP가 어떻게 손실 데이터를 복구하고, 올바른 순서로 데이터를 배열하는지 **전혀 걱정할 필요가 없어** 계층 구조의 장점이 드러난다.

<br/>
<br/>

## 비상태(stateless) 프로토콜

특정 클라이언트가 몇 초 후에 같은 객체를 두 번 요청해도 서버는 전에 보냈다고 알려주지 않고 객체를 또 보낸다.

HTTP 서버는 클라이언트에 대한 정보를 유지하지 않으므로, `비상태(stateless) 프로토콜`이라고 부른다.

<br/>
<br/>
<br/>

# 2.2.2 비지속 연결과 지속 연결

## 비지속(non-persistent) 연결

> 💡 클라이언트-서버 상호작용이 TCP 상에서 발생할 때 **각 요구/응답 쌍이 분리된 TCP 연결**을 통해 보내지는 것을 말한다.

<br>

웹 페이지를 서버에서 클라이언트로 전송하는 단계를 살펴보자.

페이지가 **기본 HTML 파일과 10개의 이미지**로 구성되고, 이 11개의 객체가 같은 서버에 있다고 가정하자.

<br>

연결 수행 과정은 다음과 같다.

1. HTTP 클라이언트는 `HTTP 기본 포트 80`을 통해 서버로 `TCP 연결`을 시도한다. TCP 연결과 관련하여 클라이언트와 서버에 각각 소켓이 있게 된다.


2. HTTP 클라이언트는 설정된 TCP 연결 소켓을 통해 서버로 HTTP 요청 메시지를 보낸다. 이 요청에 객체 경로도 포함된다.


3. HTTP 서버는 TCP 연결 소켓을 통해 요청 메시지를 받는다. 저장 장치로부터 경로의 객체를 추출한다.  
   HTTP 응답 메시지에 그 객체를 캡슐화 하여 소켓을 통해 클라이언트로 보낸다.


4. HTTP 서버는 TCP에게 연결을 끊으라고 한다. (그러나 실제로 클라이언트가 응답 메시지를 올바로 받을 때까지 끊지 않는다.)


5. HTTP 클라이언트가 응답 메시지를 받으면, TCP 연결이 중단된다. 메시지는 캡슐화된 객체가 HTML 파일인 것을 나타낸다.  
   클라이언트는 응답 메시지로부터 파일을 추출하고 HTML 파일을 조사하여 10개의 JPEG 객체에 대한 참조를 찾는다.


6. **참조되는 JPEG 객체에 대해 1 ~ 4단계를 반복한다.**

<br/>

브라우저는 웹 페이지를 수신하면서, 사용자에게 그 페이지를 보여준다. 다른 브라우저는 웹 페이지를 각기 다른 방식으로 해석하여 보여준다.

> HTTP는 통신 프로토콜만 정의할 뿐, 웹 페이지에 대한 관심은 없다.

<br/>

사용자는 앞의 단계를 동시에 받을지 순차적으로 받을지의 동시성 정도를 조절할 수 있도록 브라우저를 구성할 수 있다.

브라우저는 **여러 개의 TCP 연결을 설정하며 다중 연결상에서 웹 페이지의 각기 다른 원하는 부분을 요청**할 수 있다.

HTTP 1.0은 비지속 연결을 지원한다.

<br/>
<br/>

클라이언트가 HTML 파일을 요청하고 그 파일이 클라이언트로 수신될 때까지의 시간을 측정해보자.

이를 위해 RTT를 알아야 하는데,  
`RTT(round-trip-time)`란 **작은 패킷이 클라이언트로부터 서버까지 가고, 다시 클라이언트로 되돌아오는 데 걸리는 시간**이다.

RTT는 패킷 전파 지연, 큐잉 지연, 처리 지연 등을 포함한다. (1장에 논의되어 있다.)

<br/>

<p align="center"><img width="600" alt="HTML 요청 시간 계산" src="https://user-images.githubusercontent.com/76640167/210491569-3638ca03-2d17-4eea-8a5f-d984bb831d00.png">

<br/>
<br/>

사용자가 하이퍼링크를 클릭하면, 브라우저와 웹 서버 사이에서 TCP 연결을 시도한다. 이는 `3-way handshake`를 포함한다.

> 즉, 클라이언트가 서버로 작은 TCP 메시지를 보내고, 서버는 작은 메시지로 응답하고, 마지막으로 클라이언트가 다시 서버에 응답한다.

<br/>

서버가 작은 메시지로 응답하면 한 RTT가 계산된다. 이때, **클라이언트는 HTTP 요청 메시지를 TCP 연결로 보내면서 세 번째 응답 부분을 함께 보낸다.**

일단, 요청 메시지가 도착하면 서버는 HTML 파일을 TCP 연결로 보내고 이 요청은 또 하나의 RTT를 필요로 한다.

> 즉, 대략 총 응답 시간은 2RTT와 HTML 파일을 서버가 전송하는 데 걸리는 시간을 더한 것이다.

<br/>
<br/>

### 비지속 연결의 단점

1. **각 요청 객체에 대한 새로운 연결이 설정되고 유지되어야 한다.**
    - TCP 버퍼가 할당되어야 하고, TCP 변수들이 클라이언트와 서버 양쪽에 유지되어야 하는데  
      이는 수많은 클라이언트들의 요청을 동시에 서비스하는 웹 서버에는 심각한 부담이다.


2. 매번 2RTT를 필요로 한다.

<br/>
<br/>
<br/>

## 지속(persistent) 연결

HTTP/1.1 지속 연결에서 서버는 응답을 보낸 후에 TCP 연결을 그대로 유지한다. (비지속 연결도 지원한다.)

**같은 클라이언트와 서버 간의 이후 요청과 응답은 같은 연결을 통해 보내진다.**

즉, 같은 서버에 있는 여러 웹 페이지들을 하나의 지속 TCP 연결을 통해 보낼 수 있다.

<br/>

이들 객체에 대한 요구는 진행 중인 요구에 대한 응답을 기다리지 않고 연속해서 만들 수 있다. (파이프라이닝, pipelining)

일반적으로 HTTP 서버는 일정 기간 사용되지 않으면 연결을 닫는다.

HTTP의 디폴트 모드는 파이프라이닝을 이용한 지속 연결을 사용한다.

<br/>
<br/>

## HTTP 메시지 포맷

RFC는 HTTP 메시지 포맷을 정의한다.

### HTTP 요청 메시지

<p align="center"><img width="600" alt="요청 메시지 포맷" src="https://user-images.githubusercontent.com/76640167/210494014-891426d1-0c27-47dc-8271-c6f5bab316e9.png"></p>

<br/>
<br/>

다음은 전형적인 HTTP 요청 메시지이다.

```
GET /somedir/page.html HTTP/1.1
Host: www.someschool.edu
Connection: close
User-agent: Mozilla/5.0
Accept-language: fr
```

<br/>

#### 특징

1. ASCII 텍스트로 쓰여 있어 사람들이 읽을 수 있다.
2. 메시지가 다섯 줄로 되어 있고, **각 줄은 CR(carriage return)과 LF(line feed)로 구별된다.** 마지막 줄에 이어서 CR과 LF가 따른다.

HTTP 요청 메시지의 첫 줄은 `요청 라인`이라 부르고, 이후의 줄들은 `헤더 라인`이라고 부른다.

<br/>

### 요청 라인

요청 라인은 3개의 필드, 즉 `방식(method)` 필드, `URL 필드`, `HTTP 버전 필드`를 갖는다.

방식 필드는 `GET`, `POST`, `HEAD`, `PUT`, `DELETE` 등의 여러 가지 값을 가질 수 있다.

<br/>
<br/>

### 헤더 라인

1. Host
    - 객체가 존재하는 호스트를 명시한다.
    - 이미 호스트까지 TCP 연결이 맺어져 있어 불필요하다고 생각될 수 있지만, 2.2.5절에서 나오는 웹 프록시 캐시에서 필요로 한다.
2. Connection : 이 헤더 라인을 포함함으로써, 브라우저는 서버에게 지속 연결 사용을 원하는지 비지속 연결 사용을 원하는지 전달한다.
3. User-agent : 서버에게 요청을 하는 브라우저 타입을 명시한다.
4. Accept-language : 헤더는 사용자가 객체의 어떤 언어 버전을 원하고 있음을 나타낸다.

<br/>
<br/>

## 개체 몸체(entity body)

GET일 때는 비어있고, `POST`일 때 사용된다.

POST 메시지로 사용자는 서버에 웹 페이지를 요청하고 있으나, 웹 페이지의 특정 내용은 사용자가 폼 필드에 무엇을 입력하는가에 달려 있다.

폼으로 생성한 요구가 반드시 POST일 필요는 없다. 대신에 흔히 요청된 URL의 입력 데이터를 전송한다.

<br/>

`HEAD` 방식은 GET과 유사하다.

**서버가 HEAD 방식을 가진 요청을 받으면 HTTP 메시지로 응답하는데, 요청 객체는 보내지 않는다.** 흔히 디버깅을 위해 사용된다.

<br/>

`PUT` 방식은 웹 서버에 업로드할 객체를 필요로 하는 애플리케이션에 의해 사용된다.

`DELETE` 방식은 사용자 또는 애플리케이션이 웹 서버에 있는 객체를 지우는 것을 허용한다.

<br/>
<br/>

---

<br/>

### HTTP 응답 메시지

<p align="center"><img width="600" alt="응답 메시지" src="https://user-images.githubusercontent.com/76640167/210497215-8265b94b-c82f-4ea8-9374-5f41fc6eaed1.png">

<br/>
<br/>

다음은 전형적인 HTTP 응답 메시지를 보여준다.

```
HTTP/1.1 200 OK
Connection: close
Date: Tue, 18 Aug 2015 15:44:04 GMT
Server: Apache/2.2.3 (CentOS)
Last-Modified: Tue, 18 Aug 2015 15:11:03 GMT
Content-Length: 6821
Content-Type: text/html
(data data data data data ...)
```

<br/>

### 상태 라인과 상태 코드

`상태 라인(status line)`은 버전 필드와 상태 코드, 해당 상태 메시지를 갖는다.

상태 코드와 메시지

- 200 OK: 요청이 성공했고, 정보가 응답으로 보내졌다.
- 301 Moved Permanently: 요청 객체가 영원히 이동되었다. 이때, 새로운 URL은 응답 메시지의 Location 헤더에 나와있다.
- 400 Bad Request : 서버가 요청을 이해할 수 없다.
- 404 Not Found : 요청한 문서가 서버에 존재하지 않는다.
- 505 HTTP Version Not Supported : 요청 HTTP 프로토콜 버전을 서버가 지원하지 않는다.

<br/>

### 헤더 라인

1. Connection : 클라이언트에게 메시지를 보낸 후 TCP 연결을 닫을지 말지 결정한다.
2. Date : HTTP 응답이 서버에 의해 생성되고 보낸 날짜와 시간을 나타낸다.
3. Server : 메시지가 어떤 웹 서버에 의해 만들어졌는지 나타낸다.
4. Last-Modified : 객체가 생성되거나 마지막으로 수정된 시간과 날짜를 나타낸다.
5. Content-Length : 송신되는 객체의 바이트 수를 나타낸다.
6. Content-Type : 개체 몸체 내부(Entity body)의 객체가 어떤 타입인지 나타낸다.

<br/>

HTTP 명세서는 많은 헤더라인을 정의하고 있고, 위는 그 중 일부다.

브라우저는 브라우저 타입과 여러 설정, 캐싱하고 있는지에 따라 헤더 라인을 동적으로 생성하고 웹 서버도 비슷하다.

<br/>
<br/>
<br/>

# 2.2.4 사용자와 서버 간의 상호 작용: 쿠키(cookie)

HTTP 서버는 상태를 유지하지 않는다.

그러나 서버가 사용자 접속을 제한하거나 사용자에 따라 콘텐츠를 제공하기 원하므로  
**사용자를 확인하는 것이 바람직할 때가 있는데, 이때 HTTP는 `쿠키(cookie)`를 사용한다.**

> over stateless HTTP, `cookie` provides a **state related service layer**

<br/>

<p align="center"><img width="700" alt="쿠키를 이용한 상태 유지" src="https://user-images.githubusercontent.com/76640167/210499304-190c6dc5-ab64-4864-9f25-bb27fc63f460.png">

<br/>
<br/>

## 쿠키 동작 과정

1. 웹 서버에 HTTP 요청 메시지를 전달한다.
2. 웹 서버는 유일한 식별 번호를 만들고 이 식별 번호로 인덱싱 되는 백엔드 데이터 베이스 안에 엔트리를 만든다.
3. **HTTP 응답 메시지에** `Set-cookie: 식별 번호`의 헤더를 포함해서 전달한다.
4. 브라우저는 헤더를 보고, 관리하는 특정한 쿠키 파일에 그 라인을 덧붙인다.
5. 다시 **동일 웹 서버에 요청을 보낼 때**  
   브라우저는 쿠키 파일을 참조하고 이 사이트에 대한 식별번호를 발췌하여 `Cookie : 식별 번호`의 헤더를 요청과 함께 보낸다.

이렇게 웹 서버는 사용자를 식별할 수 있다.

<br/>
<br/>
<br/>

# 2.2.5 웹 캐싱

> `웹 캐시(cache)`(`프록시(proxy) 서버`)는 기점 웹 서버를 대신하여 HTTP 요구를 충족시키는 개체이다.

> Goal: satisfy client request **without involving origin server**

웹 캐시는 자체의 저장 디스크를 갖고 있어, **최근 호출된 객체의 사본을 저장 및 보존한다.**

<br/>

<p align="center"><img width="450" alt="웹 캐싱" src="https://user-images.githubusercontent.com/76640167/210502046-6dbbe817-240d-401d-8ec6-f3a73210e48d.png">

<br/>
<br/>

## 프록시 서버 동작 과정

1. 브라우저는 웹 캐시와 TCP 연결을 설정하고 웹 캐시에 있는 객체에 대한 HTTP 요청을 보낸다.
2. 웹 캐시는 객체의 사본이 저장되어 있는지 확인하고, **저장되어 있다면 클라이언트 브라우저로 HTTP 응답 메시지와 함께 객체를 전송한다.**
3. **갖고 있지 않다면, 기점 서버로 TCP 연결을 설정한다.**  
   이후 웹 캐시는 캐시와 서버 간의 TCP 연결로 객체에 대한 HTTP 요청을 보낸다. 기점 서버는 웹 캐시로 HTTP 응답 메시지를 보낸다.
4. 웹 캐시의 객체를 수신할 때, 객체를 지역 저장장치에 복사하고 클라이언트 브라우저에 HTTP 응답 메시지를 보낸다. (이때, 이미 설정된 TCP를 통해 보낸다.)

<br/>

> 캐시(cache)는 요청과 응답을 모두 하는 클라이언트이면서 서버이다.

- `server` for original requesting client
- `client` to origin server

<br/>

일반적으로 웹 캐시는 ISP(university, company, residential ISP)가 구입하고 설치한다.

<br/>
<br/>

## 웹 캐싱의 사용 이유

1. **클라이언트 요구에 대한 응답 시간을 줄일 수 있다.**  
   보통 클라이언트와 캐시 사이에 높은 속도의 연결이 설정되어 있어 웹서버 캐시에 객체를 갖고 있다면 병목 현상을 줄일 수 있다.


2. 웹 캐시는 한 기관에서 인터넷으로의 접속하는 링크 상의 웹 트래픽을 대폭 줄일 수 있다.


3. 인터넷 전체의 웹 트래픽을 실질적으로 줄여주어 모든 애플리케이션의 성능이 좋아진다.

<br/>

### 웹 캐시 미사용과 사용 성능 비교

<p align="center"><img width="370" alt="병목 현상" src="https://user-images.githubusercontent.com/76640167/210504713-f02f053c-e503-413f-8cbc-046c55082359.png">

<br/>
<br/>

- 평균 객체의 크기가 `1 Mb`이고, 기관 브라우저로부터 기점 서버에 대한 평균 요청 비율이 초당 `15 요청`이라고 가정하자.
- HTTP 메시지 요청이 무시할만큼 작으므로 네트워크 접속 회선에 어떤 트래픽도 발생시키지 않는다고 가정하자.
- 또한, 접속 회선의 인터넷 부분 라우터가 **HTTP 요청을 전달하고 응답을 받을 때까지 평균 소요 시간**을 `2초`라고 가정하자.  
  통상 이러한 지연을 `인터넷 지연`이라고 한다.

> 총 응답 시간 = LAN 지연 + 접속 지연 + 인터넷 지연

<br/>

LAN의 트래픽 강도는 다음과 같다.

```
(15 요청/초) X (1 Mb/요청) / 100 Mbps = 0.15
```

<br/>

접속 회선(라우터와 라우터 사이)의 트래픽 강도는 다음과 같다.

```
(15 요청/초) X (1 Mb/요청) / 15 Mbps = 1
```

<br/>

LAN의 트래픽 강도는 많아야 수십 ms의 지연을 야기하므로 LAN 지연을 무시할 수 있지만,  
트래픽 강도가 1에 가까워지면 1.4절에서 논의한 것과 같이 회선의 지연은 매우 커지고 한없이 증가한다.

**접속 회선의 접속률을 100 Mbps 수준으로 늘리면 트래픽 강도를 0.15로 낮추어 해결할 수 있겠지만, 매우 많은 비용이 들어간다.**

<br/>

---

<br/>

웹 캐시를 사용한 예시를 보자.

<p align="center"><img width="370" alt="인터넷의 구성 요소" src="https://user-images.githubusercontent.com/76640167/210506730-5cb70fea-a9ed-4817-afe0-4463bfd24a4a.png">

<br/>
<br/>

`캐시가 만족시킨 요청의 비율(hit rate)`은 일반적으로 0.2 ~ 0.7이며, 이 예시에서는 `0.4의 적중률`을 가진다고 가정하자.

- 캐시와 클라이언트는 고속 LAN으로 연결되어 있어, 요청의 40%는 캐시에 의해(10ms 이내) 즉시 만족된다.
- 나머지 60%의 요청은 여전히 기점 서버에 의해 만족되어야 하므로 트래픽 강도는 1.0에서 0.6으로 감소한다.

일반적으로 0.8 미만의 트래픽 강도는 작은 지연에 속한다. (2초에 의하면 무시할 수 있는 수준이다.)

<br/>

이들을 고려한 평균 지연은 다음과 같다.

```
0.4 X 0.01초 + 0.6 X 2.01초 = 1.2....
```

<br/>

많은 캐시가 저렴한 PC에서 실행되는 공개 소프트웨어를 사용한다.

- 콘텐츠 전송 네트워크(CDN)을 통해 웹 캐시는 인터넷에서 점진적으로 중요한 역할을 하고 있다.

- CDN 회사는 인터넷 전역을 통해 많은 지역적으로 분산된 캐시를 설치하고 있으며,  
  이를 통해 많은 트래픽을 지역화하고 있다. (전용 CDN을 사용하기도 한다.)

<br/>
<br/>

## 조건부(conditional) GET

> 웹 캐싱이 사용자가 느끼는 응답 시간을 줄일 수 있지만, **웹 캐시 내부에 있는 복사본이 새 것이 아닐 수 있다**는 문제를 야기한다.

복사본이 클라이언트에 캐싱된 이후 웹 서버에 있는 객체가 갱신되었을 수도 있기 때문이다.

<br/>

> 💡 HTTP는 클라이언트가 **브라우저로 전달되는 모든 객체가 최신의 것임을 확인하면서 캐싱**해주는데,  
> 이러한 방식을 `조건부 GET(conditional GET)` 이라고 한다.

> Goal: don't send object if cache has **up-to-date cached version**

HTTP 요청 메시지가 (1) GET 방식을 사용하고, (2) `If-modified-since 헤더`를 포함한다면, 그것이 조건부 GET이다.

<br/>

### 조건부 GET 동작 과정

1. 브라우저의 요청을 대신해서 프록시 캐시는 요청 메시지를 웹 서버로 보낸다.

```
GET /fruit/kiwi.gif HTTP/1.1
Host: www.exotiquecuisine.com
```

<br/>

2. 웹 서버는 캐시에게 객체를 가진 응답 메시지를 보낸다.

```
HTTP/1.1 200 OK
Date: Sat, 3 Oct 2015 15:39:29
Server: Apache/1.3.0 (Unix)
Last-Modified: Wed, 9 Sep 2015 09:23:24
Content-Type: image/gif
(data data data data data ...)
```

- 캐시는 요청하는 브라우저에게 객체를 보내주고 자신에게도 객체를 저장한다.
- 중요한 것은 캐시가 객체와 더불어 **마지막으로 수정된 날짜를 함께 저장한다**는 것이다.

<br/>

3. 일주일 후에 다른 브라우저가 같은 객체를 캐시에게 요청하면 캐시에 저장되어 있다.  
   이 객체는 지난주에 웹 서버에서 수정되었으므로, 브라우저는 `조건부 GET`으로 조사를 수행한다.

```
GET /fruit/kiwi.gif HTTP/1.1
Host: www.exotiquecuisine.com
If-modified-since: Wed, 9 Sep 2015 09:23:24
```

- `If-modified-since` 값이 일주일 전에 서버가 보낸 Last-Modified 값과 완벽히 일치한다.

- 이 조건부 GET은 서버에게 **If-modified-since에 명시된 값 이후 수정된 경우에만 그 객체를 보내라고 한다.**

<br/>

4. 변경되지 않았다면,

```
HTTP/1.1 304 Not Modified
Date: Sat, 10 Oct 2015 15:39:29
Server: Apache/1.3.0 (Unix)
(empty entity body)
```

- 위와 같은 응답을 보낸다.  
  데이터가 변화가 없어도 객체를 보내는 것은 대역폭을 낭비하는 것이고, 특히 그 개체가 크다면 사용자가 느끼는 응답 시간이 증가된다.
- 위 응답 메시지는 클라이언트에게 요청 객체의 캐싱된 복사본을 사용하라는 것을 의미한다.

<br/>
<br/>
<br/>

# 2.2.6 HTTP/2

2020년 현재 주요 웹 사이트 천만 개의 40%가 `HTTP/2`를 지원하고 있다.

HTTP/2의 주요 목표는 하나의 TCP 연결상에서 멀티플렉싱 요청/응답 지연 시간을 줄이는 데 있으며,  
요청 우선순위화, 서버 푸시, HTTP 헤더 필드의 효율적인 압축 기능 등을 제공한다.

HTTP/2는 클라이언트와 서버 간의 데이터 포맷 방법과 전송 방법을 변경했다.

<br/>
<br/>

## 기존의 HTTP/1.1

지속적인 연결을 사용할 때 **웹 페이지당 오직 하나의 TCP 연결**을 가짐으로써,  
아래 설명하듯이 서버에서의 소켓 수를 줄이며 전송되는 각 웹 페이지는 공정한 네트워크 대역폭을 가질 수 있다.

그러나 하나의 TCP 상에서 모든 웹페이지를 보내면 `HOL(Head of Line) 블로킹 문제`가 발생할 수 있다.

<br/><br/>

### HOL 블로킹 문제

비디오 아래 수많은 작은 객체들을 포함할 때 서버와 클라이언트 사이에 저속에서 중간 속도의 **병목 링크**가 있다고 하자.

비디오 클립은 병목 링크를 통과하는데 오래 걸리는 반면, **작은 객체들은 비디오 클립 뒤에서 기다림이 길어진다.**

즉, 비디오 클립이 객체들을 블로킹하게 된다.

<br/>

HTTP/1.1에서는 **여러 개의 병렬 TCP 연결**을 열어서 위 문제를 해결해왔다.

<br/>
<br/>

### TCP 혼잡 제어

TCP 혼잡 제어는 각 TCP 연결이 공정하게 병목 링크를 공유하여 **같은 크기의 가용한 대역폭을 공평하게 나누게 해준다.**

만일 n개의 TCP 연결이 병목 링크에서 작동하고 있다면, 각 연결은 대략 대역폭의 1/n 씩을 사용하게 된다.

<br/>

하나의 웹 페이지를 전송하기 위해 여러 개의 병렬 TCP 연결을 열게 함으로써 브라우저는 일종의 속임수로 링크 대역폭의 많은 부분을 받게 된다.

많은 HTTP/1.1 브라우저들은 6개까지 병렬 TCP 연결을 열고 HOL을 막을 뿐만 아니라 더 많은 대역폭을 사용할 수 있게 한다.

<br/>
<br/>

## HTTP/2 프레이밍(framing)

> HTTP/2의 주요 목표 중 하나는 하나의 웹 페이지를 전송하기 위한 병렬 TCP 연결의 수를 줄이거나 제거하는 데 있다.

이는 서버에서 열고 유지되는 데 필요한 소켓의 수를 줄일 뿐만 아니라 목표한 대로 TCP 혼잡 제어를 제어할 수 있게 하는 데 있다.

그러나 웹 페이지를 전송하기 위해 오직 하나의 TCP 연결만을 사용하게 될 경우에 HTTP/2는 HOL 블로킹을 피하기 위해 신중하게 구현된 메커니즘이 필요하다.

<br/>

> 💡 `HTTP/2 프레이밍(framing)`이란, HTTP 메시지를 독립된 프레임들로 쪼개고, 인터리빙(interleaving)하고, 반대편 사이트에서 재조립하는 것이다.

예를 들어, 비디오 클립과 크기가 작은 객체 8개의 요청이 들어오면, 서버는 9개의 객체를 보내기 위한 TCP 병렬 요청을 받게된다.

이때 (1) 비디오 클립을 1000개의 프레임으로 나누고 (2) 각 객체를 2개의 프레임으로 나누어 비디오 클립으로부터 하나의 프레임을 전송한다.

이후 프레임 인터리빙을 이용하여 각 소형 객체의 첫 번째 프레임을 보내고 이를 반복하여 HOL 블로킹을 피할 수 있다.

<br/>
<br/>

**HTTP 메시지를 독립된 프레임들로 쪼개고 인터리빙하고 반대편 사이트에서 재조립하는 것이야말로 HTTP/2의 가장 중요한 개선점이다.**

프레이밍은 HTTP/2 프로토콜의 프레임으로 구현된 다른 프레이밍 서브 계층에 의해 이루어진다.

서버가 HTTP 응답을 보내고자 할 때, 응답은 프레이밍 서브 계층에 의해 처리되며 프레임들로 나눠진다.

응답의 헤더필드는 하나의 프레임이 되고, 메시지 본문은 하나의 프레임으로 쪼개진다.

응답 프레임들은 서버의 프레이밍 서브 계층에 의해 인터리빙된 후 하나의 지속적인 TCP 연결상에서 전송된다.

프레임들이 클라이언트에 도착하면 프레이밍 서브 계층에서 처음 응답메시지로 재조립되며 브라우저에 의해 처리된다. (클라이언트에서 서버로 요청할 때도 마찬가지이다.)

<br/>

각 HTTP 메시지를 독립적인 프레임으로 쪼개는 것 외에도 프레이밍 서브 계층은 프레임을 바이너리 인코딩한다.

바이너리 프로토콜은 파싱하기에 효율적이고, 더 작은 프레임 크기를 갖고, 에러에 강하다.

<br/>

### 메시지 우선순위화

`메시지 우선순위화`는 개발자들로 하여금 요청들의 상대적 우선 순위를 조정할 수 있게 함으로써 애플리케이션의 성능을 최적화할 수 있게 해준다.

<br/>

클라이언트가 하나의 특정 서버로 동시에 여러 개의 요청을 할 때, 각 메시지에 1에서 256 사이의 가중치를 부여함으로써 요청에 우선순위를 매길 수 있다.

> 높은 수치일수록 높은 우선순위를 갖는다.

서버는 가장 높은 우선순위의 요청을 위한 프레임을 제일 먼저 보낼 수 있다.

<br/>

클라이언트 또한 각 의존도에 따라 메시지의 ID를 지정하여 서로 다른 메시지들 간의 의존성을 나타낼 수 있다.

<br/>

### 서버 푸싱

HTTP/2의 또 다른 특징은 서버로 하여금 **특정 클라이언트 요청에 대해 여러 개의 응답을 보낼 수 있게 해주는 데 있다.**

처음 요청에 대한 응답 외에도, 서버는 클라이언트의 요청 없이도 추가적인 객체를 클라이언트에게 **푸시**하여 보낼 수 있다.

이는 HTML 기반 페이지가 웹 페이지를 완벽하게 구동시킬 필요가 있는 객체들을 가리킬 수 있기에 가능하다.

이러한 객체에 대한 HTTP 요청을 기다리는 대신 서버는 HTML을 분석할 수 있고, 필요한 객체들을 식별할 수 있고,  
**해당 객체들에 대한 요청이 도착하기도 전에** 해당 객체들을 클라이언트로 보낸다.

서버는 해당 요청들을 기다리는 데 소요되는 추가 지연을 없앤다.

<br/>
<br/>

## HTTP/3

트랜스 프로토콜인 QUIC(3장에서 다룬다) 위에서 작동하도록 설계된 새로운 HTTP 프로토콜로서, 완전히 표준화된 상태는 아니다.
