# EntityNotFoundException

로그 조회 API를 만들던중에 마주한 오류 EntityNotFoundException\
프로젝트 초기의 테스트 데이터를 계속 유지하다보니,\
현재 데이터와 구조는 같지만, 참조하는 id값(ex. 100 = 유저조회, 200 = 계정 삭제)이 달라진것

&#x20;

처음엔 join과정에서 outerjoin이 되지 않는다고 판단하고 해당 옵션들을 찾았다.

```
@ManyToOne 의 optional = true

@JoinColumn 의 nullable = true
```

위의 두가지의 default 값은 true 였으며, 이는 outerjoin을 허용.\
당연히 해결되지 않았다.

&#x20;

@NotFound로 해결(default =NotFoundAction.EXCEPTION)

```
@NotFound(action = NotFoundAction.IGNORE)
```

&#x20;

optional과 nullable은 many에 있는 외래키 컬럼이 null 케이스에 적용되며,\
@NotFound는 컬럼의 값들은 있지만, 해당값을 PK로 가지는 데이터가 One에 해당하는 테이블에 없는 경우에 적용하는것으로 이해했다. -> (데이터를 직접 삽입하는 경우가 아니라면 오류는 존재하지 않을것으로 보임.)
