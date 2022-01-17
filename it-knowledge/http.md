---
description: Inflearn '모든 개발자를위한 HTTP 웹 기본 지식' 강의 내용 및 이미지를 통한 정리.
---

# HTTP 웹 기본 지식

## 인터넷 네트워크

### 인터넷 통신

![](<../.gitbook/assets/image (18) (1) (1) (1).png>)

클라이언트에서 다른 클라이언트(컴퓨터)로 데이터를 보낼경우 위치에따라 위성, 해저광케이블, 기타 통신서버와 같은 노드들을 거쳐서 상대 클라이언트에 도달한다.

### IP(인터넷 프로토콜)

송신/수신 클라이언트에서 정보를 주고받을 때 사용하는 정보 위주의 프로토콜

![](<../.gitbook/assets/image (3) (1).png>)

#### 역할

* 지정한 IP 주소(IP Adress)에 데이터 전달
* 패킷이라는 통신 단위로 데이터 전달

#### IP 패킷 정보

* 패킷은 전송하고자 하는 데이터의 한 블록(payload)과 주소지 정보(발신지 주소, 목적지 주소), 관리정보(Header, IPv6와 같이 망이 패킷을 목적지까지 전달하는데 필요한)로 구성된다.

![](<../.gitbook/assets/image (15) (1).png>)

#### **IP 프로토콜의 한계**

* 비연결성
  * 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
* 비신뢰성
  * 패킷 유실?
  * 패킷의 순서?
* 프로그램 구분
  * 동일한 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상인 경우



### TCP, UDP

![인터넷 프로토콜 스택의 4계층](<../.gitbook/assets/image (9).png>)

#### **인터넷 프로토콜 스택의 4 계층**

* 애플리케이션 계층 : 비즈니스로직 혹은 특정 제품을 만들어내는 지에 따라 탄생하는 데이터 전송에 대한 약속(규칙) 계층
* 전송 계층 : 물리적으로 연결하고, 경로를 지정했으면 이제 데이터를 전송해야하는데, 데이터를 전송하는 방법을 정의하는 계층.
* 인터넷 계층 : 방대한 인터넷 계층에서 어디로 보낼지 경로를 선택하는 것이 IP게층이다. 이 자체로는 비연결 지향적이며 신뢰성이 없고 데이터를 전송한 이후 발생하는 문제에 대해서는 신경쓰지 않는다.
* 네트워크 인터페이스 계층 : 물리적인 영역을 표준화 하는 계층으로 실제로 랜선을 꼽는 랜카드나 랜카드 드라이버등이 이에 속한다.

![](<../.gitbook/assets/image (17) (1).png>)

1. 프로그램이 Hello,world! 메시지(Payload) 생성
2. SOCKET 라이브러리를 통해 전달
3. TCP 정보 생성, 메시지 데이터(Payload) 포함
4. IP 패킷 생성,TCP 데이터 포함.



![](<../.gitbook/assets/image (12) (1).png>)

**TCP 특징**

전송 제어 프로토콜(Transmission Control Protocol)

* 연결 지향 - TCP 3 way handshake (가상 연결)
* 데이터 전달 보증
* 순서 보장
* 신뢰할 수 있는 프로토콜
* 현재는 대부분 TCP 사용



![](<../.gitbook/assets/image (21) (1).png>)

{% hint style="info" %}
SYN : 접속 요청&#x20;

ACK : 요청 수락

참고 : 3.ACK와 함께 데이터 전송 가능
{% endhint %}

![](<../.gitbook/assets/image (31).png>)

**데이터 전달 보증**

1. 데이터 전송
2. 데이터 수신 확인 메시지

![](<../.gitbook/assets/image (1).png>)

**순서 보장**

1. 패킷1, 패킷2, 패킷3 순서로 전송
2. 패킷1, 패킷3, 패킷2 순서로 도착
3. 서버가 클라이언트에게 패킷 2부터 다시 전송하는 것을 요청



#### UDP 특징

사용자 데이터그램 프로토콜(User Datagram Protocol)

