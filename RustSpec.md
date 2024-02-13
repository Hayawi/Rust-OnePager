##### Creating a new Rust Project
* `cargo new *project name*`

##### Errors
If you want to understand why an error is being thrown use `rustc --explain *error_code*`

---
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

---
#### Memory Safety
Rust guarantees memory safety at compile time!
* Rust will tell you at compile time is code is unsafe and will fail to compile
```
	let x: i32;
	println!("{}", x); // errors out due to Rust not having null!
```

```
	let mut x: i32;
	if true {
		x = 3; // still doesn't work due to conditionals being not type safe!
	}
	println!("{}", x); // errors out due to Rust not having null!
```

```
	let mut x: i32;
	if true {
		x = 3;
	} else {
		x = 4; // works because of the defaulting!
	}
	println!("{}", x); // does not error, there's a guaranteed value!
```

---
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

---
#### Modules
* `lib.rs` can be created in the root of the `src` directory to create the **libraries** for the project
```
lib.rs
   pub fn greet() {
      println!("hello!");
   }

main.rs
   fn main() {
      *project_name*::greet(); // prints hello!
   }
```
* You can bring a function into **scope** by adding `use` to the top of the **module**!
```
main.rs
   use *project_name*::greet;

   fn main() {
      greet(); // prints hello!
   }
```
* This is how you bring in any library into scope!
* You can use `crates.io` to find new **packages** that aren't in the **std**
* If you find a package you like, you can add it manually in the toml like `rand = 0.6.5`

---
#### Scalar Types
* There are 4 **scalar types**; **integer**, **float**, **boolean**, and **char**
##### integer
* **integer** types can be signed or unsigned, denoted by i or u before the number of bytes
	* **u8**, **u16**, **u32**, **u64**, **u128**, and **usize**
	* **i8**, **i16**, **i32**, **i64**, **i128**, and **isize**
	* **usize** and **isize** are the size of the platforms **pointer** type, it's the size used to index into an **array** or **vector** (for obvious reasons; i.e. **pointer**)
	* defaults are **i32**, because it's the fastest
	* **integers** can be specified in **decimal** (69), **hex** (0xdeadbeef), **octal** (0o77543211), **binary** (0b11110011), and **byte (u8 only)** (b'A')
		* **u8** and **byte** are interchangeable terms in **Rust**
		* underscores can be used for readability: 100_000 == 100000 == 10_0_00_0
	* You can suffix the type at the end of the literal: `let x = 3u16` == `let x: u16 = 3`
		* Useful to pass a literal to a generic **function** that accepts multiple numeric types
		* `let x = 3_u16` == `let x = 3u16`
##### float
* **float** types can be either **f32** or **f64**
	* **f64** is the default as it has more precision (but can be slow)
	* follows **IEEE-754** standard
		* look like 3.14159
		* must have 1 digit before the dot; .1 is invalid, 0.1 is valid
	* You can suffix the type at the end of the literal: `let x = 3.14f32` == `let x: f32 = 3.14`
##### boolean
* **boolean** types can be **true** or **false**
	* **boolean** types are not **integers** so you can't do arithmetic with them
	* defined like `let x: bool = true`
	* can be cast in order to do arithmetic with them `true as u8`
##### character
* **character** is actually a single [[unicode]] value
	* This means it can be english, another language, an accent, or even an emoji! Anything supported under **unicode**.
	* **character** is always 4 bytes (32 bits) which makes an **array** of **characters** a **ucs-4** or **utf-32** **string**
	* **character** types are defined like `let x: char = 'a'` or even `let y: char = '😩'`
	* **character** types are generally unused, as **string** types are **utf-8** and **character** types are not. **string** does not use **character** internally! Usually when you want to deal with a single character, it'll be a **string** and not a **character**

---
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

---
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

---
#### Strings
* There are 6 types of strings in the **Rust** **stdlib**, but we care mostly about two of them; **&str** (a borrowed string slice) and **String** 
##### &str - borrowed string slice
* **&str** is a borrowed string slice. A literal string is *always* going to be a **&str**
	* "hello world!" is of type **&str**
	* often referred to as just a *string*, which is *NOT* the same as a **String**
	* immutable, the data inside **&str** *CANNOT* be modified
	* **&str** has the `to_string()` method available to generate a **String** from it
		* `"hello".to_string();` is an example of **&str** to **String**
