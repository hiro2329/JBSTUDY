# 모던 자바스크립트 정리(ES6+)

## 1. `var`, `let`, `const`의 차이
- **`var`**
  - **함수 스코프**를 가짐 (블록 스코프를 지원하지 않음).
  - **중복 선언 가능**.
  - 선언 전에 접근 가능하지만 값은 `undefined`로 초기화됨 (**호이스팅**).
    - <sub>호이스팅: 변수 선언과 함수 선언이 해당 스코프의 최상단으로 끌어올려지는 동작.</sub>

- **`let`**
  - **블록 스코프**를 가짐.
  - **중복 선언 불가능**.
  - 선언 전에 접근 불가능 (**Temporal Dead Zone**, TDZ).
    - TDZ: 변수가 선언되기 전까지는 해당 변수에 접근할 수 없는 구간.

- **`const`**
  - **블록 스코프**를 가짐.
  - **중복 선언 불가능**.
  - 선언과 동시에 초기화가 필요.
  - **값 재할당 불가능** (단, 객체나 배열의 내부 속성은 변경 가능).
    - `const`는 변수 자체를 재할당할 수 없지만, 객체나 배열의 내부 속성은 변경 가능.(변수가 참조하는 **메모리 주소**(상수)를 변경할 수 없음)

### 주요 차이점 요약

<table>
  <thead>
    <tr>
      <th>특징</th>
      <th>var</th>
      <th>let</th>
      <th>const</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>스코프</td>
      <td>함수 스코프</td>
      <td>블록 스코프</td>
      <td>블록 스코프</td>
    </tr>
    <tr>
      <td>중복 선언</td>
      <td>가능</td>
      <td>불가능</td>
      <td>불가능</td>
    </tr>
    <tr>
      <td>선언 전 접근</td>
      <td>가능 (undefined)</td>
      <td>불가능 (TDZ)</td>
      <td>불가능 (TDZ)</td>
    </tr>
    <tr>
      <td>초기화 필요 여부</td>
      <td>선택</td>
      <td>선택</td>
      <td>필수</td>
    </tr>
    <tr>
      <td>값 재할당</td>
      <td>가능</td>
      <td>가능</td>
      <td>불가능</td>
    </tr>
    <tr>
      <td>객체/배열 내부 변경</td>
      <td>가능</td>
      <td>가능</td>
      <td>가능</td>
    </tr>
  </tbody>
</table>

### 예시
```
// var
var x = 10;
var x = 20; // 중복 선언 가능
console.log(x); // 20

// let
let y = 10;
// let y = 20; // SyntaxError: 'y' has already been declared

// const
const z = 10;
// z = 20; // TypeError: Assignment to constant variable

// 객체 내부 속성 변경 (const)
const obj = { a: 1 };
obj.a = 2; // 가능 (객체 내부 속성 변경)
console.log(obj); // { a: 2 }

// const 변수 자체 재할당 시 오류
// obj = { b: 2 }; // TypeError: Assignment to constant variable
// const는 변수가 참조하는 메모리 주소를 변경할 수 없음
// 즉, 객체 자체를 새로운 객체로 대체할 수 없음
```

## 2. 단축 프로퍼티 (Shorthand Properties)
- ES6에서 객체 리터럴을 작성할 때, 변수 이름과 프로퍼티 이름이 같으면 단축 프로퍼티를 사용할 수 있다.
- 단축 프로퍼티는 객체 리터럴에서 **변수 이름과 속성 이름이 동일할 때** 사용 가능.

### 예시
```
const name = "John";
const age = 30;

// 단축 프로퍼티 사용 전
const person1 = { name: name, age: age };

// 단축 프로퍼티 사용 후
const person2 = { name, age };

console.log(person2); // { name: "John", age: 30 }
```

## 3. 템플릿 리터럴 (Template Literals)
- ES6에서 도입된 문자열을 작성하는 새로운 방법.
- 백틱(```)을 사용하여 여러 줄 문자열을 작성할 수 있으며, `${}`를 사용하여 변수나 표현식을 삽입할 수 있다.
### 예시
```
const name = "John";
const age = 30;
const greeting = `안녕하세요, 제 이름은 ${name}이고 나이는 ${age}세입니다.`;
console.log(greeting); // 안녕하세요, 제 이름은 John이고 나이는 30세입니다.
```

## 4. 화살표 함수 (Arrow Functions)
- ES6에서 도입된 새로운 함수 표현식.
- `function` 키워드 대신 `=>`를 사용하여 함수를 정의할 수 있다.
- `this` 바인딩이 정적으로 결정되므로, 일반 함수와 다르게 **화살표 함수는 자신이 정의된 스코프의 `this`를 참조**.
 
#### 주의점 
- 화살표 함수는 this, arguments, super, new.target을 바인딩하지 않음.
- 생성자 함수로 사용할 수 없음.

### 예시
```
const add = (a, b) => a + b; // return 생략 가능 (a,b)=> return a+b
const square = x => x * x;
const greet = (name) => {
  return `안녕하세요, ${name}님!`;
};
console.log(add(2, 3)); // 5
console.log(square(4)); // 16
console.log(greet("John")); // 안녕하세요, John님!
```

## 5. 구조 분해 할당 == 비구조화 할당 (Destructuring Assignment)
- ES6에서 도입된 문법으로, 배열이나 객체의 값을 쉽게 추출할 수 있다.
- 배열과 객체 모두 지원하며, 중첩된 구조도 지원한다.
### 예시
```
// 배열 구조 분해 할당
const arr = [1, 2, 3];
const [a, b, c] = arr;
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3

// 객체 구조 분해 할당
const color = { r: 255, g: 0, b: 0 };
const { r, g, b } = color;
console.log(r); // 255
console.log(g); // 0
console.log(b); // 0

// 중첩된 구조 분해 할당
const person = {
  name: "John",
  age: 30,
  address: {
    city: "Seoul",
    country: "Korea"
  }
};
const { name, address: { city, country } } = person;
console.log(name); // John
console.log(city); // Seoul
console.log(country); // Korea
```

## 6. 나머지 매개변수 (Rest Parameters)
- ES6에서 도입된 문법으로, 함수의 매개변수로 전달된 인자를 배열로 받을 수 있다.
- `...`을 사용하여 나머지 매개변수를 정의할 수 있다.
### 예시
```
function introduce(name, ...hobbies) {
  console.log(`안녕하세요, 제 이름은 ${name}입니다.`);
  console.log(`취미는 ${hobbies.join(", ")}입니다.`);
}

introduce("John", "독서", "코딩", "등산");
// 출력:
// 안녕하세요, 제 이름은 John입니다.
// 취미는 독서, 코딩, 등산입니다.
```
## 7. 전개 연산자 (Spread Operator)
- ES6에서 도입된 문법으로, 배열이나 객체를 펼쳐서 새로운 배열이나 객체를 만들 수 있다.
- `...`을 사용하여 배열이나 객체를 펼칠 수 있다.
### 예시
```
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const mergedArr = [...arr1, ...arr2];
console.log(mergedArr); // [1, 2, 3, 4, 5, 6]
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const mergedObj = { ...obj1, ...obj2 };
console.log(mergedObj); // { a: 1, b: 2, c: 3, d: 4 }
```

