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
