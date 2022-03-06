# Names, Types and Binding

### Table of content
- [1. Keywords & Reserved Words](#1-keywords--reserved-words)
- [2. Naming requirements for variables](#2-naming-requirements-for-variables)
  - [2.1. Identifier](#21-identifier)
  - [2.2. Naming Conventions](#22-naming-conventions)
- [3. Type System](#3-type-system)
  - [3.1. Typed Language](#31-typed-language)
  - [3.2. Mutability](#32-mutability)
- [4. Data Types](#4-data-types)
  - [4.1. Scalar Types](#41-scalar-types) 
    - [4.1.1. Integer Types](#411-integer-types)
    - [4.1.2. Floating-Point Types](#412-floating-point-types)
    - [4.1.3. Numeric Operations](#413-numeric-operations)
    - [4.1.4. The Boolean Type](#414-the-boolean-type)
    - [4.1.5. The Character Type](#415-the-character-type)
    - [4.1.6. String Type](#416-string-type)
      - [4.1.6.1. String Literal(&str)](#4161-string-literalstr)
      - [4.1.6.2. String Object(String)](#4162-string-objectstring)
  - [4.2. Compound Types](#42-compound-types)
    - [4.2.1. The Tuple Type](#421-the-tuple-type)
    - [4.2.2. The Array Type](#422-the-array-type)
      - [4.2.2.1. Accessing Array Elements](#4221-accessing-array-elements)
      - [4.2.2.2. Invalid Array Element Access](#4222-invalid-array-element-access)
- [5. Coding examples](#5-coding-examples)
- [Reference](#reference)

## 1. Keywords & Reserved Words
### Keywords Currently in Use
The following keywords currently have the functionality described.

- `as` - perform primitive casting, disambiguate the specific trait containing an item, or rename items in use and extern crate statements
- `async` - return a Future instead of blocking the current thread
- `await` - suspend execution until the result of a Future is ready
- `break` - exit a loop immediately
- `const` - define constant items or constant raw pointers
- `continue` - continue to the next loop iteration
- `crate` - link an external crate or a macro variable representing the crate in which the macro is defined
- `dyn` - dynamic dispatch to a trait object
- `else` - fallback for if and if let control flow constructs
- `enum` - define an enumeration
- `extern` - link an external crate, function, or variable
- `false` - Boolean false literal
- `fn` - define a function or the function pointer type
- `for` - loop over items from an iterator, implement a trait, or specify a higher-ranked lifetime
- `if` - branch based on the result of a conditional expression
- `impl` - implement inherent or trait functionality
- `in` - part of for loop syntax
- `let` - bind a variable
- `loop` - loop unconditionally
- `match` - match a value to patterns
- `mod` - define a module
- `move` - make a closure take ownership of all its captures
- `mut` - denote mutability in references, raw pointers, or pattern bindings
- `pub` - denote public visibility in struct fields, impl blocks, or modules
- `ref` - bind by reference
- `return` - return from function
- `Self` - a type alias for the type we are defining or implementing
- `self` - method subject or current module
- `static` - global variable or lifetime lasting the entire program execution
- `struct` - define a structure
- `super` - parent module of the current module
- `trait` - define a trait
- `true` - Boolean true literal
- `type` - define a type alias or associated type
- `union` - define a union and is only a keyword when used in a union declaration
- `unsafe` - denote unsafe code, functions, traits, or implementations
- `use` - bring symbols into scope
- `where` - denote clauses that constrain a type
- `while` - loop conditionally based on the result of an expression

### Keywords Reserved for Future Use
The following keywords do not have any functionality but are reserved by Rust for potential future use.

- `abstract`
- `become`
- `box`
- `do`
- `final`
- `macro`
- `override`
- `priv`
- `try`
- `typeof`
- `unsized`
- `virtual`
- `yield`
## 2. Naming requirements for variables

### 2.1. Identifier

> Lexer:
> IDENTIFIER :
> [a-z A-Z] [a-z A-Z 0-9 _]*
> | _ [a-z A-Z 0-9 _]+

An identifier is any nonempty ASCII string of the following form:

Either

The first character is a letter
The remaining characters are alphanumeric or `_`
Or

The first character is `_`
The identifier is more than one character, `_` alone is not an identifier
The remaining characters are alphanumeric or `_`

### 2.2. Naming Conventions
Basic Rust naming conventions are described in [RFC 430](https://github.com/rust-lang/rfcs/blob/master/text/0430-finalizing-naming-conventions.md).
In general, Rust tends to use UpperCamelCase for "type-level" constructs (types and traits) and snake_case for "value-level" constructs. More precisely:
| Item | Convention |
| ---- | ---------- |
| Crates | `snake_case` (but prefer single word) |
| Modules | `snake_case` |
| Types | `UpperCamelCase` |
| Traits | `UpperCamelCase` |
| Enum variants | `UpperCamelCase` |
| Functions | `snake_case` |
| Methods | `snake_case` |
| General constructors | `new` or `with_more_details` |
| Conversion constructors | `from_some_other_type` |
| Local variables | `snake_case` |
| Static variables | `SCREAMING_SNAKE_CASE` |
| Constant variables | `SCREAMING_SNAKE_CASE` |
| Type parameters | concise `UpperCamelCase`, usually single uppercase letter: `T` |
| Lifetimes | short, lowercase: `'a` |

#### Fine points

In `UpperCamelCase`, acronyms count as one word: use `Uuid` rather than `UUID`.  In `snake_case`, acronyms are lower-cased: `is_xid_start`.

In `UpperCamelCase` names multiple numbers can be separated by a `_` for clarity: `Windows10_1709` instead of `Windows101709`.

In `snake_case` or `SCREAMING_SNAKE_CASE`, a "word" should never consist of a single letter unless it is the last "word". So, we have `btree_map` rather than `b_tree_map`, but `PI_2` rather than `PI2`.

## 3. Type system

### 3.1. Typed Language

Rust is a statically and strongly typed systems programming language. statically means that all types are known at compile-time, strongly means that these types are designed to make it harder to write incorrect programs.

Although Rust is a statically-typed language, the type of a value does not always have to be declared explicitly.

### 3.2. Mutability
Variable bindings are immutable by default, but this can be overridden using the `mut` modifier.
```rust
fn main() {
    let _immutable_binding = 1;
    let mut mutable_binding = 1;

    println!("Before mutation: {}", mutable_binding);

    // Ok
    mutable_binding += 1;

    println!("After mutation: {}", mutable_binding);

    // Error!
    _immutable_binding += 1;
    // FIXME ^ Comment out this line
}
```

Like immutable variables, constants are values that are bound to a name and are not allowed to change, but there are a few differences between constants and variables.
```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

## 4. Data Types

Every value in Rust is of a *certain data* type, which tells Rust what kind of data is being specified so it knows how to work with that data. We’ll look at two data type subsets: scalar and compound.
Rust is a statically typed language, which means that it must know the types of all variables at compile time.

### 4.1. Scalar Types
A scalar type represents a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters

#### 4.1.1. Integer Types
An integer is a number without a fractional component.
| Length |	Signed |	Unsigned |
| ------ | ------- | --------- |
| 8-bit |	`i8` |	`u8` |
| 16-bit |	`i16` |	`u16` |
| 32-bit |	`i32` |	`u32` |
| 64-bit |	`i64` |	`u64` |
| 128-bit |	`i128` |	`u128` |
| arch |	`isize` |	`usize` |

Each variant can be either signed or unsigned and has an explicit size. Signed and unsigned refer to whether it’s possible for the number to be negative

Integer Literals in Rust

| Number literals |	Example |
| --------------- | ------- |
| Decimal |	`98_222` |
| Hex |	`0xff` |
| Octal |	`0o77` |
| Binary |	`0b1111_0000` |
| Byte (u8 only) |	`b'A'` |

#### 4.1.2. Floating-Point Types
Rust also has two primitive types for floating-point numbers, which are numbers with decimal points. Rust’s floating-point types are `f32` and `f64`, which are 32 bits and 64 bits in size, respectively. The default type is `f64` because on modern CPUs it’s roughly the same speed as `f32` but is capable of more precision. All floating-point types are signed.

```rust
fn main() { 
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```
Floating-point numbers are represented according to the IEEE-754 standard. The `f32` type is a single-precision float, and `f64` has double precision.

#### 4.1.3. Numeric Operations
Rust supports the basic mathematical operations you’d expect for all of the number types: addition, subtraction, multiplication, division, and remainder. Integer division rounds down to the nearest integer. The following code shows how you’d use each numeric operation in a let statement:

```rust
fn main() {
  // addition
  let sum = 5 + 10;

  // subtraction
  let difference = 95.5 - 4.3;

  // multiplication
  let product = 4 * 30;

  // division
  let quotient = 56.7 / 32.2;
  let floored = 2 / 3; // Results in 0

  // remainder
  let remainder = 43 % 5;
}
```

Each expression in these statements uses a mathematical operator and evaluates to a single value, which is then bound to a variable.

#### 4.1.4. The Boolean Type
As in most other programming languages, a Boolean type in Rust has two possible values: `true` and `false`. Booleans are one byte in size. The Boolean type in Rust is specified using bool.

```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```

#### 4.1.5. The Character Type
Rust’s char type is the language’s most primitive alphabetic type. Here’s some examples of declaring char values:

```rust
fn main() {
    let c = 'z';
    let z = 'ℤ';
    let heart_eyed_cat = '😻';
}
```
Note that we specify `char` literals with single quotes, as opposed to string literals, which use double quotes. Rust’s `char` type is four bytes in size and represents a Unicode Scalar Value, which means it can represent a lot more than just ASCII. Accented letters; Chinese, Japanese, and Korean characters; emoji; and zero-width spaces are all valid `char` values in Rust. Unicode Scalar Values range from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF` inclusive. However, a “character” isn’t really a concept in Unicode, so your human intuition for what a “character” is may not match up with what a `char` is in Rust.

#### 4.1.6. String Type

The String data type in Rust can be classified into the following
- String Literal(`&str`)
- String Object(`String`)

##### 4.1.6.1. String Literal(&str)
`&str` is a slice (`&[u8]`) that always points to a valid UTF-8 sequence, and can be used to view into a `String`, just like `&[T]` is a view into `Vec<T>`.

String literals (`&str`) are used when the value of a string is known at compile time. String literals are a set of characters, which are hardcoded into a variable. For example, `let company="Tutorials Point"`. String literals are found in module std::str. String literals are also known as string slices.

String literals are static by default. This means that string literals are guaranteed to be valid for the duration of the entire program.

```rust
fn main() {
   let company:&'static str = "TutorialsPoint";
   let location:&'static str = "Hyderabad";
   println!("company is : {} location :{}",company,location);
}
```

>company is : TutorialsPoint location :Hyderabad

##### 4.1.6.2. String Object(String)
A `String` is stored as a vector of bytes (`Vec<u8`>), but guaranteed to always be a valid UTF-8 sequence. `String` is heap allocated, growable and not null terminated.

The String object type is provided in Standard Library. Unlike string literal, the string object type is not a part of the core language. It is defined as public structure in standard library pub struct String. String is a growable collection. It is mutable and UTF-8 encoded type. The String object type can be used to represent string values that are provided at runtime. String object is allocated in the heap.


```rust
fn main(){
   let empty_string = String::new();
   println!("length is {}",empty_string.len()); // creates an empty string

   let content_string = String::from("TutorialsPoint");
   println!("length is {}",content_string.len()); //creates a string with some default value passed as parameter to the from() method
}
```
>length is 0
> 
>length is 14

**Common Methods - String Object**

| Sr.No.| Method  | Signature | Description |
|------ |---------| --------- |-------------| 
| 1	| new() | pub const fn new() → String | Creates a new empty String.| 
| 2	| to_string() | fn to_string(&self) → String | Converts the given value to a String. | 
| 3	| replace()	 | pub fn replace<'a, P>(&'a self, from: P, to: &str) → String| Replaces all matches of a pattern with another string.| 
| 4	| as_str() | pub fn as_str(&self) → &str | Extracts a string slice containing the entire string.| 
| 5	| push() | pub fn push(&mut self, ch: char)	| Appends the given char to the end of this String. | 
| 6	| push_str() | pub fn push_str(&mut self, string: &str)	| Appends a given string slice onto the end of this String.| 
| 7	| len() | pub fn len(&self) → usize	| Returns the length of this String, in bytes. |                                                                                                                                                                          
| 8	| trim() | pub fn trim(&self) → &str | Returns a string slice with leading and trailing whitespace removed. |                                                                                                                                                 
| 9	| split_whitespace() | pub fn split_whitespace(&self) → SplitWhitespace	| Splits a string slice by whitespace and returns an iterator. |                                                                                                                      
| 10	| split() | pub fn split<'a, P>(&'a self, pat: P) → Split<'a, P> , where P is pattern can be &str, char, or a closure that determines the split. | Returns an iterator over substrings of this string slice, separated by characters matched by a pattern.|  
| 11	| chars() | pub fn chars(&self) → Chars	| Returns an iterator over the chars of a string slice. |                                                                                                                                                             

### 4.2. Compound Types
Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.

#### 4.2.1. The Tuple Type

A tuple is a general way of grouping together a number of values with a variety of types into one compound type. Tuples have a fixed length: once declared, they cannot grow or shrink in size.

We create a tuple by writing a comma-separated list of values inside parentheses. Each position in the tuple has a type, and the types of the different values in the tuple don’t have to be the same.

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

The variable `tup` binds to the entire tuple, because a tuple is considered a single compound element. To get the individual values out of a tuple, we can use pattern matching to destructure a tuple value, like this:

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}
```

We can also access a tuple element directly by using a period (`.`) followed by the index of the value we want to access.

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```
The tuple without any values, (), is a special type that has only one value, also written (). The type is called the unit type and the value is called the unit value. Expressions implicitly return the unit value if they don’t return any other value.

#### 4.2.2. The Array Type
Another way to have a collection of multiple values is with an array. Unlike a tuple, every element of an array must have the same type. Unlike arrays in some other languages, arrays in Rust have a *fixed length*.

We write the values in an array as a comma-separated list inside square brackets:

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}
```
An array isn’t as flexible as the vector type, though. A vector is a similar collection type provided by the standard library that is allowed to grow or shrink in size.

However, arrays are more useful when you know the number of elements will not need to change. For example, if you were using the names of the month in a program, you would probably use an array rather than a vector because you know it will always contain 12 elements:
```rust
fn main() {
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
}
```

You write an array’s type using square brackets with the type of each element, a semicolon, and then the number of elements in the array, like so:

```rust
fn main() {
  let a: [i32; 5] = [1, 2, 3, 4, 5];
}
```

Here, `i32` is the type of each element. After the semicolon, the number `5` indicates the array contains five elements.

You can also initialize an array to contain the same value for each element by specifying the initial value, followed by a semicolon, and then the length of the array in square brackets, as shown here:

```rust
fn main() {
let a = [3; 5];
}
```
The array named `a` will contain `5` elements that will all be set to the value `3` initially. This is the same as writing `let a = [3, 3, 3, 3, 3]`; but in a more concise way.

##### 4.2.2.1. Accessing Array Elements
An array is a single chunk of memory of a known, fixed size that can be allocated on the stack. You can access elements of an array using indexing, like this:

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```
In this example, the variable named `first` will get the value `1`, because that is the value at index `[0]` in the array. The variable named `second` will get the value `2` from index `[1]` in the array.

##### 4.2.2.2. Invalid Array Element Access

Let’s see what happens if you try to access an element of an array that is past the end of the array.

```rust
use std::io;

fn main() {
    let a = [1, 2, 3, 4, 5];

    println!("Please enter an array index.");

    let mut index = String::new();

    io::stdin()
        .read_line(&mut index)
        .expect("Failed to read line");

    let index: usize = index
        .trim()
        .parse()
        .expect("Index entered was not a number");

    let element = a[index];

    println!(
        "The value of the element at index {} is: {}",
        index, element
    );
}
```

This code compiles successfully. If you run this code using cargo run and enter 0, 1, 2, 3, or 4, the program will print out the corresponding value at that index in the array. If you instead enter a number past the end of the array, such as 10, you’ll see output like this:

>thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

The program resulted in a runtime error at the point of using an invalid value in the indexing operation. The program exited with an error message and didn’t execute the final println! statement. When you attempt to access an element using indexing, Rust will check that the index you’ve specified is less than the array length. If the index is greater than or equal to the length, Rust will panic. This check has to happen at runtime, especially in this case, because the compiler can’t possibly know what value a user will enter when they run the code later.
## 5. Coding examples

### 5.1. Limitations

>“Rust needs to know how much memory to allocate for any value of a particular type, and all values of a type must use the same amount of memory.”

> Rust is very strict when it comes to polymorphic types. As you’ve seen, there are ways to achieve it, but they don’t feel as straightforward as with other dynamic languages such as Ruby or Python. Sometimes though it’s useful to make one step back and look at the actual problem we’re trying to solve.

#### 5.1.1. How to add integer and float in rust?

```rust
fn main(){
    let x:f32=20.0;        
    let y:i32=10;
    let sum = x + y as f32;
    println!("{}",sum);
}
```

#### 5.1.2. How to store different types?

1. Using Vector
```rust
enum Thing {
    Op(Operator),
    Number(i32),
}

fn main() {
    let mut output: Vec<Thing> = Vec::new();
    let a = 2;
    let b = Operator::Add;
    let c = 3;
    output.push(Thing::Number(a));
    output.push(Thing::Op(b));
    output.push(Thing::Number(c));
}
```

2. Using Hashmap
```rust
use std::collections::HashMap;

enum Value {
    Str(&'static str),
    Int(i32),
}

fn main() {
    let mut map = HashMap::new();

    map.insert("a", Value::Str("1"));
    map.insert("b", Value::Int(2));

    for (key, value) in &map {
        println!("{}: {:?}", key, value);
    }
}
```

#### 5.1.3. How to convert between data types?

1. as
```rust
fn main() {
  let num:u8 = 10;
  let multiplied = times_ten(num as f32);

  println!("{}", multiplied); // 100
}

fn times_ten(val: f32) -> f32 {
  val * 10.0
} 
```
2. From

From in Rust is one of two traits for type conversion
```rust
struct MySpecialNumber(u32);

impl From<u32> for MySpecialNumber {
  fn from(num: u32) -> Self {
    MySpecialNumber(num)
  }
}

fn main() {
  let existing_var = 30;
  let num = MySpecialNumber::from(existing_var);
  println!("{:?}", num); // Prints -> MySpecialNumber(30)
}
```
3. Into
- First way to use Into
  ```rust
  fn main() {
    let existing_var: u32 = 30;
    let num: MySpecialNumber = existing_var.into();
    println!("{:?}", num); // Prints -> MySpecialNumber(30)
  }
  ```
  Arguably this was is more readable. The `existing_var` is being converted to `MySpecialNumber` because it has been added as a type annotation. If that get's removed Rust won't know what type to convert the existing variable to.


- Second way to use Into
  
  For this way we're going back to my `times_ten` function I used above when desiring the as keyword.
  ```rust
  fn main() {
    let num:u8 = 10;
    let multiplied = times_ten(num as f32);
  
    println!("{}", multiplied); // 100
    }
  
  fn times_ten(val: f32) -> f32 {
  val * 10.0
  }
  ```
  Instead of using as we can use the Into trait like so:
  
  ```rust
  fn main() {
    let num:u8 = 10;
    let multiplied = times_ten(num);
  
    println!("{}", multiplied); // 100
  }
  
  fn times_ten<T: Into<f32>>(val: T) -> f32 { 
    val.into() * 10.0 
  }
  ```
  So if you compare the last two code snippets, you'll see we can omit the as keyword and the type to convert to and move that logic to the `times_ten function` in the form of a generic data type `T` and the `Into` trait.

  No matter what type the argument is that is passed into the `times_ten function` it will always be converted fo an `f32` before it is multiplied by `10`.
## Reference
- https://doc.rust-lang.org/stable/book/appendix-01-keywords.html
- https://rust-lang.github.io/api-guidelines/naming.html
- https://www.tutorialspoint.com/rust/rust_string.htm
- https://www.simonewebdesign.it/rust-hashmap-insert-values-multiple-types/
- https://dev.to/richardbray/type-conversion-in-rust-as-from-and-into-493l
- https://web.mit.edu/rust-lang_v1.25/arch/amd64_ubuntu1404/share/doc/rust/html/reference/introduction.html
