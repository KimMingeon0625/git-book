---
description: 기본적인 지식 및 Apollo를 활용한 simple project
---

# GraphQL 기초

## GraphQL

* 페이스북에서 만든 쿼리 언어로 sql과 같은 쿼리 언어이지만 구조의 차이가 존재.
* gql(graphQL)은 웹 클라이언트가 서버로부터 데이터를 효율적으로 가져오는 것이 목적.
* sql은 백엔드에서 주로 작성하지만, gql의 경우 주로 클라이언트 시스템에서 작성, 호출.
* OverFetching, UnderFetching

![출처 : https://2020.stateofjs.com/en-US/technologies/datalayer/](<../.gitbook/assets/image (13) (1).png>)

{% embed url="https://graphql.org/users" %}
graphQL을 적용한 기업
{% endembed %}



### GraphQL vs RestAPI

* RestAPI
  * URL, Method(get,post,...)을 통한 다양한 Endpoint가 존재.
  * Endpoint에 따른 DB SQL 쿼리.
  * OverFetching, UnderFetching의 발생.
* gql
  * 단 하나의 Endpoint가 존재.
  * gql 스키마의 타입에 따른 DB SQL 쿼리.
  *   OverFetching, UnderFetching에서 RestAPI에 비해 유리하다.

      (OverFetching : 불필요한 데이터가 포함됨 / UnderFetching : 여러번의 API 호출)

![REST API와 GraphQL API의 사용 (출처 : https://blog.apollographql.com/graphql-vs-rest-5d425123e34b)](<../.gitbook/assets/image (39).png>)

### GraphQL 구조

* 쿼리(query) : DB 데이터 get(R).
*   뮤테이션 (mutation) :  DB 데이터 수정(C,U,D).

    (RESTAPI와 같은 개념적인 규약)

![(좌) request 쿼리문 (우) response / In ApolloServer](<../.gitbook/assets/image (36).png>)



### GraphQL을 위한 라이브러리

GraphQL은 (명세, 형식) 쿼리 언어로 구현할 솔루션이 필요하다.

* 백엔드에서 정보를 제공 및 처리
* 프론트엔드에서 요청 전송

이를 위한 라이브러리로는 GraphQL.js, GraphQL Yoga, AWS Amplift, ...

#### Apollo GraphQL

* 백엔드와 프론트엔드 모두 제공
* 간편하고 쉬운 설정



### 스키마/타입 (schema/type)

```
const { ApolloServer, gql } = require('apollo-server')
const database = require('./database')

/**
 * typeDef
 * GraphQL 명세에서 사용될 데이터, 요청 타입 지정
 * gql (template literal tag) 로 생성됨
 */ 
const typeDefs = gql`
type Query {
    teams: [Team]
    }
type Team {
    id: Int
    manager: String
    office: String
    extension_number: String
    mascot: String
    cleaning_duty: String
    project: String
  }
  
 /**
 * resolver
 * 서비스의 액션들을 함수로 지정
 * 요청에 따라 데이터를 반환, 입력, 수정, 삭제
 */
 const resolvers = {
  Query: {
    teams: () => database.teams
    .map((team) => {
      team.supplies = database.supplies
      .filter((supply) => {
          return supply.team === team.id
      })
      return team
    })
}

// ApolloServer : typeDef와 resolver를 인자로 받아 서버 생성
const server = new ApolloServer({ typeDefs, resolvers })
server.listen().then(({ url }) => {
console.log(`🚀  Server ready at ${url}`)
})
```

#### 오브젝트 타입과 필드

* 오브젝트 타입 : Team&#x20;
* 필드 : id, manager, office, ...
* 스칼라 타입 : String, ID, Int 등
* 느낌표(!) : 필수 값을 의미(non-nullable)
* 대괄호(\[, ]) : 배열을 의미(array)

#### 스칼라 타입

| 타입      | 설명                            |
| ------- | ----------------------------- |
| ID      | 기본적으로는 String, 고유 식별자 역할을 나타냄 |
| String  | UTF-8 문자열                     |
| Int     | 부호가 있는 32비트 정수                |
| Float   | 부호가 있는 부동소수점 값                |
| Boolean | 참/거짓                          |



#### ! Non Null

* Null을 반환할 수 없음.

####

#### 열거 타입&#x20;

* 미리 지정된 값들중에서만 반환.(ex. enum)



#### 리스트 타

* 특정 타입의 배열을 반환.

|     선언부     | users: null | users: \[ ] | users: \[ ..., null] |
| :---------: | :---------: | :---------: | :------------------: |
|  \[String]  |      ✔      |      ✔      |           ✔          |
|  \[String!] |      ✔      |      ✔      |           ❌          |
|  \[String]! |      ❌      |      ✔      |           ✔          |
| \[String!]! |      ❌      |      ✔      |           ❌          |



#### 객체 타입

* 사용자에 의해 정의된 타입들



#### Union

* 타입 여럿을 한 배열에 반환하고자 할 때 사용

#### Interface

* 유사한 객체 타입을 만들기 위한 공통 필드 타입
* 추상 타입 - 다른 타입에 implements 되기 위한 타입



### 인자와 인풋 타입

1.  필터

    ```
    [Query type]
    type Query {
        ...
        peopleFiltered(
            team: Int, 
            sex: Sex, 
            blood_type: BloodType, 
            from: String
        ): [People]
        ...
    }
    =======================================================================
    [DB work]
    getPeople: (args) => dataFiltered('people', args) 
            .map((person) => {
                person.tools = [
                    ...dbWorks.getEquipments({used_by: person.role}),
                    ...dbWorks.getSoftwares({used_by: person.role})
                ]
                person.givens = [
                    ...dbWorks.getEquipments({used_by: person.role}),
                    ...dbWorks.getSupplies({team: person.team})
                ]
                return person
            }),
    ======================================================================
    [request]
    query {
      peopleFiltered (
        team: 1
        blood_type: B
        from: "Texas"
      ) {
        id
        first_name
        last_name
        sex
        blood_type
        serve_years
        role
        team
        from
      }
    }
    ```
2.  페이징

    ```
    [Query type]
    type Query {
        ...
        peoplePaginated(
            page: Int!,
            per_page: Int!
        ): [People]
        ...
    }
    ======================================================================
    [DB work]
    if (args.page && args.per_page) {
        result = result.slice(
            (args.page - 1) * args.per_page, 
            args.page * args.per_page)
    }
    ======================================================================
    [request]
    query {
    	peoplePaginated(page: 1, per_page: 7) {
        id
        first_name
        last_name
        sex
        blood_type
        serve_years
        role
        team
        from
      }
    }
    ```
3.  별칭

    ```
    query {
      badGuys: peopleFiltered(sex: male, blood_type: B) {
        first_name
        last_name
        sex
        blood_type
      }
      newYorkers: peopleFiltered(from: "New York") {
        first_name
        last_name
        from
      }
    }
    ```
4.  인풋 타입

    ```
    [Query type]
    const typeDefs = gql`
        ....
        input PostPersonInput {
            first_name: String!
            last_name: String!
            sex: Sex!
            blood_type: BloodType!
            serve_years: Int!
            role: Role!
            team: ID!
            from: String!
        }
    `
    const resolvers = {
        // ...
        Mutation: {
            postPerson: (parent, args) => dbWorks.postPerson(args),
        }
    }
    ======================================================================
    [request]
    mutation {
      postPerson(input: {
        first_name: "Hanna"
        last_name: "Kim"
        sex: female
        blood_type: O
        serve_years: 3
        role: developer
        team: 1
        from: "Pusan"
      }) {
        id
        first_name
        last_name
        sex
        blood_type
        role
        team
        from
      }
    }
    ```



### 마침

리팩토링을 위한 정리

RestAPI에 비해 Back과 Front의 협업이 한층 수월해 질 것으로 보인다. 기존 response를 Back에서 전적으로 담당하던 것에 대해 그 영역이 Front로 넘어간 것으로 overfetching이나 underfetching을 해소하기 위한 중복적인 개발도 감소 할 것으로 예상된다.



### 참고

* [https://www.apollographql.com/docs/react/get-started/](https://www.apollographql.com/docs/react/get-started/)
* [https://graphql-kr.github.io/learn/queries/](https://graphql-kr.github.io/learn/queries/)
* [https://tech.kakao.com/2019/08/01/graphql-basic/](https://tech.kakao.com/2019/08/01/graphql-basic/)
* [https://www.yalco.kr/lectures/graphql-apollo/](https://www.yalco.kr/lectures/graphql-apollo/)
* [얄팍한 GraphQL과 Apollo](https://www.inflearn.com/course/%EC%96%84%ED%8C%8D%ED%95%9C-graphql-apollo/dashboard)
