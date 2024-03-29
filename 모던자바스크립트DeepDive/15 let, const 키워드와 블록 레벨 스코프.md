# `let`, `const` 키워드와 블록 레벨 스코프

## 1. `var` 키워드로 선언한 변수의 문제점

### 변수 중복 선언 허용

- 변수 중복 선언 가능 (에러 안 뜸)
  => 동일한 이름의 변수가 이미 있다는 걸 모르고 중복 선언 + 값 할당까지 해버리면, 먼저 선언된 변수의 값이 변경되는 부작용 발생

```js
var x = 1;
var x = 100;
console.log(x); // 100
```

### 함수 레벨 스코프

- 오직 함수의 코드 블록만 스코프로 인정
- 다른 코드 블록 안에서 선언한 변수는 전역 변수가 됨
  => 의도치 않게 변수를 중복 선언하는 상황 발생

```js
var x = 1;
if (true) {
  var x = 10;
}
console.log(x); // 10
```

### 변수 호이스팅

- 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작
  - 변수 선언문 이전에 참조 가능
  - 변수 할당문 이전에 참조하면 `undefined` 반환

</br>

## 2. `let` 키워드

### 변수 중복 선언 금지

- 변수 중복 선언 시 에러 발생 (SyntaxError)

### 블록 레벨 스코프

- 모든 코드 블록을 지역 스코프로 인정

```js
let foo = 1; // 전역 변수
{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

### 변수 호이스팅

- 호이스팅이 발생하지 않는 것처럼 동작

```jsx
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

- `var`의 경우,
  - 런타임 이전에 `선언 단계`와 `초기화 단계`가 진행됨
- `let`의 경우,
  - `선언 단계` - 런타임 이전
  - `초기화 단계` - 변수 선언문에 도달했을 때
    - 초기화 단계 이전에 변수 참조시, 참조에러 발생..
    - 일시적 사각지대 (Temporal Dead Zone; TDZ)
      : 변수 스코프 시작 ~ 초기화 단계 지점까지 변수 참조 불가한 구간
    - 변수가 호이스팅되지 않는 것처럼 보이지만, 사실 호이스팅 됨

```js
let foo = 1;
{
  console.log(foo); // let이 호이스팅 되지 않는다면 1이 출력되어야 하지만, 호이스팅 되므로 ReferenceError가 출력됨
  let foo = 2;
}
```

### 전역 객체와 `let`

- `var`와 달리, `let`으로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님 (`window.변수` 이런식으로 접근 불가)
- `let` 전역 변수는 보이지 않는 개념적인 블록 내에서 존재...(추후 챕터에서 설명)

</br>

## 3. `const` 키워드

### let과 공통점

- 블록 레벨 스코프 가짐
- 호이스팅이 발생하지 않는 것처럼 동작
- 대부분 `let` 키워드와 동일 (아래는 `let` 키워드와 다른 점 위주로..)

### 선언과 초기화

- `const`로 선언한 변수는 반드시 선언과 동시에 초기화해야함
- 그렇지 않은 경우 SyntaxError 발생

### 재할당 금지

- 재할당 금지

```js
const foo = 1;
foo = 2; // TypeError
```

- 원시 값 할당한 경우, 변수 값 변경 불가

  - 상수를 표현하는데 사용하기도 함
  - 상수란 재할당이 금지된 변수
  - 가독성, 유지 보수성 등에 좋음
  - 상수 표현시, 주로 스네이크 케이스로 표현

- 객체를 할당 한 경우, 변경 가능
  - 재할당이 가능하다는 의미가 아님
  - 객체 타입은 재할당 없이도 프로퍼티 동적 생성, 삭제 등을 통해 변경 가능 (당연히 변수에 할당된 참조값이 변하는 것은 아님)
