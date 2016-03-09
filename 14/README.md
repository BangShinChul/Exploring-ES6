14. 클래스를 포함한 새로운 OOP기능

클래스는 es6에 도입된 주요한 새 OOP특징이다. 하지만 오브젝트리터럴과 Objectㅇ 메소드에도 새로운 기능이 포함되어있다.
이 쳅터에서는 이들을 다룬다.

14.1 개요

14.1.1 새 오브젝트리터럴

메소드선언:
```javascript
const obj = {
    myMethod(x, y) {
        ···
    }
};
```
속성값의 간단한 표현:

```javascript
const first = 'Jane';
const last = 'Doe';

const obj = { first, last };
// Same as:
const obj = { first: first, last: last };
```
계산된 속성키:
```javascript
const propKey = 'foo';
const obj = {
    [propKey]: true,
    ['b'+'ar']: 123
};
```
계산된 속성키는 메소드선언에도 사용됨:
```javascript
const obj = {
    ['h'+'ello']() {
        return 'hi';
    }
};
console.log(obj.hello()); // hi
```
계산된 속성키의 주된 활용처는 속성키로 심볼사용을 쉽게 만들어주는 것이다.

14.1.2 Object의 새로운 메소드
Object의 새 메소드중 가장 중요한 것ㅇㄴ assign()이다. 전통적으로 이 기능은 자바스크립트세계에서 extend()라 불렸다.
고전적인 동작과는 대조적으로 Object.assign()은 오직 자신의(상속되지않은)속성만 고려한다.
```javascript
const obj = { foo: 123 };
Object.assign(obj, { bar: true });
console.log(JSON.stringify(obj));
    // {"foo":123,"bar":true}
```
14.2 오브젝트리터럴의 새로운 기능
14.2.1 메소드 정의
es5에서 메소드는 함수값을 갖는 속성이다:
```javascript
var obj = {
    myMethod: function (x, y) {
        ···
    }
};
```
es6에서는 메소드가 여전히 함수를 값으로 갖고 있는 속성이긴 하지만 훨씬 간단한 방법으로 정의할 수 있다:

```javascript
const obj = {
    myMethod(x, y) {
        ···
    }
};
```
es5에서 사용되던 Getter와 Setter도 잘 작동한다.
```javascript
const obj = {
    get foo() {
        console.log('GET foo');
        return 123;
    },
    set bar(value) {
        console.log('SET bar to '+value);
        // return value is ignored
    }
};
```
obj를 써보자:
```javascript
> obj.foo
GET foo
123
> obj.bar = true
SET bar to true
true
```
제네레이터 함수를 값으로 하는 속성을 간단하게 정의하는 방법 또한 제공된다:
```javascript
const obj = {
    * myGeneratorMethod() {
        ···
    }
};
```
이 코드의 의미는 아래와 같다.
```javascript
const obj = {
    myGeneratorMethod: function* () {
        ···
    }
};
```
14.2.2 속성값 단축표현
속성값 단축표현으로 오브젝트 리터럴에서 속성정의를 간단히 할 수 있다: 속성의 값과 키이름이 변수와 둘다 일치하는 경우 키를 생략할 수 있다. 아래와 같이 사용된다.
```javascript
const x = 4;
const y = 1;
const obj = { x, y };
```
마지막줄은 아래와 같다:
```javascript
const obj = { x: x, y: y };
```
속성값 단축표현은 해체와 함께 잘 작동한다:
```javascript
const obj = { x: 4, y: 1 };
const {x,y} = obj;
console.log(x); // 4
console.log(y); // 1
```
속성값 단축표현의 또다른 사용처는 여러개의 값을 반환하는 경우다(해체절에서 설명한다)

14.2.3 계산된 속성 키
속성을 지정할때 두가지 방법이 있다.

고정이름으로: ```obj.foo = true;```
표현식으로: ```obj['b'+'ar'] = 123;```
ES5의 오브젝트 리터럴은 오직 고정이름만 가능하다. ES6에서는 표현식으로도 키이름을 정의할 수 있다:
```javascript
const propKey = 'foo';
const obj = {
    [propKey]: true,
    ['b'+'ar']: 123
};
```
이 새로운 문법은 메소드 정의에도 사용된다:
```javascript
const obj = {
    ['h'+'ello']() {
        return 'hi';
    }
};
console.log(obj.hello()); // hi
```
계산된 속성키의 주 사용처는 심볼이다:공개심볼을 정의하고 항상 유일한 특정 키를 정의하는데 사용할 수 있다. 특히 Symbol.iterator에 저장된 심볼은 참고해볼만한 예다. 어떤 오브젝트가 이 키로 메서드를 갖고 있다면 이터러블(iterable)이 된다:이 메소드는 반드시 이터레이터(iterator)를 반환해야하는데, 이터레이터는 객체를 순회하기 위한 for-of루프 같은 구조에서 사용된다. 아래 코드에 예시가 있다.
```javascript
const obj = {
    * [Symbol.iterator]() { // (A)
        yield 'hello';
        yield 'world';
    }
};
for (const x of obj) {
    console.log(x);
}
// Output:
// hello
// world
```
A줄은 저장된 심볼인 Symbol.iterator을 계산된 키로 정의된 제네레이터메소드의 시작부다.

14.3 Object의 새로운 메소드
14.3.1 Object.assign(target, source_1, source_2, ···)
이 메소드는 소스를 타겟에게 머지한다: 우선 source_1의 모든 열거가능한 본인 소유의 속성을 target에 복사한 뒤, source_2를 복사하는 식으로 모든 인자에 주어진 source를 target에 머지한다. 마지막에는 target을 반환한다.
```javascript
const obj = { foo: 123 };
Object.assign(obj, { bar: true });
console.log(JSON.stringify(obj));
    // {"foo":123,"bar":true}
```
Object.assign()가 어떻게 작동하는지 보다 구체적으로 살펴보자:

