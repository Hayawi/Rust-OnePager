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
