---
description: Bidirectional associations and masters of associations
---

# 양방향 연관관계와 연관관계의 주인 (Bidirectional associations and masters of associations)



## 양방향 연관관계와 연관관계의 주인 (Bidirectional associations and masters of associations)

### 양방향 매핑

```
// Member Entity
@Entity
public class Member {
    
    @Id @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;
    
    @Column(name = "USERNAME")
    private String username;
    
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;        // mapped by 대상
}
// Team Entity
@Entity
public class Team {
    @Id
    @GeneratedValue
    @Column(name= "TEAM_ID")
    private Long id;
    
    private String name;
    
    @OneToMany(mappedBy = "team")  // 대상 
    private List<Member> members = new ArrayList<>();
    
}
```

#### 객체와 테이블이 관계를 맺는 차이

* 객체 연관관계 = 2개
  * 회원 -> 팀 연관관계 1개(단방향)
  * 팀 -> 회원 연관관계 1개(단방향)
* 테이블 연관관계 = 1개
  * 회원 <-> 팀의 연관관계 1개(양방향)

**객체의 양방향 관계**

* 객체의 양방향 관계는 사실 양방향 관계가 아니라 서로 다른 단방향 관계 2개다.
* 객체를 양방향으로 참조하려면 단방향 연관관계를 2개 만들어야 한다.

**테이블의 양방향 관계**

* 테이블은 외래 키 하나로 두 테이블의 연관관계를 관리
* MEMBER.TEAM\_ID 외래 키 하나로 양방향 연관관계 가짐(양쪽으로 조인할 수 있다.)

#### 연관관계의 주인(Owner)

#### 양방향 매핑 규칙

* 객체의 두 관계중 하나를 연관관계의 주인으로 지정
* 연관관계의 주인만이 외래 키를 관리(등록, 수정)
* 주인이 아닌쪽은 읽기만 가능
* 주인은 mappedBy 속성 사용 X
* 주인이 아니면 mappedBy 속성으로 주인 지정

**주인의 기준**

* 외래 키가 있는 곳을 주인으로 정해라.(N:1 이라면 N을 주인으로)
* 위의 코드에서는 Member.team이 연관관계의 주인

### 양방향 매핑시 가장 많이 하는 실수

(연관관계의 주인에 값을 입력하지 않음)

```
Team team = new Team();
team.setName("TeamA");
em.persist(team);
Member member = new Member();
member.setName("member1");
// 역방향(주인이 아닌 방향)만 연관관계 설정.
team.getMembers().add(member);
em.persist(member);
// =======================
// 방법 1. 비지니스 로직 in Member
public void changeTeam(Team team){
    this.team = team; // 연관관계의 주인에 삽입
    team.getmembers().add(this); // mapped by(read only)에 삽입
}
// 방법 2. 비지니스 로직 in Team
public void addMember(Member member){
    member.setTeam(this); // 연관관계의 주인에 삽입
    members.add(member); // mapped by(read only)에 삽입
}
// 동일한기능의 비지니스 로직은 한쪽에서만 사용을 권장
```

* 연관관계의 주인에 삽입.\
  ex) member.setTeam(team);
* 비지니스 로직(메소드)을 통해 양쪽에 다 삽입하는것을 권장.
  * list의 경우 flush가 발생하지 않는 경우,\
    list(team.members)에 추가된 값은 1차 캐시에는 반영되지 않는다.
  * 이를 위해 flush, clear를 활용하거나, 양쪽에 데이터를 반영하는 방법이 있다.
  * 1가지의 비지니스 로직은 한쪽에만 만드는 것을 권장.\
    \-> 오류 발생 방지

#### 양방향 연관관계 주의

* 순수 객체 상태를 고려해서 항상 양쪽에 값을 설정하자.
* 연관관계 편의 메소드를 생성하자.
* 양방향 매핑시에 무한 루프를 조심하자.
  * ex) toString(), lombok, JSON 생성 라이브러리
* controller에서는 entity반환 X
  * 보안 문제
  * API 스팩의 변경을 발생시킨다.
  * DTO를 통한 반환 권장.

```
// toString() example
// memebers <-> team 무한 루프
// Team Class
@Override
public String toString(){
    return  "Team{"+
            "id=" + id +
            ", name=" + name + '\' +
            ", members=" + members +
            '}';
}
// Member Class
@Override
public String toString(){
    return  "Member{"+
            "id=" + id +
            ", username=" + username+ '\' +
            ", team=" + team+
            '}';
}
```

### 양방향 매핑 정리

* 단방향 매핑만으로도 이미 연관관계 매핑은 완료
* 양방향 매핑은 반대 방향으로 조회(객체 그래프 탐색) 기능이 추가된 것 뿐
* JPQL에서 역방향으로 탐색할 일이 많음
* 단방향 매핑을 잘 하고 양방향은 필요할 때 추가해도 됨

{% hint style="success" %}
단방향 매핑이 끝난 경우 양방향 매핑의 추가는 테이블에 영향을 주지 않는다.

\-> 설계 단계에서는 단방향 매핑만으로 진행한다.

테이블에 영향을 주지 않음 -> 설계는 단방향만으로 가능
{% endhint %}

{% hint style="danger" %}
비지니스 로직을 기준으로 연관관계 주인을 선택하면 안된다.

(권장) 연관관계의 주인은 외래 키의 위치를 기준으로 정해야함.
{% endhint %}