두가지 속성키종류: Object.assign()는 문자열과 심볼 두가지다 속성키로 안다.
자신의 열거가능한 속성만: Object.assign()는 상속받은 속성이나 열거불가 속성을 무시한다.
source로부터 읽을 수 있는 값: 보통 “get” 연산 (const value = source[propKey]). 즉 source의 키가 getter라면 실행된 값이 복사될 것이다. Object.assign()으로 생성된 모든 속성은 값속성이다. target에 getter는 전달되지 않는다.
타켓에 쓸 값: 보통l “set” 연산 (target[propKey] = value). 즉 target키가 setter라면 실행된 값이 복사될 것이다.

따라서 target의 getter나 setter의 계산된 결과가 아니라 바르게 getter와 setter을 전달하면서, (열거가능한 것 뿐 아니라) 모든 속성을 복사하는 코드는 아래와 같다:
```javascript
function copyAllProperties(target, ...sources) {
    for (const source of sources) {
        for (const key of Reflect.ownKeys(source)) {
            const desc = Object.getOwnPropertyDescriptor(source, key);
            Object.defineProperty(target, key, desc);
        }
    }
    return target;
}
```
14.3.1.1 경고: Object.assign()은 이동메소드에 대해서는 잘 작동하지 않는다.
한편, super를 사용해 메소드를 이동할 수 없다: 내부속성인 [[HomeObject]]같은 메소드는 생성된 오브젝트에 묶여있다. Object.assign()으로 이를 옮겨도 여전히 원본 객체의 super속성을 참조할 것이다. 자세한 내용은 classes챕터에서 설명한다.

또다른 한편으로 열거가능성은 이동메소드를 클래스의 프로토타입에서 객체 리터럴로 생성했다면 틀리다. 보통 메소드는 모두 열거가능이지만(반대로 어쨌든 Object.assign()는 그들을 볼 수 없다), 프로토타입은 기본적으로는 오직 열거불가 메소드만 갖을 수 있다.

14.3.1.2 Object.assign()의 사용예
몇 가지 사용예를 살펴보자.

14.3.1.2.1 this에 속성을 추가하기
Object.assign() 생성자에서 this에 속성을 추가하기 위해 사용될 수 있다.
```javascript
class Point {
    constructor(x, y) {
        Object.assign(this, {x, y});
    }
}
```
14.3.1.2.2 오브젝트 속성에 기본값을 제공하기
Object.assign()는 잃어버린 속성에 대한 기본값을 채울때도 쓸모가 있다. 다음의 예에서 인자로 받은 options객체 앞서 DEFAULT오브젝트가 속성에 대한 기본값을 제공한다.
```javascript
const DEFAULTS = {
    logLevel: 0,
    outputFormat: 'html'
};
function processContent(options) {
    options = Object.assign({}, DEFAULTS, options); // (A)
    ···
}
```
A줄에서, 새 객체를 만들고 기본값을 복사한뒤 기본값을 덮어쓰며 options를 복사했다. Object.assign()은 이렇게 처리된 결과를 반환한다.

14.3.1.2.3 오브젝트에 메소드를 추가할때
오브젝트에 메소드를 추가할때도 사용할 수 있다:
```javascript
Object.assign(SomeClass.prototype, {
    someMethod(arg1, arg2) {
        ···
    },
    anotherMethod() {
        ···
    }
});
```
직접 함수를 추가할 수 있지만 매번 SomeClass.prototype를 기술해야하고 이를 대체할 마땅한 문법이 없다:
```javascript
SomeClass.prototype.someMethod = function (arg1, arg2) {
    ···
};
SomeClass.prototype.anotherMethod = function () {
    ···
};
```
14.3.1.2.4 오브젝트 클론하기
마지막으로 살펴볼 Object.assign()의 사용처는 오브젝트의 클론을 만드는 빠른 방법이다:
```javascript
function clone(orig) {
    return Object.assign({}, orig);
}
```
이 방법으로 만들어진 클론은 또한 좀 지저분한데 원본의 속성에 대한 기술을 가져오지 않기 때문이다. 이 부분을 원한다면 속성기술자를 사용해야만한다.

원본과 동일한 프로토타입의 클론을 만들고 싶다면 Object.getPrototypeOf()와 Object.create()를 사용해야한다:
```javascript
function clone(orig) {
    const origProto = Object.getPrototypeOf(orig);
    return Object.assign(Object.create(origProto), orig);
}
```
14.3.2 Object.getOwnPropertySymbols(obj)
Object.getOwnPropertySymbols(obj)은 obj의 상속받지 않은 모든 자신소유의 속성인 심볼을 찾는다.  이는 자신의 모든 문자열키를 찾는  Object.getOwnPropertyNames()를 보완한다. 속성키를 순환할 때 더 자세한 내용은 이후 섹션에서 다룬다.

14.3.3 Object.is(value1, value2)
The strict equals operator (===) treats two values differently than one might expect.

First, NaN is not equal to itself.
```javascript
> NaN === NaN
false
That is unfortunate, because it often prevents us from detecting NaN:

> [0,NaN,2].indexOf(NaN)
-1
Second, JavaScript has two zeros, but strict equals treats them as if they were the same value:
```javascript
> -0 === +0
true
Doing this is normally a good thing.

Object.is() provides a way of comparing values that is a bit more precise than ===. It works as follows:
```javascript
> Object.is(NaN, NaN)
true
> Object.is(-0, +0)
false
Everything else is compared as with ===.

14.3.3.1 Using Object.is() to find Array elements
If we combine Object.is() with the new ES6 Array method findIndex(), we can find NaN in Arrays:
```javascript
function myIndexOf(arr, elem) {
    return arr.findIndex(x => Object.is(x, elem));
}

