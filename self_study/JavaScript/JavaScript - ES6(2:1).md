# JavaScript - ES6

##  1. Block-level scope variable

### 1.1. let

기본적으로 Js의 변수는 Function-level scope를 갖는다.

```
Function-level scope
함수내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다.
```

```
Block-level scope
코드 블럭 내에서 선언된 변수는 코드 블럭 내에서만 유효하며 코드 블럭 외부에서는 참조할 수 없다.
```

> ES6는 Block-level scope를 갖는 변수를 선언하기 위해 let 키워드를 제공한다. 



```javascript
var foo = 123;
var foo = 456; // ok

let bar = 123;
let bar = 456; // Error: Identifier 'bar' has already been declared
```

> var는 중복선언이 가능하였으나 let은 중복 선언 시 에러가 발생한다.



- 자바스크립트는 ES6의 let, const를 포함하여 모든 선언(var, let, const, function, function*, class)을 호이스팅(Hoisting) 한다.


- BUT, var와 달리 let으로 선언된 변수를 선언문 이전에 참조하면 ReferenceError가 발생한다. 
- 이는 let선언된 변수는 코드블록의 시작에서 변수의 선언까지 일시적 사각지대(Temporal Dead Zone: TDZ)에 빠지게 되기 때문이다. 

```javascript
console.log(foo); //undefined
var foo;

console.log(bar); // Error: Uncaught ReferenceError: bar is not defined
let bar;
```



### let 연습

0,1,2 출력하기

```javascript
var funcs = [];

for(var i=0; i < 3; i++) {
  funcs.push(function() {
console.log(i);
})
}

for (var j = 0; j <3; j++) {
fincs[j]();
}
```

> for문의 var i가 전역변수이기 때문에 결과는 3이 3번 출력된다.



```javascript
var funcs = [];
// create a bunch of functions
for (var i = 0; i < 3; i++) {
  (function() {
    var local = i;
    funcs.push(function() {
      console.log(local);
    })
  })();
}
// call them
for (var j = 0; j < 3; j++) {
  funcs[j]();
}
```

> Function-level scope로 인한 문제를 회피하는 수단으로 클러저를 활용한 방법이다.



```javascript
var funcs = [];
// create a bunch of functions
for (let i = 0; i < 3; i++) { // Note the use of let
  funcs.push(function() {
    console.log(i);
  })
}
// call them
for (var j = 0; j < 3; j++) {
  funcs[j]();
}
```

> 반복문에서 ES6의 let키워드를 사용하면 동일한 동작을 한다.





### 1.2. const

const는 let과 대부분 동일한 특징을 갖는다. 단 let은 초기화 이후 다른 값으로 재할당이 자유로우나 const는 초기화 이후 `재할당`이 `금지`된다.

``` javascript
const foo = 123;
foo = 456; // TypeError
```



const는 반드시 `선언과 동시에 초기화`가 이루어져야 한다.

```javascript
const foo; // SyntaxError
```



const 는 let과 마찬가지로 `Block-level scope`를 갖는다.

```
{
  const foo = 10;
  console.log(foo); //10
}
console.log(foo); // ReferenceError
```



const는 `객체에서도 사용`할 수 있다. 물론 재할당 금지!!

```javascript
const obj = { foo: 123 };
obj = { bar: 456 }; // TypeError
```



But, 객체의 프로퍼티는 보호되지 않는다. 

재할당은 불가능하지만 `할당된 객체의 내용`은 `변경`할 수 있다!

```javascript
const user = {
  name: 'lee',
  address: {
    city: 'seoul'
  }
};

user.name = 'kim'; // 허용됨!!
console.log(user); // { name: 'kim', address: { city: 'seoul'}}
```



객체의 프로퍼티까지 보호하여 primitive data와 같이 변경이 불가능한 값(immutable value)으로 만들고 싶다면 Object.freeze() 메서드를 사용한다.

```javascript
const user = {
  name: 'lee',
  address: { 
  city: 'seoul'
  }
};

Object.freeze(user);

user.name = 'kim'; // 무시됨
console.log(user); // { name: 'lee'. address: { city: 'seoul'}}

console.log(Object.isFrozen(user)); // true (동결되었는가?)
```



단, 객체 내부의 객체는 변경가능하다.

```javascript
user.address.city = 'busan'; // 변경!
console.log(user); // { name: 'lee', address: { city: 'busan' } }
```



내부 객체까지 변경 불가능하게 만들려면 Deep freeze를 해야 한다.



### let && const 사용

- primitive형 변수에는 let를 사용
- 변경이 발생하지 않는(재할당이 필요없는) primitive형 변수와 객체형 변수에는 const를 사용

> 객체형 변수에 const를 사용하는 이유는 객체의 속성값이 변경된다하더라도 객체형 변수에 저장되는 주소값은 변경되지 않기 때문이다. 자바스크립트의 값은 대부분 객체(primitive형 변수를 제외한 모든 값은 객체이다)이므로 결국 대부분의 경우 const를 사용하게 된다.





## 2. Arrow function (Arrow 함수)

- Arrow function은 익명함수를 좀 더 간략하게 표현할 수 있으며 Lexical this를 제공한다.
- Arrow function은 항상 익명으로 사용한다. 

```javascript
// 매개변수 지정
	() => { ... } // 매개변수가 없을 경우
	 x => { ... } // 매개변수가 한개인 경우는 괄호를 생략할 수 있다.
(x, y) => { ... } // 매개변수가 여러개인 경우

 // 함수 몸체 지정
 	x => { return x * x } // block
 	x => x * x			  // 위 표현과 동일
```



### Arrow function의 장점

1. ##### 일반적인 함수표현식보다 표현이 간단하다.

