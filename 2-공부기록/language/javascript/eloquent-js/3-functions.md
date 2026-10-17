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
- 