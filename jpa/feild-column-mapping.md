---
description: feild, column mapping
---

# 필드와 컬럼 매핑 (feild, column mapping)



### 매핑 어노테이션

|             |                                 |
| ----------- | ------------------------------- |
| @Column     | 컬럼 매핑                           |
| @Temporal   | 날짜 타입 매핑                        |
| @Enumerated | enum 타입 매핑                      |
| @Lob        | BLOB,CLOB 매핑                    |
| @Transient  | 특정 필드를 컬럼에 매핑을 하지 않고, 메모리에서만 사용 |

#### @Column

| Attribute             | Description                                                                                                                                                       | Default                                  |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| name                  | 필드와 매핑할 테이블의 컬럼 이름                                                                                                                                                | 객체의 필드 이름                                |
| insertable, updatable | 등록, 변경 가능 여부                                                                                                                                                      | TRUE                                     |
| nullable(DDL)         | null 값의 허용 여부를 설정한다. false로 설정하면 DDL 생성 시에 not null 제약조건이 붙는다.                                                                                                    | <p>TRUE(null )</p><p>FALSE(not null)</p> |
| unique(DDL)           | @Table의 uniqueconstraints와 같지만 한 컬럼에 간단히 유니크 제약조건을 걸 때 사용한다.(@table의 uniqueConstraints를 통한 유니크 제약을 선호)                                                            |                                          |
| columnDefinition(DDL) | <p>데이터베이스 컬럼 정보를 직접 줄 수 있다.</p><p>ex) varchar(100) default 'EMPTY'</p>                                                                                            | 필드의 자바 탑이과 방언 정보를 직접 기입                  |
| length(DDL)           | 문자 길이 제약조건, String 타입에서만 사용한다.                                                                                                                                    | 255                                      |
| precision scale(DDL)  | <p>BigDecimal 타입에서 사용한다. (BigInteger도 사용 가능)</p><p>precision은 소수점을 포함한 전체 자릿수를, scale은 소수의 자릿수다.</p><p>참고로 double,float 타입에는 적용되지 않는다. 정밀한 소수를 다루어야 할때만 사용한다.</p> | precision = 19                           |

#### @Enumerated

* EnumType.ORDINAL: enum 순서를 데이터베이스에 저장. (사용 X)
* EnumType.STRING: enum 이름를 데이터베이스에 저장.(필수 O)
* default : EnumType.ORDINAL

#### @Temporal

* LocalDate, LocalDateTime을 사용할 때는 생략 가능. (java8 버전 이후)
* TemporalType.DATE : 날짜, 데이터베이스 data 타입과 매핑
* TemporalType.TIME : 시간, 데이터베이스 time 타입과 매핑
* TemporalType.TIMESTAMP : 날짜와 시간, 데이터베이스 timestamp 타입과 매핑

#### @Lob

* 지정할 수 있는 속성이 없다.
* 매핑하는 필드 타입이 문자면 CLOB, 나머지는 BLOB 매핑

#### @Transient

* 필드 매핑 X
* 데이터베이스에 저장X, 조회X
* 메모리상에서만 사용을 원할때 사용.
