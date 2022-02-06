---
description: Inflearn '모든 개발자를위한 HTTP 웹 기본 지식' 강의 내용 및 이미지를 통한 정리.
---

# HTTP 웹 기본 지식

## 1. 인터넷 네트워크

### 인터넷 통신

![](<../.gitbook/assets/image (18) (1) (1) (1) (1).png>)

클라이언트에서 다른 클라이언트(컴퓨터)로 데이터를 보낼경우 위치에따라 위성, 해저광케이블, 기타 통신서버와 같은 노드들을 거쳐서 상대 클라이언트에 도달한다.

### IP(인터넷 프로토콜)

송신/수신 클라이언트에서 정보를 주고받을 때 사용하는 정보 위주의 프로토콜

![](<../.gitbook/assets/image (3) (1).png>)

#### 역할

* 지정한 IP 주소(IP Adress)에 데이터 전달
* 패킷이라는 통신 단위로 데이터 전달

#### IP 패킷 정보

* 패킷은 전송하고자 하는 데이터의 한 블록(payload)과 주소지 정보(발신지 주소, 목적지 주소), 관리정보(Header, IPv6와 같이 망이 패킷을 목적지까지 전달하는데 필요한)로 구성된다.

![](<../.gitbook/assets/image (15) (1) (1) (1) (1).png>)

#### **IP 프로토콜의 한계**

* 비연결성
  * 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
* 비신뢰성
  * 패킷 유실?
  * 패킷의 순서?
* 프로그램 구분
  * 동일한 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상인 경우



### TCP, UDP

![인터넷 프로토콜 스택의 4계층](<../.gitbook/assets/image (9) (1).png>)

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



![](<../.gitbook/assets/image (12) (1) (1) (1).png>)

**TCP 특징**

전송 제어 프로토콜(Transmission Control Protocol)

* 연결 지향 - TCP 3 way handshake (가상 연결)
* 데이터 전달 보증
* 순서 보장
* 신뢰할 수 있는 프로토콜
* 현재는 대부분 TCP 사용



![](<../.gitbook/assets/image (21) (1) (1).png>)

{% hint style="info" %}
SYN : 접속 요청&#x20;

ACK : 요청 수락

참고 : 3.ACK와 함께 데이터 전송 가능
{% endhint %}

![](<../.gitbook/assets/image (31) (1).png>)

**데이터 전달 보증**

1. 데이터 전송
2. 데이터 수신 확인 메시지

![](<../.gitbook/assets/image (1) (1) (1).png>)

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

![](<../.gitbook/assets/image (6) (1).png>)



### DNS

**도메인 네임 시스템(Domain Name System)**

* IP는 기억하기 어렵다.
* IP는 변경될 수 있다.

\->  DNS 사용으로 해결.

* 전화번호부와 같은 역할
* 도메인 명을 IP 주소로 변환

![](<../.gitbook/assets/image (14) (1) (1) (1) (1) (1).png>)

## 2. URI와 웹 브라우저 요청 흐름



### URI

{% hint style="info" %}
URI(Uniform Resource Identifier)

URI는 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다.
{% endhint %}

![](<../.gitbook/assets/image (43).png>)

![](<../.gitbook/assets/image (30) (1) (1).png>)

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

![](<../.gitbook/assets/image (7) (1).png>)

1. DNS 조회
2. HTTPS PORT 생략(443)
3. HTTP 요청 메시지 생성

&#x20;![](<../.gitbook/assets/image (24) (1) (1).png>)



#### HTTP 메시지 전송

![](<../.gitbook/assets/image (1) (1).png>)

1. 웹 브라우저가 HTTP 메세지를 생성
2. SOCKET 라이브러리를 통해 TCP/IP계층에 전달\
   이전단계에서 찾은 IP와 PORT정보를 가지고 SYN, SYN+ACK, ACK 과정을 통해 서버와 연결\
   연결이 성공되면 TCP/IP 계층으로 데이터를 전달
3. TCP/IP 패킷을 생성, HTTP 메세지도 포함

![HTTP 메시지](<../.gitbook/assets/image (10) (1) (1).png>)

4\. 서버는 패킷을 받으면 패킷의 내부 HTTP 메서드를 해석해서 정보에 맞는 동작.

![](<../.gitbook/assets/image (11) (1) (1) (1).png>)

5\. 서버에서 HTTP 응답 메세지를 생성

![](<../.gitbook/assets/image (16) (1) (1) (1) (1).png>)

6\. 클라이언트에서는 응답메세지를 받아 맞는 동작(ex: 렌더링)



## 3. HTTP 기본

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

![](<../.gitbook/assets/image (13) (1) (1) (1).png>)

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

![](<../.gitbook/assets/image (15) (1) (1) (1).png>)![](<../.gitbook/assets/image (25) (1) (1).png>)



#### 연결을 유지하지 않는 모델

![](<../.gitbook/assets/image (26) (1) (1).png>)![](<../.gitbook/assets/image (27) (1) (1) (1) (1).png>)

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

![](<../.gitbook/assets/image (4) (1).png>)

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

![HTTP 메시지 구조](<../.gitbook/assets/image (21) (1).png>)



#### HTTP 요청 메시지

![HTTP 요청 메시지 ](<../.gitbook/assets/image (18) (1) (1) (1).png>)

