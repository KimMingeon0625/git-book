# JUnit 5

## 소개

자바 개발자가 가장 많이 사용하는 테스팅 프레임워크.

* [https://www.jetbrains.com/lp/devecosystem-2021/java/](https://www.jetbrains.com/lp/devecosystem-2021/java/)
  * “단위 테스트를 작성하는 자바 개발자 85% JUnit을 사용함.”
* 자바 8 이상에서 사용 가능.

![](<../../.gitbook/assets/image (44) (1).png>)

Platform:  테스트를 실행해주는 런처 제공. TestEngine API 제공.

Jupiter: TestEngine API 구현체로 JUnit 5를 제공.

Vintage: JUnit 4와 3을 지원하는 TestEngine 구현체.

#### junit5 docs

* [https://junit.org/junit5/docs/current/user-guide/](https://junit.org/junit5/docs/current/user-guide/)

## **시작하기**

* Spring Boot 2.2+ 버전부터 JUnit 5 의존성이 추가.
* JUnit 5 부터는 Class와 Method의 접근 지정자가 반드시 public일 필요가 없다.

#### 참고.Spring-Boot를 사용하지 않는 경우

```jsx
<dependency>

	<groupId>org.junit.jupiter</groupId>

	<artifactId>junit-jupiter-engine</artifactId>
	
	<version>5.5.2</version>
	
	<scope>test</scope>

</dependency>
```

#### 기본적인 어노테이션

* @Test
* @BeforeAll, @AfterAll
  * Test Class 안에 있는 모든 Test가 실행하기 전,후에 1번만 실행된다.
  * 반드시 static method로 작성
  * private 사용 불가
  * return type 사용 불가
* @BeforeEach, @AfterEach
  * Test Class 안에 있는 모든 Test에 대해 각 Test가 실행하기 전,후에 실행된다.
* @Disabled
  * 해당 Test 미실행

## **JUnit 5 테스트 이름 표시하기**

@DisplayNameGeneration

* Method와 Class 레퍼런스를 사용해서 테스트 이름을 표기하는 방법 설정.
* 기본 구현체로 ReplaceUnderscores 제공

@DisplayName

* 어떤 테스트인지 테스트 이름을 보다 쉽게 표현할 수 있는 방법을 제공하는 애노테이션.
* @DisplayNameGeneration 보다 우선 순위가 높다.

참고:

* [https://junit.org/junit5/docs/current/user-guide/#writing-tests-display-names](https://junit.org/junit5/docs/current/user-guide/#writing-tests-display-names)

Test Method의 naming은 snake\_case으로 많이 작성된다.

```java
@DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class) // 1
class SampleTest {

	@Test
	@DisplayName("한글가능, 공백 가능") // 2
	void create_sample() {
		Sample sample = new Sample();
		assertNotNull(sample);
	}

	...

}
```

1. 적용 전 : create\_sample 적용 뒤 : create sample
2. 위 방법에 비해 각 테스트에 대해 유연한 네이밍을 할 수 있다. 한글, 공백, 이모지 가능

### org.junit.jupiter.api.Assertions.\*

마지막 매개변수로 Supplier\<String> 타입의 인스턴스를 람다 형태로 제공할 수 있다.

* 복잡한 메시지 생성해야 하는 경우 사용하면 실패한 경우에만 해당 메시지를 만들게 할 수 있다.

[AssertJ](https://joel-costigliola.github.io/assertj/), [Hemcrest](https://hamcrest.org/JavaHamcrest/), [Truth](https://truth.dev) 등의 라이브러리를 사용할 수도 있다.
