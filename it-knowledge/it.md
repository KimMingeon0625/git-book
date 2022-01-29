---
description: 개발자들 대화 혹은, 문서 등에서 나온 생소한 용어들에 대한 간단한 정리.
---

# IT 관련 용어

### Single Sign-On(SSO)

**하나의 아이디 및 패스워드를 통해 여러 시스템에 접근할 수 있는 통합 로그인(인증) 솔루션**

#### SSO의 구축 유형

**1) 인증 대행 모델(Delegation)**

\


![](<../.gitbook/assets/image (36) (1).png>)

\- 인증 방식을 변경하기 어려울 경우, 많이 사용

\- 시스템 접근 시, 통합 Agent가 인증 작업을 대행

\
**2) 인증 정보 전달 모델(Propagation)**

\


![](<../.gitbook/assets/image (20).png>)

\- 웹 기반의 시스템에 주로 사용

\- 미리 인증된 토큰(Cookie 기능 이용)을 받아서 각 시스템 접근 시, 자동으로 전달

**참고:**[ **** https://toma0912.tistory.com/75](https://toma0912.tistory.com/75)

