#### Compound Types
* **Compound types** gather multiple values of other types into one type, these are **tuple**, **array**
##### tuple
* **tuple** stores multiple values of any type
	* They look like `let x = (1, 3.3, "hello");`
	* They can also look like `let x: (i32, f64, string) = (1, 3.3, "hello");`
	* Can be accessed in two ways
		* **dot syntax** `x.0`, `x.1`, and `x.2`
		* all at once `let (a, b, c) = x;`
	* **tuple** has a maximum arity of 12
##### array
* **array** stores multiple values of the same type
	* They can look like `let x = [1, 2, 3];`
	* They can look like `let x = [0; 3];` 0 being the value, and 3 being the size
	* They can also look like `let x: [u8; 3] = [1, 2, 3];`
	* Can be accessed like `x[0]`, `x[1]`, `x[2]`
	* Has a max size of 32!
	* Lives on the stack by default and are fixed size
	* **Vec** or **slices** of **Vec** are used instead for unfixed size 