myIndexOf([0,NaN,2], NaN); // 1
In contrast, indexOf() does not handle NaN well:
```javascript
> [0,NaN,2].indexOf(NaN)
-1
14.3.4 Object.setPrototypeOf(obj, proto)
This method sets the prototype of obj to proto. The non-standard way of doing so in ECMAScript 5, that is supported by many engines, is via assigning to the special property __proto__. The recommended way of setting the prototype remains the same as in ECMAScript 5: during the creation of an object, via Object.create(). That will always be faster than first creating an object and then setting its prototype. Obviously, it doesn’t work if you want to change the prototype of an existing object.

14.4 Iterating over property keys in ES6
In ECMAScript 6, the key of a property can be either a string or a symbol. There are now five tool methods that retrieve the property keys of an object obj:
```javascript
Object.keys(obj) : Array<string>
retrieves all string keys of all enumerable own (non-inherited) properties.
Object.getOwnPropertyNames(obj) : Array<string>
retrieves all string keys of all own properties.
Object.getOwnPropertySymbols(obj) : Array<symbol>
retrieves all symbol keys of all own properties.
Reflect.ownKeys(obj) : Array<string|symbol>
retrieves all keys of all own properties.
Reflect.enumerate(obj) : Iterator
retrieves all string keys of all enumerable properties.
14.4.1 Iteration order of property keys
The following operations in ECMAScript 6 traverse the own keys of properties:

Object.keys()
Object.getOwnPropertyNames()
Reflect.ownKeys()
Own property keys are traversed in the following order:

First, the string keys that are integer indices (what these are is explained in the next section), in ascending numeric order.
Then, all other string keys, in the order in which they were added to the object.
Lastly, all symbol keys, in the order in which they were added to the object.
Many engines treat integer indices specially, even though they are still strings (at least as far as the ES6 spec is concerned). Therefore, it makes sense to treat them as a separate category of keys.

14.4.1.1 Integer indices
Roughly, an integer index is a string that, if converted to a 53-bit non-negative integer and back is the same value. Therefore:

'10' and '2' are integer indices.
'02' is not an integer index. Coverting it to an integer and back results in the different string '2'.
'3.141' is not an integer index, because 3.141 is not an integer.
In ES6, instances of String and Typed Arrays have integer indices. The indices of normal Arrays are a subset of integer indices: they have a smaller range of 32 bits. For more information on Array indices, consult “Array Indices in Detail” in “Speaking JavaScript”.

Integer indices have a 53-bit range, because thats the largest range of integers that JavaScript can handle. For details, see Sect. “Safe integers”.

14.4.1.2 Example
The following code demonstrates the order in which the own keys of an object are iterated over:
```javascript
const obj = {
    [Symbol('first')]: true,
    '02': true,
    '10': true,
    '01': true,
    '2': true,
    [Symbol('second')]: true,
};
Reflect.ownKeys(obj);
    // [ '2', '10', '02', '01',
    //   Symbol('first'), Symbol('second') ]
Explanation:

'2' and '10' are integer indices, come first and are sorted numerically (not in the order in which they were added).
'02' and '01' are normal string keys, come next and appear in the order in which they were added to obj.
Symbol('first') and Symbol('second') are symbols and come last, in the order in which they were added to obj.
14.4.1.3 Why does the spec standardize in which order property keys are returned?
Answer by Tab Atkins Jr.:

Because, for objects at least, all implementations used approximately the same order (matching the current spec), and lots of code was inadvertently written that depended on that ordering, and would break if you enumerated it in a different order. Since browsers have to implement this particular ordering to be web-compatible, it was specced as a requirement.

There was some discussion about breaking from this in Maps/Sets, but doing so would require us to specify an order that is impossible for code to depend on; in other words, we’d have to mandate that the ordering be random, not just unspecified. This was deemed too much effort, and creation-order is reasonable valuable (see OrderedDict in Python, for example), so it was decided to have Maps and Sets match Objects.

14.4.1.4 The order of properties in the spec
Two parts of the spec are relevant:

The section on Array exotic objects has a note on what Array indices are.
The section on the internal method [[OwnPropertyKeys]] defines the order in which properties are returned.
14.5 Assigning versus defining properties
This section provides background knowledge that is needed in later sections.

There are two similar ways of adding a property prop to an object obj:

Assigning: obj.prop = 123
Defining: Object.defineProperty(obj, 'prop', 123)
There are three cases in which assigning does not create an own property prop, even if it doesn’t exist, yet:

A read-only property prop exists in the prototype chain. Then the assignment causes a TypeError in strict mode.
A setter for prop exists in the prototype chain. Then that setter is called.
A getter for prop without a setter exists in the prototype chain. Then a TypeError is thrown in strict mode. This case is similar to the first one.
None of these cases prevent Object.defineProperty() from creating an own property. The next section looks at case #3 in more detail.

14.5.1 Overriding inherited read-only properties
If an object obj inherits a property prop that is read-only then you can’t assign to that property:
```javascript
const proto = Object.defineProperty({}, 'prop', {
    writable: false,
    configurable: true,
    value: 123,
});
const obj = Object.create(proto);
obj.prop = 456;
    // TypeError: Cannot assign to read-only property
