# Docker Compose

## Docker Compose

다중 컨테이너 도커 애플리케이션을 정의하고 실행하기 위한 도구

{% hint style="info" %}
docker-compose 실행

docker-compose up
{% endhint %}

## Docker Container간 통신 에러

![](<../.gitbook/assets/image (6).png>)

{% hint style="info" %}
서로 다른 컨테이너에 있는 경우 설정 없이는 접근 할 수 없다.

멀티 컨테이너 상황에서 쉽게 네트워크를 연결 시켜주기 위해 <mark style="color:orange;">**Docker Compose**</mark>를 이용.
{% endhint %}

## Docker Compose 파일 작성

![](<../.gitbook/assets/image (9).png>)

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

## Docker compose 멈추기

{% hint style="info" %}
docker compose 중지

Docker compose down
{% endhint %}
