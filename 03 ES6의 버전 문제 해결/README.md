## 3. 단일 자바스크립트 : ECMAScript 6에서 버저닝을 피하기(One JavaScript: avoiding versioning in ECMAScript 6)

What is the best way to add new features to a language? This chapter describes the approach taken by ECMAScript 6. It is called _One JavaScript_, because it avoids versioning.

언어에 새로운 기능을 추가하는 가장 좋은 방법은 무엇일까? 3장에서는 ECMAScript6에 다가가는 법을 설명한다. 버저닝(versioning)을 피해야 하기 때문에 이를 _단일 자바스크립트(One JavaScript)_라고 한다. 

### 3.1 버저닝(Versioning)
In principle, a new version of a language is a chance to clean it up, by removing outdated features or by changing how features work. That means that new code doesn’t work in older implementations of the language and that old code doesn’t work in a new implementation. Each piece of code is linked to a specific version of the language. Two approaches are common for dealing with versions being different.

원칙적으로 한 언어의 새로운 버전은 오래된 기능을 제거하거나, 동작 방식을 변경함으로써 언어를 정비할 수 있는 기회가 된다. 이는 곧 새로운 코드가 해당 언어의 옛 구현체에서는 동작하지 않거나, 오래된 코드가 새로운 구현체에서 동작하지 않게 됨을 의미한다. 각각의 코드 조각들은 언어의 특정 버전과 연결되어 있다. 서로 다른 버전을 다루는 데는 흔히 두 가지 접근법이 사용된다.

First, you can take an “all or nothing” approach and demand that, if a code base wants to use the new version, it must be upgraded completely. Python took that approach when upgrading from Python 2 to Python 3. A problem with it is that it may not be feasible to migrate all of an existing code base at once, especially if it is large. Furthermore, the approach is not an option for the web, where you’ll always have old code and where JavaScript engines are updated automatically.

첫째, "전부 아니면 아무것도(all or nothing)" 전략을 선택할 수 있다. 만약 코드 베이스(code base)를 새로운 버전으로 사용하기 원한다면 완벽한 업그레이드가 필요하다. 파이썬은 2에서 3으로 버전을 올릴 때 이 방법을 사용했다. 이 방법이 가진 문제점 중 하나는 현실적으로 코드 베이스의 모든 것을 한번에 통합하는 것이 불가능 하다는 것이다. 특히 규모가 크다면 더더욱 어렵다. 더군다나 언제나 오래된 코드가 존재하고, 자바스크립트 엔진이 자동으로 업데이트되는 웹에서는 선택할 수 있는 방법이 아니다.

Second, you can permit a code base to contain code in multiple versions, by tagging code with versions. On the web, you could tag ECMAScript 6 code via a dedicated Internet media type. Such a media type can be associated with a file via an HTTP header:

둘째, 코드에 버전을 명시함으로써 여러가지 버전의 코드를 포함하는 코드베이스를 허용할 수도 있다. 웹에서는 전용 (인터넷 미디어 타입)[http://en.wikipedia.org/wiki/Internet_media_type] 을 통한 ECMAScript 6 코드를 태그할 수도 있다. 어떤 미디어 타입은 HTTP 해더를 통해 파일과 연결시킬 수 있다.

```
Content-Type: application/ecmascript;version=6
```

It can also be associated via the type attribute of the `<script>` element (whose default value is text/javascript):

또한 `<script>` 요소( (기본 값)[http://www.w3.org/TR/html5/scripting-1.html#attr-script-type] 은 text/javascript 이다)에 연결 시킬 수도 있다.

```
<script type="application/ecmascript;version=6">
    ···
</script>
```

This specifies the version out of band, externally to the actual content. Another option is to specify the version inside the content (in-band). For example, by starting a file with the following line:

이것은 실제 컨텐츠의 외부에(out of band) 버전을 따로 명시하는 것이다. 또 다른 옵션은 컨텐츠 내부에 버전을 명시하는 것이다.(in-band). 예를 들어 다음과 같이 파일을 시작하면 된다.

```
use version 6;
```

Both ways of tagging are problematic: out-of-band versions are brittle and can get lost, in-band versions add clutter to code.

두 방법 모두 문제가 있는데, 외부에 버전을 표시하는 방법은 다루기 힘들고 유실될 염려가 있다. 내부에 표시하는 방법은 코드를 어지럽힌다.

A more fundamental issue is that allowing multiple versions per code base effectively forks a language into sub-languages that have to be maintained in parallel. This causes problems:

또한 더욱 근본적인 문제는 코드 베이스마다 여러 버전을 허용하는 것은 한 언어를 동시에 유지 보수해야 하는 다른 하위-언어(sub-languages)로 효과적으로 포크할 수 있게 한다. 이는 다음과 같은 문제를 일으킨다.

* Engines become bloated, because they need to implement the semantics of all versions. The same applies to tools analyzing the language (e.g. style checkers such as JSLint).
* Programmers need to remember how the versions differ.
* Code becomes harder to refactor, because you need to take versions into consideration when you move pieces of code.

Therefore, versioning is something to avoid, especially for JavaScript and the web.

* 모든 버전의 구문을 해석할 수 있어야 하기 때문에 엔진이 비대해진다. 이는 언어 분석 도구도 마찬가지다. (예. JSLint 같은 스타일 점검 도구)
* 프로그래머는 버전별 차이점을 기억해야 한다.
* 코드 조각을 움직이는 순간 버전을 고려해야하기 때문에 코드 리팩토링이 어려워진다.

그러므로, 버저닝은 피해야 한다. 특히나 자바스크립트와 웹에서라면 더더욱 그러하다.

### 3.1.1 버저닝 없는 진화(Evolution without versioning)
But how can we get rid of versioning? By always being backwards-compatible. That means we must give up some of our ambitions w.r.t. cleaning up JavaScript: We can’t introduce breaking changes. Being backwards-compatible means not removing features and not changing features. The slogan for this principle is: “don’t break the web”.

그렇다면 어떻게 버저닝을 제거 할 수 있을까? 항상 이전 버전과 호환이 되야 하기 때문에, 우리는 자바스크립트를 정화하겠다는 야망을 포기해야만 한다. 즉, 주요 변경 사항을 도입할 수 없다는 것이다. 이전 버전과 호환이 된다는 것은 이전 버전의 기능을 변경하거나 없애지 않는 것을 의미한다. 이 원칙에 의거한 슬로건이 바로 "웹을 망가뜨리지 말라(don't break the web)"이다.

We can, however, add new features and make existing features more powerful.
하지만 우리는 새로운 기능을 추가하고, 기존의 기능을 더 강력하게 만들 수 있다.

As a consequence, no versions are needed for new engines, because they can still run all old code. David Herman calls this approach to avoiding versioning One JavaScript (1JS) [1], because it avoids splitting up JavaScript into different versions or modes. As we shall see later, 1JS even undoes some of a split that already exists, due to strict mode.

결론적으로, 엔진이 여전히 이전 버전의 모든 코드를 실행할 수 있기 때문에 새로운 엔진을 위한 버전은 필요가 없다. 데이비드 허만(David Herman) 은 버저닝을 피하기 위한 이러한 접근법을 '단일 자바스크립트((One JavaScript, 1JS)[http://exploringjs.com/es6/ch_one-javascript.html#one-js_1])[1]'라고 한다. 이는 자바스크립트가 다른 버전이나 다른 모드로 분열되는 것을 피할 수 있기 때문이다. 이후 다시 살펴보겠지만, 1JS는 strict 모드로 인해 이미 분리된 것을 다시 원상태로 복구하기도 한다.

One JavaScript does not mean that you have to completely give up on cleaning up the language. Instead of cleaning up existing features, you introduce new, clean, features. One example for that is let, which declares block-scoped variables and is an improved version of var. It does not, however, replace var, it exists alongside it, as the superior option.

단일 자바스크립트는 우리가 언어를 정돈하는 것을 완전히 포기해야 한다는 것을 뜻하지 않는다. 이미 존재하는 기능들을 없애는 대신에 새롭고, 말끔한 기능을 도입할 수 있다. 예를 들어 let은 블록 스코프 변수를 선언하는 기능을 하고 var의 향상된 버전이다. 그러나 let은 var를 대체하는 것이 아니라 더 상위 선택지로서 var와 나란히 존재한다.

One day, it may even be possible to eliminate features that nobody uses, anymore. Some of the ES6 features were designed by surveying JavaScript code on the web. Two examples are:

언젠가 더 이상 아무도 사용하지 않는 기능들은 제거 될 수도 있다. ES6의 기능 중 몇 가지는 웹에서 자바스크립트 코드를 조사함으로써 고안되었다. 다음 두 예제를 보자.

* let-declarations are difficult to add to non-strict mode, because let is not a reserved word in that mode. The only variant of let that looks like valid ES5 code is:

* `let` 선언은 non-strict 모드에 추가하기는 어렵다. 왜냐하면 non-strict에서는 `let`이 에약어가 아니기 때문이다. `let`을 유효한 ES5 코드로 변형하여 쓰면 다음과 같다.

    ```javascript
      let[x] = arr;
    ```

    Research yielded that no code on the web uses a variable let in non-strict mode in this manner. That enabled TC39 to add let to non-strict mode. Details of how this was done are described later in this chapter.

    웹에서 조사해보니 non-strict 모드에서 이 방법으로 `let`을 사용한 코드는 찾아 볼 수 없었다. 그래서 TC39는 non-stric 모드에 `let`을 추가할 수 있었다. 자세한 방법은 이 장의 뒷부분에서 설명하겠다.

* Function declarations do occasionally appear in non-strict blocks, which is why the ES6 specification describes measures that web browsers can take to ensure that such code doesn’t break. Details are explained later.

함수 선언은 때때로 non-strict 블록에서 나타나는데, 이는 ES6의 스펙이 자세한 내용은 이후 설명하겠다. 

###3.2 strict 모드와 ECMAScript6(Strict mode and ECMAScript 6)
Strict mode was introduced in ECMAScript 5 to clean up the language. It is switched on by putting the following line first in a file or in a function:

strict 모드는 ECMAScript 5에서 언어를 더욱 깔끔하게 만들기 위해 도입되었다. 파일이나 함수의 첫줄에 아래와 같이 추가하면 strict 모드로 전환된다.

```javascript
'use strict';
```

Strict mode introduces three kinds of breaking changes:
strict 모드는 3가지 주요 변경 사항을 도입했다.

* 문법적 변화 : 이전의 몇가지 유효한 문법이 스트릭트 모드에서는 금지된다. 예:
Syntactic changes: some previously legal syntax is forbidden in strict mode. For example:

with 문은 금지된다. with문은 사용자가 임의의 객체를 변수 스코프 체인에 추가하게 한다. 이것은 실행을 느리게 하고 변수 참조를 알아보기 어렵게 만든다. 
The with statement is forbidden. It lets users add arbitrary objects to the chain of variable scopes, which slows down execution and makes it tricky to figure out what a variable refers to.
자격이 없는 식별자(객체의 속성이 아닌 변수)를 지우는것은 금지된다.
Deleting an unqualified identifier (a variable, not a property) is forbidden.
함수는 오직 스코프의 최상단에만 선언될 수 있다.
Functions can only be declared at the top level of a scope.
더 많은 식별자들이 생겼다 : implements interface let package private protected public static yield
More identifiers are reserved: implements interface let package private protected public static yield

* More errors. For example:
선언되지 않는 변수에 할당하는 것은 ReferenceError 를 발생시킨다. non-strict mode 에서는 이와 같은 경웨 전역 변수가 생성된다.
Assigning to an undeclared variable causes a ReferenceError. In non-strict mode, a global variable is created in this case.
읽기전용 속성을 변경하는 것(string 의 length 변경 같은 것) 은 typeError 을 유발한다. non-strict mode 에서는 아무런 영향을 미치지 않는다.
Changing read-only properties (such as the length of a string) causes a TypeError. In non-strict mode, it simply has no effect.
다른 의미들: 어떤 것들은 스트릭트 모드에서 다르게 동작한다. 예 :
Different semantics: Some constructs behave differently in strict mode. For example:
arguments 는 더 이상 현재 매개변수의 값들을 추적하지 않는다.
arguments doesn’t track the current values of parameters, anymore.
메소드가 아닌 함수에서 arguments는 undefined 이다. non-strict모드에서 이것은 전역변수를 참조한다. 
this is undefined in non-method functions. In non-strict mode, it refers to the global object (window), which meant that global variables were created if you called a constructor without new.
Strict mode is a good example of why versioning is tricky: Even though it enables a cleaner version of JavaScript, its adoption is still relatively low. The main reasons are that it breaks some existing code, can slow down execution and is a hassle to add to files (let alone interactive command lines). I love the idea of strict mode and don’t nearly use it often enough.

### 3.2.1 슬로피(sloppy(non-strict)) 모드 지원

One JavaScript는 슬로피 모드를 포기 할 수 없다는 것을 의미한다. : 이것은 계속 될 것이다(예: HTML속성에서). 그러므로 우리는 스트릭트 모드 위에서 ECMAScript6의 빌드를 할 수 없고, 이 특징들을 스트릭트 모드와 슬로피 모드 양 쪽에 추가해야만한다. 그렇지 않으면 스트릭트 모드는 언어의 다른 버전이 될 수도 있고 우린 versioning을 다시 겪게 된다. 불행하게도, ECMAScript6 특징 중 2가지가 슬로피 모드에 추가되기에 어렵다. let선언과 블록레벨 함수 선언이다. 왜 이것들이 어렵고, 어떻게 추가하는지 알아보자.   means that we can’t give up on sloppy mode: it will continue to be around (e.g. in HTML attributes). Therefore, we can’t build ECMAScript 6 on top of strict mode, we must add its features to both strict mode and non-strict mode (a.k.a. sloppy mode). Otherwise, strict mode would be a different version of the language and we’d be back to versioning. Unfortunately, two ECMAScript 6 features are difficult to add to sloppy mode: let declarations and block-level function declarations. Let’s examine why that is and how to add them, anyway.


### 3.2.2 슬로피 모드에서의 let 선언
let은 블록 스코프 변수의 선언을 가능케한다. 이것은 슬로피 모드에서 적용되기는 어렵다. 왜냐하면 let은 스트릭트모드에서만 예약어이기 때문이다. 다음 2개의 문은 ES5 슬로피 모드에서 유효하다. 
let enables you to declare block-scoped variables. It is difficult to add to sloppy mode, because let is only a reserved word in strict mode. That is, the following two statements are legal ES5 sloppy code:

```javascript
var let = [];
let[x] = 'abc';
```
ECMAScript6 의 스트릭트 모드에서는 라인1에서 예외가 발생한다. 왜냐하면 예약어인 let이 변수 이름으로 사용되었기 때문이다. 그리고 두번째 라인은 let변수 선언으로 해석된다. (해체를 사용하고 있다.)
In strict ECMAScript 6, you get an exception in line 1, because you are using the reserved word let as a variable name. And the statement in line 2 is interpreted as a let variable declaration (that uses destructuring).

ECMAScript6 슬로피모드에서는, 첫 번째 라인에서 예외가 발생하지 않는다. 그러나 두 번째 라인은 여전히 let 변수로 해석된다. let 식별자의 이러한 사용 방법은 ES6로 해석되도 지장이 없는 웹에서는 드문 광경이다. let선언을 쓰는 다른 방법들은 슬로피 ES5 문법으로 오인될 수 없다.
In sloppy ECMAScript 6, the first line does not cause an exception, but the second line is still interpreted as a let declaration. This way of using the identifier let is so rare on the web that ES6 can afford to make this interpretation. Other ways of writing let declarations can’t be mistaken for sloppy ES5 syntax:

```javascript
let foo = 123;
let {x,y} = computeCoordinates();
```

### 3.2.3 슬로피모드에서의 블록레벨 함수 선언 Block-level function declarations in sloppy mode
ECMAScript 5 스트릭트 모드에는 블록 안에서의 함수선언을 금지한다. 이 사양은 슬로피 모드에서는 허용된다. 그러나 그 함수들이 어떻게 동작해야하는지에 대해서는 명시하지 않는다. 이런 이유로 자바스크립트의 다양한 구현은 그것들을 지원한다. 그러나 그것들을 다르게 다룬다.
ECMAScript 5 strict mode forbids function declarations in blocks. The specification allowed them in sloppy mode, but didn’t specify how they should behave. Hence, various implementations of JavaScript support them, but handle them differently.

ECMAScript 6 wants a function declaration in a block to be local to that block. That is OK as an extension of ES5 strict mode, but breaks some sloppy code. Therefore, ES6 provides “web legacy compatibility semantics” for browsers that lets function declarations in blocks exist at function scope.

### 3.2.4 다른 키워드들
yield와 static 식별자는 ES5 스트릭트 모드에서만 예약되어 있다. ECMAScript 6 는 슬로피 모드에서 이것들을 만들기 위해 컨텍스트 한정 문법을 사용한다.
The identifiers yield and static are only reserved in ES5 strict mode. ECMAScript 6 uses context-specific syntax rules to make them work in sloppy mode:

슬로피 모드에서, yield는 제너레이터 함수 안에서만 예약된 키워드이다.
In sloppy mode, yield is only a reserved word inside a generator function.
static is currently only used inside class literals, which are implicitly strict (see below).

### 3.2.5 Implicit strict mode
The bodies of modules and classes are implicitly in strict mode in ECMAScript 6 – there is no need for the 'use strict' marker. Given that virtually all of our code will live in modules in the future, ECMAScript 6 effectively upgrades the whole language to strict mode.

The bodies of other constructs (such as arrow functions and generator functions) could have been made implicitly strict, too. But given how small these constructs usually are, using them in sloppy mode would have resulted in code that is fragmented between the two modes. Classes and especially modules are large enough to make fragmentation less of an issue.
### 3.2.6 수정될 수 없는 것들
### 3.2.6 Things that can’t be fixed
The downside of One JavaScript is that you can’t fix existing quirks, especially the following two.

첫 째, typeof null 은 문자열 'object'가 아니라 'null'을 리턴해야한다. TC39는 이 것을 고치려고 노력했지만 그렇게 되면 기존 코드를 망가뜨리게 된다.
First, typeof null should return the string 'null' and not 'object'. TC39 tried fixing it, but it broke existing code. On the other hand, adding new results for new kinds of operands is OK, because current JavaScript engines already occasionally return custom values for host objects. One example are ECMAScript 6’s symbols:

> typeof Symbol.iterator
'symbol'
Second, the global object (window in browsers) shouldn’t be in the scope chain of variables. But it is also much too late to change that now. At least, one won’t be in global scope in modules and let never creates properties of the global object, not even when used in global scope.

### 3.3 ES6 주요 변경 사항(Breaking changes in ES6)
ECMAScript 6 does introduce a few minor breaking changes (nothing you’re likely to encounter). They are listed in two annexes:

* Annex D: Corrections and Clarifications in ECMAScript 2015 with Possible Compatibility Impact
* Annex E: Additions and Changes That Introduce Incompatibilities with Prior Editions

### 3.4 결론(Conclusion)
One JavaScript means making ECMAScript 6 completely backwards compatible. It is great that that succeeded. Especially appreciated is that modules (and thus most of our code) are implicitly in strict mode.

단일 자바스크립트는 ECMAScript 6을 완전히 하위 호환 가능하게 만드는 것을 뜻한다. 성공적이라면 굉장한 것이다. 특히나 스트릭트 모드에서 모듈(뿐만 아니라 우리의 대부분의 코드도) 절대적이라는 것을 높이 평가한다.

In the short term, adding ES6 constructs to both strict mode and sloppy mode is more work when it comes to writing the language specification and to implementing it in engines. In the long term, both the spec and engines profit from the language not being forked (less bloat etc.). Programmers profit immediately from One JavaScript, because it makes it easier to get started with ECMAScript 6.

단기적으로는 ES6 생성자를 스트릭트 모드와 슬로피(sloppy) 모드에 추가 함으로써 . 장기적으로는 스펙과 엔진의 장점 포크되지 않을 것이다.(훨씬 적게 커질 것이다. etc) 프로그래머는 당장 단일 자바스크립트에 대한 이득을 볼 수 있다. 왜냐하면 이는 ECMAScript 6 로 시작하기 훨씬 수월하게 해주기 때문이다. 

### 3.5 더 읽을거리(Further reading)
[1] The original 1JS proposal (warning: out of date): “ES6 doesn’t need opt-in” by David Herman.

[1] 데이비드 허먼(David Herman)의 1JS에 대한 제안(warning: out of date): “ES6 doesn’t need opt-in”, 

