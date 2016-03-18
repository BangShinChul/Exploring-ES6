# 1. ECMAScript 6에 대하여 (ES6)

그것 끝내기 위해서 오랜 시간이 걸렸지만 자바스크립트 다음 버전인 ECMAScript 6은 마침내 현실이 되었다.:

* 이것은 2015년 6월 17일 표준이 되었다.
* 그 기능은 천천히 자바스크립트 엔진에서 나타나고 있다. (kangax의 ES6 호환 테이블 문서 에서)
* 트렌스파일러(바벨, Traceur)은 당신의 ES5코드를 ES6으로 컴파일 해준다.
다음 섹션은 ES6의 세계에서 중요한 개념을 설명한다.

## 1.1 TC39 (Ecma Technical Committee 39)
TC39 (Ecma Technical Committee 39)은 자바스크립트를 발달시키는 위원회이다. 그 회원은 회사(주요 브라우져 벤더들)들이다. TC39는 정기적으로 모이고 그 회의는 회원들이 보낸 대리인들과 초대된 전문가들로 참석되어 진다. 그 회의의 의사록은 온라인에서 볼 수 있고, 당신의 TC39 작동하는 방식의 생각을 당신에게 제공한다.

## 1.2 ECMAScript 6은 어떻게 설계 되었나
ECMAScript 6 설계는 기능을 위한 제안의 센터로 저장된다. 제안들은 개발자 커뮤니티로 부터 요청에 의해 종종 유발되어졌다. 의원회에 의한 설계를 피하기 위해서 제안은 챔피언(1~2명 의원회 대표)들을 통해 유지되어진다.

제안은 표준안이 되기 위에 다음과 같은 과정을 겪는다.
* 초고 (비공식적: "strawman 제안"): 제안된 기능을 처음 기술
* 제안: 만약 TC39가 기능이 중요하다고 동의하면, 그것은 공식 제안 상태로 오른다. 이것이 표준이 된다는것을 보장하지 않지만 이 제안한 상당히 큰 가능성이 있다. ES6 제안의 마지막 기한은 2011년 5월 이였다. 그 이후에 더이상의 주요 제안은 고려되어 지지 않는다.
* 구현: 이상적으로 두 자바스크립트 엔진에서 제안된 기능은 반드시 구현되어진다. 구현과 커뮤티니로부터으이 피드백은 그 제안을 진화 처럼 구체화 한다.
* 표준: 만약 제안이 계속적으로 스스로를 증명하고 TC39에 의해서 받아드려진다면, 그것은 ECMAScript 표준의 판안에 결국 추가된다. 그 시점에서 제안은 표준기능이다.

[Source of this section: “The Harmony Process” by David Herman.]

### 1.2.1 ES6이후 설계 과정
ESMAScript 7은 시작되었고(공식적인 이름은 ECMAScript 2016), TC39는 타입박스를 공개할 것이다. 이 계획은 ECMAScript의 그때 준비된 기능과 함께 매년 새로운 버전은 공개되는 것이다. 이것은 지금부터 ECMAScript 버전들은 비교적 작은 업데이트라는것을 의미 한다.

ECMAScript 2016(그 이후) 작업은 이미 시작되었고, 현재 제안은 깃헙에 리스트 되어 있다. 그 과정도 변경 되고 있으며, TC39 프로세서 문서에 기술되고 있다.

## 1.3 자바스크립트 대 ECMAScript
자바스크립트는 누구에게나 프로그램 언어로 불리지만 그 이름은 트레이드마크(오라클에 의한,sun으로 부터 받은 트레이드마크)다. 그러므로, 공식적인 자바스크립트 이름은 ECMAscript이다. 이 이름은 언어의 표준을 관리하는 표준 조직인 Ecma로 부터 왔다. ECMAscript 시작 이래로 조직의 이름은 약어 "ECMA"로 부터 고유명사인 "Ecma"로 변경되었다.

