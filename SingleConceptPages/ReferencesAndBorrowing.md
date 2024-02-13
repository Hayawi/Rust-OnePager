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
