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
