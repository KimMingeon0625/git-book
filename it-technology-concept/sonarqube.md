# 소나큐브(SonarQube)

## 소나큐브(SonarQube, 이전 이름: 소나/Sonar)

20개 이상의 프로그래밍 언어에서 버그, 코드 스멜, 보안 취약점을 발견할 목적으로 정적 코드 분석으로 자동 리뷰를 수행하기 위한 지속적인 코드 품질 검사용 오픈 소스 플랫폼이다. \
소나소스(SonarSource)가 개발하였다. \
소나큐브는 중복 코드, 코딩 표준, 유닛 테스트, 코드 커버리지, 코드 복잡도, 주석, 버그 및 보안 취약점의 보고서를 제공한다&#x20;

요약 : 지속적인 코드 품질 향상과 유지 보수의 도움을 주는 정적 분석 애플리케이션 \
(정적 분석 : 실제 프로그램을 실행하지 않고 코드만의 형태에 대한 분석을 말한다)

## 정적분석 도구

* PMD&#x20;
  * 미사용 변수, 비어있는 코드 블락, 불필요한 오브젝트 생성과 같은 Defect을 유발할 수 있는 코드를 검사&#x20;
  * https://pmd.github.io
* FindBugs&#x20;
  * 정해진 규칙에 의해 잠재적인 에러 타입을 찾아줌
  * http://findbugs.sourceforge.net
* CheckStyle
  * 정해진 코딩 룰을 잘 따르고 있는지에 대한 분석&#x20;
  * http://checkstyle.sourceforge.net
* 소나큐브는 위 툴들의 종합판 느낌(?)

## 소나큐브의 장점

1. 오픈소스 프로젝트로 별도의 라이센스 비용이 없다.&#x20;
2. 프로그램 설치 후 Web Monitoring UI (Dashboard) 제공&#x20;
3. 테이블과 차트를 이용하여 시간의 흐름에 따라 개선의 정도를 확인 가능.&#x20;
4. 코딩 품질 개선을 위한 정보를 프로젝트 단위부터 파일단위까지 제공.

## ISO9126 품질 표준

* Reliability(신뢰성) : 해야할 일을 하는가&#x20;
* Coding Style : 일관된 스타일을 유지했는가&#x20;
* Readability(가독성) : 쉬운 이해가 가능한가&#x20;
* Documentation : 문서화가 잘 되어있는가&#x20;
* Testability : 테스트 가능한가

## 소나큐브 7가지 품질 요소 기준

* 코드 악취(Maintainability)&#x20;
  * 심각한 이슈는 아니지만 베스트 프렉티스에서 사소한 이슈들로 모듈성(modularity), 이해가능성(understandability), 변경 가능성(changeability), 테스트 용의성 (testability), 재사용성(reusability) 등이 포함.&#x20;
* 버그(Reliability)&#x20;
  * 일반적으로 잠재적인 버그 혹은 실행시간에 예상되는 동작을 하지 않는 코드.&#x20;
* 취약점(Vulnerabilities)&#x20;
  * 해커들에게 잠재적인 약점이 될 수 있는 보안상의 이슈. SQL 인젝션, 크로스 사이트 스크립팅과 같은 보안 취약성을 탐색.&#x20;
* 중복(Duplications)&#x20;
  * 코드 중복은 코드의 품질을 저해하는 가장 큰 요인 중 하나.&#x20;
* 단위테스트(Unit Tests)&#x20;
  * 단위 테스트 커버리지를 통해 단위 테스트의 수행 정도와 수행의 성공/실패 제공&#x20;
* 복잡도(Complexity)&#x20;
  * 순환 복잡도(Cyclomatic Complexity) 측정.(메소드나 파일이 20을 넘기면 위험성이 높음) 코드의 논리적인 흐름상에 존재하는 인지 복잡도(Cognitive Complexity) 측정.&#x20;
* 사이즈(Size)&#x20;
  * 소스 코드 사이즈와 관련된 다양한 지표를 제공.

## 지원 언어

![](https://lh4.googleusercontent.com/STxWDxInNSJXCewKV5p3o44Na1LAFJ2XmujEwlWPsLtGBikVUQ5oATDu5wiuwHFJ9n1cyN21c6GUqec2KJlMi2RvjI7v7Vm9QMdpavm2EDBrh-RJomMxZq0P7kAQctbk1NbFJOCU)
