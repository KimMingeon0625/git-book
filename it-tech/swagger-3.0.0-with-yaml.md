# 스웨거(Swagger) 3.0.0 with yaml

> API 문서를 스웨거로 관리하는 작업을 하며 yaml파일 작성법에 대해 공식홈페이지를 통해 정리해보았다.\
> 간단한 구조이므로 몇번 직접 쓰다보면 감이 잡힐 것이다. 정리한 것 외에도 다양하니 필요한 부분은 검색.

> 스웨거(Swagger)\
> API들이 가지고 있는 정보들을 명세, 관리할 수 있는 도구.\
> API를 문서화 하는 툴.\
> \
> 어노테이션을 활용할 수도 있지만 코드가 난잡해지는것을 지양해야 하기 때문에 yaml파일로 작성을 선호한다.\
> 해당글은 yaml파일 기준 작성법이다.

### 1. Metadata <a href="#1.-20metadata" id="1.-20metadata"></a>

#### 1.1 version <a href="#1.1-20version" id="1.1-20version"></a>

openapi: 3.0.0

```
openapi: 3.0.0
```

#### 1.2 info <a href="#1.2-20info" id="1.2-20info"></a>

info: title: Sample API description: Optional multiline or single-line description in \[CommonMark]\(<[http://commonmark.org/help/](http://commonmark.org/help/)>) or HTML. version: 0.1.9

```
info:
  title: Sample API
  description: Optional multiline or single-line description in [CommonMark](http://commonmark.org/help/) or HTML.
  version: 0.1.9
```

* title : API명
* description : API설명
* optional : version, information, license, terms of service, etc...

### 2. Servers <a href="#2.-20servers" id="2.-20servers"></a>

```
servers:
  - url: http://api.example.com/v1
    description: Optional server description, e.g. Main (production) server
  - url: http://staging-api.example.com
    description: Optional server description, e.g. Internal staging server for testing
```

* URL: 서버 URL (Base URL)
* description : 설명.
* 다수의 URL 등록 가능.

더 자세한 사항

[https://swagger.io/docs/specification/api-host-and-base-path/](https://swagger.io/docs/specification/api-host-and-base-path/)

### 3. Paths <a href="#3.-20paths" id="3.-20paths"></a>

```
paths:
  /users:
    get:
      summary: Returns a list of users.
      description: Optional extended description in CommonMark or HTML
      responses:
        '200':
          description: A JSON array of user names
          content:
            application/json:
              schema: 
                type: array
                items: 
                  type: string
```

* paths : Server URL을 제외한 나머지 URL
* HTTP method : ex) get / post / put /delete
* summary : 요약 (화면에서 드랍다운 박스에 표시되는 부분)
* responses : 응답
* requestBody : 리턴
* Multi-line, Markdown도 지원.

더 자세한 사항

[https://swagger.io/docs/specification/paths-and-operations/](https://swagger.io/docs/specification/paths-and-operations/)

### 4. Parameters <a href="#4.-20parameters" id="4.-20parameters"></a>

```
paths:
  /users/{userId}:
    get:
      summary: Returns a user by ID.
      parameters:
        - name: userId
          in: path
          required: true
          description: Parameter description in CommonMark or HTML.
          schema:
            type : integer
            format: int64
            minimum: 1
      responses: 
        '200':
          description: OK
```

{userId} : 파라미터

parameters : 해당 파라미터에 대한 정보들 (ex. userId)

* in : 파라미터의 위치.
* required : 필수 여부.
* schema : 해당 schema 정보.
* $ref : 중복되는 스키마 정보를 참조위치를 통해 삽입 가능. (ex. $ref: '#/components/schemas/User') info section에서는 사용 할 수 없으며, paths section에서 바로 사용은 불가. 그러나 paths section에 하위에는 사용 가능

```
paths:
  /users:
    $ref: '../resources/users.yaml'
  /users/{userId}:
    $ref: '../resources/users-by-id.yaml'
```

더 자세한 사항

[https://swagger.io/docs/specification/describing-parameters/](https://swagger.io/docs/specification/describing-parameters/)

### 5. Request Body <a href="#5.-20request-20body" id="5.-20request-20body"></a>

```
paths:
  /users:
    post:
      summary: Creates a user.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                username:
                  type: string
      responses: 
        '201':
          description: Created
```

* schema : 대상이 될 스키마명을 적으면 된다.
* properties : object의 경우 프로퍼티스 ( array의 경우에는 items)

더 자세한 사항

[https://swagger.io/docs/specification/describing-request-body/](https://swagger.io/docs/specification/describing-request-body/)

### 6. Responses <a href="#6.-20responses" id="6.-20responses"></a>

```
paths:
  /users/{userId}:
    get:
      summary: Returns a user by ID.
      parameters:
        - name: userId
          in: path
          required: true
          description: The ID of the user to return.
          schema:
            type: integer
            format: int64
            minimum: 1
      responses:
        '200':
          description: A user object.
          content:
            application/json:
              schema:
                type: object
                properties:
                  id:
                    type: integer
                    format: int64
                    example: 4
                  name:
                    type: string
                    example: Jessica Smith
        '400':
          description: The specified user ID is invalid (not a number).
        '404':
          description: A user with the specified ID was not found.
        default:
          description: Unexpected error
```

&#x20;

더 자세한 사항

[https://swagger.io/docs/specification/describing-responses/](https://swagger.io/docs/specification/describing-responses/)

### 7. Input and Output Models <a href="#7.-20input-20and-20output-20models" id="7.-20input-20and-20output-20models"></a>

&#x20;

```
{
  "id": 4,
  "name": "Arthur Dent"
}
```

```
components:
  schemas:
    User:
      properties:
        id:
          type: integer
        name:
          type: string
      # Both properties are required
      required:  
        - id
        - name
```

```
paths:
  /users/{userId}:
    get:
      summary: Returns a user by ID.
      parameters:
        - in: path
          name: userId
          required: true
          type: integer
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
  /users:
    post:
      summary: Creates a new user.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
      responses:
        '201':
          description: Created
```

### 8. Authentication <a href="#8.-20authentication" id="8.-20authentication"></a>

```
components:
  securitySchemes:
    BasicAuth:
      type: http
      scheme: basic
security:
  - BasicAuth: []
```

* oAuth2.0 [https://swagger.io/docs/specification/authentication/oauth2/](https://swagger.io/docs/specification/authentication/oauth2/)
* API Keys [https://swagger.io/docs/specification/authentication/api-keys/](https://swagger.io/docs/specification/authentication/api-keys/)

&#x20;

&#x20;

더 자세한 사항

[https://swagger.io/docs/specification/authentication/api-keys/](https://swagger.io/docs/specification/authentication/api-keys/)

&#x20;

&#x20;

공식 홈페이지 Example YAML 예제들 출처\
[Basic Structure](https://swagger.io/docs/specification/basic-structure/)

[ ](https://swagger.io/docs/specification/basic-structure/)

&#x20;

&#x20;

Example YAML 예제들 출처\
[Swagger Editor](https://editor.swagger.io)

[ ](https://editor.swagger.io)

&#x20;

&#x20;

Detail Object\
[OAI/OpenAPI-Specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#infoObject)

[ ](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.3.md#infoObject)

&#x20;

### [ ](https://machine-geon.tistory.com/168?category=975992#%C2%A0) <a href="#c2-a0" id="c2-a0"></a>

&#x20;

출처

* [swagger.io/docs/specification/about/](https://swagger.io/docs/specification/about/)
