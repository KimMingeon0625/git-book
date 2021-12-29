---
description: 어색해진 js 기초 문법 정리.
---

# JavaScript 기초 문법

## 변수 선언 및 사용

타입에 대한 구분 없이 var로 선언 후 사용.

변수의 타입은 실행시에 스크립트 엔진이 결정하지만 V8엔진의 경우 JIT compiler가 기계어로 컴파일 하기 직전에 결정.

```
// 변수 선언
var name;

// 문자열 입력 ( ' or " )
name = '김민건'

// 문자 & 숫자 관계없이 var 선언 가능.
var num1;
num1 = 21;

// 선언과 동시 초기화 가능.
var num2 = 3;

// 숫자와 문자열을 더할 경우 문자열이 된다. 아래의 sum은 '김민건21'이 된다.
var sum = name + num1;

```



## 함수의 선언 및 사용

function 함수명(파라미터) { 로직 } 형태로 선언.

```
// 함수 선언
function sum(param1, param2, param2){
    return param1 + param2 + param3;
}

// 함수 호출로 초기화
var result = sum(1,2,3)

```



## 조건문

```
var a = 10;

if( a > 11) {
    console.log('a > 11');
} else if ( a == 11) {
    console.log('a == 11');
} else {
    console.log('a < 11')'
}

```



## 반복문

```
var i = 0;

while( i < 10) {
    console.log ("result : " + i );
    i = i+1;
}
```



## 클래스

javascript는 프로토타입기반의 함수형 언어이기 때문에 특별하게 객체지향을 위한 class는 없다.

하지만 함수형 언어들의 특징은 함수자체를 하나의 객체로 취급하기 때문에 단일 함수 또는 파일 자체를 하나의 class처럼 사용할 수 있다.

```
// class 선언
function Camel(msg){
    this.name = '쌍봉이';
    this.message = msg;
    
    // this를 사용하지 않은 변수
    message2 = "Stop!";
    
    this.print = function(){
         console.log(message2);
     };
}

// 객체 생성
var myCamel = new Camel('낙타 탄생!');
console.log(myCamel.message);

// 내부 변수는 외부에서 참조할 수 없다.
console.log(myCamel.message2); // -> x

// this로 선언한 함수를 통해 내부 변수 출력
myCamel.print();


```





출처

* [https://javafa.gitbooks.io/nodejs\_server\_basic/content/chapter2.html](https://javafa.gitbooks.io/nodejs\_server\_basic/content/chapter2.html)
