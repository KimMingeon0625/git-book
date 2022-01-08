# IT Knowledge

> Inflearn '모든 개발자를위한 HTTP 웹 기본 지식' 강의 내용 및 이미지를 통한 정리.

{% embed url="https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard" %}

## 인터넷 네트워크

### IP

#### 역할

* 지정한 IP 주소(IP Adress)에 데이터 전달
* 패킷이라는 통신 단위로 데이터 전달

#### IP 패킷 정보

* 출발지 IP, 목적지 IP, 기타 ...

![](<.gitbook/assets/image (14).png>)

#### IP 프로토콜의 한계

* 비연결성
  * 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
* 비신뢰성
  * 패킷 유실?
  * 패킷의 순서?
* 프로그램 구분
  * 동일한 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상인 경우



### TCP, UDP

![인터넷 프로토콜 스택의 4계층](<.gitbook/assets/image (6).png>)

![](<.gitbook/assets/image (16).png>)

1. 프로그램이 Hello,world! 메시지 생성
2. SOCKET 라이브러리를 통해 전달
3. TCP 정보 생성, 메시지 데이터 포함
4. IP 패킷 생성,TCP 데이터 포함.



![](<.gitbook/assets/image (10).png>)

TCP 특징

전송 제어 프로토콜(Transmission Control Protocol)

* 연결 지향 - TCP 3 way handshake (가상 연결)
* 데이터 전달 보증
* 순서 보장
* 신뢰할 수 있는 프로토콜
* 현재는 대부분 TCP 사용



![](<.gitbook/assets/image (21).png>)

{% hint style="info" %}
SYN : 접속 요청&#x20;

ACK : 요청 수락

참고 : 3.ACK와 함께 데이터 전송 가능
{% endhint %}

데이터 전달 보증

1. 데이터 전송
2. 데이터 수신 확인 메시지

순서 보장

1. 패킷1, 패킷2, 패킷3 순서로 전송
2. 패킷1, 패킷3, 패킷2 순서로 도착
3. 서버가 클라이언트에게 패킷 2부터 다시 전송하는 것을 요청



#### UDP 특징

사용자 데이터그램 프로토콜(User Datagram Protocol)

* 하얀 도화지에 비유(기능이 거의 없음)
* 연결 지향 - TCP 3 way handshake X
* 데이터 순서 보증 X
* 순서 보장 X
* 데이터 전달 및 순서가 보장되진 않지만, 단순하고 빠름
* 정리
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



### DNS

도메인 네임 시스템(Domain Name System)

* IP는 기억하기 어렵다.
* IP는 변경될 수 있다.

\->  DNS 사용으로 해결.

* 전화번호부와 같은 역할
* 도메인 명을 IP 주소로 변환

![](<.gitbook/assets/image (13).png>)

