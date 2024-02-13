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
