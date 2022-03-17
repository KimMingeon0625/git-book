# File.separator

## 배경

OS에 따른 경로 차이를 신경 쓰지 않고, 파일 경로를 설정

## 사용

java.io 패키지의 File 클래스의 separator 필드 사용

```
import java.io.File;

...

String path = File.separator+"fileName"+File.separator+"sample.jpg";
```

## 주의

윈도우의 루트(root)는 윈도우가 설치된 C드라이브로 인식

파일 경로는 root 경로 아래부터 시작.