* TCP에 비교해서 기능이 거의 없다.
* 연결 지향 - TCP 3 way handshake X, 데이터 순서 보증 X, 순서 보장 X
* 데이터 전달 및 순서가 보장되진 않지만, 단순하고 빠름
* IP와 거의 같다. +PORT +체크섬 정도만 추가
* 애플리케이션에서 추가 작업 필요
* 근래에 사용하려는 추세(과정의 간소화를 위해)



### Port

같은 IP 내에서 프로세스 구분

* 0 \~ 65535 할당 가능
* 0 \~ 1023 : 잘 알려진 포트, 사용하지 않는 것이 좋음
  * FTP - 20,21
  * TELNET - 23
  * HTTP - 80
  * HTTPS - 443

![](<../.gitbook/assets/image (6).png>)



### DNS

**도메인 네임 시스템(Domain Name System)**

* IP는 기억하기 어렵다.
* IP는 변경될 수 있다.

\->  DNS 사용으로 해결.

* 전화번호부와 같은 역할
* 도메인 명을 IP 주소로 변환

![](<../.gitbook/assets/image (14) (1) (1).png>)

## URI와 웹 브라우저 요청 흐름



### URI

{% hint style="info" %}
URI(Uniform Resource Identifier)

URI는 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다.
{% endhint %}

![](<../.gitbook/assets/image (43).png>)

![](<../.gitbook/assets/image (30) (1).png>)

**URI 뜻**

* Uniform : 리소스 식별하는 통일된 방식
* Resource : 자원, URI로 식별할 수 있는 모든 것(제한 없음)
* Identifier : 다른 항목과 구분하는데 필요한 정보
* URL : Uniform Resource Locator
* URN : Uniform Resource Name

**URL, URN 뜻**

* URL - Locator : 리소스가 있는 위치를 지정
* URN - Name : 리소스에 이름을 부여
* 위치는 변할 수 있지만, 이름은 변하지 않는다.
* urn:isbn:8960777331 (어떤 책의 isbn URN)
* URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음.

#### URL 문법

> scheme://\[userinfo@]host\[:port]\[/port]\[/path]\[?query]\[#fragment]
>
> https://www.google.com:443/search?q=hello\&hl=ko

* 프로토콜(https)
* 호스트명(www.google.com)
* 포트 번호(443)
* 패스(/search)
* 쿼리 파라미터(q=hello\&hl=ko)

**scheme**

* 주로 프로토콜이 사용
* 프로토콜이란 어떤 방식으로 자원에 접근할 것인지 정해놓은 규칙
* ex) http, https, ftp ...
* http는 80 포트, https는 443포트를 주로 사용하며 생략 가능
* https는 http에 보안사용을 추가한 것(HTTP Secure)

**userinfo**

* URL에 사용자정보를 포함해서 인증
* 거의 사용하지 않는다.

**host**

* 호스트명
* 도메인명 또는 IP주소를 직접 사용 가능

**PORT**

* 접속 포트
* 일반적으로 생략, 생략시 http는 80, https는 443

**path**

* 리소스 경로(parh), 계층적 구조
* ex)
  * /home/file1.jpg
  * /members
  * /members/100, /item/iphone12

**query**

* key=value 형태
* ?로 시작하며 &로 추가가 가능 ?keyA=valueA\&keyB=valueB
* query parameter, query string등으로 불림, 웹서버에 제공하는 파라미터로 문자형태

**fragment**

* html 내부 북마크 등에 사용
* 서버에 전송하는 정보가 아니다.



### 웹 브라우저 요청 흐름

![](<../.gitbook/assets/image (7).png>)

1. DNS 조회
2. HTTPS PORT 생략(443)
3. HTTP 요청 메시지 생성

&#x20;![](<../.gitbook/assets/image (24).png>)



#### HTTP 메시지 전송

![](../.gitbook/assets/image.png)

1. 웹 브라우저가 HTTP 메세지를 생성
2. SOCKET 라이브러리를 통해 TCP/IP계층에 전달\
   이전단계에서 찾은 IP와 PORT정보를 가지고 SYN, SYN+ACK, ACK 과정을 통해 서버와 연결\
   연결이 성공되면 TCP/IP 계층으로 데이터를 전달
3. TCP/IP 패킷을 생성, HTTP 메세지도 포함

![HTTP 메시지](<../.gitbook/assets/image (10) (1).png>)

