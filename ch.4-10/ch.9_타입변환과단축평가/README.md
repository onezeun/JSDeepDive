# 9. 타입 변환과 단축 평가

## 1. 타입 변환이란?

자바스크립트의 모든 값은 타입이 있다.

개발자의 의도에 따라 다른 타입으로 변환 가능한 경우  
**명시적 타입 변환** 또는 **타입 캐스팅** 이라고 하며,

개발자의 의도와 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 경우  
**암묵적 타입 변환** 또는 **강제 타입 변환**이라고 한다

```js
var x = 10;

// 명시적 타입 변환
// 숫자를 문자열로 타입 캐스팅한다.
var str = x.toString();
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10

// 암묵적 타입 변환
// 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
var str = x + "";
console.log(typeof str, str); // string 10

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof x, x); // number 10
```

명시적 타입변환은 타입을 변경하겠다는 개발자의 의지가 코드에 드러나지만,  
암묵적 타입 강제변환은 자바스크립트 엔진에 의해 암묵적으로 드러나지 않게 변환되기 때문에 코드에 명백히 나타나지 않는다.

타입 변환이 어떻게 동작하는지 정확히 이해하고 사용하자~

## 2. 암묵적 타입 변환

개발자의 의도와 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 경우

```js
// 피연산자가 모두 문자열 타입이어야 하는 문맥
"10" + 2; // -> '102'

// 피연산자가 모두 숫자 타입이어야 하는 문맥
5 * "10"; // -> 50

// 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
!0; // -> true
if (1) {
}
```

### 2.1 문자열 타입으로 변환

`+` 연산자를 사용하면 문자열 타입으로 변환  
👉 연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작

ES6에 도입된 템플릿 리터럴의 표현식 삽입도 평가 결과를 문자열 타입으로 암묵적 변환

```js
1 +
  "2" // 템플릿 리터럴 // '12'
  `1 + 1 = ${1 + 1}`; // -> '1 + 1 = 2'
```

문자열 타입으로 암묵적 타입변환 되는 예

```js
// 숫자 타입
0 + ''         // -> '0'
-0 + ''        // -> '0'
1 + ''         // -> '1'
-1 + ''        // -> '-1'
NaN + ''       // -> 'NaN'
Infinity + ''  // -> 'Infinity'
-Infinity + '' // -> '-Infinity'

// 불리언 타입
true + ''  // -> 'true'
false + '' // -> 'false'

// null 타입
null + '' // -> 'null'

// undefined 타입
undefined + '' // -> 'undefined'

// 심벌 타입
(Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''           // -> '[object Object]'
Math + ''           // -> '[object Math]'
[] + ''             // -> ''
[10, 20] + ''       // -> '10,20'
(function(){}) + '' // -> 'function(){}'
Array + ''          // -> 'function Array() { [native code] }'
```

### 2.2 숫자 타입으로 변환

산술 연산자중 `+` 제외한 연산자 `-`, `*`, `/`, `%` 를 사용하면 숫자 타입으로 변환  
비교 연산자(`>`, `<`, `>=`, `<=`)도 숫자 타입으로 변환

피연산자를 숫자타입으로 변환할 수 없는 경우 표현식 평가 결과는 `NaN`이 된다.

```js
1 - "1"; // -> 0
1 * "10"; // -> 10
1 / "one"; // -> NaN
13 % "5"; // -> 3
"1" > 0; // -> true
```

`+` 단항 연산자를 문자열 앞에 사용하면 숫자 타입의 값으로 변환

```js
// 문자열 타입
+"" + // -> 0
  "0" + // -> 0
  "1" + // -> 1
  "string" + // -> NaN
  // 불리언 타입
  true + // -> 1
  false + // -> 0
  // null 타입
  null + // -> 0
  // undefined 타입
  undefined + // -> NaN
  // 심벌 타입
  Symbol() + // -> ypeError: Cannot convert a Symbol value to a number
  // 객체 타입
  {} + // -> NaN
  [] + // -> 0
  [10, 20] + // -> NaN
  function () {}; // -> NaN
```

### 2.3 불리언 타입으로 변환

if문의 조건식은 불리언 타입으로 암묵적 타입 변환 함

```js
if ("") console.log("1");
if (true) console.log("2");
if (0) console.log("3");
if ("str") console.log("4");
if (null) console.log("5");

// 2 4
```

자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy값(참으로 평가되는 값) 또는 Falsy값(거짓으로 평가되는 값)으로 구분한다.

