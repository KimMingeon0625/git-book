---
description: Docker with Node.js App
---

# 도커를 이용한 Node.js 어플(Docker with Node.js App)

### 예제 Node.js

{% embed url="https://nodejs.org/fr/docs/guides/nodejs-docker-webapp" %}

## Node.js 생성.

![](<../.gitbook/assets/image (38) (1) (1) (1) (1).png>)

npm init : package.json 생성.

```
// package.json
{
  "name": "study_docker",
  "version": "1.0.0",
  "description": "---\r description: intro\r ---",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "express": "4.17.2"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/KimMingeon0625/STUDY_DOCKER.git"
  },
  "author": "Mingeon",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/KimMingeon0625/STUDY_DOCKER/issues"
  },
  "homepage": "https://github.com/KimMingeon0625/STUDY_DOCKER#readme"
}
```

```
// server.js
const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// APP
const app = express();
app.get('/', (req, res) => {
    res.send('Hello World');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```

## Dokcerfile 생성.

```
// Dockerfile
FROM node:10 // npm이내장된 base Image

RUN npm install

CMD ["node", "server.js"]
```



위코드로  Docker build 에러 발생.&#x20;

![](<../.gitbook/assets/image (18) (1) (1) (1) (1) (1) (1) (1) (1).png>)

COPY package.json ./ 을 추가하여 package파일을 복사한다.

```
// Dockerfile
FROM node:10

// npm install 전에 위치
COPY package.json ./

RUN npm install

CMD ["node", "server.js"]
```

docker build -t mingeon/nodejs ./



다시 또 Error 발생. server.js 가 위치하지 않는 Error

```
// Dockerfile
FROM node:10

// 모든 파일을 docker container ./ 로 Copy
COPY ./ ./

RUN npm install

CMD ["node", "server.js"]
```



Build 성공 But loaclhost:8080 접속 불가.



## container에 접근하기 위한 설정.

{% hint style="info" %}
local file을 복사한 것과 같이 network도 연결을 해주어야 한다.&#x20;

Port mapping이 필요.
{% endhint %}

![](<../.gitbook/assets/image (19) (1) (1) (1) (1) (1) (1).png>)

docker run -p 5000:8080 mingeon/nodejs 를 통해 실행.

![설정한 local port 5000을 통해 접속.](<../.gitbook/assets/image (26) (1) (1) (1) (1) (1).png>)

## Working Directory

{% hint style="info" %}
Woking Directory

이미지안에서 app 소스 코드를 가지고 있을 디렉토리.
{% endhint %}

WorkDir를 설정하지 않는 경우

1. 파일이름이 중복일 경우. (기존 파일이 덮어 씌워진다.)&#x20;
2. 난잡하다.

```
FROM node:10

WORKDIR /usr/src/app

COPY ./ ./

RUN npm install

CMD ["node", "server.js"]
```



재빌드 후 실행.

* docker build -t mingeon/nodejs ./
* docker run -it mingeon/nodejs sh

![](<../.gitbook/assets/image (17) (1) (1) (1) (1) (1) (1) (1).png>)

WorkDir 설정을 하는 경우, container 접속시 WorkDir이 첫화면이다.



## 어플리케이션 소스 변경으로 재빌드하는 문제

![](<../.gitbook/assets/image (43) (1) (1).png>)

## 재빌드 효율적으로 하는 법

![](<../.gitbook/assets/image (22) (1) (1) (1) (1) (1) (1) (1).png>)

{% hint style="info" %}
npm insatll을 실행 할때 cache를 사용하기 위한 전략.

종속성을 먼저 복사하여 package.json의 변화가 없는 경우,

도커 내부의 캐시를 통해 종속성 다운로드의 중복 피할 수 있다.
{% endhint %}

```
// Dockerfile
FROM node:10

WORKDIR /usr/src/app

COPY package.json ./

RUN npm install

COPY ./ ./

CMD ["node", "server.js"]
```

![](<../.gitbook/assets/image (25) (1) (1) (1) (1) (1).png>)

## Docker Volume

![](<../.gitbook/assets/image (29) (1) (1) (1) (1) (1).png>)

![](<../.gitbook/assets/image (4) (1) (1) (1) (1) (1) (1).png>)

{% hint style="info" %}
node\_modules = 종속성 다운로드 dir

vscode powerShell 환경 : $(pwd) ->${pwd}
{% endhint %}

![](<../.gitbook/assets/image (41) (1).png>)

![](<../.gitbook/assets/image (11) (1) (1) (1) (1) (1) (1).png>)![](<../.gitbook/assets/image (28) (1) (1) (1) (1) (1) (1) (1).png>)

re-build 없이 매핑된 파일의 변경으로 적용.

\-> docker restart를 통해 재실행 해주어야 한다.
