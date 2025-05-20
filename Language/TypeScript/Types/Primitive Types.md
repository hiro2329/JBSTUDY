# 🧩 타입스크립트 기본 타입(Primitive Types)

타입스크립트의 기본 타입은 자바스크립트의 원시 타입과 동일하며, 값 자체를 저장합니다.

---

## 1. 문자열 타입 (`string`) 📝

문자열 데이터를 저장할 때 사용합니다.

```typescript
let lang: string = "Typescript";
// lang = 10; // 에러: string 타입에 number 할당 불가
```

---

## 2. 숫자 타입 (number) 🔢

정수, 실수 등 모든 숫자 데이터를 저장할 때 사용합니다.

```typescript
let age: number = 30;
let pi: number = 3.14;
```

---

## 3. 불리언 타입 (`boolean`) ✅

참(true) 또는 거짓(false) 값을 저장할 때 사용합니다.

```typescript
let isAdult: boolean = true;
let hasLicense: boolean = false;
```

---

## 4. 널 타입 (`null`) ❌

널 값이 할당될 수 있는 변수를 정의할 때 사용합니다. 타입스크립트에서는 `null`과 `undefined`를 구분합니다.

```typescript
let value: null = null;
// value = 10; // 에러: null 타입에 number 할당 불가
```

---

## 5. 언디파인드 타입 (`undefined`) ❓

정의되지 않은 값을 저장할 때 사용합니다. 변수가 선언되었지만 값이 할당되지 않은 상태를 나타냅니다.

```typescript
let name: undefined;
// name = "John"; // 에러: undefined 타입에 string 할당 불가
```

---

## 6. 심볼 타입 (`symbol`) 🔑

고유하고 변경 불가능한 값을 생성할 때 사용합니다. 주로 객체의 프로퍼티 키로 사용됩니다.

```typescript
let uniqueId: symbol = Symbol("id");
let anotherId: symbol = Symbol("id");
console.log(uniqueId === anotherId); // false
```

---

## 7. 빅인트 타입 (`bigint`) 🔢

정수 값을 저장할 때 사용합니다. 일반 숫자 타입보다 더 큰 정수를 표현할 수 있습니다.

```typescript
let bigNumber: bigint = 1234567890123456789012345678901234567890n;
let anotherBigNumber: bigint = BigInt(
  "1234567890123456789012345678901234567890"
);
console.log(bigNumber + anotherBigNumber); // 2469135780246913578024691357802469135780n
```

---

## 💡 타입 추론

타입스크립트는 변수 선언 시 초기값을 통해 타입을 추론합니다. 예를 들어:

```typescript
let name = `Typescript`;
name = 10; // 에러 name은 string 타입으로 선언했기 때문에 number를 넣을 수 없음
// 타입추론도 도움이 되지만 타입을 명시적으로 지정하는 것이 좋음
```

```typescript
//배열 타입 추론

let numbers = [1, 2, 3, 4, 5];

numbers.push(`6`); // 에러 numbers는 number[] 타입으로 선언했기 때문에 string을 넣을 수 없음
```

```typescript
// 함수 타입 추론
function add(a: number, b: number) {
  return a + b;
}
console.log(add(1, 2)); // 3
add(1, `2`); // 에러 // add 함수는 number 타입의 매개변수를 받기 때문에 string을 넣을 수 없음 return 타입도 number로 추론\
```

```typescript
// 객체 리터럴 타입 추론

let person = {
  name: `Typescript`,
  age: 10,
};
person.name = 20; // 에러 person은 { name: string; age: number; } 타입으로 선언했기 때문에 number를 넣을 수 없음
```

## 📝기본 타입 정리 & 참고

- 타입스크립트의 기본 타입은 자바스크립트의 원시 타입과 동일하지만, 타입 시스템이 추가되어 더 엄격하게 동작합니다.
- 기본 타입은 값 자체를 저장하며, 객체 타입과 구분됩니다.
- 타입을 명시적으로 지정하면 코드의 안정성과 가독성이 높아집니다.
- 타입 추론이 강력하지만, 타입을 **명시적으로 지정하는 것이 좋습니다.**
- `null`과 `undefined`는 타입스크립트에서 엄격하게 구분되므로, 혼용하지 않도록 주의해야 합니다.
- `bigint`와 `number`는 서로 연산이 불가능하니 타입 혼동에 주의하세요.
- `symbol`은 고유한 값이 필요할 때만 사용하며, 일반적인 변수에는 잘 사용하지 않습니다.