> **Falsy 값**  
> `false`, `undefined`, `null`, `0`, `-0`, `NaN`, `''(빈 문자열)`

Falsy값 이외의 모든 값은 true로 평가되는 Truthy값

## 3. 명시적 타입 변환

개발자의 의도에 따라 다른 타입으로 변환 가능한 경우

### 3.1 문자열 타입으로 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법

- String 생성자 함수를 new 연산자 없이 호출하는 방법
- Object.prototype.toString 메서드를 사용하는 방법
- 문자열 연결 연산자를 이용하는 방법

```js
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
// 숫자 타입 => 문자열 타입
String(1); // -> '1'
String(NaN); // -> 'NaN'
String(Infinity); // -> 'Infinity'
// 불리언 타입 => 문자열 타입
String(true); // -> 'true'
String(false); // -> 'false'

// 2. Object.prototype.toString 메서드를 사용하는 방법
// 숫자 타입 => 문자열 타입
(1).toString(); // -> '1'
NaN.toString(); // -> 'NaN'
Infinity.toString(); // -> 'Infinity'
// 불리언 타입 => 문자열 타입
true.toString(); // -> 'true'
false.toString(); // -> 'false'

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 => 문자열 타입
1 + ""; // -> '1'
NaN + ""; // -> 'NaN'
Infinity + ""; // -> 'Infinity'
// 불리언 타입 => 문자열 타입
true + ""; // -> 'true'
false + ""; // -> 'false'
```

> **[💡참고] =new연산자 사용 여부에 따라 다른 타입**
>
> ```js
> // new 연산자를 사용하지 않으면 원시값이 생성
> var primitive = String("a");
> typeof primitive; // 'string'
>
> // new 연산자와 함께 사용하면 object 타입이 생성
> var obj = new String("a");
> typeof obj; // 'object'
> ```

### 3.2 숫자 타입으로 변환

숫자 타입이 아닌 값을 숫자 타입으로 변환 하는 방법

- Number 생성자 함수를 new연산자 없이 호출하는 방법
- parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
- `+` 단항 산술 연산자를 이용하는 방법
- `*` 산술 연산자를 이용하는 방법

```js
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 숫자 타입
Number("0"); // -> 0
Number("-1"); // -> -1
Number("10.53"); // -> 10.53
// 불리언 타입 => 숫자 타입
Number(true); // -> 1
Number(false); // -> 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
// 문자열 타입 => 숫자 타입
parseInt("0"); // -> 0
parseInt("-1"); // -> -1
parseFloat("10.53"); // -> 10.53

// 3. + 단항 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
+"0"; // -> 0
+"-1"; // -> -1
+"10.53"; // -> 10.53
// 불리언 타입 => 숫자 타입
+true; // -> 1
+false; // -> 0

// 4. * 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
"0" * 1; // -> 0
"-1" * 1; // -> -1
"10.53" * 1; // -> 10.53
// 불리언 타입 => 숫자 타입
true * 1; // -> 1
false * 1; // -> 0
```

### 3.3 불리언 타입으로 변환

- Boolean생성자 함수를 new연산자 없이 호출하는 방법
- !부정 논리 연산자를 두 번 사용하는 방법

```js
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 불리언 타입
Boolean("x"); // -> true
Boolean(""); // -> false
Boolean("false"); // -> true
// 숫자 타입 => 불리언 타입
Boolean(0); // -> false
Boolean(1); // -> true
Boolean(NaN); // -> false
Boolean(Infinity); // -> true
// null 타입 => 불리언 타입
Boolean(null); // -> false
// undefined 타입 => 불리언 타입
Boolean(undefined); // -> false
// 객체 타입 => 불리언 타입
Boolean({}); // -> true
Boolean([]); // -> true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// 문자열 타입 => 불리언 타입
!!"x"; // -> true
!!""; // -> false
!!"false"; // -> true
// 숫자 타입 => 불리언 타입
!!0; // -> false
!!1; // -> true
!!NaN; // -> false
!!Infinity; // -> true
// null 타입 => 불리언 타입
!!null; // -> false
// undefined 타입 => 불리언 타입
!!undefined; // -> false
// 객체 타입 => 불리언 타입
!!{}; // -> true
!![]; // -> true
```

## 4. 단축 평가

### 4.1 논리 연산자를 사용한 단축 평가

논리곱`&&`연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환하며  
좌항에서 우항으로 평가가 진행됨

```js
"Cat" && "Dog"; // -> "Dog"
```

