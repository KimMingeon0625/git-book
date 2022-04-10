# 기본적인 도커 클라이언트 명령어(Docker command)



## 도커 이미지 내부 파일 구조 보기

{% hint style="info" %}
image 실행

docker run image-name

클라인어트 실행 이미지&#x20;

ex) docker run hello-world
{% endhint %}

* 이미지 내부 파일 시스템 구조 보기\
  command : docker run image-name ls\
  (ls 위치는 시작명령어를 무시하고 해당 커맨드를 실행)

![](<../.gitbook/assets/image (7) (1) (1) (1) (1) (1) (1).png>)

ex) docker run alpine ls

![](<../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png>)

ex) docker run hello-world ls

![](<../.gitbook/assets/image (44) (1) (1) (1) (1).png>)

hello-world는 ls가 작동하지 않는것과 같이 image 마다 다른 명령어를 제공한다.



## 컨테이너들 나열하기

{% hint style="info" %}
컨테이너 상태

docker ps

클라이언트   process status

ex) docker ps
{% endhint %}

![](<../.gitbook/assets/image (35) (1) (1).png>)

1. CONTAINER ID : 컨테이너 고유한 아이디 해쉬값.
2. IMAGE : 컨테이너 생성시 사용한 도커 이미지.
3. COMMAND : 컨테이너 시작시 실행될명령어.
4. CREATED : 컨테이너 생성 시간.
5. STATUS : 컨테이너의 상태. ( Up, Exited, Pause)
6. PORT : 컨테이너가 개발한 포트와 호스트에 연결한 포트.
7. NAMES : 컨테이너 고유한 이름. \
   생성시 --name 옵션으로 설정, 미설정의 경우docker engine이 임의로 설정. &#x20;

#### 원하는 항목만 보기

![](<../.gitbook/assets/image (33) (1) (1) (1) (1).png>)

docker ps --format 'table\{{.\~\~\~\}} \t table\{{\~\~\~\}}' (\t은 간격을 의미) &#x20;

ex) docker ps --format 'table\{{.Names\}}\ttable\{{.Image\}}'



모든 컨테이너 나열

{% hint style="info" %}
모든 컨테이너 나열

docker ps -a
{% endhint %}

![](<../.gitbook/assets/image (40) (1).png>)

## 도커 컨테이너의 생명주기

![](<../.gitbook/assets/image (23) (1) (1) (1).png>)

docker run image = docker create image-name\
&#x20;                                \+ docker start -a container-name/id ( -a옵션은 출력을 위해 설정)

![](<../.gitbook/assets/image (42) (1) (1) (1).png>)

### Docker Stop vs Docker Kill

![](<../.gitbook/assets/image (8) (1) (1) (1).png>)

{% hint style="info" %}
container 정지

docker stop <아이디/이름>

docker kill <아이디/이름>
{% endhint %}

#### Stop과 Kill의 차이점

* Stop : 진행하던 작업을 완료하고 중지. \
  정리하는 시간(Grace Period)
* Kill : 바로 중지.

### Container 삭제

{% hint style="info" %}
container 삭제

docker rm <아이디/이름>

실해중인 컨테이너는 중지한 후 삭제 가능.&#x20;

\
모든 container 삭제

docker rm 'docker ps -a -q' \


이미지 삭제

docker rmi \<image id>

\
컨테이너, 이미지, 네트워크 모두 삭제

docker system prune

* 도커를 쓰지 않을때 모두 정리.
* 실행중인 컨테이너에는 영향이 없음.
{% endhint %}

## 실행 중인 컨테이너에 명령어 전달

{% hint style="info" %}
실행중인 컨테이너에 명령어 전달

docker exec \<container id / name>
{% endhint %}

### docker run vs docker exec

docker run : 새로 컨테이너를 만들어서 실행.

docker exec : 이미 실행중인 컨테이너에 명령어 전달.

## 레디스를 이용한 컨테이너 이해

1. docker run redis
2. 새로운터미널에서  redis-cli
3. error 발생

\-> redis 서버를 컨테이너 밖에서 접근하여 erro 발생 / redis-cli도 컨테이너 안에서 실행해야 한다.

2번쩨 터미널에서 docker exec -it <컨테이너 아이디> redis-cli 입력

{% hint style="info" %}
\-it 명령어를붙여줘야 실행 한 뒤  명령어를 입력할 수 있다.&#x20;

붙이지 않는 경우 redis-cli 실행 뒤 밖으로 나온다.&#x20;

interactive(상호적인) terminal 의 약자.
{% endhint %}

![](<../.gitbook/assets/image (6) (1) (1) (1) (1) (1) (1).png>)

redis 명령어

set \<key> \<value>

get \<key>

위 명령어를 통해 client 동작 확인이 가능하다.

## 실행 중인 컨테이너에서 터미널 사용.

{% hint style="info" %}
docker exec -it \<container id> 명령어

위와 같은 작업을 반복하기는 매우 번거롭다.

docker exec -it \<container id> sh

위를 통해 원하는 쉘 환경으로 접속 가능. ( sh 대신 bash, zsh, powershall, .... 사용 가능)
{% endhint %}

![](<../.gitbook/assets/image (30) (1) (1) (1) (1) (1) (1) (1).png>)

{% hint style="info" %}
터미널 환경 종료

* Control + D
* exit 입력
{% endhint %}
