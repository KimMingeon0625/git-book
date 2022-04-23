---
description: Make image
---

# 도커 이미지 만들기(Make image)



## Docker image 생성

1. Dockerfile 작성
2. Docker Client : 도커파일에 입력된 것들이 도커 클라이언트에 전달되어야 한다.  &#x20;
3. Docker Server : 도커 클라이언트에 전달된 모든 중요한 작업들을 하는 곳.   &#x20;
4. image 생성

{% hint style="info" %}
Docker File

Docker Image를 생성하기 위한 설정 파일.

컨테이너가 어떻게 행동해야 하는지에 대한 설정.
{% endhint %}

## Dockerfile 만들기

1. 베이스 이미지를 명시 (파일 스냅샷에 해당)
2. 추가적으로 필요한 파일을 다운 받기 위한 명령어를 명시. (파일 스냅샷에 해당)
3. 컨테이너 시작시 실행 될 명령어를 명시. (시작시 실행 될 명령어에 해당)

{% hint style="info" %}
Base Image

도커 어미지는 여러개의 레이어로 되어있다.

그중에서 베이스 이미지는 기반이 되는 부분이다.(like OS)
{% endhint %}



{% hint style="info" %}
FROM

이미지 생성시 기반이 되는 이미지 레이어.

<이미지 이름>:<태그> 형식으로 작성

태그를  안붙이면 자동적으로 가장 최신것으로 다운 받음

ex) ubuntu:14.01



RUN

도커 이미지가 생성되기 전에 수행할 쉘 명령어



CMD

컨테이너가 시작되었을 때 실행 할 실행 파일 또는 셸 스크립트.

DokcerFile내 1개만 존재.&#x20;
{% endhint %}

#### Hello 문구 출력하기

1. DockerFile 폴더 만들기.
2. 폴더를 에디터로 실행. (ex. VS code)
3. 파일 생성. 이름은 Dockerfile
4. 기본적인 토대를 명시
5. 베이스 이미지부터 실제 값으로 추가.(사이즈가 작은 alpine 베이스 이미지 사용)
6. 문자 출력은 echo를 사용하지만 alpine 내부에 echo가 있기에 RUN생략.
7. 컨테이너 시작시 실행 될 명령어(CMD) echo hello를 적어준다.

```
# 베이스 이미지 명시
FROM alpine

# 추가적으로 필요한 파일들을 다운로드
# RUN command

# 컨테이너 시작시 실행 될 명령어를 명시
CMD ["echo","hello"]
```



## Dockerfile로 Docker Image 만들기

docker build ./

Build 명령어

* 해당 디렉토리 내에서 dockerfile 파일을 찾아 도커 클라이언트에 전달.

![Build 내부 과정](<../.gitbook/assets/image (24) (1) (1) (1) (1) (1) (1) (1).png>)

* build를 할때 내부적으로 임시 컨테이너를 만든 후\
  &#x20;해당 컨테이너를 토대로 새로운 이미지가 만들어진다.\
  이후 임시 container는 삭제된다.

![](<../.gitbook/assets/image (16) (1) (1) (1) (1) (1) (1) (1) (1) (1).png>)

## Image Naming

![](<../.gitbook/assets/image (27) (1) (1) (1) (1) (1) (1) (1).png>)
