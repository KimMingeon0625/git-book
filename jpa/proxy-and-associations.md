# 프록시와 연관관계 정리(Proxy and Associations)

## 프록시

### Member를 조회할 때 Team도 함께 조회해야 할까?

#### 회원과 팀 함께 출력

```java
public void printUserAndTeam(String memberId) {
 Member member = em.find(Member.class, memberId);
 Team team = member.getTeam();
 System.out.println("회원 이름: " + member.getUsername());
 System.out.println("소속팀: " + team.getName());
}
```

#### 회원만 출력

```java
public void printUser(String memberId) {
 Member member = em.find(Member.class, memberId);
 Team team = member.getTeam();
 System.out.println("회원 이름: " + member.getUsername());
}
```

### 프록시 기초

* em.find() vs em.getReference()
* em.find(): 데이터베이스를 통해서 실제 엔티티 객체 조회
* em.getReference(): 데이터베이스 조회를 미루는 가짜(프록시) 엔티티 객체 조회

![](<../.gitbook/assets/image (30) (1).png>)

### 프록시 특징

![](<../.gitbook/assets/image (39).png>)

* 실제 클래스를 상속 받아서 만들어짐
* 실제 클래스와 겉 모양이 같다.
* 사용하는 입장에서는 진짜 객체인지 프록시 객체인지 구분하지 않고 사용하면 됨(이론상)
* 프록시 객체는 실제 객체의 참조(target)를 보관
* 프록시 객체를 호출하면 프록시 객체는 실제 객체의 메소드 호출

### 프록시 객체의 초기화

```java
Member member = em.getReference(Member.class, “id1”);
member.getName();
```

![](<../.gitbook/assets/image (13).png>)

### 프록시의 특징

* 프록시 객체는 처음 사용할 때 한 번만 초기화
* 프록시 객체를 초기화 할 때, 프록시 객체가 실제 엔티티로 바뀌는 것은 아님, 초기화되면 프록시 객체를 통해서 실제 엔티티에 접근 가능
* 프록시 객체는 원본 엔티티를 상속받음, 따라서 타입 체크시 주의해야함 (== 비교 실패, 대신 instance of 사용)
* 영속성 컨텍스트에 찾는 엔티티가 이미 있으면 em.getReference()를 호출해도 실제 엔티티 반환 → 반대 상황(ex. proxy로 반환)도 마찬가지
* 영속성 컨텍스트의 도움을 받을 수 없는 준영속 상태일 때, 프록시를 초기화하면 문제 발생(하이버네이트는 org.hibernate.LazyInitializationException 예외를 터트림) → 초기화 요청이 영속성 컨텍스트를 통해서 이루어지기 때문에 영속성 컨텍스트를 detach, clear 혹은 close 하게 된다면 초기화가 이루어질 수 없다.

### 프록시 확인

* 프록시 인스턴스의 초기화 여부 확인 PersistenceUnitUtil.isLoaded(Object entity) → emf.getPersistenceUnitUtil().isLoaded(Object entity)
* 프록시 클래스 확인 방법 entity.getClass().getName() 출력(..javasist.. or HibernateProxy…)
* 프록시 강제 초기화 org.hibernate.Hibernate.initialize(entity);
* 참고: JPA 표준은 강제 초기화 없음 강제 호출: member.getName()

## 즉시 로딩과 지연 로딩

#### 단순히 member 정보만 사용하는 비즈니스 로직&#x20;

#### println(member.getName());

![](<../.gitbook/assets/image (33) (1).png>)

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

![](<../.gitbook/assets/image (17).png>)

![](<../.gitbook/assets/image (6).png>)

Member member = em.find(Member.class, 1L);

![](<../.gitbook/assets/image (31) (1).png>)

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

![](<../.gitbook/assets/image (32) (1).png>)

즉시 로딩(EAGER), Member조회시 항상 Team도 조회

![](<../.gitbook/assets/image (21) (1).png>)

JPA 구현체는 가능하면 조인을 사용해서 SQL 한번에 함께 조회

### 프록시와 즉시로딩 주의

* 가급적 지연 로딩만 사용(특히 실무에서)
* 즉시 로딩을 적용하면 예상하지 못한 SQL이 발생
* 즉시 로딩은 JPQL에서 N+1 문제를 일으킨다.
* @ManyToOne, @OneToOne은 기본이 즉시 로딩 -> LAZY로 설정
* @OneToMany, @ManyToMany는 기본이 지연 로딩