![](<../.gitbook/assets/image (18) (1) (1).png>)

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

![HTTP 응답 메시지](<../.gitbook/assets/image (12) (1) (1).png>)



![](<../.gitbook/assets/image (32) (1) (1).png>)

* start-line = status-line
* status-line = HTTP-version SP status-code SP reason-phrase CRLF



* HTTP 버전
* HTTP 상태 코드 : 요청 성공, 실패를 나타냄
  * 200 : 성공
  * 400 : 클라이언트 요청 오류
  * 500 : 서버 내부 오류
* 이유 문구 : 사람이 이해할 수 있는 짧은 상태 코드 설명 글 (ex. OK)

#### HTTP 헤더

![](<../.gitbook/assets/image (45) (1).png>)

* header-field = field-name ":" OWS field-value OWS (OWS:띄어쓰기 채용)
* field-name 은 대소문자 구문 없음

#### 용도

* HTTP 전송에 필요한 모든 부가정보\
  ex) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보, ....
* 표준 헤더가 너무 많다.
* 필요시 임의의 헤더 추가 가능



#### HTTP 메시지 바디

![](<../.gitbook/assets/image (13) (1) (1).png>)&#x20;

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

## 4. HTTP 메서드

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

![GET](<../.gitbook/assets/image (18) (1).png>)

#### GET

* 리소스 조회
* 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
* 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않음

![POST](<../.gitbook/assets/image (44) (1).png>)

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



### HTTP 메서드 - PUT, PATCH, DELETE

![PUT](<../.gitbook/assets/image (13) (1).png>)

#### PUT

* 리소스를 **완전히 대체**
  * 리소스가 있으면 대체
  * 리소스가 없으면 생성
  * 덮어쓰기
* 클라이언트가 리소스를 식별
  * 클라이언트가 리소스 위치를 알고 URI 지정
  * POST와 차이점

![PATCH](<../.gitbook/assets/image (19) (1).png>)

#### PATCH

* 리소스 부분 변경

![DELETE](<../.gitbook/assets/image (12) (1).png>)

#### DELETE

* 리소스 제거

### HTTP 메서드의 속성

* 안전(Safe Methods)
  * 호출해도 리소스를 변경하지 않는다.
* 멱등(Idempotent Methods)
  * f(f(x)) = f(x)
  * 한번 호출하든 두번 호출하든 100번 호출하든 결과가 같다.
  * 멱등 메서드
    * GET : 한번 조회, 두번 조회하든 같은 결과 조회된다.
    * PUT : 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.
    * DELETE : 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 같다.
    * POST : **멱등이 아니다!** 두 번 호출하면 같은 결제가 중복해서 발생할 수 있다.
  * 활용
    * 자동 복구 메커니즘
    * 서버가 TIMEOUT 등으로 정상 응답을 못 주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가? 판단 근거
  * 외부 요인으로 중간에 리소스가 변경되는 것 까지는 고려하지 않는다.
* 캐시가능(Cacheable Methods)
  * 응답 결과 리소스를 캐시해서 사용해도 되는가?
  * GET, HEAD, POST, PATCH 캐시가능
  * 실제로는 GET,HEAD 정도만 캐시로 사용
    * POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 쉽지 않음.

![출처 : 위키 피디아](<../.gitbook/assets/image (20) (1) (1) (1).png>)

## 5. HTTP 메서드 활용

### 클라이언트에서 서버로 데이터 전송

#### 데이터 전달 방식은 크게 2가지

* 쿼리 파라미터를 통한 데이터 전송
  * GET
  * 주로 정렬 필터(검색어)
* 메시지 바디를 통한 데이터 전송
  * POST, PUT, PATCH
  * 회원 가입, 상품 주문, 리소스 등록, 리소스 변경

#### 4가지 상황

* 정적 데이터 조회
  * 이미지, 정적 텍스트 문서
  * 조회는 GET 사용
  * 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능
* 동적 데이터 조회
  * 주로 검색, 게시판 목록에서 정렬 필터(검색어)
  * 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
  * 조회는 GET 사용
  * GET은 쿼리 파라미터 사용해서 데이터 전달
* HTML Form을 통한 데이터 전송
  * HTML Form submit 시 POST 전송
    * ex) 회원 가입, 상품 주문, 데이터 변경
  * Content-Type: application/x-www-form-urlencoded 사용
    * form의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)
    * 전송 데이터를 url encoding 처리
      * ex) abc김 → abc%EA%B9%80
  * HTML Form은 GET 전송도 가능 → GET 전송 저장은 보안상 좋지 않다 사용 X
  * Content-Type: multipart/form-data
    * 파일 업로드 같은 바이너리 데이터 전송시 사용
    * 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능(그래서 이름이 multipart)
  * 참고: HTML Form 전송은 GET, POST만 지원
* HTTP API를 통한 데이터 전송
  * 서버 to 서버
    * 백엔드 시스템 통신
  * 앱 클라이언트
    * 아이폰, 안드로이드
  * 웹 클라이언트
    * HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
    * ex) React, VueJs 같은 웹 클라이언트와 API 통신
  * POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송
  * GET: 조회, 쿼리 파라미터로 데이터 전달
  * Content-Type: application/json 을 주로 사용(사실상 표준)
    * TEXT. XML, JSON 등등

### HTTP API 설계 예시

