---
description: Primary key Mapping
---

# 기본키 매핑 (Primary key Mapping



## 기본키 매핑 (Primary key Mapping

기본 키 매핑 어노테이션

직접 할당 : @Id 만 사용

자동 생성 : @GeneratedValue(strategy = GenerationType.'전략')

#### IDENTITY : 데이터베이스에 위임

ex) MYSQL - AUTO\_INCREMENT

{% hint style="info" %}
JPA는 보통 트랜잭션 커밋 시점에 INSERT SQL을 실행하지만

AUTO\_INCREMENT는 DB에 INSERT SQL을 실행해야 ID를 알 수 있기 때문에

em.persist() 시점에 즉시 INSERT SQL을 실행 하고 DB에서 식별자를 조회하게 된다.
{% endhint %}

```
@Entity
public class Member {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        ...
}
```

#### SEQUENCE : 데이터베이스 시퀀스 오브젝트 사용

SequenceGenerator 필요

{% hint style="info" %}
영속성 컨텍스트에 저장되기 전 DB에 접근해 SEQUENCE를 얻어온다.

네트워크를 많이 타는 이슈를 해결하기위한(최적화) 방법

\-> allocationSize 옵션을 통해 DB에서 allocationSize 만큼 Sequence를 가져온다.

이후 메모리에 올려놓은 채로 사용하게 된다.

해당 개수에 달했을 경우 다시 allocationSize만큼의 SEQUENCE를 DB에서 가져온다.

주의) 웹서버를 내린경우 공백이 발생. ( 50 \~ 100 권장)
{% endhint %}

```
@Entity
@SequenceGenerator(
        name = "MEMBER_SEQ_GENERATOR",
        sequenceName = "MEMBER_SEQ", // 매핑할 데이터베이스 시쿼스 이름
        initialValue = 1, allocationSize = 1)
public class Member {
@Id
        @GeneratedValue(strategy = GenerationType.SEQUENCE, generatoer = "MEMBER_SEQ_GENERATOR")
        private Long id;
        ...
}
```

#### TABLE : 키 생성용 테이블 사용, 모든 DB에서 사용

장점 : 모든 DB에 적용 가능 / 단점 : 성능

@TableGenerator 필요

```
@Entity
@TableGenerator(
        name = "MEMBER_SEQ_GENERATOR",
        table = "MY_SEQUENCES",
        pkColumnValue = "MEMBER_SEQ", allocationSize = 1)
public class Member {
        @Id
        @GeneratedValue(strategy = GenerationType.TABLE, generatoer = "MEMBER_SEQ_GENERATOR")
        private Long id;
        
        ...
}
```

#### AUTO : 방언에 따라 자동 지정, 기본값

#### 권장하는 식별자 전략

* 기본 키 제약 조건 : Not Null, 유일, 불변
* 미래까지 이 조건을 만족하는 자연키는 찾기 어렵다. 대리키(대체키, ex) GenerateValue)를 사용하자.
* ex) 주민등록번호도 기본키로 적절하지 않다. (ex.정책변경으로 주민등록번호 보관 불가 상황)
* 권장 : Long형 + 대체키 + 키 생성전략 사용
