---
description: JPA Intro
cover: ../.gitbook/assets/64585171-96511580-d3d2-11e9-947d-8f1e98e46100.png
coverY: 5.777777777777778
---

# 소개(about JPA)



ORM

* Object-relational mapping (객체 관계 매핑)
* ORM 프레임워크가 중간에서 매핑.

JPA

JAVA 애플리케이션과 JDBC 사이에서 동작

1. SQL 중심적인 개발에서 객체 중심으로 개발
2. 생산성
   * 저장 : jpa.persist
   * 조회 : Member member = jpa.find(memberId)
   * 수정 : member.setName("변경할 이름")
   * 삭제 : jpa.remove(member)
3. 유지보수
   * 기존 : 필드 변경시 모든 SQL 수정
4. 패러다임의 불일치 해결
   * JPA와 상속
   * JPA와 연관관계
   * JPA와 객체 그래프 탐색
   * JPA와 비교하기 (동일성 보장)
5. 성능
   * 1차 캐시와 동일성 보장
   * 트랜잭션을 지원하는 쓰기 지연
   *   지연 로딩 : 객체가 실제 사용될 때 로딩

       (즉시 로딩 : JOIN SQL로 한번에 연관된 객체까지 미리 조회)
6. 데이터 접근 추상화와 벤더 독립성
7. 표준