This is similar to how an inherited property works that has a getter, but no setter. It is in line with viewing assignment as changing the value of an inherited property. It does so non-destructively: the original is not modified, but overridden by a newly created own property. Therefore, an inherited read-only property and an inherited setter-less property both prevent changes via assignment. You can, however, force the creation of an own property by defining a property:
```javascript
const proto = Object.defineProperty({}, 'prop', {
    writable: false,
    configurable: true,
    value: 123,
});
const obj = Object.create(proto);
Object.defineProperty(obj, 'prop', {value: 456});
console.log(obj.prop); // 456
14.6 __proto__ in ECMAScript 6
The property __proto__ (pronounced “dunder proto”) has existed for a while in most JavaScript engines. This section explains how it worked prior to ECMAScript 6 and what changes with ECMAScript 6.

For this section, it helps if you know what prototype chains are. Consult Sect. “Layer 2: The Prototype Relationship Between Objects” in “Speaking JavaScript”, if necessary.

14.6.1 __proto__ prior to ECMAScript 6
14.6.1.1 Prototypes
Each object in JavaScript starts a chain of one or more objects, a so-called prototype chain. Each object points to its successor, its prototype via the internal property [[Prototype]] (which is null if there is no successor). That property is called internal, because it only exists in the language specification and cannot be directly accessed from JavaScript. In ECMAScript 5, the standard way of getting the prototype p of an object obj is:
```javascript
var p = Object.getPrototypeOf(obj);
There is no standard way to change the prototype of an existing object, but you can create a new object obj that has the given prototype p:
```javascript
var obj = Object.create(p);
14.6.1.2 __proto__
A long time ago, Firefox got the non-standard property __proto__. Other browsers eventually copied that feature, due to its popularity.

Prior to ECMAScript 6, __proto__ worked in obscure ways:

You could use it to get or set the prototype of any object:
```javascript
  var obj = {};
  var p = {};

  console.log(obj.__proto__ === p); // false
  obj.__proto__ = p;
  console.log(obj.__proto__ === p); // true
However, it was never an actual property:
```javascript
  > var obj = {};
  > '__proto__' in obj
  false
14.6.1.3 Subclassing Array via __proto__
The main reason why __proto__ became popular was because it enabled the only way to create a subclass MyArray of Array in ES5: Array instances were exotic objects that couldn’t be created by ordinary constructors. Therefore, the following trick was used:
```javascript
function MyArray() {
    var instance = new Array(); // exotic object
    instance.__proto__ = MyArray.prototype;
    return instance;
}
MyArray.prototype = Object.create(Array.prototype);
MyArray.prototype.customMethod = function (···) { ··· };
Subclassing in ES6 works differently than in ES5 and supports subclassing builtins out of the box.

14.6.1.4 Why __proto__ is problematic in ES5
The main problem is that __proto__ mixes two levels: the object level (normal properties, holding data) and the meta level.

If you accidentally use __proto__ as a normal property (object level!), to store data, you get into trouble, because the two levels clash. The situation is compounded by the fact that you have to abuse objects as maps in ES5, because it has no built-in data structure for that purpose. Maps should be able to hold arbitrary keys, but you can’t use the key '__proto__' with objects-as-maps.

In theory, one could fix the problem by using a symbol instead of the special name __proto__, but keeping meta-operations completely separate (as done via Object.getPrototypeOf()) is the best approach.

14.6.2 The two kinds of __proto__ in ECMAScript 6
Because __proto__ was so widely supported, it was decided that its behavior should be standardized for ECMAScript 6. However, due to its problematic nature, it was added as a deprecated feature. These features reside in Annex B in the ECMAScript specification, which is described as follows:

The ECMAScript language syntax and semantics defined in this annex are required when the ECMAScript host is a web browser. The content of this annex is normative but optional if the ECMAScript host is not a web browser.

JavaScript has several undesirable features that are required by a significant amount of code on the web. Therefore, web browsers must implement them, but other JavaScript engines don’t have to.

In order to explain the magic behind __proto__, two mechanisms were introduced in ES6:

A getter and a setter implemented via Object.prototype.__proto__.
In an object literal, you can consider the property key '__proto__' a special operator for specifying the prototype of the created objects.
14.6.2.1 Object.prototype.__proto__
ECMAScript 6 enables getting and setting the property __proto__ via a getter and a setter stored in Object.prototype. If you were to implement them manually, this is roughly what it would look like:
```javascript
Object.defineProperty(Object.prototype, '__proto__', {
    get() {
        const _thisObj = Object(this);
        return Object.getPrototypeOf(_thisObj);
    },
    set(proto) {
        if (this === undefined || this === null) {
            throw new TypeError();
        }
        if (!isObject(this)) {
            return undefined;
        }
        if (!isObject(proto)) {
            return undefined;
        }
        const status = Reflect.setPrototypeOf(this, proto);
        if (! status) {
            throw new TypeError();
        }
    },
});
function isObject(value) {
    return Object(value) === value;
}
The getter and the setter for __proto__ in the ES6 spec:
```javascript
get Object.prototype.__proto__
set Object.prototype.__proto__
14.6.2.2 The property key __proto__ as an operator in an object literal
If __proto__ appears as an unquoted or quoted property key in an object literal, the prototype of the object created by that literal is set to the property value:
```javascript
> Object.getPrototypeOf({ __proto__: null })
null
> Object.getPrototypeOf({ '__proto__': null })
null
Using the string value '__proto__' as a computed property key does not change the prototype, it creates an own property:

> const obj = { ['__proto__']: null };
> Object.getPrototypeOf(obj) === Object.prototype
true
> Object.keys(obj)
[ '__proto__' ]
The special property key '__proto__' in the ES6 spec:
```javascript
__proto__ Property Names in Object Initializers
14.6.3 Avoiding the magic of __proto__
14.6.3.1 Defining (not assigning) __proto__
In ECMAScript 6, if you define the own property __proto__, no special functionality is triggered and the getter/setter Object.prototype.__proto__ is overridden:
```javascript
const obj = {};
Object.defineProperty(obj, '__proto__', { value: 123 })

Object.keys(obj); // [ '__proto__' ]
console.log(obj.__proto__); // 123
14.6.3.2 Objects that don’t have Object.prototype as a prototype
The __proto__ getter/setter is provided via Object.prototype. Therefore, an object without Object.prototype in its prototype chain doesn’t have the getter/setter, either. In the following code, dict is an example of such an object – it does not have a prototype. As a result, __proto__ now works like any other property:
```javascript
> const dict = Object.create(null);
> '__proto__' in dict
false
> dict.__proto__ = 'abc';
> dict.__proto__
'abc'
14.6.3.3 __proto__ and dict objects
If you want to use an object as a dictionary then it is best if it doesn’t have a prototype. That’s why prototype-less objects are also called dict objects. In ES6, you don’t even have to escape the property key '__proto__' for dict objects, because it doesn’t trigger any special functionality.

