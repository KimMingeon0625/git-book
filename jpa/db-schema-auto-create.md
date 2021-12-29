---
description: DB Schema auto create
---

# 데이터베이스 스키마 자동 생성 (DB Schema auto create)



### 데이터베이스 스키마 자동 생성

* DDL을 애플리케이션 실행 시점에 자동 생성
* 테이블 중심 -> 객체 중심
* 데이터베이스 방언(dialect)을 활용해서 데이터베이스에 맞는 적절한 DDL 생성
* 이렇게 **생성된 DDL은 개발 장비에서만 사용**
* 생성된 DDL은 운영서버에서는 사용하지 않거나, 적절히 다듬은 후 사용

#### 데이터 베이스 스키마 자동생성 - 속성

* hibernate.hbm2ddl.auto

|             |                                  |
| ----------- | -------------------------------- |
| create      | 기존 테이블 삭제 후 다시 생성(DROP + CREATE) |
| create-drop | create와 같으나 종료시점에 테이블 drop       |
| update      | 변경분만 반영(운영DB에는 사용 X)             |
| validate    | 엔티티와 테이블이 정상 매핑되었는지만 확인          |
| none        | 사용하지 않음                          |

### 데이터베이스 스키마 자동생성 주의

* 운영 장비에는 절대 create, create-drop, update 사용 X

#### DDL 생성 기능

* 제약 조건 추가 : 회원 이름은 필수, 10자 초가X
  * @Column(nullable = false, length = 10)
* 유니크 제약조건 추가
  * @Table(uniqueConstraints = {@UniqueConstraint(name = "NAME\_AGE\_UNIQUE", columnNaes = {"NAME", "AGE"})})
* DDL 생성 기능은 DDL을 자동 생성할 때만 사용되고 JPA의 실행 로직에는 영향 X
