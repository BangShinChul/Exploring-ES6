# 5. 새로운 수 와 Math 기능

이 챕터는 ECMAScript6의 새로운 수와 Math 기능을 설명한다.

## 5.1 개요
당신은 이제 수를 2진수과 8진수으로 표기 할 수 있다:

```javascript
> 0xFF // ES5: hexadecimal
255
> 0b11 // ES6: binary
3
> 0o10 // ES6: octal
8
```

### 5.1.2 새로운 Number 프로퍼티

이 전역 객체 Number는 몇 가지 새로운 프로퍼티를 얻었다. 다른 것들 사이에서:

* 반올림 오류에 대한 허용 오차와 부동 소수점 숫자와의 비교를 위한 Number.EPSILON.
* 자바스크립트 정수형이 안전한지 아닌지를 결정하는 메소드와 상수(정밀도 손실이 없는 부호를 포함한 53bit 범위):
  * Number.isSafeInteger(number)
  * Number.MIN_SAFE_INTEGER
  * Number.MAX_SAFE_INTEGER
 
### 5.1.3 새로운 Math 메소드
전역 객체 Math는 수치, 삼각 함수와 비트 연산에 대한 새로운 매소두를 가진다. 4가지 예제를 보자.

Max.sign()는 수의 부호를 반환한다.:

```javascript
> Math.sign(-8)
-1
> Math.sign(0)
0
> Math.sign(3)
1
```

Math.trunc() 는 수의 소수를 제거한다.:

```javascript
> Math.trunc(3.1)
3
> Math.trunc(3.9)
3
> Math.trunc(-3.1)
-3
> Math.trunc(-3.9)
-3
```

Math.log10() 기수가 10인 로그를 계산한다.

```javascript
> Math.log10(100)
2
```

Math.hypot() 인자값의 제곱의 합의 루트 값을 계산한다.

```javascript
> Math.hypot(3, 4)
5   
```


## 5.2 새로운 정수 리터럴

ECMAScript 5는 이미 16진수 정수에 대한 리터럴을 가진다.:

```javascript
> 0x9
9
> 0xA
10
> 0x10
16
> 0xFF
255
```

ECMAScript 6 두개의 새로운 종류의 리터럴을 갖는다.:

2진수 리터럴은 0b또는 0B의 접두사를 갖는다:

```javascript
  > 0b11
  3
  > 0b100
  4
```

8진수 리터럴은 0o또는 0O의 접두사를 갖는다. (그래, 0뒤에 대문자 O이다. ; 처음 변형을 사용하는 경우에도 괜찬다.)

```javascript
  > 0o7
  7
  > 0o10
  8
```

Number.prototype.toString(기수)메소드가 수를 다시 변환하는데 사용될 수 있다는것을 기억하라.:

```javascript
> (255).toString(16)
'ff'
> (4).toString(2)
'100'
> (8).toString(8)
'10'
```

### 5.2.1 8진법 리터럴 사용 예: 유닉스 스타일 파일 퍼미션
Node.js 파일 시스템 모듈에서 몇몇 함수는 모드에 대한 파라미터를 갖는다. 유닉스로 부터 남은 인코딩을 통해 이 값은 파일 퍼미션을 지정하는데 사용된다.:
* 퍼미션은 세가지 사용자 카테고리를 통해 지정된다.:
  * User: 파일의 소유자
  * Group: 파일에 연관된 그룹의 맴버
  * All: 모든 사람
* 각 카테고리는 아래의 퍼미션을 부여할 수 있다:
  * r (read): 이 카테고리에 있는 사용자는 파일을 읽는 것을 허용한다.
  * w (write): 이 카테고리에 있는 사용자는 파일을 변경하는 것을 허용한다.
  * x (execute): 이 카테고리에 있는 사용자는 파일을 실행하는 것을 허용한다.

이것은 퍼미션이 9bits로 표현된다는것을 의미한다 (3 카테고리 마다 3개의 퍼미션).:

| |User|Group|All|
|---|---|---|---|
|Permissions|r, w, x|r, w, x|r, w, x|
|Bit|8, 7, 6|5, 4, 3|2, 1, 0|

사용자의 단일 카테고리의 퍼미션의 3비트로 저장된다.:

|Bits|Permissions|Octal digit|
|000|–––|0|
|001|––x|1|
|010|–w–|2|
|011|–wx|3|
|100|r––|4|
|101|r–x|5|
|110|rw–|6|
|111|rwx|7|

