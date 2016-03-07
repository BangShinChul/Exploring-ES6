#  9. Variables and scoping
변수와 스코핑

이번 장은 변수와 스코핑이 ECMAScript 6 에서 어떻게 핸들링 되는지 고찰해본다

##  9.1 개요
ES6에서는 두가지의 새로운 변수 선언을 제공한다. let 과 const 이다. var 를 사용하는 ES5의 변수 선언법을 거의 대체한다.

###  9.1.1 let
let 은 var 와 비슷하게 동작하지만, 그것이 선언하는 변수는 블럭 스코프이다. 그것은 단지 현재 블럭에서만 존재한다. var 는 함수 스코프이다.

다음 코드에 따르면 당신은 let 선언 변수인 tmp 가 단지 A 라인에서 시작한 블럭에만 존재하는걸 볼 수 있다


```javascript
function order(x, y) {
    if (x > y) { // (A)
        let tmp = x;
        x = y;
        y = tmp;
    }
    console.log(tmp===x); // ReferenceError: tmp is not defined
    return [x, y];
}
```

### 9.1.2 const
const 는 let 과 비슷하게 동작하지만 당신은 나중에 값을 변경할 수 없으며, 즉시 초기화로만 선언해야하는 변수이다.

```javascript
const foo; // SyntaxError: missing = in const declaration

const bar = 123;
bar = 456; // TypeError: `bar` is read-only
```

for-of 루프 순회당 한번의 바인딩 (변수를 위한 저장 공간을) 생성하기에 루프 변수로서의 const 선언은 OK 이다. 

```javascript
for (const x of ['a', 'b']) {
    console.log(x);
}
// Output:
// a
// b
```

###  9.1.3 변수 선언 방법들
다음 테이블은 ES6에서 변수 선언이 가능한 여섯가지 방법을 보여주고 있다.

| | Hoisting | Scope | Creates global properties |
| -------- | ----- | ------- | ------ | ---------- |
| var | Declaration | Function | Yes |
| let | Temporal dead zone | Block | No |
| const | Temporal dead zone | Block | No |
| function | Complete | Block | Yes |
| class | No | Block | No |
| import | Complete | Module-global | No |

> [Temporal dead zone (TDZ)]
>
> 일시적 사각 지대. 해당 영역에 선언은 되지만 참조 불가의 undefined 상태로 선언되는 것을 의미한다.
> let, const 가 이에 해당되고 함수의 기본 파라미터도 TDZ 에 해당하기에 조심해야 한다

##  9.2 let 과 const 를 통한 블럭 스코핑

let 과 const 는 블럭 스코프 변수를 생성한다 - 그것들은 단지 자신을 둘러싸고 있는 블럭 내부에서만 존재한다. 다음 코드는 const 로 선언된 변수 tmp 가 단지 if 문의 다음 블럭 안에서만 존재하는걸 보여준다

```javascript
function func() {
    if (true) {
        const tmp = 123;
    }
    console.log(tmp); // ReferenceError: tmp is not defined
}
```

대조적으로, var 선언 변수는 함수 스코프이다

```javascript
function func() {
    if (true) {
        var tmp = 123;
    }
    console.log(tmp); // 123
}
```

블럭 스코핑은 당신이 변수를 함수 안에서 가릴 수 있다는 걸 뜻한다

```javascript
function func() {
  const foo = 5;
  if (···) {
     const foo = 10; // shadows outer `foo`
     console.log(foo); // 10
  }
  console.log(foo); // 5
}
```

9.3 const 는 불변 (immutable) 변수를 생성한다
let 으로 생성한 변수는 변한다 (mutable)

```javascript
let foo = 'abc';
foo = 'def';
console.log(foo); // def
```

const 로 생성된 상수들, 변수들은 불변이다. - 당신은 그 변수들에 다른 값을 할당할 수 없다.

```javascript
const foo = 'abc';
foo = 'def'; // TypeError
```

스펙에 따르면 const 변수의 변경은 항상 TypeError 를 던진다
