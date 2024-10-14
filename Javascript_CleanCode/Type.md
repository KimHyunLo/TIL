## 타입 검사

자바스크립트로 코딩하다 보면 변수의 타입에 따라 로직을 작성해야 하는 순간이 발생한다. 이런 타입 검사가 필요할 때 많이 사용되는 방법이 `typeof`다.

```javascript
typeof 'Max'; // 'string'
typeof true; // 'boolean'
typeof undefined; // 'undefined'
typeof 123; // 'number'
typeof Symbol(); // 'symbol'
```

`typeof`는 변수의 타입을 문자열로 반환하는 자바스크립트 연산자다. 하지만 `typeof`로 모든 변수의 타입을 완벽하게 가려낼 수는 없다.

자바스크립트는 변수의 타입을 원시타입과 참조타입 두 가지로 분류한다. 위의 예시에서 사용한 변수들은 모두 원시값이며, 원시값의 특징은 불변성을 가진다는 것이다.

```javascript
function checkType() {}
class CheckClass {}
const str = new String('Max');

typeof checkType; // 'function'
typeof CheckClass; // 'function'
typeof str; // 'object'
typeof null; // 'object'
```

참조타입은 객체 속성에서 분류되는 함수, 배열, 날짜(Date) 등의 타입을 의미한다. `typeof`를 통해 이러한 타입의 변수를 검사할 경우, 같은 타입을 가진 다른 값들이 여럿 존재하기 때문에 변수의 타입을 정확하게 분류하기 어렵다.

무엇보다 자바스크립트는 변수의 타입이 동적으로 바뀔 수 있는 언어이므로, 타입 검사를 더욱 주의 깊게 해야 한다.

```javascript
const arr = [];
const func = function() {};
const date = new Date();

arr instanceof Array; // true
func instanceof Function; // true
date instanceof Date; // true
```

참조타입은 공통된 타입을 가진 변수들이 많기 때문에 `typeof`로 검사하기 애매하다. 이러한 이유로 참조타입을 검사하는 `instanceof` 함수를 사용할 수 있다. `instanceof`는 변수의 타입과 프로토타입을 비교하여 같은지 검사하는 함수다.

```javascript
const arr = [];
const func = function() {};
const date = new Date();

arr instanceof Object; // true
func instanceof Object; // true
date instanceof Object; // true
```

하지만 `instanceof`의 단점은 프로토타입이 같을 경우에도 `true` 값을 출력한다는 점이다. 배열, 함수, 날짜 객체 등은 모두 객체 타입의 하위 타입이기 때문에 프로토타입이 되는 객체로 검사했을 때 `true`가 나오는 것을 볼 수 있다.

```javascript
Object.prototype.toString.call([]); // '[object Array]'
Object.prototype.toString.call(function() {}); // '[object Function]'
Object.prototype.toString.call(new Date()); // '[object Date]'
```

`instanceof`와 `typeof`는 타입 검사를 할 때 몇 가지 단점이 있다. 이러한 상황에서 정확한 타입을 검사하기 위해 고안된 방법은 객체 프로토타입을 역으로 이용하여 변수의 타입을 검사하는 방법이다.

`prototype`에 `call`을 바인딩하여 변수를 검사할 경우, 객체 안에서 어떤 타입으로 분류되는지 문자열 값으로 출력할 수 있다. 이 방법은 무조건 맞는 방법은 아니지만, `typeof`와 `instanceof`보다 더 정확한 값을 확인할 수 있기 때문에 유용한 방법이다. 