__proto__ as an operator in an object literal lets you create dict objects more concisely:
```javascript
const dictObj = {
    __proto__: null,
    yes: true,
    no: false,
};
Note that in ES6, you should normally prefer the built-in data structure Map to dict objects, especially if keys are not fixed.

14.6.3.4 __proto__ and JSON
Prior to ES6, the following could happen in a JavaScript engine:
```javascript
> JSON.parse('{"__proto__": []}') instanceof Array
true
With __proto__ being a getter/setter in ES6, JSON.parse() works fine, because it defines properties, it doesn’t assign them (if implemented properly, an older version of V8 did assign).

JSON.stringify() isn’t affected by __proto__, either, because it only considers own properties. Objects that have an own property whose name is __proto__ work fine:
```javascript
> JSON.stringify({['__proto__']: true})
'{"__proto__":true}'
14.6.4 Detecting support for ES6-style __proto__
Support for ES6-style __proto__ varies from engine to engine. Consult kangax’ ECMAScript 6 compatibility table for information on the status quo:
```javascript
Object.prototype.__proto__
__proto__ in object literals
The following two sections describe how you can programmatically detect whether an engine supports either of the two kinds of __proto__.

14.6.4.1 Feature: __proto__ as getter/setter
A simple check for the getter/setter:
```javascript
var supported = {}.hasOwnProperty.call(Object.prototype, '__proto__');
A more sophisticated check:

var desc = Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');
var supported = (
    typeof desc.get === 'function' && typeof desc.set === 'function'
);
14.6.4.2 Feature: __proto__ as an operator in an object literal
You can use the following check:

var supported = Object.getPrototypeOf({__proto__: null}) === null;
14.6.5 __proto__ is pronounced “dunder proto”
Bracketing names with double underscores is a common practice in Python to avoid name clashes between meta-data (such as __proto__) and data (user-defined properties). That practice will never become common in JavaScript, because it now has symbols for this purpose. However, we can look to the Python community for ideas on how to pronounce double underscores.

The following pronunciation has been suggested by Ned Batchelder:

An awkward thing about programming in Python: there are lots of double underscores. For example, the standard method names beneath the syntactic sugar have names like __getattr__, constructors are __init__, built-in operators can be overloaded with __add__, and so on. […]

My problem with the double underscore is that it’s hard to say. How do you pronounce __init__? “underscore underscore init underscore underscore”? “under under init under under”? Just plain “init” seems to leave out something important.

I have a solution: double underscore should be pronounced “dunder”. So __init__ is “dunder init dunder”, or just “dunder init”.

Thus, __proto__ is pronounced “dunder proto”. The chances for this pronunciation catching on are good, JavaScript creator Brendan Eich uses it.

14.6.6 Recommendations for __proto__
It is nice how well ES6 turns __proto__ from something obscure into something that is easy to understand.

However, I still recommend not to use it. It is effectively a deprecated feature and not part of the core standard. You can’t rely on it being there for code that must run on all engines.

More recommendations:

Use Object.getPrototypeOf() to get the prototype of an object.
Prefer Object.create() to create a new object with a given prototype. Avoid Object.setPrototypeOf(), which hampers performance on many engines.
I actually like __proto__ as an operator in an object literal. It is useful for demonstrating prototypal inheritance and for creating dict objects. However, the previously mentioned caveats do apply.
14.7 Enumerability in ECMAScript 6
Enumerability is an attribute of object properties. This section explains how it works in ECMAScript 6. Let’s first explore what attributes are.

14.7.1 Property attributes
Each object has zero or more properties. Each property has a key and three or more attributes, named slots that store the data of the property (in other words, a property is itself much like a JavaScript object or like a record with fields in a database).

ECMAScript 6 supports the following attributes (as does ES5):

All properties have the attributes:
enumerable: Setting this attribute to false hides the property from some operations.
configurable: Setting this attribute to false prevents several changes to a property (attributes except value can’t be change, property can’t be deleted, etc.).
Normal properties (data properties, methods) have the attributes:
value: holds the value of the property.
writable: controls whether the property’s value can be changed.
Accessors (getters/setters) have the attributes:
get: holds the getter (a function).
set: holds the setter (a function).
You can retrieve the attributes of a property via Object.getOwnPropertyDescriptor(), which returns the attributes as a JavaScript object:
```javascript
> const obj = { foo: 123 };
> Object.getOwnPropertyDescriptor(obj, 'foo')
{ value: 123,
  writable: true,
  enumerable: true,
  configurable: true }
This section explains how the attribute enumerable works in ES6. All other attributes and how to change attributes is explained in Sect. “Property Attributes and Property Descriptors” in “Speaking JavaScript”.

14.7.2 Constructs affected by enumerability
ECMAScript 5:

for-in loop: iterates over the string keys of own and inherited enumerable properties.
Object.keys(): returns the string keys of enumerable own properties.
JSON.stringify(): only stringifies enumerable own properties with string keys.
ECMAScript 6:

Object.assign(): only copies enumerable own properties (both string keys and symbol keys are considered).
Reflect.enumerate(): returns all property names that for-in iterates over.
for-in and Reflect.enumerate() are the only built-in operations where enumerability matters for inherited properties. All other operations only work with own properties.

14.7.3 Use cases for enumerability
Unfortunately, enumerability is quite an idiosyncratic feature. This section presents several use cases for it and argues that, apart from protecting legacy code from breaking, its usefulness is limited.

14.7.3.1 Use case: Hiding properties from the for-in loop
The for-in loop iterates over all enumerable properties of an object, own and inherited ones. Therefore, the attribute enumerable is used to hide properties that should not be iterated over. That was the reason for introducing enumerability in ECMAScript 1.

14.7.3.1.1 Non-enumerability in the language
Non-enumerable properties occur in the following locations in the language:

All prototype properties of built-in classes are non-enumerable:
```javascript
  > const desc = Object.getOwnPropertyDescriptor.bind(Object);
  > desc(Object.prototype, 'toString').enumerable
  false
All prototype properties of classes are non-enumerable:
```javascript
  > desc(class {foo() {}}.prototype, 'foo').enumerable
  false
In Arrays, length is not enumerable, which means that for-in only iterates over indices. (However, that can easily change if you add a property via assignment, which is makes it enumerable.)
```javascript
  > desc([], 'length').enumerable
  false
  > desc(['a'], '0').enumerable
  true
The main reason for making all of these properties non-enumerable is to hide them (especially the inherited ones) from legacy code that uses the for-in loop or $.extend() (and similar operations that copy both inherited and own properties; see next section). Both operations should be avoided in ES6. Hiding them ensures that the legacy code doesn’t break.

14.7.3.2 Use case: Marking properties as not to be copied
14.7.3.2.1 Historical precedents
When it comes to copying properties, there are two important historical precedents that take enumerability into consideration:

Prototype’s Object.extend(destination, source)
```javascript
  const obj1 = Object.create({ foo: 123 });
  Object.extend({}, obj1); // { foo: 123 }

  const obj2 = Object.defineProperty({}, 'foo', {
      value: 123,
      enumerable: false
  });
  Object.extend({}, obj2) // {}
jQuery’s $.extend(target, source1, source2, ···) copies all enumerable own and inherited properties of source1 etc. into own properties of target.
```javascript
  const obj1 = Object.create({ foo: 123 });
  $.extend({}, obj1); // { foo: 123 }

  const obj2 = Object.defineProperty({}, 'foo', {
      value: 123,
      enumerable: false
  });
  $.extend({}, obj2) // {}
Problems with this way of copying properties:

Turning inherited source properties into own target properties is rarely what you want. That’s why enumerability is used to hide inherited properties.
Which properties to copy and which not often depends on the task at hand, it rarely makes sense to have a single flag for everything. A better choice is to provide the copying operation with a predicate (a callback that returns a boolean) that tells it when to consider a property.
The only instance property that is non-enumerable in the standard library is property length of Arrays. However, that property only needs to be hidden due to it magically updating itself via other properties. You can’t create that kind of magic property for your own objects (short of using a Proxy).

14.7.3.2.2 ES6: Object.assign()
In ES6, Object.assign(target, source_1, source_2, ···) can be used to merge the sources into the target. All own enumerable properties of the sources are considered (that is, keys can be either strings or symbols). Object.assign() uses a “get” operation to read a value from a source and a “set” operation to write a value to the target.

With regard to enumerability, Object.assign() continues the tradition of Object.extend() and $.extend(). Quoting Yehuda Katz:

Object.assign would pave the cowpath of all of the extend() APIs already in circulation. We thought the precedent of not copying enumerable methods in those cases was enough reason for Object.assign to have this behavior.

In other words: Object.assign() was created with an upgrade path from $.extend() (and similar) in mind. Its approach is cleaner than $.extend’s, because it ignores inherited properties.

14.7.3.3 Marking properties as private
If you make a property non-enumerable, it can’t by seen by Object.keys() and the for-in loop, anymore. With regard to those mechanisms, the property is private.

However, there are several problems with this approach:

When copying an object, you normally want to copy private properties. That clashes making properties non-enumerable that shouldn’t be copied (see previous section).
The property isn’t really private. Getting, setting and several other mechanisms make no distinction between enumerable and non-enumerable properties.
When working with code either as source or interactively, you can’t immediately see whether a property is enumerable or not. A naming convention (such as prefixing property names with an underscore) is easier to discover.
You can’t use enumerability to distinguish between public and private methods, because methods in prototypes are non-enumerable by default.
14.7.3.4 Hiding own properties from JSON.stringify()
JSON.stringify() does not include properties in its output that are non-enumerable. You can therefore use enumerability to determine which own properties should be exported to JSON. This use case is similar to marking properties as private, the previous use case. But it is also different, because this is more about exporting and slightly different considerations apply. For example: Can an object be completely reconstructed from JSON?

An alternative for specifying how an object should be converted to JSON is to use toJSON():
```javascript
const obj = {
    foo: 123,
    toJSON() {
        return { bar: 456 };
    },
};
JSON.stringify(obj); // '{"bar":456}'
I find toJSON() cleaner than enumerability for the current use case. It also gives you more control, because you can export properties that don’t exist on the object.

14.7.4 Naming inconsistencies
In general, a shorter name means that only enumerable properties are considered:

Object.keys() ignores non-enumerable properties
Object.getOwnPropertyNames() lists all property names
However, Reflect.ownKeys() deviates from that rule, it ignores enumerability and returns the keys of all properties. Additionally, starting with ES6, the following distinction is made:

Property keys are either strings or symbols.
Property names are only strings.
Therefore, a better name for Object.keys() would now be Object.names().

14.7.5 Looking ahead
It seems to me that enumerability is only suited for hiding properties from the for-in loop and $.extend() (and similar operations). Both are legacy features, you should avoid them in new code. As for the other use cases:

I don’t think there is a need for a general flag specifying whether or not to copy a property.
Non-enumerability does not work well as a way to keep properties private.
The toJSON() method is more powerful and explicit than enumerability when it comes to controlling how to convert an object to JSON.
I’m not sure what the best strategy is for enumerability going forward. If, with ES6, we had started to pretend that it didn’t exist (except for making prototype properties non-enumerable so that old code doesn’t break), we might eventually have been able to deprecate enumerability. However, Object.assign() considering enumerability runs counter that strategy (but it does so for a valid reason, backward compatibility).

In my own ES6 code, I’m not using enumerability, except (implicitly) for classes whose prototype methods are non-enumerable.

Lastly, when using an interactive command line, I occasionally miss an operation that returns all property keys of an object, not just the own ones (Reflect.ownKeys) or not just string-valued enumerable ones (Reflect.enumerate). Such an operation would provide a nice overview of the contents of an object.

14.8 Customizing basic language operations via well-known symbols
This section explains how you can customize basic language operations by using the following well-known symbols as property keys:
```javascript
Symbol.hasInstance (method)
Lets an object C customize the behavior of x instanceof C.
Symbol.toPrimitive (method)
Lets an object customize how it is converted to a primitive value. This is the first step whenever something is coerced to a primitive type (via operators etc.).
Symbol.toStringTag (string)
Called by Object.prototype.toString() to compute the default string description of an object obj: ‘[object ‘+obj[Symbol.toStringTag]+’]’.
Symbol.unscopables (Object)
Lets an object hide some properties from the with statement.
14.8.1 Property key Symbol.hasInstance (method)
An object C can customize the behavior of the instanceof operator via a method with the key Symbol.hasInstance that has the following signature:
```javascript
[Symbol.hasInstance](potentialInstance : any)
x instanceof C works as follows in ES6:

If C is not an object, throw a TypeError.
If the method exists, call C[Symbol.hasInstance](x), coerce the result to boolean and return it.
Otherwise, compute and return the result according to the traditional algorithm (C must be callable, C.prototype in the prototype chain of x, etc.).
14.8.1.1 Uses in the standard library
The only method in the standard library that has this key is:

Function.prototype[Symbol.hasInstance]()
This is the implementation of instanceof that all functions (including classes) use by default. Quoting the spec:

This property is non-writable and non-configurable to prevent tampering that could be used to globally expose the target function of a bound function.

The tampering is possible because the traditional instanceof algorithm, OrdinaryHasInstance(), applies instanceof to the target function if it encounters a bound function.

Given that this property is read-only, you can’t use assignment to override it, as mentioned earlier.

14.8.1.2 Example: checking whether a value is an object
As an example, let’s implement an object ReferenceType whose “instances” are all objects, not just objects that are instances of Object (and therefore have Object.prototype in their prototype chains).
```javascript
const ReferenceType = {
    [Symbol.hasInstance](value) {
        return (value !== null
            && (typeof value === 'object'
                || typeof value === 'function'));
    }
};
const obj1 = {};
console.log(obj1 instanceof Object); // true
console.log(obj1 instanceof ReferenceType); // true

const obj2 = Object.create(null);
console.log(obj2 instanceof Object); // false
console.log(obj2 instanceof ReferenceType); // true
14.8.2 Property key Symbol.toPrimitive (method)
Symbol.toPrimitive lets an object customize how it is coerced (converted automatically) to a primitive value.

Many JavaScript operations coerce values to the types that they need.

The multiplication operator (*) coerces its operands to numbers.
new Date(year, month, date) coerces its parameters to numbers.
parseInt(string , radix) coerces its first parameter to a string.
The following are the most common types that values are coerced to:

Boolean: Coercion returns true for truthy values, false for falsy values. Objects are always truthy (even new Boolean(false)).
Number: Coercion converts objects to primitives first. Primitives are then converted to numbers (null → 0, true → 1, '123' → 123, etc.).
String: Coercion converts objects to primitives first. Primitives are then converted to strings (null → 'null', true → 'true', 123 → '123', etc.).
Object: The coercion wraps primitive values (booleans b via new Boolean(b), numbers n via new Number(n), etc.).
Thus, for numbers and strings, the first step is to ensure that a value is any kind of primitive. That is handled by the spec-internal operation ToPrimitive(), which has three modes:

Number: the caller needs a number.
String: the caller needs a string.
Default: the caller needs either a number or a string.
The default mode is only used by:

Equality operator (==)
Addition operator (+)
new Date(value) (exactly one parameter!)
If the value is a primitive then ToPrimitive() is already done. Otherwise, the value is an object obj, which is converted to a primitive as follows:

Number mode: Return the result of obj.valueOf() if it is primitive. Otherwise, return the result of obj.toString() if it is primitive. Otherwise, throw a TypeError.
String mode: works like Number mode, but toString() is called first, valueOf() second.
Default mode: works exactly like Number mode.
This normal algorithm can be overridden by giving an object a method with the following signature:

[Symbol.toPrimitive](hint : 'default' | 'string' | 'number')
In the standard library, there are two such methods:

Symbol.prototype[Symbol.toPrimitive](hint) prevents toString() from being called (which throws an exception).
Date.prototype[Symbol.toPrimitive](hint) This method implements behavior that deviates from the default algorithm. Quoting the specification: “Date objects are unique among built-in ECMAScript object in that they treat 'default' as being equivalent to 'string'. All other built-in ECMAScript objects treat 'default' as being equivalent to 'number'.”
14.8.2.1 Example
The following code demonstrates how coercion affects the object obj.
```javascript
const obj = {
    [Symbol.toPrimitive](hint) {
        switch (hint) {
            case 'number':
                return 123;
            case 'string':
                return 'str';
            case 'default':
                return 'default';
            default:
                throw new Error();
        }
    }
};
console.log(2 * obj); // 246
console.log(3 + obj); // '3default'
console.log(obj == 'default'); // true
console.log(String(obj)); // 'str'
14.8.3 Property key Symbol.toStringTag (string)
In ES5 and earlier, each object had the internal own property [[Class]] whose value hinted at its type. You could not access it directly, but its value was part of the string returned by Object.prototype.toString(), which is why that method was used for type checks, as an alternative to typeof.