자바스크립트 버전은 언어의 공식적 이름을 수행하는 명세에 의해서 정의 된다. 따라서 처음 자바스크립트 표준 버전은 "ECMAScript Language Specification, Edition 1"를 줄여서 ECMAScript 1이다. ECMAScript X는 종종 ESX로 줄여 부른다.

## 1.4 ES6으로 업그레이드
웹 이해 관계자들:
* 자바스크립트 엔진 구현자
* 웹 애플리케이션 개발자
* 사용자

이 그룹들은 서로 매우 작은 제어를 가진다. 그것이 왜 웹언어 업그레이드가 매우 도전적인 이유이다.

한편 엔진을 업그레이드하는 것은 도전이다. 왜냐하면 그들은 모든 종류의 웹코드와 때때로 매우 오래된 코드에 직면해 있기 때문이다. 당신 역시 자연스럽고 사용자가 인지하지 못하게 엔진 업그레이드되길 원한다. 그러므로 ES6은 ES5의 상위 집합으로 아무것도 제거 된 것이 없다. ES6는 언어를 버전이나 모드를 도입하지 않고 업그래이드 한다. 이것은 심지어 슬리피 모드와의 틈 없이 엄격모드를 사실상 기본값으로 관리 한다. 이 "하나의 자바스크립트"로 불리는 접근는 별도에 장에서 설명한다.

반면에 코드를 업그레이드 하는것은 도전이다. 왜냐하면 당신의 코드는 반드시 당신이 타겟으로 하는 사용자가 사용하는 모든 자바스크립트 엔진에서 돌아야 하기 때문이다. 그러므로 만약 당신이 ES6을 당신의 코드에서 사용하길 원한다면 당신은 단지 두가지 선택이 있다. 당신은 당신의 타겟 사용자가 ES6를 지원하지 않는 엔진을 더이상 사용하지 않을 때까지 기다릴 수 있다. 그것은 몇 해 걸릴 것이다. 주요 ES5 2009년 11월 표준화 되었다.! 또는 당신은 ES6을 ES5로 컴파일 할 수 있다. 이 작업을 수행하는 자세한 방법은 "Setting up ES6"에 나와 있고 이것은 온라인에서 공짜다.

ES6 설계에서 목표 와 요구사항의 충돌

* 목표는 자바스크립트의 함정을 수정하고 새로운 기능을 추가
* 요구 사항은 존재하는 코드의 깨짐이나 언어의 경량특성을 변경 없이 되는 것. 

## 1.5 ES6의 목표
하모니/ES6의 원래 프로젝트 페이지는 몇몇개의 목표를 가진다. 다음 소단원에서 나는 목표의 몇개를 설명하겠다.

### 1.5.1 목표: 더나은 언어

목표는 쓰기에 더 나은 언어:

1. 복잡한 어플리케이션;
2. 애플리케이션에 의해 공유된 라이브러리(아마도 DOM을 포함);
3. 새 판을 겨냥한 코드 생성기.

소 목적 (1) 자바스크립트로 쓰여진 애플리케이션 사례는 크게 증가되었다. 목표를 실현하는 ES6 기능의 키는 내장 모듈이다.

모듈들은 또한 목표(2)의 해답이다. 반면에 DOM은 자바스크립트에서 구현하기 어렵기로 악명이 높다. ES6 프록시는 이것을 도와준다(이 쳅터에서 설명).

몇몇의 기능들은 자바스크립트 개선이 아닌 특별하게 추가 되었다. 그러나 이 기능은 자바스크립트로 컴파일을 쉽게 해준다. 두가지 예를 보면:

```
Math.fround() – rounding Numbers to 32 bit floats
Math.imul() – multiplying two 32 bit ints
```

이것들은 예를 들면 C/C++을 Emscripten을 통해 자바스크립트로 컴파일 할때 유용하다. 

### 1.5.2 목표: 상호 개선 
이 목표는 상호 개선, 가능한 사실상의 표준을 채택이다.
예를 들면:

