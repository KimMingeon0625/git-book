---
description: flush
---

# 플러시 (Flush)

### 플러시 발생

* 변경 감지
* 수정된 엔티티 쓰기 지연 SQL 저장소에 등록
* 쓰기 지연 SQL 저장소의 쿼리를 DB에 전송

### 플러시 하는 방법

* em.flush() - 직접 호출
* transaction commit - 플러시 자동 호출
* JPQL 쿼리 실행 - 플러시 자동 호출
  * exception 방지

### 플러시 모드 옵션

* FlushModeType.AUTO
  * 커밋이나 쿼리를 실행할 때 플러시(default)
* FlushModeType.COMMIT
  * 커밋할때만 플러시

#### 플러시는 영속성 컨텍스트를 비우는 것이 아닌 영속성 컨텍스트의 변경 내용을 DB에 동기화.