1. HTTP API - 컬렉션
   1. POST 기반 등록
   2. ex) 회원 관리 API 제공
2. HTTP API - 스토어
   1. PUT 기반 등록
   2. ex) 정적 컨텐츠 관리, 원격 파일 관리
3. HTML FORM 사용
   1. 웹 페이지 회원 관리
   2. GET, POST만 지원

####

#### 회원 관리 시스템

#### 1. API 설계 - POST 기반 등록

* 회원 목록 /members → GET
* 회원 등록 /members → POST
* 회원 조회 /members/{id} → GET
* 회원 수정 /members{id} → PATCH, PUT, POST
* 회원 삭제 /members{id} → DELETE

#### POST - 신규 자원 등록 특징

* 클라이언트는 등록될 리소스의 URI를 모른다.
  * 회원 등록 /members → POST
  * POST /members
* 서버가 새로 등록된 리소스 URI를 생성해준다.
  * HTTP/1.1 201 Created Location: /members/100
* 컬렉션(Coleection)
  * 서버가 관리하는 리소스 디렉토리
  * 서버가 리소스의 URI를 생성하고 관리
  * 여기서 컬렉션은 /members

####

#### 파일 관리 시스템

#### 2. API 설계 - PUT 기반 등록

* 파일 목록 /files → GET
* 파일 조회 /files/{filename} → GET
* 파일 등록 /files/{filename} → PUT
* 파일 삭제 /files/{filename} → DELETE
* 파일 대량 등록 /files → POST

#### PUT - 신규 자원 등록 특징

* 클라이언트가 리소스 URI를 알고 있어야 한다.
  * 파일 등록 /files/{filename} → PUT
  * PUT /files/star.jpg
* 클라이언트가 직접 리소스의 URI를 지정한다.
* 스토어(Store)
  * 클라이언트가 관리하는 리소스 저장소
  * 클라이언트가 리소스의 URI를 알고 관리
  * 여기서 스토어는 /files



3\. HTML FORM 사용

* HTML FORM은 GET, POST만 지원
* AJAX 같은 기술을 사용해서 해결 가능 → 회원 API 참고
* 여기서는 순수 HTML, HTML FORM 이야기
* GET, POST만 지원하므로 제약이 있음
* 회원 목록 /members → GET
* 회원 등록 폼 /members/new → GET
* 회원 등록 /members/new, /members → POST
* 회원 조회 /members/{id} → GET
* 회원 수정 폼 /members/{id}/edit → GET
* 회원 수정 /members/{id}/edit, /members/{id} → POST
* 회원 삭제 /members/{id}/delete → POST



* HTML FORM은 GET,POST만 지원
* 컨트롤 URI
  * GET, POST만 지원하므로 제약이 있음
  * 이런 제약을 해결하기 위해 **동사**로 된 리소스 경로 사용
  * POST의 /new, /edit, /delete가 컨트롤 URI
  * HTTP 메서드로 해결하기 애매한 경우 사용(HTTP API 포함)



### 참고하면 좋은 URI 설계 개념

* 문서(document)
  * 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
  * ex) /members/100, /files/star.jpg
* 컬렉션(collection)
  * 서버가 관리하는 리소스 디렉토리
  * 서버가 리소스의 URI를 생성하고 관리
  * ex) /members
* 스토어(store)
  * 클라이언트가 관리하는 자원 저장소
  * 클라이언트가 리소스의 URI를 알고 관리
  * ex) /files
* 컨트롤러(controller), 컨트롤 URI
  * 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
  * 동사를 직접 사용
  * ex) /members/{id}/delete

{% hint style="info" %}
참고:  https://restfulapi.net/resource-naming
{% endhint %}





## 6. HTTP 상태코드

### HTTP 상태코드 소개

#### 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능

* 1xx (Informational) : 요청이 수신되어 처리중
* 2xx (Successful) : 요청 정상 처리
* 3xx (Redirection) : 요청을 완료하려면 추가 행동이 필요
* 4xx (Client Error) : 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
* 5xx (Server Error) : 서버 오류, 서버가 정상 요청을 처리하지 못함

{% hint style="info" %}
### 만약 모르는 상태 코드가 나타나면?

* 클라이언트가 인식할 수 없는 상태코드를 서버가 반환하면?
* 클라이언트는 상위 상태코드로 해석해서 처리
* 미래에 새로운 상태 코드가 추가되어도 클라이언트를 변경하지 않아도 됨
* ex)
  * 299 ??? → 2xx (Successful)
  * 451 ??? → 4xx (Client Error)
  * 599 ??? → 5xx (Server Error)
{% endhint %}

### 1xx (Informational)

요청이 수신되어 처리중

* 거의 사용하지 않는다.

### 2xx (Succesful)

클라이언트의 요청을 성공적으로 처리

* 200 OK
  * 요청 성공
* 201 Created
  * 요청 성공해서 새로운 리소스가 생성됨
  * 생성된 리소스는 응답의 Location 헤더 필드로 식별
* 202 Accepted
  * 요청이 접수 되었으나 처리가 완료되지 않았음
  * 배치 처리 같은 곳에서 사용
  * ex) 요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리함