* Classes: 는 생성자 함수들이 현재 사용되는 방법을 기반한다.
* Modules: 은 CommonJS 모델 포멧으로 부터 설계 아이디어를 가져왔다.
* Arrow functions: 은 CoffeeScript로 부터 빌려온 문법을 가진다.
* Named function parameters: 기명 파라미터에 대한 지원이 내장되어 있지 않다. 대신에 객체 리터럴를 통한 기명 파라미터의 기존 관행은 파라미터 정의 시 해체를 통해 제공된다.

### 1.5.3 목표: 버져닝
목표는: 가능한 간단하고 선형적으로 버저닝을 유지 하는 것 이다.

ES5에 대한 관련된 부분도 없으며, 모든것이 ES6이고, 이전에 언급했던것 처럼 ES6은 ES6코드 기반 안에서 "하나의 자바스크립트"를 통해 버저닝을 피한다.

## 1.6 ES6기능 개요
ECMAScript 6 스팩의 도입을 인용:

모듈, 클래식 정의, 어휘적 블록 스코프, 반복자와 제너레이터, 비동기 프로그래밍을 위한 프로미스, 패턴 패체와 꼬리 재귀는 ECMAScript 6의 주요 개선의 일부로 포함된다. 내장된 ECMAScript 라이브러리는 맵, 셋, 문자열의 유니코드 보조문자, 정규표현식 표현식을 추가적으로 잘 제공하기 위한 바이너리 수형 배열을 포함한 추가적인 추상 데이터을 제공하기 위해서 확장되었다. 현재 내장 모듈은 서브클래스를 통해 확장 가능하다.

여기 세개의 주요 기능의 기능 그룹이 있다.:

* 이미 존재하는 기능을 위한 더 나은 문법 (예: 라이브러리를 통해). 예를 들면:
   * Classes
   * Modules
* 표준 라이브러리의 새로운 기능. 예를 들면:
   * 문자열, 배열을 위한 새로운 메소드
   * 프로미스
   * 맵, 셋
* 완전한 새로운 기능. 예를 들면:
   * 제너레이터
   * 프록시
   * 위크맵
  
## 1.7 ECMAScript 역사 요약
이 섹션은 ECMAScript 6으로 가는길에서 무슨일이 있었는지 설명한다.

### 1.7.1 초기: ECMAScript 1-3
* ECMAScript 1 (1997.06) 자바스크립트 언어 표준의 첫 버전이다.
* ECMAScript 2 (1998.06) 자바스크립트에 대한 별도의 ISO 표준과 동기화 사양을 유지하기 위해, 작은 변화를 포함되었다.
* ECMAScript 3 (1999.12) 은 언어의 인기있는 부분이 되기 위한 많은 기능 시도 되었다.
 
### 1.7.2 ECMAScript 4 (2008.07 버려짐)
ES4의 작업은 ES3이 배포된 1999 이후에 시작되었다. 2003년 중간보고서가 배포된 후에 ES4 작업은 중단되었다. 중간 보고서에 기술된 언어의 하위 집합은 어도비(액션스크립트)와 마이크로소프트(JScript.NET)에 의해 구현되었다.

2005년 2월에 제시 제임스 개럿은 자바스크립트를 통한 동적 프론트 앱 개발의 인기를 얻게 된 새로운 기술을 목격하게 된다. 그는 이 기술을 Ajax로 불렀다. Ajax는 완전하게 새로운 웹 앱의 세계를 가능하게 하였고 자바스크립트안에서의 흥미의 파도로 이끌었다.

그것은 아마도 TC38가 2005년 가을에 ES4를 다시 시작하게 기여하혔다. 그들은 ES4를 ES3, ES4 중간 보고서,  액션 스크립트와 JScript.NET을 통한 경험 기반으로 만들었다.

