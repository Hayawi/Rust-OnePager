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
