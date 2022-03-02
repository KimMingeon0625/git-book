# Docker Compose

## Docker Compose

다중 컨테이너 도커 애플리케이션을 정의하고 실행하기 위한 도구

{% hint style="info" %}
docker-compose 실행

docker-compose up
{% endhint %}

### Docker Container간 통신 에러

![](<../.gitbook/assets/image (6) (1) (1) (1).png>)

{% hint style="info" %}
서로 다른 컨테이너에 있는 경우 설정 없이는 접근 할 수 없다.

멀티 컨테이너 상황에서 쉽게 네트워크를 연결 시켜주기 위해 <mark style="color:orange;">**Docker Compose**</mark>를 이용.
{% endhint %}

### Docker Compose 파일 작성

![](<../.gitbook/assets/image (9) (1) (1) (1) (1) (1).png>)

```
// docker-compose.yml
version: "3"
services:
    redis-serverL
        image: "redis"
    node-app:
        bbuild:
        ports:
            - "5000:8000"
```

### Docker compose 멈추기

{% hint style="info" %}
docker compose 중지

Docker compose down
{% endhint %}

## 운영환경을 위한 dockerFile

![](<../.gitbook/assets/image (2) (1) (1).png>)

![](<../.gitbook/assets/image (9) (1) (1) (1) (1).png>)

{% hint style="info" %}
상단의 초록선 : Builder stage

하단의 초록선 : run stage



Builder stage에서 생성된 파일과 폴더들은 \~/build로 들어간다.
{% endhint %}

```
copy #1 --from=builder #2 /usr/src/app/build #3 /usr/share/nginx/html
1. 다른 stage에 있는 파일을 복사할 때 다른 stage 이름 명시.
    ex) as builder
2,3 . #2 에서 #3로 복사.
```