미래 ECMAScript 버전 만드는 두 그룹이 있다.:
* ECMAScript 4는 어도비, 모질라, 오페라, 구글에 의해서 설계 되었고, 대규모 업그레이드 되었다. 그것의 계획의 기능 집합은 아래를 포함한다.:
   * 큰 (클래스, 인터페이스, 네임스페이스, 패키지, 프로그램 유닛, 옵션형 어노테이션, 옵션형 스태틱 타입, 체크, 확인) 프로그래밍
   * 진화적 프로그래밍과 스크립팅 (구조상의 타입, 덕 타입, 다입 정의, 멀티메소드)
   * 자료구조 생성자 (매개변수 타입, 겟터와 셋터, 메타레벨 메소드)
   * 제어 추상 (꼬리 재귀 선호, 반복자, 제너레이터)
   * Introspection(리플랙션)(메타 객체 타입, 스택 마크)
* ECMAScript 3.1은 마이크로소프트와 야훟에 의해 설계되었다. 그것은 ES4의 부분집합으로, ECMAScript 3 점진적인 업그레이드, 버그 수정과 작은 새 기능을 위한 계획이다. ECMAScript3.1은 마침내 ECMAScript 5가 되었다.

두 그룹은 자바스크립트의 미래에 동의 하지 않았고, 계속 둘 사이의 긴장은 증가되고 있다.

이 섹션의 원인:
“Proposed ECMAScript 4th Edition – Language Overview”. 2007-10-23

“ECMAScript Harmony” by John Resig. 2008-08-13

### 1.7.3 ECMAScript Harmony
2008년 7월말에 오슬로에서 TC39 모임이 있었고, 그 결론은 브랜트 아이크에 의하여 아래와 같이 서술 된다.:

자바스크립트 표준의 핵심인 에크마 기술 의원회 39는 분할된 채로 수년간 지낸건 비밀이 아니다. 어떤 사람은 ES4를 좋아하고 다른 사람은 ES3.1을 옹호한다. 나는 즐겁게 말한다. 분리를 끝났다고.

회의에서 산정된 동의은 4가지 포인트로 구성된다.

1. ECMAScript의 점차적 업그래이드를 개발(이것은 ECMAScript 5가 된다.)
2. 주요 새로운 기능 개발. 이 기능은 더 ECMAScript4 보다 적당하지만 ECMAScript 3 이후 버전보다 더 큰 범위를 가진다. 이 버전은 코드 네임 하모니이다. 왜냐하면 그것이 태어날때의 회의의 성격에 기인 된다.
3. ECMAScript4 로부터의 기능은 없어질 것 이다. :packages, namespace, early binding.
4. 다른 아이디어는 TC39의 모두의 동의 속에서 개발되어진다. 

따라서: ES4 그룹은 ES4보다 덜 급진적으로 만들길 동의하였고, EC39의 나머진 앞으로 나아가길 동의하였다.

ECMAScript의 다음버전은:

ECMAScript 5 (December 2009). This is the version of ECMAScript that most browsers support today. It brings several enhancements to the standard library and updated language semantics via a strict mode.
ECMAScript 5.1 (June 2011). ES5 was submitted as an ISO standard. In the process, minor corrections were made. ES5.1 contains those corrections, it is the same text as ISO/IEC 16262:2011.
ECMAScript 6 (June 2015). This version went through several name changes:
ECMAScript Harmony: was the initial code name for JavaScript improvements after ECMAScript 5.
ECMAScript.next: It became apparent that the plans for Harmony were too ambitious for a single version, so its features were split into two groups: The first group of features had highest priority and was to become the next version after ES5. The code name of that version was ECMAScript.next, to avoid prematurely comitting to a version number, which proved problematic with ES4. The second group of features had time until after ECMAScript.next.
ECMAScript 6: As ECMAScript.next matured, its code name was dropped and everybody started to call it ECMAScript 6.
ECMAScript 2015: In late 2014, TC39 decided to change the official name of ECMAScript 6 to ECMAScript 2015, in light of upcoming yearly spec releases. However, given how established the name “ECMAScript 6” already is and how late TC39 changed their minds, I expect that that’s how everybody will continue to refer to that version.
ECMAScript 2016 was previously called ECMAScript 7. Starting with ES2016, the language standard will see smaller yearly releases.
