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
