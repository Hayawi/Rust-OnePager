#### Closures
**Closures** are used when spawning **threads** or doing **functional** programming with **iterators**. A **Closure** is an anonymous **function** that can **borrow** or capture data from the **scope** that it is nested in. 
* The syntax of a **closure** is `|x, y| { x + y }`, this creates an anonymous **function** called a **closure** that can be called later! The types of the values and the return value are all inferred by how the **closure** is used.
	* A **closure** doesn't have to have any parameters
	* A **closure** will borrow a **reference** to values in the enclosing **scope**
		* This is great if the **closure** does not **outlive** the **variable** it is **referencing**
		* The **compiler** will not let you send a closure that **borrows references** to another **thread**, as that **thread** may live longer than the value that the **closure** is **borrowing**.
			* **Closures** support **move** operations, which allows them to be sent to different **threads**!
```
let add = |x, y| { x + y };
add(1, 2); // returns 3!

let s = "uwu".to_string();
let f = || {
   println!("{}", s);
};

f(); // prints uwu!

let f_m = move || {
   println!("{}", s);
}
return f_m // works!

let mut v = vec![2, 4, 6];
v.iter()
   .map(|x| {x * 3})
   .filter(|x| {*x > 10}) // note the derefence as closures take a reference of x
   .fold(0, |acc, x| {acc + x}); // outputs 30!
```