* 204 No Content
  * 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음
  * ex) 웹 문서 편집기에서 save 버튼
  * save 버튼의 결과로 아무 내용이 없어도 된다.
  * save 버튼을 눌러도 같은 화면을 유지해야 한다.
  * 결과 내용이 없어도 204 메시지(2xx)만으로 성공을 인식할 수 있다.

### 3xx (Redirection)

요청을 완료하기 위해 유저 에이전트의 추가 조치 필요

* 300 Multiple Choices
* 301 Moved Permanently
* 302 Found
* 303 See Other
* 304 Not Modified
* 307 Temporary Redirect
* 308 Permanent Redirect

#### 리다이렉션의 이해

웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리 다이렉트)

![](<../.gitbook/assets/image (20) (1) (1).png>)

#### 종류

* 영구 리다이렉션 - 특정 리소스의 URI가 영구적으로 이동
  * ex) /members → /users
  * ex) /event → /new-event
* 일시 리다이렉션 - 일시적인 변경
  * 주문 완료 후 주문 내역 화면으로 이동
  * PRG : POST/Redirect/Get
* 특수 리다이렉션
  * 결과 대신 캐시를 사용

#### 영구 리다이렉션 (301, 308)

* 리소스의 URI가 영구적으로 이동
* 원래의 URL를 사용 X, 검색 엔진 등에서도 변경 인지
* 301 Moved Permanently
  * 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)

![](<../.gitbook/assets/image (9).png>)

* 308 Permanent Redirect
  * 301과 기능은 같음
  * 리다이렉트시 요청 메서드와 본문 유지(처음 POST를 보내면 리다이렉트도 POST)

![](<../.gitbook/assets/image (11) (1).png>)

#### 일시적인 리다이렉션 (302, 307,303)

* 리소스의 URI가 일시적으로 변경
* 따라서 검색 엔진 등에서 URL을 변경하면 안됨
* 302 FOUND
  * 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)
* 307 Temporary Redirect
  * 302와 기능은 같음
  * 리다이렉트시 요청 메서드와 본문 유지(요청 메서드를 변경하면 안된다. MUST NOT)
* 303 See Other
  * 302와 기능은 같음
  * 리다이렉트시 요청 메서드가 GET으로 변경

#### PRG : Post/Redirect/Get

일시적인 리다이렉션 - 예시

* POST로 주문 후에 웹 브라우저를 새로고침하면?
* 새로고침은 다시 요청
* 중복 주문이 될 수 있다.

![](<../.gitbook/assets/image (23).png>)

* POST로 주문후에 새로 고침으로 인한 중복 주문 방지
* POST로 주문후에 주문 결과 화면을 GET 메서드로 리다이렉트
* 새로고침해도 결과 화면을 GET으로 조회
* 중복 주문 대신에 결과 화면만 GET으로 다시 요청

![](<../.gitbook/assets/image (22).png>)

* PRG 이후 리다이렉트
  * URL이 이미 POST → GET으로 리다이렉트 됨
  * 새로 고침 해도 GET으로 결과 화면만 조회

{% hint style="info" %}
### 그래서 무엇을 써야할까

* 역사
  * 처음 302 스펙의 의도는 HTTP 메서드를 유지하는 것
  * 그런데 웹 브라우저들이 대부분 GET으로 바꾸어버림(일부는 다르게 동작)
  * 그래서 모호한 302를 대신하는 명확한 307, 303이 등장함(301 대응으로 308도 등장)
* 현실
  * 307, 303을 권장하지만 현실적으로 이미 많은 애플리케이션 라이브러리들이 302를 기본값으로 사용
  * 자동 리다이렉션시에 GET으로 변해도 되면 그냥 302를 사용해도 큰 문제 없음

#### 기타 리다이렉션 (300, 304)

* 300 Miltiple Choices : 안쓴다.
* 304 Not Modified
  * 캐시를 목적으로 사용
  * 클라이언트에게 리소스가 수정되지 않았음을 알려준다. 따라서 클라이언트는 로컬PC에 저장된 캐시를 재사용한다. (캐시로 리다이렉트 한다.)
  * 304 응답은 응답에 메시지 바디를 포함하면 안된다. (로컬 캐시를 사용해야 하므로)
  * 조건부 GET, HEAD 요청시 사용
{% endhint %}

### 4xx (Client Error)

클라이언트 오류

* 클라이언트의 요청에 잘못된 문법등으로 서버가 요청을 수행할 수 없음
* 오류의 원인이 클라이언트에 있음
* 중요! 클라이언트가 이미 잘못된 요청, 데이터를 보내고 있기 때문에, 똑같은 재시도가 실패함

400 Bad Request

클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음.

* 요청 구문, 메시지 등등 오류
* 클라이언트는 요청 내용을 다시 검토하고, 보내야함
* ex) 요청 파라미터가 잘못되거나, API 스펙이 맞지 않을 때

401 Unauthorized

클라이언트가 해당 리소스에 대한 인증이 필요함

* 인증(Authentication) 되지 않음
* 401 오류 발생시 응답에 WWW-Authenticate 헤더와 함께 인증 방법을 설명
* 참고
  * 인증(Authentication) : 본인이 누구인지 확인, (로그인)
  * 인가(Authorization) : 권한 부여 (ADMIN 권한처럼 특정 리소스에 접근할 수 있는 권한, 인증이 있어야 인가가 있음)
  * 오류 메시지가 Unauthorized 이지만 인증 되지 않음 (이름이 아쉬움)

