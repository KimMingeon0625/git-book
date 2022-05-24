# VScode로 SSH를 통해 원격서버 접속

## **Remote Development** <a href="#1.-remote-development" id="1.-remote-development"></a>

1. &#x20;Extensions에서 Remote Development 설치
2. 2\) Remote Explorer에서 SSH Targets를 선택 후, Add New 클릭
3. ssh \[계정]@\[ip주소] 입력
4. SSH configuration file을 저장할 장소 선택

## Remote 실행하기 <a href="#2.-remote" id="2.-remote"></a>

1. Remote Explore에서 Connect to Host in New Window 선택
2. 서버 Platform 선택(Linux, Window, macOS)
3. 비밀번호 입력
4. 실행 확인 (좌측 하단)

## **Remote Explore에 별명으로 표시** <a href="#extra-1.-remote-explore" id="extra-1.-remote-explore"></a>

1. Remote Explore에서 해당 SSH TARGETS을 선택하고 Configure 선택
2. SSH configuration file 선택
3. Host의 값 수정 후 저장

* Host : vscode 내의 별명
* HostName : 서버주소
* Port : 다른 port 설정 (언급이 없다면 default : 22)
* User : 서버접속계정

## **비밀번호 입력없이 바로 접속하기** <a href="#extra-2." id="extra-2."></a>

1. Windows PowerShell(Windows) 또는 Termial(Ubuntu)에서 아래의 명령 실행
2. 아래의 명령 실행 (\<user>와 \<host>는 각자의 계정과 ip주소 입력)

```
cat ./.ssh/id_rsa.pub | ssh <user>@<host> "mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh && cat >> ~/.ssh/authorized_keys"
```