4\. 서버는 패킷을 받으면 패킷의 내부 HTTP 메서드를 해석해서 정보에 맞는 동작.

![](<../.gitbook/assets/image (11) (1).png>)

5\. 서버에서 HTTP 응답 메세지를 생성

![](<../.gitbook/assets/image (16) (1) (1).png>)

6\. 클라이언트에서는 응답메세지를 받아 맞는 동작(ex: 렌더링)



## HTTP 기본

### 모든 것이 HTTP

**HTTP(HyperText Transfer Protocol)**

#### HTTP 메시지에 모든 것을 전송

* HTML, TEXT
* IMAGE, 음성, 영상, 파일
* JSON, XML(API)
* 거의 모든 형태의 데이터 전송 가능
* 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용

#### HTTP의 역사

* HTTP/0.9 1991년: GET 메서드만 지원, HTTP 헤더 X
* HTTP/1.0 1996년: 메서드, 헤더 추가
* HTTP/1.1 1997년: 가장 많이 사용하며, 우리에게 가장 중요한 버전
  * RFC2068(1997) → RFC2616(1999)(개정) → RFC7230\~7235(2014)(개정)
  * 1.1에 대부분의 기능, 2와 3에서는 성능 개선에 초점
* HTTP/2 2015년: 성능 개선
* HTTP/3 진행중: TCP 대신에 UDP 사용, 성능 개선

#### 기반 프로토콜

* TCP: HTTP/1.1, HTTP/2
* UDP: HTTP/3&#x20;
* 현재 HTTP/1.1을 주로 사용
  * HTTP/2, HTTP/3도 점차 증가

{% hint style="info" %}
기존 TCP는 3 way hanshake를 포함, 내부적으로 작업들이 많아서 신뢰성이나 연결성은 보장되지만 속도가 떨어진다.

이를 해결하고자 UDP프로토콜을 애플리케이션 레벨에서 재설계를 한 것이 HTTP/3이다.
{% endhint %}

#### HTTP 특징

* 클라이언트 서버 구조
* 무상태 프로토콜(스테이스리스), 비연결성
* HTTP 메시지
* 단순함, 확장 가능



### 클라이언트와 서버 구조

* Request Response 구조
* 클라이언트는 서버에 요청을 보내고, 응답을 대기
* 서버가 요청에 대한 결과를 만들어서 응답

클라이언트와 서버의 독립성을 보장.

\-> 더 많은 확장성과 유연성을 가질 수 있다.

\-> 이슈 발생의 경우 해당 부분만 수정이 가능



### 무상태 프로토콜

스테이스리스(Stateless)

* 서버가 클라이언트의 상태를 보존 X
* 장점 : 서버 확장성 높음(스케일 아웃)
* 단점 : 클라이언트가 추가 데이터 전송

{% hint style="info" %}
### 상태 유지(stateful)

* 고객 : 이 노트북 얼마인가요?
* 점원A : 100만원 입니다. (노트북 상태 유지)



* 고객 : 2개 구매하겠습니다.
* 점원A : 200만원 입니다. 신용카드, 현금중 어떤 걸로 구매 하시겠어요?\
  (노트북, 2개 상태 유지)



* 고객 : 신용카드로 구매하겠습니다.
* 점원A : 200만원 결제 완료되었습니다.(노트북, 2개, 신용카드 상태 유지)



해당 과정에서는 점원이 바뀌면 안된다.
{% endhint %}

{% hint style="info" %}
### 무상태(stateless)

* 고객 : 이 노트북 얼마인가요?
* 점원A : 100만원 입니다.



* 고객 : 노트북 2개 구매하겠습니다.
* 점원B : 200만원 입니다. 신용카드, 현금중 어떤 걸로 구매 하시겠어요?



* 고객 : 노트북 2개를 신용카드로 구매하겠습니다.
* 점원C : 200만원 결제 완료되었습니다.
{% endhint %}



#### Stateful, Stateless

* 상태 유지 : 중간에 다른 서버로 바뀌면 안된다.
* 무상태 : 중간에 서버가 바뀌어도 된다.
  * 무상태는 응답 서버를 쉽게 바꿀 수 있다. → 서버 증설에 유리

#### 스케일 아웃 - 수평 확장 유리

