---
title: "Unraveling Rust Strings: A Journey into Complexity"
seoTitle: "Unraveling Rust Strings: A Journey into Complexity"
seoDescription: "Rust handles strings efficiently with UTF-8 encoding. Rust offers two types of strings: &str, a non-owning string slice, and String, an owned, mutable strin"
datePublished: Tue Aug 01 2023 07:12:16 GMT+0000 (Coordinated Universal Time)
cuid: clkryonpq000k09ju2a5051pj
slug: unraveling-rust-strings-a-journey-into-complexity
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690515988267/ed5eef4a-8c05-4da9-aa9d-382011dfec8f.webp
tags: rust, strings

---

Strings in Rust are not as simple as they might be in some other programming languages. Rust takes a unique approach to handling strings, ensuring safety and efficiency.

### Rust stores text in the form of **<mark>UTF-8</mark>** encoded data in strings.

Before delving into the implementation of how strings are used in Rust, it's essential to grasp the concept of UTF-8 encoding.

Languages like C and C++ use **ASCII** (American Standard Code for Information Interchange) to store data in strings. However, one major disadvantage of using ASCII is its limited character set, which includes only 128 different characters, comprising the English alphabet, numbers, and a few special characters.  
At the inception of the World Wide Web, characters from various languages and emojis emerged as part of the online communication landscape and ASCII was enabled to encode them.

UTF-8 is a variable-width encoding scheme for Unicode characters, which includes a vast range of characters from different languages and symbols. The size of each character can vary between 1 byte to 4 bytes.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Some of the prominent languages like Python, JavaScript and Java use UTF-8 encoding for strings.</div>
</div>

---

### Now, let's explore how Rust handles strings in practice.

Surprisingly there are two types of strings in rust: `&str` and `String`.

1. ## `&str`
    
    It is a type that represents a **string slice**. It's a way to refer to a sequence of characters (text) without owning the actual data. Instead, it points to a portion of memory that contains the characters.
    
    ```rust
    let greeting: &str = "Hello, World!";
    
    let word = "Hello";
    //String literals are stored as &str by default
    
    let s = String::from("hello world"); //Actual string allocated on heap memory
    
    //String slices
    let hello = &s[0..5];
    let world = &s[6..11];
    ```
    
    `&str` is commonly used as a function parameter when you want to pass strings without transferring ownership, as it's more efficient than passing `String` by value.
    
    ```rust
    fn print_greeting(greeting: &str) {
        println!("{}", greeting);
    }
    
    fn main() {
        // Create a String
        let my_string = String::from("Hello, Rust!"); 
        // Create a &str from a portion of the String
        let string_slice = &my_string[0..5]; 
        
        print_greeting(string_slice); // Output: "Hello"
    }
    ```
    
    In simple words, `&str` is a pointer that points to a section of the actual string data. Since it's just a reference, you can't change the data it points to, it's read-only.
    
2. ## `String`
    
    It is an **owned** data type used to store UTF-8 encoded characters on the heap. Strings are **growable** and **mutable** so you can add, remove, and modify their content at runtime.
    

```rust
//Creating a new string
let mut s = String::new();//We can load data into the string whenever we want

//Adding data directly using literals
let mut s = String::from("Hello World");

//Another way of storing data into the string
let mut s = "Hello World".to_string();
```

---

Let's see some operations on strings:

### ***Updating the string***

We can use `push_str` or `push`(for a single character) for updating or adding the contents in the string.

```rust
    let mut s1 = String::from("Hello ");
    let s2 = "World"; //String slice
    s1.push_str(s2);
    println!("s1 is {s1}"); //s1 is Hello World
    println!("s2 is {s2}"); //s2 is World
```

Note that in the above example `push_str()` method takes a string slice because we don’t necessarily want to take ownership of the parameter

We can use `+` operator for concatenation of strings.

```rust
let s1 = String::from("Hello");
let s2 = String::from("world");
let s3 = s1 + &s2; // note s1 has been moved here and can no longer be used  println!("s3 is {s3}");//s3 is Hello world  println!("s1 is {s1}");//This will give error because its ownership has been moved to s3
```

We can use `format!` macro for more complicated string combining.

```rust
let s1 = String::from("Lets");
let s2 = String::from("do");
let s3 = String::from("it");

let s = format!("{s1}-{s2}-{s3}"); //It returns a String 

println!{"s is {s}"} //s is Lets-do-it 

println!{"s1 is {s1}"} //s1 is Lets
println!{"s2 is {s2}"} //s1 is do
println!{"s1 is {s3}"} //s1 is it
```

Note that `format!` macro returns a `String` and doesn't take ownership of any of its parameters

### ***Indexing into strings***

Accessing a single character by referencing by it's index is not possible in Rust.

```rust
let variable = String::from("Rust");
let character = s1[0]; // This will give an error
```

Let's dive deeper and understand how strings are stored in Rust. Let's take one example:

```rust
let variable = String::from("Rust");
```

In this case the `len` will be 4 bytes i.e. each character is of 1 byte. But now let's take a different example which includes a character which is not a part of ASCII.

```rust
let variable = String::from("🦀👍");
```

By looking at the above string you might have guessed its `len` to be 2 bytes, but the actual `len` of the string would be 8 bytes(4 bytes each).  
**🦀 -&gt; \[240, 159, 166, 128\]**

**👍 -&gt; \[ 240, 159, 145, 141\]**  
As we had mentioned before in UTF-8 encoding the size of the character can vary from 1 byte to 4 bytes.  
This is the reason indexing is not allowed because it gives us the character of a particular byte itself. In the above example if we try to access the value at index 2, the byte we will get will not be a valid character.

Rather than indexing using `[]` with a single number, you can use `[]` with a range to create a string slice containing particular bytes.

```rust
 let variable = String::from("🦀👍");
 let slice1 = &variable[0..4];
 let slice2 = &variable[4..8];
 println!("slice1 is {}",slice1); // slice is 🦀
 println!("slice2 is {}",slice2); // slice is 👍
```

### *There are various methods for iterating over strings:*

\-&gt;For individual Unicode scalar values, use the `chars` method.

```rust
for x in "🦀👍".chars() {
    println!("{x}");
}
//Output
//🦀
//👍
```

\-&gt;The `bytes` method returns each raw byte.

```rust
for x in "🦀👍".bytes() {
    println!("{x}");
}
//Output
//240
//159
//166
//128
//240
//159
//145
//141
```

---

> I have referred to the official [Rust lang book](https://doc.rust-lang.org/book/ch08-02-strings.html) for writing this blog.

### Summary

In conclusion, Rust handles strings efficiently with UTF-8 encoding. Rust offers two types of strings: `&str`, a non-owning string slice, and `String`, an owned, mutable string. Due to UTF-8's variable character size, direct indexing is disallowed. Instead, string slices are used to access specific byte ranges. The blog covers string operations like updating, concatenation, and iteration using methods like `chars` and `bytes`. Rust's approach ensures safety, efficiency, and broad Unicode support.

**Happy coding!**