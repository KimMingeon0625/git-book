---
description: one-way association
---

# 단방향 연관관계 (one-way association)



* 객체와 테이블연관관계의 차이를 이해
* 객체의 참조와 테이블의 외래 키를 매핑
* 용어
  * 방향 : 단방향, 양방향
  * 다중성 : 다대일(N:1), 일대다(1:N), 일대일(1:1), 다대다(N:M)
  * 연관관계의 주인(Owner) : 객체 양방향 연관관계는 관리 주인이 필요

예제 시나리오

* 회원과 팀이 있다.
* 회원은 하나의 팀에만 소속될 수 있다.
* 회원과 팀은 다대일 관계다.

### 객체를 테이블에 맞추어 모델링 (연관 관계가없는 객체)

![](<../.gitbook/assets/image (1) (1) (1).png>)

```
// Member Entity
@Entity
public class Member {
    
    @Id @GeneratedValue
    private Long id;
    
    @Column(name = "USERNAME")
    private String username;
    
    @Column(name = "TEAM_ID")
    private Long teamId;
}

// Team Entity
@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    
    @Column(name = "USERNAME")
    private String userName;
    
    @Column(name = "TEAM_ID")
    private Long teamId;
}
```

객체를 테이블에 맞추어 데이터 중심으로 모델링하면, 협력 관계를 만들 수 없다.

ex) find(member) -> find(member.getTeamId)

* table은 외래 키로 조인을 사용해서 연관된 객체를 찾는다.
* 객체는 참조를 사용해서 연관된 객체를 찾는다.
* 테이블과 객체 사이에는 이런 큰 간격이 있다.

### 객체 지향 모델링 (객체 연관관계 사용)

![](<../.gitbook/assets/image (6) (1) (1) (1).png>)

```
// Member Entity
@Entity
public class Member {
    
    @Id @GeneratedValue
    private Long id;
    
    @Column(name = "USERNAME")
    private String username;
    
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
}

// Team Entity
@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    
    @Column(name = "USERNAME")
    private String userName;
    
    @Column(name = "TEAM_ID")
    private Long teamId;
}
```

ex) find(member) -> member.getTeam()