![](<../.gitbook/assets/image (13) (1).png>)

#### Statless

#### 실무한계

* 모든 것을 무상태로 설계 할 수 있는 경우도 있고 없는 경우도 있다.
* 무상태
  * ex) 로그인이 필요 없는 단순한 서비스 소개 화면
* 상태 유지
  * ex) 로그인
* 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지
* 일반적으로 브라우저 쿠키와 서버 세션들을 사용해서 상태 유지
* 상태 유지는 최소한만 사용.



### 비 연결성(connectionless)

#### 연결을 유지하는 모델

![](<../.gitbook/assets/image (15).png>)![](<../.gitbook/assets/image (25) (1).png>)



#### 연결을 유지하지 않는 모델

![](<../.gitbook/assets/image (26).png>)![](<../.gitbook/assets/image (27).png>)

#### 비 연결성

* HTTP는 기본이 연결을 유지하지 않는 모델
* 일반적으로 초 단위 이하의 빠른 속도로 응답
* 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시 처리하는 요청은 수십개 이하로 매우 작음
  * ex) 웹 브라우저에서 계속 연속해서 검색 버튼을 누르지는 않는다.
* 서버 자원을 매우 효율적으로 사용할 수 있음

#### 한계와 극복

* TCP/IP 연결을 새로 맺어야 함 - 3 way handshake 시간 추가
* 웹 브라우저로 사이트를 요청하면 HTMl 뿐만 아니라 자바스크립트, css, 추가 이미지 등등 수 많은 자원이 함께 다운로드
* 지금은 HTTP 지속 연결(Persistent Connextions)로 문제 해결
* HTTP/2, HTTP/3에서 더 많은 최적화

#### HTTP 초기 - 연결, 종료 낭비

![](<../.gitbook/assets/image (4).png>)

#### HTTP 지속 연결(Persistent Connections)

![](<../.gitbook/assets/image (3).png>)

{% hint style="info" %}
**스테이스리스를 기억하자**

* 같은 시간에 딱 맞추어 발생하는 대용량 트래픽
* ex) 선착순 이벤트, 명절 KTX예약,  수업 등록
* 수만명 동시 요청
{% endhint %}



### HTTP 메시지

#### HTTP 메시지 구조

![HTTP 메시지 구조](<../.gitbook/assets/image (21).png>)



#### HTTP 요청 메시지

![HTTP 요청 메시지 ](<../.gitbook/assets/image (18) (1) (1).png>)

![](<../.gitbook/assets/image (18) (1).png>)

* start-line = request-line
* request-line = method SP(공백) request-target SP HTTP-version CRLF(엔터)



* HTTP 메서드 (Get)
* 요청 대상(/search?q=hello\&hl=ko)
* HTTP Version

#### 종류 : GET, POST, PUT, DELETE, ...

* 서버가 수행해야 할 동작 지정
  * GET : 리소스 조회
  * POST : 요청 내역 처리

#### 요청 대상

* absoulte-path\[?query] (절대경로\[?쿼리])
* 절대 경로 = "/" 로 시작하는 경로
* 참고 : \*, http://....?x=y

#### HTTP 응답 메시지

![HTTP 응답 메시지](<../.gitbook/assets/image (12).png>)



![](<../.gitbook/assets/image (32).png>)

* start-line = status-line
* status-line = HTTP-version SP status-code SP reason-phrase CRLF



* HTTP 버전
* HTTP 상태 코드 : 요청 성공, 실패를 나타냄
  * 200 : 성공
  * 400 : 클라이언트 요청 오류
  * 500 : 서버 내부 오류
* 이유 문구 : 사람이 이해할 수 있는 짧은 상태 코드 설명 글 (ex. OK)

#### HTTP 헤더

![](<../.gitbook/assets/image (45).png>)

* header-field = field-name ":" OWS field-value OWS (OWS:띄어쓰기 채용)
* field-name 은 대소문자 구문 없음

#### 용도

* HTTP 전송에 필요한 모든 부가정보\
  ex) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보, ....
* 표준 헤더가 너무 많다.
* 필요시 임의의 헤더 추가 가능



#### HTTP 메시지 바디

![](<../.gitbook/assets/image (13).png>)&#x20;

