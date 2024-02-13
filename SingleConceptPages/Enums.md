#### Enums
**Enums** in **Rust** are like **enums** in other languages, but with additional functionality!
* **Enums** can be created as a block of *predetermined* variants, just like in other languages, however the real power of **enums** in **Rust** comes from associating **data** and **methods** with **enum** variants!
* **Enum** variants can be created to have no associated **data**, a single type of **data**, a **tuple** of **data**, or an anonymous **struct** of data!
* **Enums** can also have **functions** and **methods** that are implemented to function across that **enums** variants!
* Because **enums** can contain so many different types of **data**, patterns need to be used to examine them
	* The **if-let** pattern is good at checking if a variable matches a type of enum variant. If the pattern in the **if-let** matches the type of variable, then the condition in the **if-let** block is executed!
	* If you need to check multiple types of variants, **match expressions** are used.
		* **Match expressions** require you to write a pattern for every variant, although there's a catchall pattern using `_ => {`
		* **Match expressions** can return values, but either all branch arms need to return nothing, or all branch arms need to return the same type
```
enum MeowNum {
   NormalEmptyVariant,
   SingleDataVariant(i32),
   TupleVariant(String, i32),
   StructVariant {x:i32, y:i32},
}

use MeowNum::*;
let item = NormalEmptyVariant;
let single = SingleDataVariant(69);
let tuple_variant = TupleVariant("blaze it".to_string(), 420);
let struct_variant = StructVariant { x: 4, y: 20 };

impl MeowNum {
   fn display(&self) {}
}

enum Option<T> { // An example of the Option enum from the stdlib
   Some(T),
   None,
}

if let Some(x) == my_variable { // The if-let pattern to examine single-type data enums
   println!("value is {}", x)
}

match my_variable { // the match expression checking multiple variants
   Some(x) => {
      println!("value is {}", x);
   },
   None => {
      println!("no value");
   },
   _ => { // default catch all pattern to match anything that's left
      println!("meow");
   }
}

let meow = match my_variable { // the match expression returning a &str
   Some(x) => "meow",
   None => "no meow",
   _ => "lazy meow",
};
```
##### Option\<T\>
* **Option** is a generic **enum** in the **stdlib** that is used in **Rust** all the time!
	* it replaces **null** behavior in other languages!
	* The idea is that **Option** wraps over values that could be **null**. 
		* You either have **Some** value in the **Option**, or you have **None**!
	* Since **Option** and its variant are used so much, they don't need to be imported into a module, and can just be called
	* A **None** variant of an **Option** can be created like this: `let mut x: Option<i32> = None`;
		* This specifically returns a potential **i32** value that is "**null**" in this case.
		* The method `x.is_none();` will return true is x is a **None** variant!
	* A **Some** variant can be created like this: `let mut x = Some(5);`
		* This specifically returns a potential **i32** value that evaluates to 5.
		* Because **Some** has to be supplied a value, you don't need to define the type
		* The method `x.is_some();` will return true is x is a **Some** variant!
	* **Option** also implements the **iterator** **trait**, so it can be used in **for loops** as a **Vector** of 0 or 1 items!
```
let mut x = None;
x.is_none(); //true
x = Some(5);
x.is_some(); // true
for i in x {
   println!("{}", i); // prints 5
}
```
##### Result
* **Result** is another important **enum** in the **stdlib** that is often used to denote a value that may exist or be an **error**. This is used very often in the **IO** module.
* **Ok** and **Err** in **Result** map over two different independent generics! 
* **Result** is annotated by the **must_use** annotation, which means it will **error** out if you silently drop a **Result**!
	* Cannot ignore errors!
* The `is_ok()` and `is_err()` methods can be used to quickly check the variant of **Result**
```
#[must_use]
enum Result<T, E> { // this is what the Result enum looks like in stdlib
   Ok(T),
   Err(E),
}

use std::fs::File;

fn main() {
   File::open("foo"); // because this Result is being dropped, this will error!
   let res = File::open("foo");
   let file = res.unwrap(); // gives file if Result is Ok, crashes if it is Err!
   let file = res.expect("error message!"); // same as unwrap, but prints string in 
										   // crash output!
   if res.is_ok() {
      let file = res.unwrap(); // will never crash because we're checking if it's Ok!
   }

   match res {
      Ok(f) => { /* do stuff with file */},
      Err(e) => { /* do stuff with error */ },
   }
}
```
