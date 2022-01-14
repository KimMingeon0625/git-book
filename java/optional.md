---
description: JAVA 8 Optional
---

# Optional

## Optional이란?

Optional\<T>는 null이 올 수 있는 값을 감싸는 Wrapper 클래스.

**Java 8** 에서 도입 되었으며, 각종 메소드를 통해 Null에 대응한다.

{% hint style="info" %}
Optional은 null 또는 실제 값을 value로 갖는 wrapper로 감싸서 NPE(NullPointerException)로부터 자유로워지기 위해 나온 Wrapper 클래스이다.&#x20;

따라서 Optional을 반환하는 메소드는 절대 null을 갖는 value를 반환해서는 안된다. 또한 Optional은 값을 Wrapping하고 다시 풀고, null일 경우에는 대체하는 함수를 호출하는 등의 오버헤드가 있으므로 성능이 저하될 수 있다. 그렇기 때문에 메소드의 반환 값이 절대 null이 아니라면 Optional을 사용하지 않는 것이 성능저하가 적다.&#x20;

즉, Optional은 메소드의 결과가 null이 될 수 있으며, 클라이언트가 이 상황을 처리해야 할 때 사용하는 것이 좋다.\
\
출처: [https://mangkyu.tistory.com/70](https://mangkyu.tistory.com/70) \[MangKyu's Diary]
{% endhint %}

## Optional 단점

* 코드는 줄일 수 있지만 가독성이 저하된다.
* 단순 null 체크 용도로 사용하기에는 성능에 대한 트레이드 오프가 꽤 크다.
* Optional 이 사용된 약 95% 이상의 코드에서의 사용은 무의미하거나 손실이 더 크다.
* 여전히 Optional 관련 method는 발전 중이며, 정돈되지 않았다.

## Optional 객체 생성

### Optional.of (java8)

value가 null인 경우 NPE 예외를 던진다

### Optional.ofNullable (java8)

value가 null인 경우 비어있는 Optional을 반환

### Optional.empty (java8)

비어있는 옵셔널 객체 생성

## Optional 중간

### filter (java8)

predicate 값이 참인 경우 필터를 통과

### map (java8)

mapper 함수를 통해 변환

### flatMap (java8)

mapper 함수를 통해 변환. map() 메서드와 다른점은 **반환 값이 Optional이다.**

### or (java9)

기본값을 제공할 수 있 함수를 정의 연산 중 비어있는 옵셔널이 된다면, 다음 or() 메서드 진행

### stream (java9)

stream 객체로 전환이 가능.

## Optional 종단

### ifPresent (java8)

연산을 끝낸 후 값이 비어있지 않은 경우 실행

만약 비어있는 옵션널 객체를 받게 되는 경우 실행되지 않는다.

### isPresent (java8)

최종적으로 연산을 끝낸 뒤 객체 존재 여부 판별 (true/false)

### get (java8)

연산이 끝난 후 객체를 꺼낸다.

비어있는 경우 예외 발생.

### orElse (java8)

연산이 끝난 뒤에도 옵셔널 객체가 비어있다면, 기본값으로 제공할 객체를 지정

### orElseGet (java8)

연산이 끝난 뒤에도 옵셔널 객체가 비어있다면, 기본값으로 제공할 함수(비지니스 로직)를 지정

### orElseThrow (java8)

연산이 끝난 뒤에도 옵셔널 객체가 비어있다면, 예외 발생.

### ifpresentOrElse (java9)

두개의 파라미터를 받아 유효한 객체를 받는 경우, 첫번째 매개변수 실행.

객체가 유효하지 않은 경우, 두번째 매개변수 실행.