* 실제 전송할 데이터
* HTML 문서, 이미지, 영상, JSON 등등 byte로 표현할 수 있는 모든 데이터 전송 가능



#### 단순함 확장 가능&#x20;

* HTTP는 단순하다.
* HTTP 메시지도 매우 단순
* 크게 성공하는 표준 기술은 단순하지만 확장 가능한 기술

{% hint style="info" %}
### HTTP 정리

* HTTP 메시지에 모든 것을 전송
* HTTP 역사 HTTP/1.1을 기준으로 학습
* 클라이언트 서버 구조
* 무상태 프로토콜(스테이스리스)
* HTTP 메시지
* 단순함. 확장 가능
{% endhint %}

&#x20;

## HTTP 메서드

### HTTP API를 만들어 보자.

#### API URI 고민

가장 중요한 것은 **리소스 식별**

\-> URI는 리소스만 식별

* 리소스의 의미?
  * 회원을 등록하고 수정하고 조회하는게 리소스가 아니다!
  * 회원이라는 개념 자체가 바로 리소스다.
* 리소스를 식별하는 방법?
  * 회원을 등록하고 수정하고 조회하는 것을 모두 배제
  * 회원이라는 리소스만 식별. -> 회원 리소스를 URI에 매핑
* 리소스와 해당 리소스를 대상으로 하는 행위를 분리
  * 리소스 : 회원
  * 행위 : 조회, 등록, 삭제, 변경 (메서드 ex. GET, POST, DELETE, PUT, ... )

#### API URI 설계(리소스 식별, URI 계층 구조 활용)

* 회원 목록 조회  /members
* 회원 조회 /members/{id}
* 회원 등록 /members/{id}
* 회원 수정 /members/{id}
* 회원 삭제 /members/{id}

### HTTP 메서드 - GET,POST

#### HTTP 메서드 종류

**주요 메서드**

* GET : 리소스 조회
* POST : 요청 데이터 처리, 주로 등록에 사용
* PUT : 리소스를 대체, 해당 리소스가 없으면 생성
* PATCH : 리소스 부분 변경
* DELETE : 리소스 삭제

**기타 메서드**

* HEAD : GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
* OPTIONS : 대상 리소스에 대한 통신 기능 옵션(메서드)을 설명(주로 CORS에서 사용)
* CONNECT : 대상 자원으로 식별되는 서버에 대한 터널을 설정
* TRACE : 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

![GET](<../.gitbook/assets/image (18).png>)

#### GET

* 리소스 조회
* 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
* 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않음

![POST](<../.gitbook/assets/image (44).png>)

#### POST

* 요청 데이터 처리
* 메시지 바디를 통해 서버로 요청 데이터 전달
* 서버는 요청 데이터를 처리
  * 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행
* 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용

{% hint style="info" %}
### 요청 데이터 처리 예시

#### 스팩 : POST 메서드는 대상 리소스가 리소스의 고유한 의미 체계에 따라 요청에 포함된 표현을 처리하도록 요청합니다.

* HTML 양식에 입력된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공
  * ex) HTML FORM에 입력한 정보로 회원 가입, 주문 등에서 사용
* 게시판, 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시
  * ex) 게시판 글쓰기, 댓글 달기
* 서버가 아직 식별하지 않은 새 리소스 생성
  * ex) 신규 주문 생성
* 기존 자원에 데이터 추가
  * ex) 한 문서 끝에 내용 추가하기

#### 리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야 함.
{% endhint %}

1. 새 리소스 생성(등록)
   1. 서버가 아직 식별하지 않은 새 리소스 생성
2. 요청 데이터 처리
   1. 단순히 데이터를 생성하거나, 변경하는 것을 넘어서 프로세스를 처리해야 하는 경우\
      ex) 결제완료 → 배달 시작 → 배달 완료
   2. POST의 결과로 새로운 리소스가 생성되지 않을 수도 있음\
      ex) POST /orders/{orderId}/start-delivery (컨트롤URI)
3. 다른 메서드로 처리하기 애매한 경우
   1. ex) JSON으로 조회 데이터를 넘겨야 하는데, GET 메서드를 사용하기 어려운 경우
   2. 애매하면 POST

