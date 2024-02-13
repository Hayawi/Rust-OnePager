#### Variables
Rust is a strongly typed braced language (; terminated). 
* **This is how to declare new variables** `let foo = 2;`
Notice that even though it is **strongly typed**, the compiler can figure out the type of the variable.
* **You can initialize a variable with a specific type like this**`let foo: i32 = 2;` 
* **You can initialize multiple variables like this in a tuple** `let (foo, bar) = (6, 9);`
Variables are **immutable** by default in Rust; this is for safety, concurrency, and speed
* **To make a mutable variable** `let mut foo = 3;`
##### const
You can declare a **const** in order to create an even more immutable variable
* `const FOO_BAR: f64 = 9.9;`
	* use **const** instead of **let**
	* capital snake case
	* type
	* value must be determinable at compile time
		* there's a number of [[stdlib]] functions that actually compile to **const** at compile time
* **const** variables are immutable global
* **const** variables are very fast
Variables have a [[scope]] for where and until when they can be used
* **scope** is defined by the block that the variable is created under
``` 
	{
	   let mut y = 99;
	   {
	      let mut x = 9;
	      y = 2; //legal
	   }
	   x = 3; //illegal
	}
```
* There's no garbage collector, instead, values are immediately dropped at the end of **scope**
* Variables can be **shadowed** (i.e. they are local to their **scope**)
	* The value of the original variable isn't accessible while it's being **shadowed**
```
	{
	   let x = 2; 
	   {
	      let x = 3; // no error even though x not mut; this is a different x
	      println!("{}", x); // prints 3
	   }
	   println!("{}", x); //prints 2
	}
```

```
	{
	   let mut x = 2;
	   let x = x; // this shadowing redefines the variable in the same scope to not mut
	   println!("{}", x); //prints 2
	   let y = 3;
	   let y = "hello"; // this shadowing redefines type!
	   println!("{}", y); // prints hello
	}
```
