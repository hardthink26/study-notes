## Defining a Function 

- **function** 
	- def: wrapping a piece of program in a value has many uses 
		- a way to structure larger programs 
		- to reduce repetition 
		- to associate names with subprograms 
		- to isolate these subprograms from each other. 
	- parameter to a function behave like regular bindings ,but their initial values are given by the caller of the function, not. the code in the function itself. 

## Bindings and Scopes 
- Each binding has a scope
	- For bindings defined in block, or module called **global** 
	- Bindings created for function parameters or declared inside a function can be referenced only in that function called **local**  bindings. 

- Bindings declared with **let** and **const** are local to the block in which they are declared 
```javascript 
let x = 10 ; // global 
if (true){
let y = 20; //Local to block 
var z = 30 ; //Also global 
}
```
- so in this ex, the code before and after the loop can't see it. 

## Nested Scope
- Blocks and functions can be created inside other blocks and functions producing multiple degress of locality. 
- the set of bindings visible inside o block is determined by the place of that block in the program text. 
	- Each local scope can see all the local scopes that contain it 
	- all scopes can see the global scope .
- This approach to binding visibility is called **lexical scoping**. 

## Functions as Value 
- 핵심 개념
	- j·s에서 함수는 하나의 값(function)으로 취급된다. 
		- Function Value: 호출 가능, 변수에 할당하거나 다른 함수에 전달 가능
		- Binding: 이름과 값을 연결하는 것 
		- First-class function: 함수를 다른 일반적인 값처럼 취급 가능 
	- **함수 자체와 함수를 참조하는 이름은 구분!!!** 
- 함수와 바인딩의 관계 
```javascript
function square(x){
return x * x;
}
let f = square;
console.log(f(5)); // -> 25 
```
- square는 함수 값을 참조하는 이름이다. 
- f = square는 함수를 호출하는 것이 아니라 함수 값을 다른 바인딩에 할당 
- 결과적으로 square롸 f는 동일한 함수 객체를 참조 

- 함수 바인딩의 재할당 
```javascript
let f = function(){
return 10;
};

let g = f ;
f = function(){
return 20
};
console.log(f()) // 20
console.log(g()) // 10 
```
- 원리 
	1. 처음에는 f가 첫 번째 함수 객체를 참조 
	2. g = f를 실행하면 g도 같은 함수 객체를 참조 
	3. f에 새로운 함수 객체를 할당해도 g의 참조도 변경되지 않는다. 
- **따라서 바인딩을 재할당하는 것과 기존 함수 객체를 수정하는 것은 다르다.** 

## Declaration Notation 
- Shorter way to create a function binding 
```javascript
function square(x){
return x * x;
} 
```
- This is a function **declaration**. 
	- the statement defines the binding `square` and points it at the given function.
	- doesn't require a semicolon after the function. 

```javascript
console.log("The future says:", future());

function future(){
return "You'll never have flying cars";
}
```
 - This code works!! 
	 - Function declarations are not part of the regular top-to-bottom flow of control.

## Arrow Functions 
- It uses an arrow(=>) made up of an equal sign and a greater-than character.
```javascript
const roundTo = (n, step) => {
let remainder = n % step;
return n - remainder + (remainder < step / 2 ? 0 : step)};
}; 
```
- parameter 이후 arrow 이후엔 함수 body가 나옴 
	- 이 para가 이 내용들을 만들거야를 의미 

- 만약 인자가 하나라면 괄호 생략 가능, 바디가 single expression이면(괄호 없음) 그 내용이 반환 
```javascript 
const square = x => x *x ; 
```

## The call stack 
- definition 
	- The place where the computer stores the context 
- Every time a function is called, the current context is stored on top of this stack. 
	- so, when a function returns, it removes the top context from the stack and uses that context to continue execution. 
- 주의사항 
	- Storing stack requires space in the computer's memory. 
		- when the stack grows too big => Will Fail!!! 

## Optional Arguments 
```javascript 
function square(x) {return x * x;}
console.log(square(4, true)); 
```
- 이 코드는 작동할까요? 
	- 작동합니다. 자바스크립트는 인자 개수를 정확히 맞추는 것을 신경쓰지 않습니다. 

```javascript 
function roundTo(n, step=1){
let remainder = n % step;
return n - remainder + (remainder < step/2 ? 0: step);
};
```
- 함수가 받는 인자에 = 연산자를 사용 가능합니다. 
	- 의미는 만약 인풋이 없다면 by default로 =1값을 부여합니다. 

## Closure 