## 변수 선언 타입

자바스크립트에서 변수를 선언할 때 사용할 수 있는 키워드는 `var`, `let`, `const` 세 가지다. 이들 키워드는 우선순위가 `const` > `let` > `var` 순서로 나뉘는데, 그 이유를 이해하기 위해서는 `var`의 함수 스코프와 `let`, `const`의 블록 스코프에 대해 알아야 한다.

```
var name = 'Max';
var name = 'Jon';
var name = 'Amy';

console.log(name); // 'Amy'
```

`var` 키워드로 선언한 변수는 동일한 이름의 변수를 여러 번 선언해도 오류가 발생하지 않는다. 이 경우, 가장 마지막에 할당된 값이 최종적으로 변수의 값이 된다.

```
console.log(name); // undefined

var name = 'Max';
```

반대로, 변수를 선언하기 전에 `var` 키워드로 선언된 변수를 호출하면 `undefined`가 출력되는 것을 확인할 수 있다. 이처럼 `var` 키워드는 재할당과 재선언이 자유로워 코드가 길어질수록 관리가 어려워질 수 있다.

```
let name = 'Max';
let name = 'Jon'; // 오류 발생
let name = 'Amy';
```

하지만 `var` 키워드를 `let`으로 변경하면 변수의 재선언을 방지할 수 있다. `let` 키워드는 동일한 이름의 변수를 재선언할 경우 문법 오류를 발생시키므로, 컴파일 단계에서부터 이러한 재선언을 제한한다.

```
let name = 'Max';
name = 'Amy';
console.log(name); // 'Amy'

const age = 18;
age = 19; // 오류 발생
```

`const` 키워드도 재선언을 제한하지만, `let`과 `const`의 주요 차이는 변수값의 재할당에서 나타난다. `let`으로 선언한 변수는 자유롭게 값 재할당이 가능하지만, `const`로 선언한 변수는 초기값만 유지하며 다른 값으로 재할당할 경우 문법 오류가 발생한다.

## 함수 스코프 & 블록 스코프

변수를 선언할 때 전역변수를 되도록 사용하지 말라는 이야기가 있다. 전역변수는 전역 스코프에서 선언한 변수를 의미하며, 코드 어디서나 호출할 수 있다. 그러나 이로 인해 원치 않는 영역에서도 변수에 영향을 줄 수 있어 변수가 설계와 다른 용도로 사용될 위험이 있다.

```
var global = '전역';

if (global === '전역') {
  var global = '지역';

  console.log(global); // 지역
}

console.log(global); // 지역
```

같은 이름의 `global` 변수를 선언했지만, 하나는 전역에서, 다른 하나는 `if` 문 블록 내에서 선언되었다. 그러나 `if` 문 블록에서 선언한 값이 전역 변수의 값까지 변경한 것을 마지막 줄에서 확인할 수 있다. 이처럼 `var` 키워드는 함수 스코프를 가지므로, 함수로 나뉘지 않은 영역에서 모든 스코프에 영향을 미칠 수 있다.

```
let global = '전역';

if (global === '전역') {
  let global = '지역';

  console.log(global); // 지역
}

console.log(global); // 전역
```

하지만 `var` 키워드를 `let`으로 변경하면 결과가 달라지는 것을 볼 수 있다. `let`과 `const` 키워드는 블록 스코프를 가지기 때문에, `if` 문 블록 내에서 변수를 선언해도 전역에 있는 변수에 영향을 주지 않는다.

결론적으로, 변수를 선언할 때 `var` 키드 대신 `let` 또는 `const` 키워드를 사용하는 것이 코드를 더 안전하게 작성할 수 있음을 알 수 있다. 여기서 추가로 `let` 대신 `const`를 더 우선적으로 사용하는 것이 코드 일관성 측면에서 더 효과적이다.

```
const person = {
  name: 'Max',
  age: '19',
};

person.name = 'Kim';
person.age = '24';

console.log(person); // { name: 'Kim', age: '24' }
```

`const` 키워드는 변수의 값 재할당을 제한하지만, 배열이나 객체 타입을 사용할 경우 내부 값을 변경할 수 있다. 값의 재할당이 자유로운 `let`은 실수로 값이 원치 않게 변경될 수 있지만, `const`는 값이 의도치 않게 재할당되는 상황을 미리 방지해준다.

## 전역 사용 최소화

전역 스코프에서 생성된 변수를 전역변수라고 하며, 개발자들은 최대한 전역변수를 생성하지 않으면서 코드를 작성해야 한다. 이 규칙은 일종의 불문율로 여겨지며, 자바스크립트의 런타임이 어떻게 동작하는지를 이해하면 그 이유를 쉽게 알 수 있다.

```
var foo = 'foo';
console.log(window.foo); // 'foo'
```

