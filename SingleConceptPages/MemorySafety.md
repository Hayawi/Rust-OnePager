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
