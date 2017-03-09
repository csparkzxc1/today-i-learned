# IIFE

##### (Immediately Invoked Function Expressions)

즉시실행함수



##### - 기본형태

```
(fuction () {
   // do something
  }
)();
```

and more

```
!function() { ... }();
(function() { ... }());
(function() { ... })();
```



- ##### 함수 선언(Declaration)과 표현(Expression)

  - 함수 선언은 미리 자바스크립트의 실행 컨텍스트에 로딩 되어 있으므로 언제든지 호출할수있다.
  - 표현식은 인터프리터가 해당 라인에 도달 하였을 때만 실행된다. 

  ​

```
 함수 정의(선언과 표현)의 차이
 
 foo();
 function foo() {
   alert('foo');
 }
 
 foo();
 var foo = function() {
   alert('foo');
 };
 
 alert(foo); 
 (function foo () {});
 alert(foo);
 
```



### 즉시 실행 함수 표현 작동원리

- 괄호쌍이 익명의 함수를 감싸서 함수 선언을 함수 표현식으로 표현될 수 있다. So, 단순한 익명의 함수를 global 스콮프에 선언하지 않고 어디서든 익명의 함수 표현식을 가질 수 있다.

  ```
  x = function() {};
  (x = function() {});
  변수 x에는 함수의 값이 할당된다. 괄호로 둘러쌓인 함수는 익명의 함수 표현식이 된다.
  ```



```
(showName = function (name) {
  console.log(name || "No Name")
  }
)(); 			 // No Name

showName('csp'); // csp
showName(); 	 // No Name
```



- showName 함수는 선언과 동시에 실행된다.
- But, 익명의 표현식은 나중에 다시 호출할 수 없다. 참조할 방법이 없기 때문에
- 익명의 함수를 괄호안에(group context) 위치시킬 경우, 전체 그룹을 평가(evaluate)하고 값을 리턴한다. 리턴값은 익명합수 자신이다. 



### IIFE는 언제 왜 사용하는가?



##### IIFE를 사용하는 주된 이유는 변수를 *전역 영역(Global Scope)으로 선언하는 것을* 피하기 위해서이다.

### 왜?

- 외부와의 충돌을 방지!
  - 지역 변수를 익명함수로 위치시켜 외부와의 충돌을 방지
- 클로저에서 값의 중복 현상 해결







------