자바스크립트의 전역은 두 가지 종류가 있다. Node.js 기반으로 실행될 때는 global이 전역이 되며, 브라우저 기반으로 실행될 때는 window가 전역이 된다. 전역변수는 이러한 전역 객체의 프로퍼티로 생성되어 파일 어디서나 호출할 수 있도록 한다.

```
// test1.js

var foo = 'foo';
```

```
// test2.js

console.log(window.foo); // 'foo'
console.log(foo); // 'foo'
```

여기서 흥미로운 점은 전역 공간이 모든 JS 파일을 하나로 묶어 한 덩어리로 인식한다는 것이다. 따라서 변수가 생성된 파일이 아니어도 다른 파일에서 변수를 호출할 수 있다. 즉, 같은 변수명이 다른 파일과 섞일 수 있으며, var 키워드로 생성한 경우 재할당도 자유롭게 할 수 있어 원래의 목적이 퇴색될 위험이 있다.

이처럼 전역 공간은 어디서나 접근 가능하고 스코프 분리가 불가능하기 때문에 사용을 최소화해야 한다. 전역 공간 사용을 줄이기 위해서는 다음과 같은 방법을 고려할 수 있다.

1. 전역변수 대신 지역변수로만 함수를 작성한다.
2. window 또는 global, 즉 전역 객체에 접근하거나 조작하지 않는다.
3. var 키워드 대신 const 또는 let 키워드를 사용한다.
4. 즉시 실행 함수(IIFE), 모듈, 클로저를 사용하여 스코프를 나눈다.

## 임시변수

임시변수는 함수 내에서 연산 처리를 위해 임시로 생성한 변수를 의미한다. 그러나 이러한 임시변수도 함수의 길이가 길어질수록 전역변수처럼 활용될 수 있기 때문에 사용을 최소화해야 한다.

```
fuction getElements() {
  const result = {}; // 임시변수

  result.title = document.querySelector('.title');
  result.text = document.querySelector('.text');
  result.value = document.querySelector('.value');

  return result;
}
```

임시변수의 문제점은 미래의 자신이나 주변 팀원들이 임시변수에 추가로 연산을 덧붙일 수 있다는 점이다. 이렇게 되면 함수의 크기가 커질 뿐만 아니라, 기존 사용 용도가 바뀔 수 있어 코드 관리가 복잡해진다.

```
fuction getElements() {
  return {
    title = document.querySelector('.title');
    text = document.querySelector('.text');
    value = document.querySelector('.value');
  };
}
```

임시변수를 최소화하는 가장 간단한 방법은 변수를 선언하지 않는 것이다. 이렇게 하면 함수에 추가적인 연산을 처리하기 애매해지고, 무엇보다 함수의 사용 목적이 뚜렷하게 나타나기 때문에 나중에 코드를 이해하기 쉬워진다.

```
function getDateTime(targetDate) {
  let month = targetDate.getMonth();
  let day = targetDate.getDate();
  let hour = targetDate.getHours();

  month = month >= 10? month : '0' + month;
  day = day >= 10? day : '0' + day;
  hour = hour >= 10? hour : '0' + hour;

  return {
    month, day, hour
  }
}
```

현업에서 기획이 바뀌거나 추가 로직이 필요한 경우, 개발자는 두 가지 방법으로 문제를 해결할 수 있다. 첫 번째는 기존 함수의 코드를 수정하여 재사용하는 것이고, 두 번째는 바뀐 로직을 담은 새로운 함수를 만드는 것이다.

하지만 기존 함수를 수정하면 기존 함수를 사용하는 모든 곳에서 아웃풋이 바뀌기 때문에 예상치 못한 버그가 발생할 수 있다. 또한, 한 번 연산이 바뀐 함수는 계속해서 로직이 바뀔 수 있어 함수에 대한 신뢰가 사라질 위험이 있다.

```
function getDateTime(targetDate) {
  const month = targetDate.getMonth();
  const day = targetDate.getDate();
  const hour = targetDate.getHours();

  return {
    month: month >= 10? month : '0' + month;
    day: day >= 10? day : '0' + day;
    hour: hour >= 10? hour : '0' + hour;
  }
}

function getCurrentTime() {
  const currentDateTime = getDateTime(new Date());

  return {
    month: currentDateTime.month + '월';
    day: currentDateTime.day + '일';
    hour: currentDateTime.hour + '시';
  }
}
```

코드의 일관성을 유지하기 위해서는 기존 함수를 수정하기보다는 기존 함수의 결과값을 다시 연산하는 새로운 함수를 만드는 것이 좋다. 이를 위해 변수 선언 시 let이 아닌 const를 사용하여 변수의 값이 바뀌지 않도록 설계해야 한다.

그러나 const를 사용하더라도 객체나 배열을 다루는 변수는 내부값의 수정이 가능하므로, 최대한 함수를 하나의 연산만 하도록 설계하여 임시변수 사용을 최소화해야 한다.
