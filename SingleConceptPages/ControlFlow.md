#### Control Flow
* There are multiple types of **Control Flows** in **Rust**. These are **if expressions**, **loops**, **while**, and **for** 
##### if expressions
* **if expressions** in **Rust** don't use parenthesis, as everything after the if and before the block are part of the expression
	* The condition *Must* evaluate to a **boolean** as **Rust** does not like type coercion 
	* **if expressions** are actually **expressions**, *Not* **statements**. The difference being **statements** don't return values, but **expressions** do! This means the entire **if expressions** can be assigned to a **variable**!
		* You can do this by using **tail expressions** (i.e. no ; after the value in the if). You can't use **return** as it's only used for function blocks
```
if x == y && y < 0 && true {
   foo();
} else if false {
   println!("hello");
} else {
   bar();
}

let msg = if num < 5 {
   "five"
} else {
   "four"
}; 
println("{}", msg); // prints five or four depending!

let num = if a { "hi" } else { "hello" }; Replaces a ? "hi" : "hello" turnary
```
##### unconditional loops
* **loops** in **Rust** are unconditional
	* unconditional **loops** can use **break** and **continue** in order to manually perform **loop** operations
		* if you want to **break** out of a nested **loop**, you can use tick identifiers to do so. Can also use this with **continue**
```
         'bob: loop {
            loop {
               break 'bob; // this breaks out of the outermost loop!
            }
         }

         'bob: loop {
            loop {
               continue 'bob; // iterates the outermost loop!
            }
         }
```

##### while loops
* **while** loops in **Rust** are the conditional method of iteration!
	* **while** loops specify conditions and will terminate when this condition evaluates to false. Otherwise they have all the same behavior as normal **loops**
	* there is no "do while" in **Rust** (i.e. condition evaluation after iteration), but you can achieve this with the **loop** construct and a break statement at the end of the block
```
      while condition() {
         // do stuff
      }

      loop { // this loop is the same as the above while, just uglier
         if !condition() { break }
         // do stuff
      }

      loop { // the equivelant of a do while loop!
         // do stuff
         if !condition() { break }
      }
```
##### for loops
* **for** loops in **Rust** iterate over an iterable value
	* most **compound types** actually have ways to get an iterable value, usually the `iter()` method. The iterator used determines which items are returned and the order they are returned in.
	* the **for** loop can actually take a pattern to destructure the items it receives from the `iter()` method!
	* ranges in **for** loops are also available!
```
      for num in [7, 8, 9].iter() {
	     println!("{}", num); // prints 7 8 9
      }	

      for (x, y) in [(1, 2), (3, 4)].iter() { // destructuring
	     // do stuff
      }

      for num in 0..50 { // range 0 inclusive to 50 exclusive
                         // using 0..=50 makes both inclusive
         // do stuff
      }
```
