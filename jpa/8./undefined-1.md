# 즉시 로딩과 지연 로딩

## 즉시 로딩과 지연 로딩

#### 단순히 member 정보만 사용하는 비즈니스 로직&#x20;

#### println(member.getName());

![](<../../.gitbook/assets/image (33) (1) (1).png>)

### 지연 로딩 LAZY을 사용해서 프록시로 조회

```java
@Entity
 public class Member {
 @Id
 @GeneratedValue
 private Long id;
 @Column(name = "USERNAME")
 private String name;
 @ManyToOne(fetch = FetchType.LAZY) //**
 @JoinColumn(name = "TEAM_ID")
 private Team team;
 ..
 }
```

#### 지연 로딩

![](<../../.gitbook/assets/image (17) (1) (1) (1) (1).png>)

![](<../../.gitbook/assets/image (6) (1) (1) (1).png>)

Member member = em.find(Member.class, 1L);

![](<../../.gitbook/assets/image (31) (1) (1) (1) (1) (1) (1).png>)

Team team = member.getTeam();&#x20;

team.getName(); // 실제 team을 사용하는 시점에 초기화(DB 조회)

### 즉시 로딩 EAGER를 사용해서 함께 조회

```
@Entity
 public class Member {
 @Id
 @GeneratedValue
 private Long id;
 @Column(name = "USERNAME")
 private String name;
 @ManyToOne(fetch = FetchType.EAGER) //**
 @JoinColumn(name = "TEAM_ID")
 private Team team;
 ..
 }
```

#### 즉시 로딩

![](<../../.gitbook/assets/image (32) (1) (1) (1) (1).png>)

즉시 로딩(EAGER), Member조회시 항상 Team도 조회

![](<../../.gitbook/assets/image (21) (1) (1) (1) (1).png>)

JPA 구현체는 가능하면 조인을 사용해서 SQL 한번에 함께 조회

### 프록시와 즉시로딩 주의

* 가급적 지연 로딩만 사용(특히 실무에서)
* 즉시 로딩을 적용하면 예상하지 못한 SQL이 발생
* 즉시 로딩은 JPQL에서 N+1 문제를 일으킨다.
* @ManyToOne, @OneToOne은 기본이 즉시 로딩 -> LAZY로 설정
* @OneToMany, @ManyToMany는 기본이 지연 로딩
