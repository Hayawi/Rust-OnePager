#### Functions
* functions are defined using `fn` keyword, pronounced *fun*
* functions are defined in lower snake case
```
fn foo_func() {
	do_stuff();
}
```
* function parameters are defined by name: type with returns specified before the brace with an arrow. **tuple** returns are valid!
```
fn bar(x: i32, y: string) -> (f64, string) {
	do stuff();
}
```
* You can return a value using **return** or just doing a **tail expression**
```
fn foo_bar() -> i8 {
	4 - 2 // this is a tail expression (an expression at the end with no ;)
}
```
* A single rust function does not support variable number of argument of different types for the same argument, but [[macros]] do!