```
// ES5 

var arr = [ 1, 2, 3];
var pow = arr.map(function(x) {
  return x * x;
});

console.log(pow); // [ 1, 4, 9]
```

```
//ES6

const arr = [1, 2, 3];
const pow = arr.map(x=> x * x);

console.log(pow); // [1, 4, 9]
```



2. ##### 직관적인  this를 사용할 수 있다.

- JavaScript this는 해당 함수 호출 패턴에 따라 바인딩되는 객체가 달라진다. 콜백함수 내부의 this는 전역 객체 window를 가리킨다.
- Arrow function은 위의 규칙을 따르지 않고 언제나 자신을 포함하는 외부 scope에서 this를 계승 받는다. 
- 이를 Lexical this 라 한다.



…..(추가 설명)



콜백함수 내부의 this가 메서드를 호출한 객체를 가리키게 하기 위해서는 아래 4가지 방법이 있다.

### Solution 1:  that = this

```
Prefixer.prototype.prefixArray = function(arr) {
  var that = this; // (a)
  return arr.map(fuction (x) {
    return that.prefix + x;
  });
};
```



### Solution 2: map(function, this)

```
Prefixer.prototype.prefixArray = function(arr) {
  return arr.map(function(x) {
    return this.prefix + x;
  }, this); // (a)
};
```



### Solution 3: bind(this)

ES5에 추가된 Function.prototype.bind()로 this를 바인딩한다.

call(), apply() 사용 가능하다.

```
Prefixer.prototype.prefixArray = function(arr) {
  return arr.map(function (x) {
    return this.prefix + x;
  }.bind(this)); //(a)
};
```

Arrow function은 Solution 3의 Syntatic sugar이다.

```javascript
Prefixer.prototype.prefixArray = function(arr) {
  return arr.map((x)=> this.prefix + x);
};
```

이것을 class로 표현하면,

```javascript
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }
  prefixArray(arr) {
    return arr.map(x => this.prefix + x); //(a)
  }
}
```





### 3. Template Strings (템플릿 문자열)

새로운 종류의 문자열 표기법으로 따옴표 문자 대신 `백틱(backtick) 문자`를 사용한다.

```javascript
let template = `hello how are you`;

console.log(template);
```



템플릿 문자열은 +연산자를 사용하지 않아도 간단한 방법으로 문자열에 새로운 문자열을 삽입할 수 있는 기능을 제공한다.

이를 String Interpolation(문자열 삽입)이라 한다.

```javascript
const first = 'ung-mo';
const last = 'lee';

console.log('my name is ' + first + ' ' + last + '.');
console.log(`my name is ${first} ${last}.`);

console.log(`1 and 1 make ${1+1}`); // 1 and 1 make 2
```



${text}, ${1 + 1}을 템플릿 대입문이라 한다. 

템플릿 대입문에는 문자열 뿐만아니라 모든 JavaScript 표현식이 사용될 수 있다. 

```javascript

function authorize(user, action) {
 if (!user.hasPrivilege(action)) {
  throw new Error(
  `User ${user.name} is not ... do ${action}.`);
  }
 }

```





## 4. Extended Parameter Handing(함수 파라미터 확장)



### 4.1. Default Parameter value(기본 파라미터 초기값)

파라미터에 초기값을 설정하여 함수 내에서 수행하던 파라미터 체크 및 초기화를 간편화할 수 있다. 

```javascript
// ES5
function plus(x, y) {
  x = x || 0;
  y = y || 0;
  return x +y;
}

console.log(pluse());  	// 0
console.log(plus(1, 2)); // 3
```

```javascript
// ES6
function plus(x = 0; y = 0) {
  return x + y;
}

console.log(plus());	// 0
console.log(plus(1, 2)); // 3
```





### 4.2. Rest Parameter (Rest  파라미터)



인자의 갯수를 사전에 알 수 없는 가변 인자 함수의 경우, 함수를 호출할 때 인수들과 함께 암묵적으로 arguments 객체가 함수 내부로 전달되는 함수 객체의 arguments 속성을 사용하여 인자값을 확인하여야 한다. 

```javascript
// ES5
function sum() {
  var array = Array.prototype.slice.call(arguments);
  return array.reduce(function(pre, cur) {
    return pre + cur;
  });
}

console.log(sum(1, 2, 3, 4, 5));
```

ES6의 Rest 파라미터는 가변인지를 함수 내부에 배열로 전달한다. 따라서 유사배열인 arguments객체를 배열로 반환하는 등의 번거로움을 피할 수 있다.

```javascript
// ES6
function sum(...args) {
  console.log(array.isArray(args)); true
  return args.reduce((pre, cur) => pre + cur);
}

console.log(1, 2, 3, 4, 5);
```



### 4.3. Spread Operator (Spread 연산자)

Spread 연산자(…)는 배열을 다른 내부에 삽입시킨다.

```javascript
 //ES5
 var arr = [1, 2, 3];
 console.log(arr.concat([4, 5, 6])); // [1, 2, 3, 4, 5, 6]
```

```javascript
// ES6
var arr = [1, 2, 3];
console.log([...arr, 4, 5, 6]); //  [1, 2, 3, 4, 5, 6]
```



배열을 함수의 인수로 사용하고 싶은 경우, Function.prototype.apply를 사용하는 것이 일반적이다. 

하지만, Spread 연산자를 사용하면 Function.prototype.apply를 사용할 필요가 없다. 

```javascript
// ES5
function sum() {
  var array = Array.prototype.slice.call(arguments);
  return array.reduce(function(pre, cur) {
    return pre + cur;
  });
}

var arr = [1, 2, 3, 4, 5];

console.log(sum.apply(null, arr));
```

```javascript
// ES6
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur);
}

var arr = [1, 2, 3, 4, 5];

console.log(sum(...arr));
```



