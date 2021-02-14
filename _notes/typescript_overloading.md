---
title: 'TypeScript 함수 오버로딩'
author: 'vlwkaos'
created: '2021-01-25'
published: true
---

TypeScript(이하 TS)는 JavaScript(이하 JS)의 문법적 설탕(Syntactic Sugar)이기 때문에 JS의 특징을 모두 갖고 있다. JS가 동적 타입 언어이다보니 함수의 인자도 가변적일 수 밖에 없고, 이 때문에 함수를 고유하게 식별할 수 있는 것은 이름 밖에 없다. 이는 자바의 함수(메소드) 서명이 인자의 개수와 타입에 따라 달라지는 것과는 대조된다.

```java
// Java
public void calculate(int a, int b)
public void calculate(double a, double b)
```

위처럼 자바는 함수 이름이 다르더라도 인자의 타입이 다르면 새로운 함수를 선언할 수 있다. 같은 함수이름을 사용하여 추상화를 잘 한다면 [[관리하기 쉬운 코드|코드를 관리하기]] 한결 수월해진다.

TS는 한 이름당 하나의 함수만 구현할 수 있기 때문에 오버로딩을 하려면 조금은 특이한 방법을 사용해야한다. [TS 공식문서](https://www.typescriptlang.org/docs/handbook/2/functions.html)에 나온 예제를 보자.

```typescript
// TypeScript
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  } else {
    return new Date(mOrTimestamp);
  }
}
const d1 = makeDate(12345678); // makeDate(number)
const d2 = makeDate(5, 5, 5); // makeDate(number, number, number)
const d3 = makeDate(1, 3); // 이건 구현되어 있지 않다.
```

TS에서는 오버로딩을 위한 함수 선언을 먼저 하고 아래에 오버로딩한 함수의 인자를 형태를 모두 포함한 함수를 구현한다. 아래 함수의 인자는 오버로딩하기 위해 선언한 모든 함수의 인자 타입 및 개수를 고려해야하고 따라서 네이밍이 `mOrTimestamp` 처럼 복잡해질 수 밖에 없다. 구현로직 역시 마찬가지로 한눈에 들어오지는 않는다. 각 함수의 인자 타입에 맞는 조건분기로 함수의 내용을 작성해야한다. 해당 조건분기가 어떤 함수를 오버로딩하는 것인지 주석을 달아놓는 편이 나을 것 같다. 

이런 방식의 오버로딩은 나쁜 코드가 될 가능성을 막지 못한다. 아래 예제를 보면 구현부의 인자형을 느슨하게 `any`로 선언하고 있다. 

```typescript
// TypeScript
function len(s: string): number;
function len(arr: any[]): number;
function len(x: any) {
  return x.length;
}
```

다행히도 `string`과 `any[]`가 모두 length라는 속성을 갖고 있어 괜찮지만 어떤 오버로딩을 구현하는 것인지 명확하게 분기로 나뉘어있지 않아 위험하다. 또한 `string`과 `any[]`형에 대해서만 함수가 선언되어 있으므로 둘 중 하나의 타입을 가진 인자만을 가질 수 있다. `any`지만 `string | any[]` (이런 타입을 Union Type이라고 함)는 아닌 것이다.

이런 경우에는 차라리 오버로딩을 하지 않고 Union Type을 이용하여 명확하게 다음처럼 구현하는 것이 낫다. 

```typescript
function len(arrOrS: any[] | string) {
  return arrOrS.length;
}
```

TS 공식 문서에서도 가능한 오버로딩보다는 Union Type을 이용하라고 되어있다. OOP와 유사한 문법적 설탕을 제공하기 위해 오버로딩을 할 수는 있도록 만들어 놨지만 그 뿐이라는 의미로 해석된다.



---

### 참조

- https://beginnersbook.com/2013/05/method-overloading/
- https://www.typescriptlang.org/docs/handbook/2/functions.html