논리합`||`연산자는 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환하며  
좌항에서 우항으로 평가가 진행됨

```js
"Cat" || "Dog"; // -> "Cat"
```

첫 번째 피연산자 'Cat'은 Truthy값이므로 true로 평가되고, 이 시점에서는 두 번째 피연산자까지 평가 할 필요없이 표현식이 평가되기 때문에 'Cat'문자열을 그대로 반환

논리곱`&&`, 논리합`||` 연산자는 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환하는데, 이를 **단축 평가** 라고 한다.

**단축 평가**  
표현식을 평가하는 도중에 평가 결과가 확정된경우 나머지 평가 과정을 생략하는 것

단축 평가의 규칙

| 단축 평가 표현식    | 평가 결과 |
| ------------------- | --------- |
| true \|\| anything  | true      |
| false \|\| anything | anything  |
| true && anything    | anything  |
| false && anything   | true      |

```js
// 논리합(||) 연산자
"Cat" || "Dog"; // -> "Cat"
false || "Dog"; // -> "Dog"
"Cat" || false; // -> "Cat"

// 논리곱(&&) 연산자
"Cat" && "Dog"; // -> "Dog"
false && "Dog"; // -> false
"Cat" && false; // -> false
```

> **단축 평가를 사용하여 if문 대체하기**
>
> - 조건이 Truthy값인 경우 논리곱&&연산자 표현식으로 if문을 대체
> - 조건이 Falsy값 인경우 논리합||연산자 표현식으로 if문을 대체
>
> ```js
> var done = true;
> var message = "";
>
> // 주어진 조건이 true일 때
> if (done) message = "완료";
>
> // if 문은 단축 평가로 대체 가능하다.
> // done이 true라면 message에 '완료'를 할당
> message = done && "완료";
> console.log(message); // 완료
>
> // ----------------------------------------
>
> var done = false;
> var message = "";
>
> // 주어진 조건이 false일 때
> if (!done) message = "미완료";
>
> // if 문은 단축 평가로 대체 가능하다.
> // done이 false라면 message에 '미완료'를 할당
> message = done || "미완료";
> console.log(message); // 미완료
> ```

**단축 평가가 유용하게 사용되는 상황들**

1. 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

    ```js
    // 타입에러 
    var elem = null;
    var value = elem.value; // TypeError: Cannot read property 'value' of null

    // 단축 평가 사용
    var elem = null;
    // elem이 null이나 undefined와 같은 Falsy 값이면 elem으로 평가되고
    // elem이 Truthy 값이면 elem.value로 평가된다.
    var value = elem && elem.value; // -> null
    ```

2. 함수 매개변수에 기본값을 설정할 때  
함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당되는데, 단축 평가를 사용하면 이를 방지할 수 있다.

    ```js
    // 단축 평가를 사용한 매개변수의 기본값 설정
    function getStringLength(str) {
      str = str || '';  // 단축 평가
      return str.length;
    }

    getStringLength();     // -> 0
    getStringLength('hi'); // -> 2


    // ES6의 매개변수의 기본값 설정
    function getStringLength(str = '') {
      return str.length;
    }

    getStringLength();     // -> 0
    getStringLength('hi'); // -> 2
    ```

### 4.2 옵셔널 체이닝 연산자

ES11에서 도입된 옵셔널 체이닝 연산자 `?.`  

좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어감

```js
var elem = null;

// elem이 null 또는 undefined이면 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
var value = elem?.value;
console.log(value); // undefined
```

좌항의 피연산자가 false로 평가되는 Falsy(false, undefined, 0, NaN, ' ')값이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다

```js
var str = '';

// 문자열의 길이(length)를 참조한다. 이때 좌항 피연산자가 false로 평가되는 Falsy 값이라도
// null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.
var length = str?.length;
console.log(length); // 0
```

### 4.3 null 병합 연산자

ES11에서 도입된 null 병합 연산자 `??`  

좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환

변수의 기본값을 설정할 때 유용함

```js
// 좌항의 피연산자가 null 또는 undefined이면 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
var foo = null ?? 'default string';
console.log(foo); // "default string"
```

옵셔널 체이닝과 마찬가지로 좌항의 피연산자가 false로 평가되는 Falsy(false, undefined, 0, NaN, ' ')값이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.

```js
// 좌항의 피연산자가 Falsy 값이라도 null 또는 undefined이 아니면 좌항의 피연산자를 반환한다.
var foo = '' ?? 'default string';
console.log(foo); // ""
```