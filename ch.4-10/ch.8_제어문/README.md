# 8. 제어문

조건에 따라 코드 블록을 실행(조건문)하거나 반복실행(반복문)할 때 사용  
 일반적인 코드는 위에서 아래로 순차적으로 실행하지만, 제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있다.

## 1. 블록문

0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 함  
 자바스크립트는 블록문을 하나의 실행 단위로 취급하며 단독으로 사용할 수도 있으나 제어문이나 함수를 정의할 때 사용하는 것이 일반적

> **주의**  
> 문의 끝에는 세미콜론을 붙이는것이 일반적이지만  
> 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖기 때문에 블록문의 끝에는 세미콜론을 붙이지 않음

```js
// 블록문
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```

## 2. 조건문

주어진 조건식의 평가 결과에 따라 블록문의 실행을 결정  
불리언 값으로 평가될 수 있는 표현식

### 2.1 if...else문

```js
if (조건식1) {
  // 조건식1이 참이면 실행되는 코드 블록
} else if (조건식2) {
  // 조건식2가 참이면 실행되는 코드 블록
} else {
  // 조건식이 거짓이면 실행되는 코드 블록
}

// 코드 블록 내의 문이 하나뿐이라면 중괄호 생략 가능
if (num % 2) result = "홀수"; // 2 % 2는 0 -> false로 강제 변환

// 삼항 조건 연산자로 바꿔 쓰기 가능
var result = num % 2 ? "홀수" : "짝수";
```

## 2. switch 문

주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮김  
다양한 상황에 따라 실행할 코드 블록을 결정할 때 사용

```js
// 월을 영어로 변환한다. (11 → 'November')
var month = 11;
var monthName;

switch (month) {
  case 1: monthName = 'January'; break;
  case 2: monthName = 'February'; break;
  case 3: monthName = 'March'; break;
  case 4: monthName = 'April'; break;
  case 5: monthName = 'May'; break;
  case 6: monthName = 'June'; break;
  case 7: monthName = 'July'; break;
  case 8: monthName = 'August'; break;
  case 9: monthName = 'September'; break;
  case 10: monthName = 'October'; break;
  case 11: monthName = 'November'; break;
  case 12: monthName = 'December'; break;
  default: monthName = 'Invalid month'; break;
}

console.log(monthName); // November
```

default문에는 break문을 생략하는 것이 일반적

> `break`문을 쓰지 않으면 문을 탈출하지 않아 default 값인 Invalid month가 출력된다  
> 이를 **폴스루(fall through)**라고 함

**🤔 일부러 폴스루를 활용하는 경우**  
윤년인지 판별해서 2월의 일수를 계산하는 예제

```js
var year = 2000; // 2000년은 윤년으로 2월이 29일이다.
var month = 2;
var days = 0;

switch (month) {
  case 1:
  case 3:
  case 5:
  case 7:
  case 8:
  case 10:
  case 12:
    days = 31;
    break;
  case 4:
  case 6:
  case 9:
  case 11:
    days = 30;
    break;
  case 2:
    // 윤년 계산 알고리즘
    // 1. 연도가 4로 나누어떨어지는 해(2000, 2004, 2008, 2012, 2016, 2020...)는 윤년이다.
    // 2. 연도가 4로 나누어떨어지더라도 연도가 100으로 나누어떨어지는 해(2000, 2100, 2200...)는 평년이다.
    // 3. 연도가 400으로 나누어떨어지는 해(2000, 2400, 2800...)는 윤년이다.
    days = (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0 ? 29 : 28;
    break;
  default:
    console.log("Invalid month");
}

console.log(days); // 29
```

## 3. 반복문

### 3.1 for문

조건식이 거짓으로 평가될 때까지 코드블록을 반복 실행

```js
for(변수 선언문 또는 할당문; 조건식; 증감식){
    //조건식이 참인 경우 반복 실행될 문
}

for (var i = 0; i < 2; i++) {
  console.log(i);
}

// 무한루프
for (;;) {
  // 무한히 반복 실행되는 문
}
```

### 3.2 while문

조건식의 평가 결과가 참이면 코드 블록을 계속 해서 반복 실행  
for문은 반복 횟수가 명확할 때 주로 사용하고 while문은 반복횟수가 불명확할 때 주로 사용

```js
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행
while (count < 3) {
  console.log(count); // 0 1 2
  count++;
}

// 무한루프
while (true) {
  console.log(count);
  count++;
  // count가 3이면 코드 블록을 탈출
  if (count === 3) break;
} // 0 1 2
```

### 3.3 do...while

코드 블록을 먼저 실행하고 조건식을 평가함 따라서 코드 블록은 무조건 한 번 이상 실행

```js
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행
do {
  console.log(count);
  count++;
} while (count < 3); // 0 1 2
```

## 4. break 문

레이블 문(식별자가 붙은 문), 반복문 또는 switch문의 코드 블록을 탈출

```js
// foo라는 레이블 식별자가 붙은 레이블 문
foo: console.log("foo");

// foo라는 식별자가 붙은 레이블 블록문
foo: {
  console.log(1);
  break foo; // foo 레이블 블록문을 탈출한다.
  console.log(2);
}

console.log("Done!");
```

레이블 문은 프로그램의 실행 순서를 제어하는 데 사용  
switch문의 case문과 default문도 레이블 문임

레이블문을 탈출하려면 breke문에 레이블 식별자를 지정

이중 for문을 탈줄하기 위해서도 사용됨

```js
// outer라는 식별자가 붙은 레이블 for 문
outer: for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    // i + j === 3이면 outer라는 식별자가 붙은 레이블 for 문을 탈출한다.
    if (i + j === 3) break outer;
    console.log(`inner [${i}, ${j}]`);
  }
}

console.log("Done!");
```
레이블 문을 사용하면 프로그램의 흐름이 복잡해져 가독성이 나빠지고 오류를 발생시킬 가능성이 높아지기 때문에 권장하지 않음

## 5. continue문

반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동  
break문 처럼 반복문을 탈출하지는 않음

문자열에서 특정 문자의 개수를 세는 예
```js
var string = 'Hello World.';
var search = 'l';
var count = 0;

// 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 'l'이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
  if (string[i] !== search) continue;
  count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
}

console.log(count); // 3

// 참고로 String.prototype.match 메서드를 사용해도 같은 동작을 한다.
const regexp = new RegExp(search, 'g');
console.log(string.match(regexp).length); // 3
```