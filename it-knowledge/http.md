---
description: Inflearn '모든 개발자를위한 HTTP 웹 기본 지식' 강의 내용 및 이미지를 통한 정리.
---

# HTTP 웹 기본 지식

## 인터넷 네트워크

### 인터넷 통신

![](<../.gitbook/assets/image (18).png>)

클라이언트에서 다른 클라이언트(컴퓨터)로 데이터를 보낼경우 위치에따라 위성, 해저광케이블, 기타 통신서버와 같은 노드들을 거쳐서 상대 클라이언트에 도달한다.

### IP(인터넷 프로토콜)

송신/수신 클라이언트에서 정보를 주고받을 때 사용하는 정보 위주의 프로토콜

![](<../.gitbook/assets/image (3).png>)

#### 역할

* 지정한 IP 주소(IP Adress)에 데이터 전달
* 패킷이라는 통신 단위로 데이터 전달

#### IP 패킷 정보

* 패킷은 전송하고자 하는 데이터의 한 블록(payload)과 주소지 정보(발신지 주소, 목적지 주소), 관리정보(Header, IPv6와 같이 망이 패킷을 목적지까지 전달하는데 필요한)로 구성된다.

![](<../.gitbook/assets/image (15).png>)

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

![](<../.gitbook/assets/image (17).png>)

1. 프로그램이 Hello,world! 메시지(Payload) 생성
2. SOCKET 라이브러리를 통해 전달
3. TCP 정보 생성, 메시지 데이터(Payload) 포함
4. IP 패킷 생성,TCP 데이터 포함.



![](<../.gitbook/assets/image (12).png>)

**TCP 특징**

전송 제어 프로토콜(Transmission Control Protocol)

* 연결 지향 - TCP 3 way handshake (가상 연결)
* 데이터 전달 보증
* 순서 보장
* 신뢰할 수 있는 프로토콜
* 현재는 대부분 TCP 사용



![](<../.gitbook/assets/image (21).png>)

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

![](<../.gitbook/assets/image (14).png>)

## URI와 웹 브라우저 요청 흐름



### URI

{% hint style="info" %}
URI(Uniform Resource Identifier)

URI는 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다.
{% endhint %}

![](<../.gitbook/assets/image (43).png>)

![](<../.gitbook/assets/image (30).png>)

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

![HTTP 메시지](<../.gitbook/assets/image (10).png>)

4\. 서버는 패킷을 받으면 패킷의 내부 HTTP 메서드를 해석해서 정보에 맞는 동작.

![](<../.gitbook/assets/image (11).png>)

5\. 서버에서 HTTP 응답 메세지를 생성

![](<../.gitbook/assets/image (16).png>)

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

