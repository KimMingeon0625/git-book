# Error: Could not find or load main class Controller Caused by: java.lang.ClassNotFoundException: Con

## 발생 배경

이전 프로젝트를 오랜만에 빌드하던 과정에서 발생

```
Error: Could not find or load main class Controller
Caused by: java.lang.ClassNotFoundException: Controller
```

VsCode, IntelliJ 두개의 IDE 모두에서 발생.

여러글들에서 해결법으로 제안했던 방법 시도

* 환경변수 ClassPath 수정
* clean java language server workspace
* IDE 삭제 후 재설치
* Root Dir 확인
* 등등...

모두 실패

## 해결

rootProject.name 프로젝트 이름 설정에 공백을 제거

기존에는 작동이 잘 되었지만, IDE가 업데이트되면서 막힌것으로 추측된다.

\-> IDE의 버전이 낮은 팀원들은 문제없이 동작.

\-> eclipse는 공백에 관계없이 동작하는 것을 확인(2022/04/19 기준)



## 주의

오류 방지를 위해 rootProject.name에 공백 지양.
