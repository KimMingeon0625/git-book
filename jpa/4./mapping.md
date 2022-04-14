---
description: entity, Object, table, ... mapping
---

# 객체와 테이블 매핑 (mapping)

### 엔티티 매핑 <a href="#undefined" id="undefined"></a>

* 객체와 테이블 매핑 : @Entity, @Table
* 필드와 컬럼 매핑 : @Column
* 기본 키 매핑 : @Id
* 연관관계 매핑 : @ManyToOne, @JoinColumn

#### @Entity <a href="#entity" id="entity"></a>

* JPA를 사용해서 테이블과 매핑할 클래스는 필수
* 기본 생성자 필수
* final 클래스, enum, interface, inner 클래스 사용 X
* 저장할 필드에 final 사용 X

#### @Entity 속성 정리 <a href="#entity-1" id="entity-1"></a>

name

* JPA에서 사용할 엔티티 이름을 지정
* 기본값 : 클래스 이름을 그대로 사용
* 같은 클래스 이름이 없으면 가급적 기본값을 사용한다.

#### @Table <a href="#table" id="table"></a>

|                        |                       |
| ---------------------- | --------------------- |
| name                   | 매핑할 table name        |
| catalog                | DB catalog mapping    |
| shcema                 | DB schema mapping     |
| uniqueConstraints(DDL) | DDL 생성 시에 유니크 제약조건 생성 |
