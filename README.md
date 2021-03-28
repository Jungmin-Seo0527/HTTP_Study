# HTTP

## 목차

[1. Internet-network](#1-internet-network)

* [1-1 인터넷 통신](#1-1-인터넷-통신)
* [1-2. IP(Internet Protocol)](#1-2-ipinternet-protocol)
* [1-3. TCP, UDP](#1-3-tcp-udp)
* [1-4. PORT](#1-4-port)
* [1-5. DNS](#1-5-dns)

[2. URI와 웹 브라우저 요청 흐름](2-uri와-웹-브라우저-요청-흐름)

* [2-1. 2-uri와-웹-브라우저-요청-흐름](#2-1-uri-uniform-resource-identifier)
* [2-2. 웹 브라우저 요청 흐름](#2-2-웹-브라우저-요청-흐름)

[3. HTTP 기본](3-http-기본)

* [3-1. 모든것이 HTTP](#3-1-모든것이-http)
* [3-2. 클라이언트 서버 구조](#3-2-클라이언트-서버-구조)
* [3-3. Stateful, Stateless](#3-3-stateful-stateless)
* [3-4. 비 연결성(connectionless)](#3-4-비-연결성connectionless)
* [3-5. HTTP 메시지](#3-5-http-메시지)

[4. HTTP 메서드](#4-http-메서드)

* [4-1. HTTP API를 만들어보자](#4-1-http-api를-만들어보자)
* [4-2. HTTP 메서드 - GET, POST](#4-2-http-메서드---get-post)
* [4-3. HTTP 메서드 - PUT, PATCH, DELETE](#4-3-http-메서드---put-patch-delete)
* [4-4. HTTP 메서드의 속성](#4-4-http-메서드의-속성)

---

## 1. Internet-network

### 1-1. 인터넷 통신

![인터넷망](image/인터넷망.jpg)

* 클라이언트와 서버와의 통신은 복잡한 인터넷 망이 필요하다.
* 단순한 요청도 다양한 노드를 거쳐서 서버로 전송된다.

### 1-2. IP(Internet Protocol)

#### IP 인터넷 프로토콜 역할

* 지정된 IP 주소(IP Address)에 데이터 전달
* 패킷(Packet)이라는 통신 단위로 데이터 전달

#### IP 패킷 정보

* 출발지 IP
* 목적지 IP
* 기타...
* 전송 데이터

#### 클라이언트 패킷 전달

![클라리언트 패킷 전달](./image/1-2_클라이언트_패킷_전달.jpg)

* 출발 IP Address를 가진 패킷이 목적 IP Address 까지 전송이 된다.
* 중간에는 노드간에 패킷을 전달하여 최종 목적지까지 도달한다.

#### 서버 패킷 전달

* 서버에서도 패킷을 전달할때 출발 IP Address 정보를 토대로 노드간에 패킷을 전달하여 클라이언트까지 도달한다.

#### IP 프로토콜의 한계

* 비연결성
    * 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
* 비신뢰성
    * 중간에 패킷이 사라지면? - 패킷 소실
    * 패킷이 순서대로 안오면? - 패킷 전달 순서 문제 발생
* 프로그램 구분
    * 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이면?

### 1-3. TCP, UDP

#### 인터넷 프로토콜 스택의 4계층

![인터넷 프로토콜 스택의 4계층](./image/1-3_인터넷_프로토콜_스택의_4계층.jpg)

#### 프로토콜 계층

![프로토콜 계증](./image/1-3_프로토콜_계층.jpg)

#### IP 패킷 정보

* 패킷 : package + bucket
* 통신망을 통해 전송하기 쉽도록 자른 데이터의 전송 단위

#### TCP/IP 패킷 정보

![TCP/IP 패킷 정보](./image/1-3_TCP_IP패킷_정보.jpg)

* 기존의 IP 패킷에 추가로 출발지 port, 목적지 port, 전송제어 순서 검증 정보 추가
* IP 만으로는 해결하지 못했던 문제를 해결

#### TCP 특징

전송 제어 프로토콜(Transmission Control Protocol)

* 연결 지향 - TCP 3 wqy handshake (가상 연결)
* 데이터 전달 보증
* 순서 보장
* 신뢰할 수 있는 프로토콜
* 현재는 대부분 TCP 사용

#### TCP 3 way handshake

![TCP 3 way handshake](./image/1-3_TCP3way_handshake.jpg)

1. 클라이언트가 서버로 SYN(연결 요청)을 보낸다.
2. 서버가 클라이언트에게 연결요청을 받았다는 의미로 ACK 를 보냄과 동시에 SYN 을 보낸다.
3. 클라이언트가 서버에게 SYN에 대한 ACK 를 전송한다.
4. 데이터를 전송한다. (3번 단계에서 ACK와 함께 데이터 전송 가능)

* SYN(연결 요청)을 보내면 연결 요청에 승인한다는 ACK를 보내는 작업을 클라이언트와 서버가 모두 수행해야 클라이언트와 서버가 논리적으로 연결이 되었다고 말할 수 있다.

#### 데이터 전달 보증

* 클라이언트가 서버로 데이터를 전송하면 서버는 클라이언트에게 데이터를 잘 받았다고 알려준다.

#### 순서 보장

![순서 보장](./image/1-3_순서보장.jpg)

* 더 최적화된 방법도 존재한다.
* 우선 개념적으로 순서를 보장할 수 있다는 것만 알고 넘어가자.

#### UDP 특징

사용자 데이터그램 프로토콜(User Datagram Protocol)

* 하얀 도화지에 비유(기능이 거의 없음)
* 연결 지향 - TCP 3 way handshake X
* 데이터 전달 보증 X
* 순서 보장 X
* 데이터 전달 및 순서가 보장되지 않지만, 단순하고 빠름
* 정리
    * IP와 거의 같다. + PORT + 체크섬 정도만 추가
    * 애플리케이션에서 추가 작업 필요

### 1-4. PORT

#### 한번에 둘 이상 연결해야 하면?

![](./image/1-4_한변에_둘이상_연결.jpg)

* 하나의 클라이언트가 여러 서버와 통신할때, 각 서버에서 날아오는 패킷들이 어떤 애플리케이션에 요청한 패킷인가?

#### TCP/IP 패킷 정보

* 출발지 PORT, 목적지 PORT

#### PORT - 같은 IP 내에서 프로세스 구분

![](./image/1-4_같은IP내에서_프로세스_구분.jpg)

* IP : 아파트
* PORT : 몇동 몇호?

#### PORT

* 0 ~ 65535 : 할당 가능
* 0 ~ 1023 : 잘 알려진 포트, 사용하지 않는 것이 좋음
    * FTP - 20, 21
    * TELNET - 23
    * HTTP - 80
    * HTTPS - 443

### 1-5. DNS

#### IP 주소의 단점

* IP 주소는 사람이 기억하기 어렵다.
* IP 주소는 변경될 수 있다. -> 새로운 IP 주소를 알지 못하면 전송 불가능

#### DNS

**도메인 네임 시스템(Domain Name System)**

* 전화번호부
* 도메인 명을 IP 주소로 변환

![도메인 사용](./image/1-5_도메인_사용.jpg)

## 2. URI와 웹 브라우저 요청 흐름

* URL
* 웹 브라우저 요청 흐름

### 2-1. URI (Uniform Resource Identifier)

* URI? URL? URN?
* URI는 로케이터(locator), 이름(name) 또는 둘다 추가로 분류될 수 있다.

![](./image/2-1_URI.jpg)

![](./image/2-1_URI_URL.jpg)

#### URI 단어 뜻

* **U**niform : 리소스 식별하는 통일된 방식
* **R**esource : 자원, URI로 식별할 수 있는 모든 것(제한 없음)
* **I**dentifier : 다른 항목과 구분하는데 필요한 정보

* URL : Uniform Resource Locator
* URN : Uniform Resource Name

#### URL, URN 단어 뜻

* URL - Locator : 리소스가 있는 위치를 지정
* URN - Name : 리소스에 이름을 부여
* 위치는 변할 수 있지만, 이름은 변하지 않는다.
* urn:isbn:8960777331 (어떤 책의 isbn URN)
* URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음
* **앞으로 URI를 URL과 같은 의미로 이야기하겠음**

#### URL 분석

<https://www.google.com/search?q=hello&hl=ko>

#### URL 전체 문법

* scheme://[userinfo@]host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko
  <br><br>

* 프로토콜(https)
* 호스트명(www.google.com)
* 포트 번호(443)
* 패스(/search)
* 쿼리 파라미터(q=hello&hl=ko)

#### URL scheme

* **scheme:**//[userinfo@]host[:port][/path][?query][#fragment]
* **https:**//www.google.com:443/search?q=hello&hl=ko
  <br><br>
* 주로 프로토콜 사용
* 프로토콜 : 어떤 방식으로 자원에 접근할 것인가 하는 약속, 규칙
    * 예) http, https, ftp 등등
* http는 80 포트, https는 443 포트를 주로 사용, 포트는 생략 가능
* https는 http에 보안 추가 (HTTP Secure)

#### URL userinfo

* scheme://**[userinfo@]**host[:port][/path][?query][#fragment]
* https://www.google.com:443/search?q=hello&hl=ko <br><br>

* URL에 사용자 정보를 포함해서 인증
* 거의 사용하지 않음

#### URL host

* scheme://[userinfo@]**host**[:port][/path][?query][#fragment]
* https:// **www.google.com**:443/search?q=hello&hl=ko <br><br>
* 호스트명
* 도메인명 또는 IP 주소를 직접 사용가능

#### URL PORT

* scheme://[userinfo@]host **[:port]**[/path][?query][#fragment]
* https://www.google.com: **443**/search?q=hello&hl=ko<br><br>
* 포트(PORT)
* 접속 포트
* 일반적으로 생략, 생략시 http는 80, https는 443

#### URL path

* scheme://[userinfo@]host[:port] **[/path]**[?query][#fragment]
* https://www.google.com:443 **/search**?q=hello&hl=ko<br><br>
* 리소스 경로(path), 계층적 구조
* 예)
    * /home/file1.jpg
    * /members
    * /members/100,/items/iphone12

#### URL query

* scheme://[userinfo@]host[:port] **[/path]**[?query][#fragment]
* https://www.google.com:443/search **?q=hello&hl=ko**<br><br>
* key = value 형태
* ?로 시작, &로 추가 가능 ?keyA=valueA&keyB=valueB
* query parameter, query string 등으로 불림, 웹서버에 제공하느 파라미터, 문자형태

#### URL fragment

* scheme://[userinfo@]host[:port][/path][?query] **[#fragment]**
* http://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html/
  **#getting-started-introducing-spring-boot** <br><br>
* fragment
* html 내부 북마크 등에 사용
* 서버에 전송하는 정보 아님

### 2-2. 웹 브라우저 요청 흐름

####                                                                                                                                                                                                                                                    

* https://www.google.com/search?q=hello&hl=ko
* https://**www.google.com:443**/search?q=hello&hl=ko
    * DNS 조회
    * HTTPS PORT 생략, 443
    * HTTP 요청 메시지 생성

#### HTTP 요청 메시지

![](./image/2-2_HTTP요청메시지.jpg)<br><br>

#### HTTP 메시지 전송

![](./image/2-2_HTTP메시지전송.jpg)<br><br>

* TCP/IP 계층에서 생성된 패킷은 HTTP 메시지, 출발지 IP, PORT, 목적지 IP, PORT 를 포함하고 있다.
* 패킷이 웹 브라우저 (클라이언트) 에서 서버로 전송된다.
* 서버는 HTTP 응답 메시지를 웹 브라우저에게 전송한다.

<br><br>

* HTTP 응답메시지 <br>
  ![](./image/2-2_HTTP응답메시지.jpg)

* 웹 브라우저는 응답 패킷을 받아서 HTML를 랜더링하여 사용자에게 페이지를 보여준다.

## 3. HTTP 기본

* 모든것이 HTTP
* 클라이언트 서버 구조
* Stateful, Stateless
* 비 연결성(connectionless)
* HTTP 메시지<br><br>
* **HTTP** (**H**yper**T**ext **T**ransfer **P**rotocol)

### 3-1. 모든것이 HTTP

HTTP 메시지에 모든 것을 전송<br><br>

* HTML, TEXT
* IMAGE, 음성, 영상, 파일
* JSON, XML (API)
* 거의 모든 형태의 데이터 전송 가능
* 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용
* **지금은 HTTP 시대**!

#### HTTP 역사

* HTTP/0.9 1991년: GET 메서드만 지원, HTTP 헤더 X
* HTTP/1.0 1996년: 메서드, 헤더 추가
* **HTTP/1.1 1997년: 가장 많이 사용, 우리에게 가장 중요한 버전**
    * RFC2068(1997) -> RFC2626(1999) -> RFC7230~7235(2014)
* HTTP/2 2015년: 성능 개선
* HTTP/2 진행중: TCP 대신에 UDP 사용, 성능 개선

#### 기반 프로토콜

* TCP : HTTP/1.1, HTTP/2
* UDP : HTTP/3
* 현재 HTTP/1.1 주로 사용
    * HTTP/2, HTTP/3 도 점점 증가

#### HTTP 특징

* 클라이언트 서버 구조
* 무상태 프로토콜(스테이스리스), 비연결성
* HTTP 메시지
* 단순함, 확장 기능<br><br>
* 위의 내용은 이후 더 자세히 설명...

### 3-2. 클라이언트 서버 구조

* Request Response 구조
* 클라이언트는 서버에 요청을 보내고, 응답을 대기
* 서버가 요청에 대한 결과를 만들어서 응답<br><br>
* 클라이언트와 서버가 분리되어 있다는 개념이 중요함 (이전에는 분리되어 있지 않고 통합되어 있던 시절이 있었음)
* 서버 -> 비즈니스 로직, 데이터등, 클라이언트 -> UI, 사용성
* 클라이언트와 서버가 각각 독립적으로 진화가 가능!!!

### 3-3. Stateful, Stateless

#### 무상태 프로토콜

스테이스리스(StateLess)

* 서버가 클라이언트의 상태를 보존X
* 장점: 서버 확장성 높음(스케일 아웃)
* 단점: 클라이언트가 추가 데이터 전송

#### Stateful, Stateless 차이

**< 상태 유지 - Stateful >**

* 고객: 이 **노트북** 얼마인가요?
* 점원: 100만원 입니다.<br><br>
* 고객: **2개** 구매하겠습니다.
* 점원: 200만원 입니다. **신용카드, 현금중**에 어떤 걸로 구매하시겠어요?<br><br>
* 고객: 신용카드로 구매하겠습니다.
* 점원: 200만원 결제 완료되었습니다.

**< 상태유지 - Stateful, 점원이 중간에 바뀌면? >**

* 고객: 이 **노트북** 얼마인가요?
* 점원**A**: 100만원 입니다.<br><br>
* 고객: **2개** 구매하겠습니다.
* 점원**B**: ? 무엇을 2개 구매하시겠어요?<br><br>
* 고객: 신용카드로 구매하겠습니다.
* 점원**C**: ? 무슨 제품을 몇개 신용카드로 구매하시겠어요?

**< 상태 유지 - Stateful, 정리 >**

* 고객: 이 **노트북** 얼마인가요?
* 점원: 100만원 입니다.**(노트북 상태 유지)**<br><br>
* 고객: **2개** 구매하겠습니다.
* 점원: 200만원 입니다. **신용카드, 현금**중에 어떤 걸로 구매하시겠어요?
  <br>**(노트북, 2개 상태 유지)**<br><br>
* 고객: 신용카드로 구매하겠습니다.
* 점원: 200만원 결제 완료되었습니다. **(노트북, 2개, 신용카드 상태 유지)**

**< 무상태 - Stateless >**

* 고객: 이 **노트북** 얼마인가요?
* 점원: 100만원 입니다.<br><br>
* 고객: **노트북 2개** 구매하겠습니다.
* 점원: 노트북 2개는 200만원 입니다. **신용카드, 현금중**에 어떤 걸로 구매 하시겠어요?<br><br>
* 고객: **노트북 2개를 신용카드**로 구매하겠습니다.
* 점원: 200만원 결제 완료되었습니다.

**< 무상태 - Stateless, 점원이 중간에 바뀌면? >**

* 고객: 이 **노트북** 얼마인가요?
* 점원**A**: 100만원 입니다.<br><br>
* 고객: **노트북 2개** 구매하겠습니다.
* 점원**B**: 노트북 2개는 200만원 입니다. **신용카드, 현금중**에 어떤 걸로 구매 하시겠어요?<br><br>
* 고객: **노트북 2개를 신용카드**로 구매하겠습니다.
* 점원**C**: 200만원 결제 완료되었습니다.

**< 정리 >**

* **상태 유지**: 중간에 다른 점원으로 바뀌면 안된다.<br>
  (중간에 다른 점원으로 바뀔 때 상태 정보를 다른 점원에게 미리 알려줘야 한다.)
* **무상태**: 중간에 다른 점원으로 바뀌어도 된다.
    * 갑자기 고객이 증가해도 점원을 대거 투입할 수 있다.
    * 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있다.
* 무상태는 응답 서버를 쉽게 바꿀 수 있다. -> **무한한 서버 증설 가능**

#### Stateless <br>실무 한계

* 모든 것을 무상태로 설계 할 수 있는 경우도 있고 없는 경우도 있다.
* 무상태
    * 예) 로그인이 필요 없는 단순한 서비스 소개 화면
* 상태 유지
    * 예) 로그인
* 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지

### 3-4. 비 연결성(connectionless)

#### 연결을 유지하는 모델

* 서버는 연결을 계속 유지
* 서버 자원 소모

#### 연결을 유지하지 않는 모델

* 서버는 연결 유지 X
* 최소한의 자원 유지

#### 비 연결성

* HTTP는 기본이 연결을 유지하지 않는 모델
* 일반적으로 초 단위 이하의 빠른 속도로 응답
* 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
    * 예) 웹 브라우저에서 계속 연속해서 검색 버튼을 누르지는 않는다.
* 서버 자원을 매우 효율적으로 사용 가능

**비 연결성의 한계와 극복**

* TCP/IP 연결을 새로 맺어야 함 - 3 way handshake 시간 추가
* 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립드, css, 추가 이미지 등등 수 많은 자원이 함께 다운로드
* 지금은 HTTP 지속 연결(Persistent Connections)로 문제 해결
* HTTP/2, HTTP/3에서 더 많은 최적화

#### 스테이스리스를 기억하자!!!<br>**서버 개발자들이 어려워하는 업무**

* 정말 같은 시간에 딱 맞추어 발생하는 대용량 트래픽
* 예) 선착순 이벤트, 명정 KTX 예약, 학과 수업 등록
* 예) 저녁 6:00 선착순 1000명 치킨 할인 이벤트 -> 수만명 동시 요청

### 3-5. HTTP 메시지

* HTTP 메시지에 모든 것을 전송
* HTML, TEXT
* IMAGE, 음성, 영상, 파일
* JSON, XML
* 거의 모든 형태의 데이터 전송 가능
* 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용
* **지금은 HTTP 시대!**

#### HTTP 메시지 구조

![HTTP 메시지 구조](https://i.ibb.co/TLvKFQ4/HTTP-message-structure.jpg)

#### 시작 라인 - 요청 메시지

* start-line = **request-line** / status - lline
* **request-line** = method SP(공백) request-target SP HTTP-version CRLF(엔터)
* HTTP 메서드 (GET: 조회)
* 요청 대산(/search?q=hello&hl=ko)
* HTTP Version

#### 시작 라인 - 요청 메시지 - HTTP메서드

* 종류 : GET, POST, PUT, DELETE ...
* 서버가 수행해야 할 동작 지정
    * GET : 리소스 조회
    * POST : 요청 내역 처리

#### 시작 라인 - 요청 메시지 - 요청 대상

![](https://i.ibb.co/thBGwFF/bandicam-2021-03-26-15-35-28-120.jpg)

* absolute - path[?query](절대경로[?쿼리])
* 절대경로 = "/"로 시작하는 경로
* 참고 : *, http://...?x=y 와 같이 다른 유형의 경로 지정 방법도 있다.

#### 시작 라인 - 요청 메시지 - HTTP 버전

* HTTP Version

#### 시작 라인 - 응답 메시지

![](https://i.ibb.co/GVs7Mjj/HTTP-response-message.jpg)

* start-line = request-line / **status-line**
* **status-line** = HTTP-version SP status-code SP reason-phrase CRLF
* HTTP 버전
* HTTP 상태 코드 : 요청 성공, 실패를 나타냄
    * 200 : 성공
    * 400 : 클라이언트 요청 오류
    * 500 : 서버 내부 오류
* 이유 문구 : 사람이 이해할 수 있는 짧은 상태 코드 설명 글

#### HTTP 헤더

* header-field = filed-name":"OWS fields-value OWS (OWS: 띄어쓰기 허용)
* field-name은 대소문자 구분 없음

![HTTP header](https://i.ibb.co/xDHJhkD/HTTP-header.jpg)

#### HTTP 헤더 - 용도

* HTTP 전송에 필요한 모든 부가 정보
* 예( 메시지 바디의 내용, 메세지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저)정보, 서버 애플리케이션 정보, 캐시 관리 정보...)
* 표준 헤더가 너무 많음
* 필요시 임의의 헤더 추가 가능
    * helloworld: hihi

#### HTTP 메시지 바디 - 용도

* 실제 전송할 데이터
* HTML 문서, 이미지, 영상, JSON 등등 byte로 표현할 수 있는 모든 데이터 전송 가능

#### 단순함 확장 가능

* HTTP는 단순하다. 스펙도 읽어볼만...
* HTTP 메시지도 매우 단순
* 크게 성골하는 표준 기술은 단순하지만 확장 가능한 기술

#### HTTP 정리

* HTTP 메시지에 모든 것을 전송
* HTTP 역사 HTTP/1.1을 기준으로 학습
* 클라이언트 서버 구조
* 무상태 프로토콜(스테이스리스)
* HTTP 메시지
* 단순함, 확장 가능
* **지금은 HTTP시대**

## 4. HTTP 메서드

### 4-1 HTTP API를 만들어보자

#### URI 설계에서 가장 중요한 것은 리소스 식별

* 리소스의 의미
    * 회원을 등록하고 수정하고 조회하는 것이 리소스가 아니다!
    * 예) 미네랄을 캐라 -> 미네랄이 리소스
    * **회원이라는 개념 자체가 바로 리소스이다.**

* 리소스를 어떻게 식별하는게 좋을까?
    * 회원을 등록하고 수정하고 조회하는 것을 모두 배제
    * **회원이라는 리소스만 식별하면 된다. -> 회원 리소스를 URI에 매핑**

* API URI 설계
* **회원** 목록 조회/members
* **회원** 조회 /members{id}
* **회원** 등록 /members{id}
* **회원** 수정 /members{id}
* **회원** 삭제 /members{id}
* 참고 : 계층 구조상 상위를 컬렉션으로 보고 복수단어 사용 권장(member -> members)

* 리소스와 행위를 분리
* **URI는 리소스만 식별!!**
* **리소스**와 해당 리소스를 다상으로 하는 **행위**를 분리
    * 리소스 : 회원
    * 행위 : 조회, 등록, 삭제, 변경

* 리소스는 명사, 행위는 동사(미네랄을 캐라)
* 행위(메서드)는 어떻게 구분?

### 4-2. HTTP 메서드 - GET, POST

#### HTTP 메서드 종류

* 주요 메서드
    * GET : 리소스 조회
    * POST : 요청 데이터 처리, 주로 등록에 사용
    * PUT : 리소스를 대체, 해당 리소스가 없으면 생성
    * PATCH : 리소스 부분 변경
    * DELETE : 리소스 삭제

* 기타 메서드
    * HEAD : GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
    * OPTIONS : 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
    * Connect : 대상 자원으로 식별되는 서버에 대한 터널을 설정
    * TRACE : 대상 리소스에 대한 경로를 다라 메시지 루프백 테스트를 수행

#### GET

* 리소스 조회
* 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
* 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않음

#### POST

* 요청 데이터 처리
* **메시지 바디를 통해 서버로 요청 데이터 전달**
* 서버는 요청 데이터를 처지
    * 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.

* 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용

* 요청 데이터를 어떻게 처리한다는 뜻일까? 예시
    * 스펙 : POST 메서드는 **대상 리소스가 리소스의 고유 한 의미 체계에 따라 요청에 포함 된 표현을 처리하도록 요청**한다. (구글 번역...)
    * 예를 들어 POST는 다음과 같은 기능에 사용된다.
        * HTML 양식에 입력된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공
            * 예) HTML FORM 에 입력한 정보로 회원 가입, 주문 등에서 사용
        * 게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시
            * 예) 게시판 글쓰기, 댓글 달기
        * 서버가 아직 식별하지 않은 새 리소스 생성
            * 예) 신규 주문 생성
        * 기존 자원에 데이터 추가
    * **정리 : 이 리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야 함 -> 정해진 것이 없음**

#### POST 정리

1. 새 리소스 생성(등록)
    * 서버가 아직 식별하지 않은 새 리소스 생성

2. 요청 데이터 처리
    * 단순히 데이터를 생성하거나, 변경하는 것을 넘어서 프로세스를 처리해야 하는 경우
    * 예) 주문에서 결제완료 -> 배달 시작 -> 배달 완료 처럼 단순히 값 변경을 넘어 프로세스의 상태가 변경되는 경우
    * POST 의 결과로 새로운 리소스가 생성되지 않을 수도 있음
    * 예) POST/orders/{orderId}/start-delivery (컨트롤 URI)

3. 다른 메서드로 처리하기 애매한 경우
    * 예) JSON으로 조회 데이터를 넘겨야 하는데, GET 메서드를 사용하기 어려운 경우
    * 애매하면 POST

### 4-3. HTTP 메서드 - PUT, PATCH, DELETE

#### PUT

* **리소스를 대체**
    * 리소스가 있으면 대체
    * 리소스가 없으면 생성
    * 쉽게 이야기 해서 덮어버림

* **중요! 클라이언트가 리소스를 식별**
    * 클라이언트가 리소스 위치를 알고 URI 지정
    * POST와 차이점

#### PATCH

* 리소스 부분 변경
* PUT에서 리소스를 부분 변경하려다가 완전히 변경되는 사고 발생

#### DELETE

* 리소스 제거

### 4-4. HTTP 메서드의 속성

----

# Note

----