That means that octal numbers are a compact representation of all permissions, you only need 3 digits, one digit per category of users. Two examples:

755 = 111,101,101: I can change, read and execute; everyone else can only read and execute.
640 = 110,100,000: I can read and write; group members can read; everyone can’t access at all.
5.2.2 parseInt() and the new integer literals
parseInt() has the following signature:

parseInt(string, radix?)
It provides special support for the hexadecimal literal notation – the prefix 0x (or 0X) of string is removed if:

radix is missing or 0. Then radix is set to 16.
radix is already 16.
For example:

> parseInt('0xFF')
255
> parseInt('0xFF', 0)
255
> parseInt('0xFF', 16)
255
In all other cases, digits are only parsed until the first non-digit:

> parseInt('0xFF', 10)
0
> parseInt('0xFF', 17)
0
However, parseInt() does not have special support for binary or octal literals!

> parseInt('0b111')
0
> parseInt('0b111', 2)
0
> parseInt('111', 2)
7

> parseInt('0o10')
0
> parseInt('0o10', 8)
0
> parseInt('10', 8)
8
If you want to parse these kinds of literals, you need to use Number():

> Number('0b111')
7
> Number('0o10')
8
Alternatively, you can also remove the prefix and use parseInt() with the appropriate radix:

> parseInt('111', 2)
7
> parseInt('10', 8)
8
5.3 New static Number properties
This section describes new properties that the constructor Number has picked up in ECMAScript 6.

5.3.1 Previously global functions
Four number-related functions are already available as global functions and have been added to Number, as methods: isFinite and isNaN, parseFloat and parseInt. All of them work almost the same as their global counterparts, but isFinite and isNaN don’t coerce their arguments to numbers, anymore, which is especially important for isNaN. The following subsections explain all the details.

5.3.1.1 Number.isFinite(number)
Is number an actual number (neither Infinity nor -Infinity nor NaN)?

> Number.isFinite(Infinity)
false
> Number.isFinite(-Infinity)
false
> Number.isFinite(NaN)
false
> Number.isFinite(123)
true
The advantage of this method is that it does not coerce its parameter to number (whereas the global function does):

> Number.isFinite('123')
false
> isFinite('123')
true
5.3.1.2 Number.isNaN(number)
Is number the value NaN? Making this check via === is hacky. NaN is the only value that is not equal to itself:

> const x = NaN;
> x === NaN
false
Therefore, this expression is used to check for it

> x !== x
true
Using Number.isNaN() is more self-descriptive:

> Number.isNaN(x)
true
Number.isNaN() also has the advantage of not coercing its parameter to number (whereas the global function does):

> Number.isNaN('???')
false
> isNaN('???')
true
5.3.1.3 Number.parseFloat and Number.parseInt
The following two methods work exactly like the global functions with the same names. They were added to Number for completeness sake; now all number-related functions are available there.

Number.parseFloat(string)1
Number.parseInt(string, radix)2
5.3.2 Number.EPSILON
Especially with decimal fractions, rounding errors can become a problem in JavaScript3. For example, 0.1 and 0.2 can’t be represented precisely, which you notice if you add them and compare them to 0.3 (which can’t be represented precisely, either).

> 0.1 + 0.2 === 0.3
false
Number.EPSILON specifies a reasonable margin of error when comparing floating point numbers. It provides a better way to compare floating point values, as demonstrated by the following function.

function epsEqu(x, y) {
    return Math.abs(x - y) < Number.EPSILON;
}
console.log(epsEqu(0.1+0.2, 0.3)); // true
5.3.3 Number.isInteger(number)
JavaScript has only floating point numbers (doubles). Accordingly, integers are simply floating point numbers without a decimal fraction.

Number.isInteger(number) returns true if number is a number and does not have a decimal fraction.

> Number.isInteger(-17)
true
> Number.isInteger(33)
true
> Number.isInteger(33.1)
false
> Number.isInteger('33')
false
> Number.isInteger(NaN)
false
> Number.isInteger(Infinity)
false
5.3.4 Safe integers
JavaScript numbers have only enough storage space to represent 53 bit signed integers. That is, integers i in the range −253 < i < 253 are safe. What exactly that means is explained momentarily. The following properties help determine whether a JavaScript integer is safe:

Number.isSafeInteger(number)
Number.MIN_SAFE_INTEGER
Number.MAX_SAFE_INTEGER
The notion of safe integers centers on how mathematical integers are represented in JavaScript. In the range (−253, 253) (excluding the lower and upper bounds), JavaScript integers are safe: there is a one-to-one mapping between them and the mathematical integers they represent.

Beyond this range, JavaScript integers are unsafe: two or more mathematical integers are represented as the same JavaScript integer. For example, starting at 253, JavaScript can represent only every second mathematical integer:

> Math.pow(2, 53)
9007199254740992

> 9007199254740992
9007199254740992
> 9007199254740993
9007199254740992
> 9007199254740994
9007199254740994
> 9007199254740995
9007199254740996
> 9007199254740996
9007199254740996
> 9007199254740997
9007199254740996
Therefore, a safe JavaScript integer is one that unambiguously represents a single mathematical integer.

5.3.4.1 Static Number properties related to safe integers
The two static Number properties specifying the lower and upper bound of safe integers could be defined as follows:

Number.MAX_SAFE_INTEGER = Math.pow(2, 53)-1;
Number.MIN_SAFE_INTEGER = -Number.MAX_SAFE_INTEGER;
Number.isSafeInteger() determines whether a JavaScript number is a safe integer and could be defined as follows:

Number.isSafeInteger = function (n) {
    return (typeof n === 'number' &&
        Math.round(n) === n &&
        Number.MIN_SAFE_INTEGER <= n &&
        n <= Number.MAX_SAFE_INTEGER);
}
For a given value n, this function first checks whether n is a number and an integer. If both checks succeed, n is safe if it is greater than or equal to MIN_SAFE_INTEGER and less than or equal to MAX_SAFE_INTEGER.

5.3.4.2 When are computations with integers correct?
How can we make sure that results of computations with integers are correct? For example, the following result is clearly not correct:

> 9007199254740990 + 3
9007199254740992
We have two safe operands, but an unsafe result:

> Number.isSafeInteger(9007199254740990)
true
> Number.isSafeInteger(3)
true
> Number.isSafeInteger(9007199254740992)
false
The following result is also incorrect:

> 9007199254740995 - 10
9007199254740986
This time, the result is safe, but one of the operands isn’t:

> Number.isSafeInteger(9007199254740995)
false
> Number.isSafeInteger(10)
true
> Number.isSafeInteger(9007199254740986)
true
Therefore, the result of applying an integer operator op is guaranteed to be correct only if all operands and the result are safe. More formally:

isSafeInteger(a) && isSafeInteger(b) && isSafeInteger(a op b)
implies that a op b is a correct result.

Source of this section
“Clarify integer and safe integer resolution”, email by Mark S. Miller to the es-discuss mailing list.

5.4 Math
The global object Math has several new methods in ECMAScript 6.

5.4.1 Various numerical functionality
5.4.1.1 Math.sign(x)
Returns the sign of x as -1 or +1. Unless x is either NaN or zero; then x is returned4.

> Math.sign(-8)
-1
> Math.sign(3)
1

> Math.sign(0)
0
> Math.sign(NaN)
NaN

> Math.sign(-Infinity)
-1
> Math.sign(Infinity)
1
5.4.1.2 Math.trunc(x)
Removes the decimal fraction of x. Complements the other rounding methods Math.floor(), Math.ceil() and Math.round().

> Math.trunc(3.1)
3
> Math.trunc(3.9)
3
> Math.trunc(-3.1)
-3
> Math.trunc(-3.9)
-3
You could implement Math.trunc() like this:

function trunc(x) {
    return Math.sign(x) * Math.floor(Math.abs(x));
}
5.4.1.3 Math.cbrt(x)
Returns the cube root of x (∛x).

> Math.cbrt(8)
2
5.4.2 Using 0 instead of 1 with exponentiation and logarithm
A small fraction can be represented more precisely if it comes after zero. I’ll demonstrate this with decimal fractions (JavaScript’s numbers are internally stored with base 2, but the same reasoning applies).

Floating point numbers with base 10 are internally represented as mantissa × 10exponent. The mantissa has a single digit before the decimal dot and the exponent “moves” the dot as necessary. That means if you convert a small fraction to the internal representation, a zero before the dot leads to a smaller mantissa than a one before the dot. For example:

(A) 0.000000234 = 2.34 × 10−7. Significant digits: 234
(B) 1.000000234 = 1.000000234 × 100. Significant digits: 1000000234
Precision-wise, the important quantity here is the capacity of the mantissa, as measured in significant digits. That’s why (A) gives you higher precision than (B).