* Internally made up of a pointer to some bytes that represent the contents and the length
	* **&str** will be made up of ptr -> ["h", "e", "l", "l", "o"] and len = 5 for "hello"
##### String
* You can create a **String** by passing a **&str** to the `::from()` macro in **String**
	* `let msg = String::from("meow");` creates a **String**
* Internally made up of a pointer to some bytes, the length of the current byte contents, and a capacity that may be higher than the current length
	* **String** *may* be made up of ptr -> ["h", "i', "", ""] and len = 2, and capacity = 4 for "hi"
	* in other words, a **&str** is a subset of a **String**
##### Indexing
* Both **&str** and **String** are valid **utf-8**
* Both **&str** and **String** cannot be indexed by character position
	* i.e. my_string[3] is *INVALID*
* Both **&str** and **String** are **unicode**!
	* Because of this, indexing by the character position doesn't work as **unicode** may not index each character to the exactly one **byte**, depending on what that character is
	* A character can be represented by up to 4 **bytes**! These are called **unicode** **scalars**
	* **diacritics** (accents) are **unicode scalars** that combine with other **unicode scalars** to create a **unicode grapheme**, e.g. a combination of a letter and accent. The **grapheme** is usually what we care about and want to retrieve!
		* **grapheme** decomposes into variable amounts of **scalars** which decompose into variable amounts of **bytes**
* Taking all this into account, there's a few ways to index into a string
	* You can use `.bytes()` to get the **Vector** of **bytes** to iterate over
		* This works fine for simple English in **ASCII** as the **grapheme** size of each letter is known beforehand
	* You can use `.chars()` to get an **iterator** of **unicode scalars**
		* This works fine for **graphemes** that are made up of one **scalar** only
	* You can also use packages, like **unicode-segmentation** which have functions that can handle different **unicode graphemes**
	* If using an **iterator** you can use the `.nth(3)` method to return a position at the **iterator**

---
#### Ownership
* There are 3 rules to ownership
	1. Each value has an owner; there's no value in memory that does not have a variable that owns it
	2. Each value has *ONLY* one owner; i.e. a single value in memory cannot be held by two different variables. No variables may *share* **ownership**, but other variables can **borrow** ownership.
	3. Values get dropped *immediately* when the owner gets out of **scope**.
```
let s1 = String::from("abc"); // s1 is the owner of String "abc"
let s2 = s1; // s2 becomes the new owner of "abc" and s1 is dropped. There is no copy
println!("{}", s1); // this is an error
println!("{}", s2); // this prints "abc"
```
##### Move/Copy
* Owners (i.e. variables) are stored on the stack as **ptr**, **len**, and **capacity**. The **ptr** points to the actual data on the heap. The **len** defines the size of the data. The **capacity** defines the maximum size that the data can reach.
	* When a new variable takes **ownership** of the same piece of data, the **ptr**, **len**, and **capacity** are all copied to the new variable on the stack. The old variable is then *IMMEDIATELY* invalidated. 
##### Clone
* To make an explicit copy of a variable, you would use the `.clone();` function. **Cloning** copies the **len**, and **capacity** just like with a **move**, but importantly it also copies the **heap data** and has the new **ptr** point to the new **heap data**. Thus both variables point to their own data, and the three **ownership** rules are satisfied. 
##### Dropping data
* When a value is **dropped** as a result of going out of **scope**, then three things happen
	1. The **destructor** (if there is one) is called
	2. The data on the **heap** is freed
	3. The **ptr**, **len**, and **capacity** are popped off the **stack**
* As a result, we have no **memory leaks** and no **dangling pointers**!
##### Functions
* When calling a **function** with a **variable** as an argument, the **variable** is **moved** as part of the process. This means we can no longer use that **variable** in the original **function** as it has lost **ownership**!
```
let x = String::from("abc");
do_stuff(x);
println!("{}", x); // this will error as x has lost ownership of the data "abc"!
```

---
#### References and Borrowing
* What if we want a **variable** to be passed into a **function** as an argument and still have that **variable** be usable in the original **scope**
	* By passing in a **reference** of the variable as an argument, the original **variable** in the original **scope** retains **ownership**, while the **function** **borrows** **ownership** without causing the **variable** to get dropped at the end of its **scope**.
	* As a result, the **reference**, not the value gets **moved** into the function, at the end of the **function**, the **reference** goes out of **scope** and is **dropped**, but the original value is unaffected.
```
let x = String::from("abc");
do_stuff(&x);
println!("{}", x); // works at the reference is being passed as an argument to do_stuff!
```
* When a **reference** is created, a **ptr** to the **variable** on the **stack** (i.e. **ptr**, **len**, **capacity**) is created.
##### Lifetimes
* **References** use a concept called **lifetimes**, which essentially mean that **references** must always point to valid data.
* A **reference** cannot point to data that it can outlive, and it can never point to **null**!
* This means **references** will always point to **non-null** valid data.
* **References** default to being **immutable**, even if the **variable** being **referenced** is itself **mutable**!
	* To make a **mutable** **reference**, you must pass in a **mutable reference**.
	* You cannot make a **mutable reference** to an **immutable variable**!
	* Note that we do not need to **dereference** the **variable** before calling `.push();`. This is because the `.` operator in **Rust** auto **dereferences** for us!
```
let mut x = String::from("abc");
do_stuff(&mut x);

fn do_stuff(x: &mut String) {
   x.push("def"); // makes the original value abcdef!
   *x = String::from("Replace"); // we need to dereference when not using . operators!
}
```
* It is important to note, *AT ANY GIVEN TIME* you can either have *ONE* **mutable reference** or *MULTIPLE* **immutable references**.

---
#### Structs
Traditional **Object-Oriented languages** use **classes** to create Object **data structures**. In **Rust**, **Structs** are used instead
* **Structs** can have **data fields**, **methods**, and **associated functions**
* **Associated functions** and **methods** are defined in the **impl** block and can be called using either an instance of the **struct** or the **struct's** namespace
	* An **associated function** differs from a **method** in that it does not have **Self** as a parameter of the **function**
		* **Associated functions** are also accessed using the `StructName::func_name();` style
	* A **method** includes **self** or **&self** (i.e. an instance of the **struct**) as a parameter.
		* **Methods** are called using traditional **dot syntax** `struct_instance.method_name();`
```
struct FooBar {
   x: i32,
   text: String,
}

let foo_bar = FooBar {
   x: 5,
   text: "hello".to_string(),
}; // every single field in a struct needs to be populated when creating a new struct

impl FooBar { // the impl block is when functions and methods are defined
   // associated function
   fn new() -> Self {
      Self {
         x: 5,
         text: "hello".to_string(),
      }
   }

   // methods
   fn move(self)...
   fn borrow(&self)...
   fn mut_borrow(&mut self)...
} // this is how you create functions linked to structs (this is a constructor)

let foo_bar = FooBar::new();
foo_bar.move();
```

---
#### Traits
**Rust** does not have the **concept** of **inheritance**! **Traits** are used instead and are similar to **interfaces** in other languages
* **Rust** takes the **composition** over **inheritance** approach
* **Traits** define required behaviour, i.e. **functions** and **methods** that a **struct** *MUST* implement if it wants to have that **trait**
	* **Data fields** cannot be defined as part of **traits**! The workaround is using **setter** and **getter** **methods** instead!
* **Traits** are useful because we can write **generic functions** that target **traits** and not **structs** 
* **Traits** can be implemented on any **struct**, even unowned existing ones!
```
struct FooBar {
   x: i32,
   text: String,
}

trait Cute {
   fn get_meow(&self) -> &str;
}

impl Cute for FooBar { // FooBar now has the trait 'Cute' as it implements it
   fn get_meow(&self) -> &str { "meow" }
}

fn print_noise<T: Cute>(item: T) { // here the method is acting upon the generic trait
   println!("{}", item.get_meow());
}

impl Cute for i32 { // traits can even be implemented for existing unowned structs!
   fn get_meow(&self) -> &str { "m30w" }
}
```
* Unique existing **traits** can also be implemented for **structs** in order to give them that behaviour
	* **Copy** is a **trait** that can be implemented for a **struct** if you want to have values for that **struct** copied instead of moved!
		* The **Copy** trait makes sense for small data values that fit on the **stack**, which is why simple primitive **variable** types like **integers**, **booleans**, **floats**, **characters** implement it! If a type uses the **heap**, it cannot implement **Copy**
		* A **struct** can only implement **Copy** if it only uses **data fields** that also implement the **Copy trait**!
##### Trait Inheritance
* **Traits** actually implement **inheritance**, i.e. **traits** can implement other **traits**!
* **Traits** can also have default behaviors, if you want to make a **trait** that **structs** can use without having to implement any behaviour!
```
trait NoDefaultBehaviour {
   fn meow(&self);
}

trait DefaultBehaviour {
   fn meow(&self) {
      println!("meow");
   }
}

struct FooBar {}
impl DefaultBehaviour for FooBar {} // has access to meow even though it doesn't  
								   // implement the behaviour itself!
```

---
#### Collections
**Collections** are **data structures** in the **stdlib** that hold a number of values, these include **Vector\<T\>**, **HashMap<K, V>**, **VecDeque**, **HashSet**, **BTreeMap**, **LinkedList**, **BinaryHeap**, **BTreeSet**
* We'll only focus on the most used ones here
##### Vector\<T\>
* **Vector** is the most commonly used **collection**. It is used to hold values of a single type and is similar to **Lists** in other languages!
* You can create a **Vector** by specifying the type of value that it will store inside of it
	* You can also create a **Vector** from the values that you will store inside of it using the **vec!** macro!
* Since **Vectors** store objects of known size inside of memory, it can be indexed into!
	* **Rust** will **panic** if your index is out of bounds!
```
let mut v: Vec<i32> = Vec::new();
v.push(2);
v.push(4);
v.push(6);
let x = v.pop(); // x = 6!
println!("{}", v[1]); // prints 4!

let mut v2 = vec![2, 4, 6];
```
##### HashMap<K,V>
* **HashMap** is a collection where you specify the type for the **key** and the type for the **value** and you access the **value** by **key**!
```
let mut map: HashMap<&str, i32> = HashMap::new();
map.insert("hi", 5);
map.insert("bye", 6);
let hi_value = map.remove("hi").unwrap();
```
##### Other Collections Quickly
* **VecDeque** uses a **ring buffer** to implement a **double-ended queue** which makes adding items to the front and back real quick, but everything else slower than a normal **Vec**
* **LinkedList** is very fast at adding or removing items at arbitrary indices in the list, but slower at anything else
* **HashSet** is a **hashing** implementation of a **set** that performs **set** operations really efficiently!
* **BinaryHeap** is like a **priority queue** that always pops off the max value!
* **BTreeMap** and **BTreeSet** are alternate **binary tree** implementations of **Map** and **Set**
	* Usually only used over the normal variants if you need the **map keys** or **set values** to always be *sorted*!

---
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

---
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

#### Threads
**Rust threading** is portable, so **threads** should work across **Mac**, **Linux**, and **Windows**, and any other platform!
* When creating a **thread**, the **spawn** function takes a **closure** with no arguments. The **closure** is executed as the main **function** of the **thread**! Commonly different **functions** are called from this **thread closure** for brevity.
* The **spawn** function returns a **thread handle**. The **handle** can be used to call `join()` which will pause the **thread** that **handle** is being called from, until the child **thread** has completed and exited.
	* The **thread** that gets **joined** can have an **error** that causes it to **panic**, or it could have a **result** that we are interested in, so `join()` returns a **Result** that wraps a possible success value returned from the **thread**, or an **error.
```
use std::thread;

fn main() {
   let handle = thread::spawn(move || {
      // do stuff in a child thread
   });

   // do stuff simultaneously in the main thread

   handle.join().unwrap(); // wait until thread has exited
}
```
* **Threading** is a bit heavy-weight, as creating a new **thread** will allocate an **OS**-dependent amount of **RAM** that becomes the **threads** own **stack** (a couple of **megabytes** or so).
* The **CPU** has to do an expensive **context-switch** when switching from running one **thread** to another. The more **threads** share a **CPU** core, the more overhead you will have to deal with when **content-switching**.
* **Threading** is useful when you need to have different streams of work simultaneously using **memory** and **CPU**, however if you just need to wait for **OS** or **IO** operations, **async await** might be the better choice to avoid **context-switching**!
