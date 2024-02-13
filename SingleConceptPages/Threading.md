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
