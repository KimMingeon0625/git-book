---
description: Docker Basic
---

# 도커 기본(Docker Basic)

### 도커를 쓰는 이유

container를 사용함으로 서버, 패키지 버전, 운영체제 등의 영향으로부터 자유롭다.

\-> container에 설치.

### 도커(Docker)란?

Container컨테이너를 사용하여 응용프로그램을 더 쉽게 만들고 배포하고 실행할 수 있도록 설계된 도구이며, 컨테이너 기반의 오픈소스 가상화 플랫폼이며 생태계이다.

### 도커 이미지와 도커 컨테이너 정의

#### Container

코드와 모든 종속성을 패키지화하여 응용 프로그램이 한 컴퓨팅 환경에서 다른 컴퓨팅 환경으로 빠르고 안정적으로 실행되도록 하는 소프트웨어의 표준 단위이다.

{% hint style="info" %}
컨테이너 안에 다양한 프로그램, 실행환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단수하게 한다.

\-> AWS, Azure, Google cloud 등 어디서든 실행 가능하게 만든다.
{% endhint %}

#### Container image

코드, 런타임, 시스템 도구, 시스템 라이브러리 및 설정과 같은 응용 프로그램을 실행하는데 필요한 모든 것을 포함하는 가볍고 독립적이며, 실행 가능한 소프트웨어 패키지.

{% hint style="info" %}
컨테이너 이미지는 런타임에 컨테이너가 되고 도커 컨테이너의 경우 도커 엔진에서 실행될 때 이미지 컨테이너가 된다.

컨테이너는 소프트웨어를 환경으로부터 격리시키고 개발과 스테이징의 차이에도 불구하고 균일하게 작동하도록 보장한다.
{% endhint %}

### 도커 설치

{% embed url="https://www.docker.com/get-started" %}
본인의OS에 맞게 설치
{% endembed %}

### 도커의 흐름

![](https://github.com/KimMingeon0625/docker/raw/master/.gitbook/assets/image%20\(1\).png)

1. 도커 클라이언트에 커맨드를 입력 -> 도커 서버로 요청.
2. 서버에서 hello-world라는 이미지가 local cache에 있는지 확인.
3. 최초 실행인 경우엔 local cache에 image가 없기 때문에 Unable to find image \~ 문구 출력.
4. Docker Hub에서 image pull -> local cache에 보관.
5. 이후 실행의 경우 local cache의 image를 활용하여 container 생성.\
   (Unable to find image\~ 가 출력되지 않음)
6. 생성된 container는 image의 설정, 조건에 따라 실행.

### 도커와 기존 가상화 기술 차이를 통한 컨테이너 이해

#### 가상화 기술이 나오기 전

* 한대의 서버를 하나의 용도로만 사용.
* 서버공간의 낭비.
* 하나의 서버에 하나의 운영체제, 하나의 프로그램.
* 안정적이지만, 비효율적이다.

#### 하이퍼 바이저 기반의 가상화 출현

* 논리적으로 공간을 분할하여 VM이라는 독립적인 가상 환경의 서버 이용 가능.
* 하이퍼 바이저는 호스트 시스템에서 다수의 **게스트 OS를 구동**할 수 있게 하는 소프트웨어,\
  그리고 하드웨어를 가상화하면서 하드웨어와 각각의 VM을모니터링 하는 **중간 관리자**.

![](<../.gitbook/assets/image (34) (1).png>)

![](<../.gitbook/assets/image (5) (1) (1).png>)

#### 도커 컨테이너 & 가상 머신

* 공통점
  * 기본 하드웨어에서 격리된 환경 내에 애플리케이션을 배치하는 방법.
* 차이점
  * 격리된 환경을 얼마나 격리를 시키는지의 차이\
    \-> container는 하이퍼 바이저와 OS가 필요하지않아 더 가볍다.(**image를 통한 배포**)

> <mark style="color:orange;">Docker container</mark>에서 돌아가는 application은 container가 제공하는 격리 기능 내부에 샌드박스가 있지만, 여전히 같은 호스트의 다른 컨테이너와 동일한 커널을 공유한다. 결과적으로, 컨테이너 내부에서 실행되는 프로세스는 호스트 시스템에서 볼 수 있다.
>
> ex) docker와 mongo DB container를 시작하면 호스트의 일반 쉘에 ps-e grep 몽고를 실행하면 프로세스가 표신된다.
>
> **container가 전체 OS를 내장할 필요가 없는 결과, 일반적으로 5\~100 MB이다.**

> <mark style="color:orange;">Virtual machine</mark>과 함께 VM 내부에서 실행되는 모든 것은 호스트 운영체제 또는 하이퍼바이저와 독립되어 있다. 가상머신 플랫폼은 특정 VM에 대한 가상화 프로세스를 관리하기 위헤 프로세스를 시작하고 호스트 시스템은 그것의 하드웨어 자원의 일부를 VM에 할당한다.
>
> 그러나 시작 시간에 VM환경을위해 커널을 부팅하고 운영 체제 프로세스 등을 시작한다.
>
> 이것은 응용 프로그램만 포함하는 일반적인 container보다 VM의 크기를 훨씬 크게 만든다.
>
> **비교적 사용법이 간단할 수 있지만 굉장히 느리다.**

{% hint style="info" %}
**격리 시키는 방법은?**

리눅스에서 쓰이는 C Group(Control groups) / 네임스페이스(Nnamespace)

* C Group\
  CPU, Memory, Network Bandwith, HD i/o 등 프로세스 그룹의 시스템 리소스 사용량을 관리 -> 사용량이 많은 app을 C group에 넣어 CPU와 memory 사용 제한.
* 네임스페이스\
  하나의 시스템에서 프로세스를 격리시킬 수 있는 가상화 기술\
  별개의 독립된 공간을 사용하는 것처럼 격리된 환경을 제공하는 경량 프로세스 가상화 기술
{% endhint %}

### 이미지로 컨테이너 만들기

이미지는 응용 프로그램을 실행하는데 필요한 모든 것을 포함.

1. container 시작 명령어.\
   ex) run kakao
2. 파일 스냅샷(directory or file 카피한 것).

#### 순서

1. Docker client에서 docker run <이미지> 입력.
2. docker image에 있는 파일 스냅샷을 container hard disk로 옮김.
3. image에서 가지고 있는 명령어(컨테이너가 실행 때 사용 할 명령어)를 이용해서 실행.

### C-group, namespace 도커 환경에서 쓸 수있는 이유

### C-group, namespace 도커 환경에서 쓸 수있는 이유

docker version 입력.

OS/Arch: linux/amd64 를 확인 가능

{% hint style="info" %}
Docker는 Linux환경에서 동작하므로, 리눅스 기능이 사용 가능하다.
{% endhint %}

![](<../.gitbook/assets/image (32) (1) (1) (1) (1) (1).png>)