In ES6, there is no internal property [[Class]], anymore, and using Object.prototype.toString() for type checks is discouraged. In order to ensure the backwards-compatibility of that method, the public property with the key Symbol.toStringTag was introduced. You could say that it replaces [[Class]].
```javascript
Object.prototype.toString() now works as follows:

Convert this to an object obj.
Determine the toString tag tst of obj.
Return '[object ' + tst + ']'.
14.8.3.1 Default toString tags
The default values for various kinds of objects are shown in the following table.

Value	toString tag
undefined	'Undefined'
null	'Null'
An Array object	'Array'
A string object	'String'
arguments	'Arguments'
Something callable	'Function'
An error object	'Error'
A boolean object	'Boolean'
A number object	'Number'
A date object	'Date'
A regular expression object	'RegExp'
(Otherwise)	'Object'
Most of the checks in the left column are performed by looking at internal properties. For example, if an object has the internal property [[Call]], it is callable.

The following interaction demonstrates the default toString tags.
```javascript
> Object.prototype.toString.call(null)
'[object Null]'
> Object.prototype.toString.call([])
'[object Array]'
> Object.prototype.toString.call({})
'[object Object]'
> Object.prototype.toString.call(Object.create(null))
'[object Object]'
14.8.3.2 Overriding the default toString tag
If an object has an (own or inherited) property whose key is Symbol.toStringTag then its value overrides the default toString tag. For example:
```javascript
> ({}.toString())
'[object Object]'
> ({[Symbol.toStringTag]: 'Foo'}.toString())
'[object Foo]'
Instances of user-defined classes get the default toString tag (of objects):
```javascript
class Foo { }
console.log(new Foo().toString()); // [object Object]
One option for overriding the default is via a getter:
```javascript
class Bar {
    get [Symbol.toStringTag]() {
      return 'Bar';
    }
}
console.log(new Bar().toString()); // [object Bar]
In the JavaScript standard library, there are the following custom toString tags. Objects that have no global names are quoted with percent symbols (for example: %TypedArray%).

Module-like objects:
JSON[Symbol.toStringTag] → 'JSON'
Math[Symbol.toStringTag] → 'Math'
Actual module objects M: M[Symbol.toStringTag] → 'Module'
Built-in classes
ArrayBuffer.prototype[Symbol.toStringTag] → 'ArrayBuffer'
DataView.prototype[Symbol.toStringTag] → 'DataView'
Map.prototype[Symbol.toStringTag] → 'Map'
Promise.prototype[Symbol.toStringTag] → 'Promise'
Set.prototype[Symbol.toStringTag] → 'Set'
get %TypedArray%.prototype[Symbol.toStringTag] → 'Uint8Array' etc.
WeakMap.prototype[Symbol.toStringTag] → 'WeakMap'
WeakSet.prototype[Symbol.toStringTag] → 'WeakSet'
Iterators
%MapIteratorPrototype%[Symbol.toStringTag] → 'Map Iterator'
%SetIteratorPrototype%[Symbol.toStringTag] → 'Set Iterator'
%StringIteratorPrototype%[Symbol.toStringTag] → 'String Iterator'
Miscellaneous
Symbol.prototype[Symbol.toStringTag] → 'Symbol'
Generator.prototype[Symbol.toStringTag] → 'Generator'
GeneratorFunction.prototype[Symbol.toStringTag] → 'GeneratorFunction'
All of the built-in properties whose keys are Symbol.toStringTag have the following property descriptor:
```javascript
{
    writable: false,
    enumerable: false,
    configurable: true,
}
As mentioned earlier, you can’t use assignment to override those properties, because they are read-only.

14.8.4 Property key Symbol.unscopables (Object)
Symbol.unscopables lets an object hide some properties from the with statement.

The reason for doing so is that it allows TC39 to add new methods to Array.prototype without breaking old code. Note that current code rarely uses with, which is forbidden in strict mode and therefore ES6 modules (which are implicitly in strict mode).

Why would adding methods to Array.prototype break code that uses with (such as the widely deployed Ext JS 4.2.1)? Take a look at the following code. The existence of a property Array.prototype.values breaks foo(), if you call it with an Array:
```javascript
function foo(values) {
    with (values) {
        console.log(values.length); // abc (*)
    }
}
Array.prototype.values = { length: 'abc' };
foo([]);
Inside the with statement, all properties of values become local variables, shadowing even values itself. Therefore, if values has a property values then the statement in line * logs values.values.length and not values.length.

Symbol.unscopables is used only once in the standard library:

Array.prototype[Symbol.unscopables]
Holds an object with the following properties (which are therefore hidden from the with statement): copyWithin, entries, fill, find, findIndex, keys, values
14.9 FAQ: object literals
14.9.1 Can I use super in object literals?
Yes you can! Details are explained in the chapter on classes.