You can see this in the following interaction: The first number (1 × 10−16) registers as different from zero, while the same number added to 1 registers as 1.

> 1e-16 === 0
false
> 1 + 1e-16 === 1
true
5.4.2.1 Math.expm1(x)
Returns Math.exp(x)-1. The inverse of Math.log1p().

Therefore, this method provides higher precision whenever Math.exp() has results close to 1. You can see the difference between the two in the following interaction:

> Math.expm1(1e-10)
1.00000000005e-10
> Math.exp(1e-10)-1
1.000000082740371e-10
The former is the better result, which you can verify by using a library (such as decimal.js) for floating point numbers with arbitrary precision (“bigfloats”):

> var Decimal = require('decimal.js').config({precision:50});
> new Decimal(1e-10).exp().minus(1).toString()
'1.000000000050000000001666666666708333333e-10'
5.4.2.2 Math.log1p(x)
Returns Math.log(1 + x). The inverse of Math.expm1().

Therefore, this method lets you specify parameters that are close to 1 with a higher precision.

We have already established that 1 + 1e-16 === 1. Therefore, it is no surprise that the following two calls of log() produce the same result:

> Math.log(1 + 1e-16)
0
> Math.log(1 + 0)
0
In contrast, log1p() produces different results:

> Math.log1p(1e-16)
1e-16
> Math.log1p(0)
0
5.4.3 Logarithms to base 2 and 10
5.4.3.1 Math.log2(x)
Computes the logarithm to base 2.

> Math.log2(8)
3
5.4.3.2 Math.log10(x)
Computes the logarithm to base 10.

> Math.log10(100)
2
5.4.4 Support for compiling to JavaScript
Emscripten pioneered a coding style that was later picked up by asm.js: The operations of a virtual machine (think bytecode) are expressed in static subset of JavaScript. That subset can be executed efficiently by JavaScript engines: If it is the result of a compilation from C++, it runs at about 70% of native speed.

The following Math methods were mainly added to support asm.js and similar compilation strategies, they are not that useful for other applications.

5.4.4.1 Math.fround(x)
Rounds x to a 32 bit floating point value (float). Used by asm.js to tell an engine to internally use a float value.

5.4.4.2 Math.imul(x, y)
Multiplies the two 32 bit integers x and y and returns the lower 32 bits of the result. This is the only 32 bit basic math operation that can’t be simulated by using a JavaScript operator and coercing the result back to 32 bits. For example, idiv could be implemented as follows:

function idiv(x, y) {
    return (x / y) | 0;
}
In contrast, multiplying two large 32 bit integers may produce a double that is so large that lower bits are lost.

5.4.5 Bitwise operations
Math.clz32(x)
Counts the leading zero bits in the 32 bit integer x.
  > Math.clz32(0b01000000000000000000000000000000)
  1
  > Math.clz32(0b00100000000000000000000000000000)
  2
  > Math.clz32(2)
  30
  > Math.clz32(1)
  31
Why is this interesting? Quoting “Fast, Deterministic, and Portable Counting Leading Zeros” by Miro Samek:

Counting leading zeros in an integer number is a critical operation in many DSP algorithms, such as normalization of samples in sound or video processing, as well as in real-time schedulers to quickly find the highest-priority task ready-to-run.

5.4.6 Trigonometric methods
Math.sinh(x)
Computes the hyperbolic sine of x.
Math.cosh(x)
Computes the hyperbolic cosine of x.
Math.tanh(x)
Computes the hyperbolic tangent of x.
Math.asinh(x)
Computes the inverse hyperbolic sine of x.
Math.acosh(x)
Computes the inverse hyperbolic cosine of x.
Math.atanh(x)
Computes the inverse hyperbolic tangent of x.
Math.hypot(...values)
Computes the square root of the sum of the squares of its arguments.
5.5 FAQ: numbers
5.5.1 How can I use integers beyond JavaScript’s 53 bit range?
JavaScript’s integers have a range of 53 bits. That is a problem whenever 64 bit integers are needed. For example: In its JSON API, Twitter had to switch from integers to strings when tweet IDs became too large.

At the moment, the only way around that limitation is to use a library for higher-precision numbers (bigints or bigfloats). One such library is decimal.js.

Plans to support larger integers in JavaScript exist, but may take a while to come to fruition.
