---
description: Persistence Context
---

# 영속성 컨텍스트(Persistence Context)

* 영속성 컨텍스트 : 엔티티를 영구 저장하는 환경
*   엔티티 매니저를 통해 영속성 컨텍스트에 접근

    ex) EntityManager.persist(entity);

### 엔티티의 생명주기

* 비영속 (new/transient)
  * 영속성 컨텍스트와 전혀 관계 없는 새로운 상태
* 영속 (managed)
  * 영속성 컨텍스트에 관리되는 상태
* 준영속 (detached)
  * 영속성 컨텍스트에 저장되었다가 분리된 상태
* 삭제 (removed)
  * 삭제된 상태

### 영속성 컨텍스트의 이점

* 1차 캐시
* 동일성 보장 (identiyy)
* **트랜잭션을 지원하는 쓰기 지연 (transactional write-behind)**
  * (커밋을 하는 순간 SQL문을 DB에 보낸다.)
* **변경 감지 (Dirty checking)**
  1. flush()
  2. 엔티티와 스냅샷 비교
  3. UPDATE SQL생성
  4. flush (영속성 컨텍스트의 변경내용을 DB에 반영)
  5. commit
* 지연 로딩 (lazy loading)