403 Forbidden

서버가 요청을 이해했지만 승인을 거부함

* 주로 인증 자격 증명은 잇지만, 접근 권한이 불충분한 경우
* ex) 어드민 등급이 아닌 사용자가 로그인은 했지만, 어드민 등급의 리소스에 접근하는 경우

404 Not Found

요청 리소스를 찾을 수 없음

* 요청 리소스가 서버에 없음
* 또는 클라이언트가 권한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶을 때

### 5xx (Server Error)

서버 오류

* 서버 문제로 오류 발생
* 서버에 문제가 있기 때문에 재시도 하면 성공할 수도 있음(복구 되거나 등등)

500 Internal Server Error

서버 문제로 오류 발생, 애매하면 500 오류

* 서버 내부 문제로 오류 발생
* 애매하면 500 오류

503 Service Unavailable

서비스 이용 불가

* 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음
* Retry-After 헤더 필드로 얼마뒤에 복구되는지 보낼 수도 있음

{% hint style="danger" %}
Server Error는 반드시 서버 자체에 문제가있는 경우에 활용.

아래와같은 비지니스 로직의 문제는 서버 에러에 해당되지 않는다.

ex) 출금하려는 고객의 잔고가 부족, 19금에 청소년 접근
{% endhint %}

## 7. HTTP 헤더1 - 일반 헤더

### HTTP 헤더 개요

#### 용도

* HTTP
* ex) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보,...
* 표준 헤더가 너무 많음
* 필요시 임의의 헤더 추가 가능



분류 - RFC2616(과거)

![header](<../.gitbook/assets/image (41).png>)

* General header : 메시지 전체에 적용되는 정보, ex) Connection: close
* Request header : 요청 정보, ex) User-Agent:Mozilla/5.0 (Macintosh;..)
* Response header : 응답 정보, ex) Server: Apache
* Entity header : 엔티티 바디 정보, ex) Content-Type: text/html, Content-Length: 3423

![message body](<../.gitbook/assets/image (40).png>)

* 메시지 본문은 엔티티 본문을 전달하는데 사용
* 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
* 엔티티 헤더는 엔티티 본문의 데이터를 해석할 수 있는 정보 제공
  * 데이터 유형(html, json), 데이터 길이, 압축 정보

RFC2616 -> RFC7230 \~ 7235 표준 변경

* 엔티티(Entity) → 표현(Representation)
* Representation = representation Metadata + Representation Data
* 표현 = 표현 메타데이터 + 표현 데이터

![message body - RFC7230(최신)](<../.gitbook/assets/image (14) (1).png>)

* 메시지 본문(message body)을 통해 표현 데이터 전달
* 메시지 본문 = 페이로드(payload)
* 표현은 요청이나 응답에서 전달할 실제 데이터
* 표현 헤더는 표현 데이터를 해석할 수 있는 정보를 제공
  * 데이터 유형(html,json), 데이터 길이, 압축 정보 등등
* 참고 : 표현 헤더는 표현 메타데이터와, 페이로드 메시지를 구분해야 하지만, 이를 생략

### 표현

![](<../.gitbook/assets/image (15) (1) (1).png>)

* Content-Type: 표현 데이터의 형식
* Content-Encoding: 표현 데이터의 압축 방식
* Content-Language: 표현 데이터의 자연 언어
* Content-Length: 표현 데이터의 길이
* 표현 헤더는 전송, 응답 둘다 사용

#### Content-Type

표현 데이터의 형식 설명

* 미디어 타입, 문자 인코딩
* ex)
  * text/html; charset=utf-8
  * application/json
  * image/png

#### Content-Encoding

표현 데이터 인코딩

* 표현 데이터를 압축하기 위해 사용
* 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
* 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
* ex)
  * gzip
  * deflate
  * identity

#### Content-Language

표현 데이터의 자연 언어

* 표현 데이터의 자연 언어를 표현
* ex)
  * ko
  * en
  * en-US

#### Content-Length

표현 데이터의 길이

* 바이트 단위
* Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨

### 콘텐츠 협상(콘텐츠 네고시에이션)

클라이언트가 선호하는 표현 요청

* Accept: 클라이언트가 선호하는 미디어 타입 전달
* Accept-Charset: 클라이언트가 선호하는 문자 인코딩
* Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
* Accept-Language: 클라이언트가 선호하는 자연 언어
* 협상 헤더는 요청시에만 사용

#### Accept-Language 적용 전

![](<../.gitbook/assets/image (36).png>)

#### Accept-Language 적용 후

![](<../.gitbook/assets/image (44).png>)

#### Accept-Language 복잡한 예시

![](<../.gitbook/assets/image (25).png>)

#### 협상과 우선순위 1

Quality Values(q)

![](<../.gitbook/assets/image (18).png>)

* Quality Values(q) 값 사용
* 0\~1, 클수록 높은 우선 순위
* 생략하면 1
* Accept-Language: ko-KR,ko;q=0/9,en-US;q=0.8,en;q=0.7
  1. ko-KR;q=1 (q생략)
  2. ko;q=0.9
  3. en-US;q=0.8
  4. en;q=0.7

#### 협상과 우선순위 2

Quality Values(q)

