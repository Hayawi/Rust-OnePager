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
