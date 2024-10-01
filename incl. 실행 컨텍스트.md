### 변수 호이스팅 (Variable Hoisting)

```javascript
console.log(A); // undefined
var A = 5;
console.log(A);
```

- **호이스팅 동작**: 자바스크립트는 다른 언어와 달리 변수 선언 전에 변수를 사용할 수 있지만 오류가 발생하지 않습니다.
- **실행 과정**:
  1. 코드 실행 시 전역 실행 컨텍스트가 생성되어 콜 스택에 쌓입니다.
  2. 전체 코드를 스캔하면서 `var`로 선언된 변수들을 미리 선언합니다.
  3. 선언 과정에서 실행 컨텍스트의 환경 레코드에 식별자 `A`를 추가하고, `undefined`로 초기화합니다. (생성 단계)
- **`let`과 `const`의 차이**:
  - `let` 또는 `const`로 선언된 변수는 선언 이전에 참조할 수 없습니다. (일시적 사각지대)
  - 이는 `var`와 달리 선언만 하고 초기화를 하지 않기 때문에 발생합니다.
  - 이러한 특성은 변수 선언 이전에 사용을 방지하여 일반적인 프로그래밍 패턴을 따르도록 언어 차원에서 보완한 것입니다.

### 함수 호이스팅 (Function Hoisting)

#### 변수에 할당된 함수 (함수 표현식)

```javascript
study(); // TypeError: study is not a function

var study = () => {
    // todo
};
```

- **설명**: `var`로 선언된 `study` 변수는 호이스팅 시 `undefined`로 초기화됩니다. 따라서 `study()` 호출 시 `undefined`는 함수가 아니므로 `TypeError`가 발생합니다.

```javascript
study(); // ReferenceError: Cannot access 'study' before initialization

const study = () => {
    // todo
};
```

- **설명**: `const`로 선언된 `study` 변수는 호이스팅되지만 초기화되지 않아 `ReferenceError`가 발생합니다.

#### 함수 선언문

```javascript
study(); // 정상적으로 실행됨

function study() { // 함수 선언문
    // todo
}
```

- **설명**: 함수 선언문은 호이스팅 시 전체 함수가 초기화되므로 코드의 어느 위치에서든 함수 호출이 가능합니다.

### 스코프 체이닝 (Scope Chaining)

```javascript
let name = 'jason';

function wow3() {
    console.log(name);
}

function wow2() {
    let name = 'shelly';
    wow3();
}

function wow1() {
    let name = 'kevin';
    wow2();
}

wow1();
```

- **실행 컨텍스트 생성**:
  1. **Global 실행 컨텍스트** 생성
  2. `wow1` 실행 시 **wow1 실행 컨텍스트** 생성
  3. `wow2` 실행 시 **wow2 실행 컨텍스트** 생성
  4. `wow3` 실행 시 **wow3 실행 컨텍스트** 생성
- **스코프 체이닝 동작**:
  - `wow3` 함수 내부에서 `name`을 참조하려고 할 때:
    1. **wow3 실행 컨텍스트**에서 `name`을 찾습니다. 없으므로 다음 단계로 넘어갑니다.
    2. **wow2 실행 컨텍스트**에서 `name`을 찾습니다. `name = 'shelly'`가 있으므로 이를 사용합니다.
- **변수 섀도잉 (Variable Shadowing)**:
  - `wow1`과 `global` 실행 컨텍스트에 있는 `name` 변수는 `wow2`의 `name` 변수에 의해 가려집니다.
- **스코프 체인**:
  - 식별자를 결정할 때 연결된 스코프들의 리스트를 스코프 체인이라고 하며, 이를 통해 외부 스코프에서 변수를 검색하는 과정을 **스코프 체이닝**이라고 부릅니다.

### 실행 컨텍스트 (Execution Context)

- **정의**: 코드를 실행하는 데 필요한 환경을 제공하는 객체입니다.
- **구성 요소**:
  - **변수 객체 (Variable Object)**: 변수, 함수 선언, 매개변수 등을 저장
  - **스코프 체인 (Scope Chain)**: 현재 실행 중인 컨텍스트의 스코프와 외부 스코프를 연결하는 체인
  - **`this` 바인딩**: 현재 컨텍스트에서의 `this` 값

- **실행 단계**:
  1. **생성 단계**: 변수와 함수가 호이스팅되고 초기화됩니다.
  2. **실행 단계**: 코드가 순차적으로 실행됩니다.