* 구체적인 것이 우선한다.
* Accept: text/\*, text/plain, text/plain;format=flowed, **/**
  1. text/plain;format=flowed
  2. text/plain
  3. text/\*
  4. _/_

#### 협상과 우선순위 3

Quality Values(q)

![](<../.gitbook/assets/image (15) (1).png>)

* 구체적인 것을 기준으로 미디어 타입을 맞춘다.
* Accept: text/\*;q=0.3, text/html;q=0.7, text/html;level=1, text.html;level=2;q=0.4, _/_;q=0.5

### 전송 방식

* 단순 전송
* 압축 전송
  * Content-Encoding 추가
* 분할 전송
  * Transfer-Encoding 추가
  * Content-Length 사용 X
* 범위 전송
  * Content-Range 추가

### 일반 정보

* From(유저 에이전트의 이메일 정보)
  * 일반적으로 잘 사용되지 않음
  * 검색 엔진 같은 곳에서, 주로 사용
  * 요청에서 사용
* Referer(이전 웹 페이지 주소)
  * 현재 요청된 페이지의 이전 웹 페이지 주소
  * A → B 로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
  * Referer를 사용해서 유입 경로 분석 가능
  * 요청에서 사용
  * 참고 : referer는 단어 referrer의 오타
* User-Agent(유저 에이전트 애플리케이션 정보)
  * user-agent: Mozila/5.0 (Macintosh; Intel Mac OS X 10\_15\_7) AppleWebKit/537.36 (LHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36)
  * 클라이언트의 애플리케이션 정보(웹 브라우저 정보, 등등)
  * 통계 정보
  * 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
  * 요청에서 사용
* Server(요청을 처리하는 ORIGIN 서버의 소프트웨어 정보)
  * Server : Apache/2.2.22 (Debian)
  * server : nginx
  * 응답에서 사용
* Date(메시지가 발생한 날짜와 시간)
  * Date: Tue, 15 Nov 1994 08:12:31 GMT
  * 응답에서 사용

### 특별한 정보

![](<../.gitbook/assets/image (45).png>)

* Host(요청한 호스트 정보(도메인))
  * 요청에서 사용
  * 필수
  * 하나의 서버가 여러 도메인을 처리해야 할 때
  * 하나의 IP 주소에 여러 도메인이 적용되어 있을 때
* Location(페이지 리다이렉션)
  * 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동
  * 201 (Created) : Location 값은 요청에 의해 생성된 리소스 URI
  * 3xx (Redirection) : Location 값은 요청을 자동으로 리다이렉션하기 위한 대상 리소스를 가리킴
* Allow(허용 가능한 HTTP 메서드)
  * 405 (Method Not Allowed) 에서 응답에 포함 해야함
  * Allow : GET, HEAD, PUT
* Retry-After(유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간)
  * 503 (Service Unavailable) : 서비스가 언제까지 불능인지 알려줄 수 있음
  * Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
  * Retry-After: 120(초단위 표기)

### 인증

* Authorization(클라이언트 인증 정보를 서버에 전달)
* Authorization: Basic xxxxxxxxxxxxxxxxxxxxxxxx
* WWW-Authenticate(리소스 접근시 필요한 인증 방법 정의)
  * 리소스 접근시 필요한 인증 방법 정의
  * 401 Unauthorized 응답과 함께 사용
  * WWW-Authenticate : Newauth realm=”apps”,type=1,title=”Login to\”apps\””,Basic realm=”simple”

### 쿠키

* Set-Cookie : 서버에서 클라이언트로 쿠키 전달(응답)
* Cookie : 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

#### 쿠키 미사용

![](<../.gitbook/assets/image (16) (1).png>)

Stateless

* HTTP는 무상태(Stateless) 프로토콜
* 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다.
* 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다.
* 클라이언트와 서버는 서로 상태를 유지하지 않는다.

#### 대안 - 모든 요청에 사용자 정보 포함?

→ 모든 요청에 사용자 정보가 포함되도록 개발 해야함

→ 브라우저를 완전히 종료하고 다시 열면?

이를 해결하기 위해 쿠키를 사용

####

#### 쿠키 로그인

![](<../.gitbook/assets/image (24).png>)

![](<../.gitbook/assets/image (27) (1) (1) (1).png>)

![](<../.gitbook/assets/image (26) (1).png>)

* ex)set-cookie:sessionId=abcde1234; expires=Sat,26-Dec-2022:00:00:00 GMT;path=/;[domain=.google.com](http://domain=.google.com);Secure
* 사용처
  * 사용자 로그인 세션 관리
  * 광고 정보 트래킹
* 쿠키 정보는 항상 서버에 전송됨
  * 네트워크 트래픽 추가 유발
  * 최소한의 정보만 사용(세션 id, 인증 토큰)
  * 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 (localStorage, sessionStorage) 참고
* 주의!
  * 보안에 민감한 데이터는 저장하면 안됨(주민번호, 신용카드, 등등)

#### 쿠키 - 생명주기

Expires, max-age

* Set-Cookie : expires=Sat,26-Dec-2022:00:00:00 GMT
  * 만료일이 되면 쿠키 삭제
* Set-Cookie:max-age=3600 (3600초)
  * 0이나 음수를 지정하면 쿠키 삭제
* 세션 쿠키 : 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
* 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지

#### 쿠키 - 도메인

* ex) [domain=example.org](http://domain=example.org)
* 명시 : 명시한 문서 기준 도메인 + 서브 도메인 포함
  * domain=example.org를 지정해서 쿠키 생성
    * example.org와 dev.example.org도 쿠키 접근
* 생략 : 현재 문서 기준 도메인만 적용
  * [example.org](http://example.org)에서 쿠키를 생성하고 domain 지정을 생략
  * [example.org](http://example.org)에서만 쿠키 접근
  * dev-example.org는 쿠키 미접근

#### 쿠키 - 경로

* ex) path=/home
* 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
* 일반적으로 path=/루트로 지정
* ex)
  * path =/home 지정
  * /home → 가능
  * /home/level1 → 가능
  * /home/level1/level2 → 가능
  * /hello → 불가능

#### 쿠키 - 보안

* Secure
  * 쿠키는 http,https를 구분하지 않고 전송
  * Secure를 적용하면 https인 경우에만 전송
* HttpOnly
  * XSS 공격 방지
  * 자바스크립트에서 접근 불가(document.cookie)
  * HTTP 전송에만 사용
* SameSite
  * XSRF 공격 방지
  * 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송



## 8. HTTP 헤더2 - 캐시와 조건부 요청

### 캐시 기본 동작

#### 캐시가 없을 때

![](<../.gitbook/assets/image (29).png>)

* 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다.
* 인터넷 네트워크는 매우 느리고 비싸다.
* 브라우저 로딩 속도가 느리다.
* 느린 사용자 경험

#### 캐시 적용

#### 첫번째 요청&#x20;

![](<../.gitbook/assets/image (14).png>)

#### 두번째 요청&#x20;

![](<../.gitbook/assets/image (32) (1).png>)![](<../.gitbook/assets/image (11).png>)

* 캐시 덕분에 캐시 가능 시간 동안 네트워크를 사용하지 않아도 된다.
* 비싼 네트워크 사용량을 줄일 수 있다.
* 브라우저 로딩 속도가 매우 빠르다.
* 빠른 사용자 경험

#### 캐시 시간 초과

* 캐시 유효 시간이 초과하면, 서버를 통해 데이터를 다시 조회하고, 캐시를 갱신한다.
* 이때 다시 네트워크 다운로드가 발생한다.



### 검증 헤더와 조건부 요청1

#### 캐시 시간 초과

* 캐시 유효 시간이 초과해서 서버에 다시 요청하면 다음 두 가지 상황이 나타난다.
  * 서버에서 기존 데이터를 변경함
  * 서버에서 기존 데이터를 변경하지 않음
* 캐시 만료후에도 서버에서 데이터를 변경하지 않음
* 데이터를 전송하는 대신 저장해 두었던 캐시를 재사용 할 수 있다.
* 단 클라이언트의 데이터와 서버의 데이터가 같다는 사실을 확인할 수 있는 방법 필요

#### 검증 헤더 추가

**첫 번째 요청**

![](<../.gitbook/assets/image (28).png>)

![](<../.gitbook/assets/image (27) (1) (1).png>)

**두 번째 요청 - 캐시 시간 초과**

![](<../.gitbook/assets/image (10).png>)

![](<../.gitbook/assets/image (15).png>)



* 캐시 유효 시간이 초과해도, 서버의 데이터가 갱신되지 않으면 304 Not Modified + 헤더 메타 정보만 응답(바디X)
* 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신
* 클라이언트는 캐시에 저장되어 있는 데이터 재활용
* 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드
* 매우 실용적인 해결책

### 검증 헤더와 조건부 요청2

* 검증 헤더
  * 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
  * Last-Modified, ETag
* 조건부 요청 헤더
  * 검증 헤더로 조건에 따른 분기
  * If-Modified-Since : Last-Modified 사용
  * If-Node-Match : ETag 사용
  * 조건이 만족하면 200 OK
  * 조건이 만족하지 않으면 304 Not Modified

#### 예시

If-Modified-Since : 이후에 데이터가 수정되었으면?

* 데이터 미변경 예시
  * 캐시 : 2022년 02월 01일 10:00:00 vs 서버 : 2022년 02월 01일 10:00:00
  * 304 Not Modified, 헤더 데이터만 전송(BODY 미포함)
  * 전송 용량 0.1M (헤더 0.1M, 바디 1.0M)
* 데이터 변경 예시
  * 캐시 : 2022년 02월 01일 10:00:00 vs 서버 : 2022년 02월 01일 11:00:00
  * 200 OK, 모든 데이터 전송(BODY 포함)
  * 전송 용량 1.1M(헤더 0.1M, 바디 1.0M)

#### Last-Modified, If-Modified-Since 단점

* 1초 미만(0.x초) 단위로 캐시 조정이 불가능
* 날짜 기반의 로직 사용
* 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우
* 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
  * ex) 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시를 유지하고 싶은 경우

#### ETage, If-None-Match

* ETag(Entity Tag)
* 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
  * ex) ETag:”v1.0”, ETag: “a2jiodwjekjl3”
* 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시 생성)
  * ex) ETag:”aaaaa” → ETag: “bbbbb”
* 진짜 단순하게 ETag만 보내서 같으면 유지, 다르면 다시 받기.

**ETag 첫 번째 요청**

![](../.gitbook/assets/image.png)

**ETag 두 번째 요청 - 캐시 시간 초과**

![](<../.gitbook/assets/image (30).png>)

![](<../.gitbook/assets/image (6).png>)

#### 정리

* 진짜 단순하게 ETag만 서버에 보내서 같으면 유지, 다르면 다시 받기.
* 캐시 제어 로직을 서버에서 완전히 관리
* 클라이언트는 단순히 이 값을 서버에 제공(클라이언트는 캐시 매커니즘을 모름)
* ex)
  * 서버는 베타 오픈 기간인 3일 동안 파일이 변경되어도 ETag를 동일하게 유지
  * 애플리케이션 배포 주기에 맞추어 ETag 모두 갱신

### 캐시와 조건부 요청 헤더

#### 캐시 제어 헤더

* Cache-Control : 캐시 제어
* Pragma : 캐시 제어(하위 호환)
* Expires : 캐시 유효 기간(하위 호환)

#### Cache-Control

캐시 지시어(directives)

* Cache-Control : max-age
  * 캐시 유효 시간, 초 단위
* Cache-Control : no-cache
  * 데이터는 캐시해도 되지만, 항상 원(origin) 서버에 검증하고 사용
* Cache-Control : no-store
  * 데이터에 민감한 정보가 있으므로 저장하면 안됨(메모리에서 사용하고 최대한 빨리 삭제)

#### Pragma

* Pragma : no-cache
* HTTP 1.0 하위 호환
* 현재는 거의 사용되지 않는다.&#x20;

#### Expires

캐시 만료일 지정(하위 호환)

* expires : Mon, 01 Jan 1990 00:00:00 GMT
* 캐시 만료일을 정확한 날짜로 지정
* HTTP 1.0부터 사용
* 지금은 더 유연한 Cache-Control: max-age 권장
* Cache-Control: max-age와 함께 사용하면 Expires는 무시

### 프록시 캐시

#### 원 서버 직접 접근

**Origin 서버**

![](<../.gitbook/assets/image (26).png>)

#### 프록시 캐시 도입

**첫 번째 요청**

![](<../.gitbook/assets/image (33).png>)

* 한국에 프록시 캐시 서버를 두고 한국의 클라이언트는 프록시 캐시서버를 통해 캐시를 가져온다.
* 여러 사람이 찾은 자료는 이미 캐시에 등록이 되어 있으므로 빠르게 가져올 수 있다.
  * ex) 유튜브 다운로드 속도

#### Cache-Control

캐시 지시어(directives) - 기타

* Cache-Control: public
  * 응답이 public 캐시에 저장되어도 됨
* Cache-Control: private
  * 응답이 해당 사용자만을 위한 것임, private 캐시에 저장해야 함(기본값)
* Cache-Control: s-maxage
  * 프록시 캐시에만 적용되는 max-age
* Age: 60(HTTP 헤더)
  * 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)

### 캐시 무효화

#### Cache-Control

**확실한 캐시 무효화 응답**

* Cache-Control: no-cache, no-stroe, must-revalidate
* Pragma: no-cache
  * HTTP 1.0 하위 호환
* Cache-Control: no-cache
  * 데이터는 캐시해도 되지만, 항상 원 서버에 검증하고 사용(이름에 주의!)
* Cache-Control: no-store
  * 데이터에 민감한 정보가 있으므로 저장하면 안됨 (메모리에서 사용하고 최대한 빨리 삭제)
* Cache-Control: must-revalidate
  * 캐시 만료후 최초 조회시 원 서버에 검증해야함
  * 원 서버 접근 실패시 반드시 오류가 발생해야함 - 504(Gateway Timeout)
  * must-revalidate는 캐시 유효 시간이라면 캐시를 사용함
* Pragma: no-cache
  * HTTP 1.0 하위 호환

#### no-cache vs must-revalidate

**no-cache 기본 동작**

![](<../.gitbook/assets/image (16).png>)

![](<../.gitbook/assets/image (27) (1).png>)

**must-revalidate**

![](<../.gitbook/assets/image (2).png>)

{% hint style="info" %}
확실한 캐시 무효화 응답으로 작성한 헤더를 사용하게 되면 no-cache로 무조건 원 서버에서 검증을 하고, must-revalidate롤 원서버와 검증이 안되면 오류가 발생하도록 한다. 그리고 Pragma를 사용해서 1.0이하 버전의 하위호환도 적용한다.
{% endhint %}

> 왜 no-store만으로 캐시 무효화를 하는 것이 아닌가?
>
>
>
> no-store만으로 캐시가 무효화 되어야 하는 것이 맞지만, HTTP 스펙을 디테일하게 들어가면 모호한 부분들이 존재한다.
>
> 예를 들어서 웹 브라우저에서 앞으로가기, 뒤로가기를 했을 때도 이것을 캐시로 볼 것인가? 이때는 no-store만 보고 판단할 것인가? no-cache를 보고 판단할 것인가?
>
> HTTP 1.1을 지원하지만 조금 오래된 브라우저와의 호환, 그리고 버그
>
> 수 많은 프록시 캐시 업체들과 그 구현 서버 등등...
>
> 이런 문제들 때문에 no-store만으로 해결하지는 못하고, 나머지 옵션들도 함께 사용한다.
>
> ex) 구글이나 네이버 등 주요 메이저 사이트의 응답은 cache-control: no-cache, no-store, must-revalidate를 함께 가져간다.
