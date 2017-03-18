# JavaScript - this

### 함수 호출 패턴에 따라 결정되는 this!!!



- 자바스크립트의 함수는 호출될 때, 매개변수로 전달되는 `인자값` 이외에,` arguments객체`와 `this `를 암묵적으로 전달 받는다. 


- 자바스크립트는 해당 함수 호출 패턴에 따라 this에 바인딩되는 객체가 달라진다.



### # 함수 호출 패턴과 this 바인딩

자바스크립트의 경우 함수 호출 패턴에 따라 어떤 객체를 `this`에 바인딩할 지가 결정된다. 

즉, 함수 호출 패턴에 따라 this의 참조값이 달라진다.

함수 호출 패턴

```

1. 매서드 호출 패턴 (Method Invocation Pattern)
2. 함수 호출 패턴 (Function Invocation Pattern)
3. 생성자 호출 패턴 (Constructor Invocation Pattern)
4. apply 호출 패턴 (Apply Invocation Pattern)

```





### 1. 매서드 호출 패턴 (Method Invocation Pattern)



함수가 `객체`의 속성이면 `매서드 호출패턴`으로 호출된다.

이때, 매서드 내부의 this는 해당 매서드를 소유한 객체 즉 `해당 매서드를 호출한 객체`에 바인딩된다. 

```javascript

var obj1 = {
  name: 'lee',
  sayName: function() {
    console.log(this.name);
  }
}

var obj2 = {
  name = 'kim'
}


obj2.sayName = obj1.sayName;

obj1.sayName();  // lee
obj2.sayName();  // kim


```



프로토타입 객체도 메서드를 가질 수 있다. 프로토타입객체 메서드 내부에서 사용된 this도 일반 메서드방식과 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다.

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function() {
  return this.name;
}

var me = new Person('Lee');
console.log(me.getName());

Person.prototype.name = 'Kim';
console.log(Person.prototype.getName());
```



### 2. 함수 호출 패턴(Function Invovation Pattern)

- 전역객체는 모든 객체의 유일한 최상위 객체를 의미하며 일반적으로 Browser-side에서는 `window`, server-side에서는 `global`객체를 의미한다.

- 전역객체는 전역 스코프(global scope)를 갖게 되며 전역변수(global variable)를 속성으로 가지게 되며 글로벌 영역에 선언한 함수는 전역객체에 속성으로 접근할 수 있는 전역 변수의 메서드이다.

  ```javascript
  var ga = "Global variable";

  console.log(ga);
  console.log(window.ga);

  function foo() {
    console.log("invoked!");
  }
  window.foo();
  ```

   

  기본적으로 `this`는 `전역객체(global object)`에 바인딩된다. 전역함수는 물론이고 심지어 내부함수의 경우도 this는 외부함수가 아닌 전역객체에 바인딩된다.

  ```javascript
  function foo() {
    console.log(this);
    function bar() {
      console.log(this);
    }
    bar(); // Window { ... }
  }
  foo(); // Window { ... }
  ```



또한 매서드의 내부함수일 경우에도  `this`는 전역객체에 바인딩된다.

```javascript
var value = 1;

var obj = {
  value: 100,
  foo: function() {
    console.log("foo's this: ",  this);  // obj
    console.log("foo's this.value: ",  this.value); // 100
    function bar() {
      console.log("bar's this: ",  this); // window
      console.log("bar's this.value: ", this.value); // 1
    }
    bar();
  }
}
obj.foo();
```



이러한 내부함수의 `this`가 전역객체를 참조하는 것을 회피하는 방법은 아래와 같다.

```javascript

var value = 1;

var obj = {
  value: 100,
  foo: function() {
    var that = this;  // Workaround : this === obj

    console.log("foo's this: ",  this);  // obj
    console.log("foo's this.value: ",  this.value); // 100
    function bar() {
      console.log("bar's this: ",  this); // window
      console.log("bar's this.value: ", this.value); // 1

      console.log("bar's that: ",  that); // obj
      console.log("bar's that.value: ", that.value); // 100
    }
    bar();
  }
}
obj.foo();

```

메서드 호출 패턴이든 함수 호출패턴이든 내부함수의 this는 모두 전역객체에 바인딩된다. 이러한 문제를 해소하기 위해 자바스크립트는 this 바인딩을 명시적으로 할 수 있는 call, apply 메서드를 제공한다. 



### 3. 생성자 호출 패턴(Constructor Invocation Pattern)

- 자바와 같은 객체지향 언어의 생성자 함수와는 다르게 그 형식이 정해져 있는 것이 아니라 기존 함수에 new연산자를 붙여서 호출하면 해당 함수는 생성자 함수로 동작한다.
- 일반적으로 함수명은 첫문자를 대문자로 기술하여 혼란을 방지한다. 
- new 연산자와 함께 생성자 함수를 호출하면 this 바인딩이 메서드나 함수 호출 때와는 다르게 동작한다.



### 3.1 생성자 함수 동작 방식

##### 	1. 빈 객체 생성 및 this 바인딩

생성자 함수의 코드가 실행되기 전 빈 객체가 생성된다. 이 빈 객체가 생성자 함수가 새로 생성하는 객체이며 this에 이 객체가 바인딩된다. 이후 생성자 함수 내에서 사용되는 this는 이 빈 객체를 가리킨다. 그리고 생성된 빈 객체는 생성자 함수의 prototype프로퍼티가 가리키는 객체를 자신의 프로토 타임 객체로 설정한다. 

##### 	2. this를 통한 프로퍼티 생성

this를 사용하여 생성된 빈 객체에 동적으로 프로퍼티나 메서드를 생성할 수 있다. this는 새로 생성된 객체를 가리키므로 this를 통해 생성된 프로퍼티와 메서드는 새로 생성된 객체에 추가된다.



##### 	3. 생성된 객체 반환

반환문이 없는 경우, this에 바인딩된 새로 생성한 객체가 반환된다. 명시적으로 this를 반환하여도 결과는 같다.

반환문이 this가 아닌 다른 객체를 반환하는 경우, this가 아닌 해당 객체가 반환된다.

```javascript
var Person = function(name) {
  // 생성자 함수 코드 실행전 -----(1)
  this.name = name; // -----(2)
  // 생성자 함수 반환 ----------(3)
}

var me = new Person('lee');
console.log(me.name);
```



### 3.2. 객체 리터럴 방식과 생성자 함수 방식의 차이



### 3.3. 생성자 함수에 new연산자를 붙이지 않고 호출할 경우



### 4. apply 호출 패턴(Apply Invocation Pattern)







