---
description: detached from Context
---

# 준영속 상태 (Detached)

## 준영속 상태

* 영속 상태의 엔티티가 영속성 컨텍스트에서 분리(detached)된 경우
*   영속성 컨텍스트가 제공하는 기능을 사용 못함.

    ex) dirty checking, ...

#### 준영속 상태로 만드는 방법

1.  em.detach(entity)

    특정 엔티티만 준영속 상태로 전환
2.  em.clear()

    영속성 컨텍스트를 완전히 초기화
3.  em.close()

    영속성 컨텍스트를 종료
