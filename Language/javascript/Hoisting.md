# Hoisting

> hoist: 끌어올리다.

자바스크립트에서 끌어올려지는 것은 변수이다. `var keyword` 로 선언된 모든 변수 선언은 호이스트 된다. 

변수가 함수 내에서 정의되었을 경우, 선언이 함수의 최상위로, 함수 바깥에서 정의되었을 경우, 전역 컨텍스트의 최상위로 변경이 된다.

```javascript
var x;
console.log(x)
x = 100
console.log(x)
```
다른 언어의 경우 변수 x를 선언하지 않고 출력하려 한다면 에러를 발생할 것 이다. 
하지만 자바스크립트의 경우 `undefined`라고 하고 넘어간다. 
`var x = 100` 구문에서 `var x`를 호이스트 하기 때문이다.

선언문은 항시 자바스크립트 엔진 구동시 가장 최우선으로 해석하므로 호이스팅 되고, **할당 구문은` 런타임 과정`에서 이루어지기 때문에** 호이스팅 되지 않는다.

### 함수 선언문과 함수 표현식
- 함수 선언문: function 정의부만 존재하고 별도의 할당 명령이 없는 것을 의미한다. 함수명이 정의되어야 한다.
- 함수 표현식: 정의한 function을 별도의 변수에 할당하는 것을 의미한다. 함수명이 없어도 된다.

```javascript
function a() { } //함수 서언문 함수명 a가 곧 변수명
a()

var b = function() { }//익명 함ㅅ 표현식 변수명 b가 곧 함수명
b()

var b = function d() { } //기명 함수 표현식 변수명은 c 함수명은 d
c()//실행 ok
d() // 에러!
```
`함수 선언문`은 전체를 호이스팅한 반면 `함수 표현식`은 변수 선언부만 호이스팅한다.

```js
foo( );
function foo( ){
  console.log(‘hello’);
};
// console> hello
```
foo 함수에 대한 선언을 호이스팅하여 global 객체에 등록시키기 때문에 hello가 제대로 출력된다.

```js
foo( );
var foo = function( ) {
  console.log(‘hello’);
};
// console> Uncaught TypeError: foo is not a function
```
이 두번째 예제의 함수 표현은 함수 리터럴을 할당하는 구조이기 때문에 호이스팅 되지 않으며 그렇기 때문에 런타임 환경에서 Type Error를 발생시킨다.

**호이스팅 예시**
```js
function a() {
    console.log(b) // (1)
    var b = 'bbb' // 수집 대상 1(변수 선언)
    console.log(b) // (2)
    function b() {} // 수집 대상 2(함수 선언)
    console.log(b)
}
```

호이스팅 된 결과로 보면

```js
function a() {
    var b
    function b() {} //함수 선언은 전체를 끌어올린다

    console.log(b) //(1) b함수
    b = 'bbb'
    console.log(b) //(2) 'bbb'
    console.log(b) //(3) 'bbb'
}

a()
```

## 면접 질문

> 1. 호이스팅(Hoisting)"에 대해서 설명하세요.

호이스팅은 인터프리터가 자바스크립트 코드를 해석함에 있어서, Global 영역 또는 함수 영역 안에 대해서 주어진 선언들을 모두 끌어올려서 해당 유효 범위 최상단에 선언하는 것을 의미한다.