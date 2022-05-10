# Objects and Classes

### Table of content
- [1. Characteristics of Object-Oriented Languages](#1-characteristics-of-object-oriented-languages)
  - [1.1. Objects Contain Data and Behavior](#11-objects-contain-data-and-behavior)
  - [1.2. Encapsulation that Hides Implementation Details](#12-encapsulation-that-hides-implementation-details)
  - [1.3. Inheritance and Polymorphism](#13-inheritance-and-polymorphism)
- [2. The Rust Standard Library](#2-the-rust-standard-library)
- [3. Object Oriented](#3-object-oriented)
  - [3.1. Structs](#31-structs)
  - [3.2. Method](#32-method)
  - [3.3. Enums](#33-enums)
  - [3.4. Trait](#34-trait)
- [4. Project Management](#4-project-management)
  - [4.1. Packages and Crates](#41-packages-and-crates)
  - [4.2. Module](#42-module)
  - [4.3. Path](#43-path)
  - [4.4. Separating Modules into Different Files](#)
- [Reference](#reference)


## 1. Characteristics of Object-Oriented Languages
Rust is influenced by many programming paradigms, including OOP

### 1.1. Objects Contain Data and Behavior

> Object-oriented programs are made up of objects. An object packages both data and the procedures that operate on that data. The procedures are typically called methods or operations.

Using this definition, Rust is object oriented: `structs` and `enums` have data, and `impl` blocks provide _methods_ on `structs` and `enums`. Even though `structs` and `enums` with methods aren’t called objects, they provide the same functionality.

### 1.2. Encapsulation that Hides Implementation Details

Another aspect commonly associated with OOP is the idea of **encapsulation**, which means that the implementation details of an object aren’t accessible to code using that object. Therefore, the only way to interact with an object is through its public API; code using the object shouldn’t be able to reach into the object’s internals and change data or behavior directly. This enables the programmer to change and refactor an object’s internals without needing to change the code that uses the object.

we can use the `pub` keyword to decide which modules, types, functions, and methods in our code should be public, and by default everything else is private.

```rust
pub struct AveragedCollection {
    list: Vec<i32>,
    average: f64,
}
```

The struct is marked `pub` so that other code can use it, but the fields within the struct remain **private**. This is important in this case because we want to ensure that whenever a value is added or removed from the list, the average is also updated. We do this by implementing `add`, `remove`, and `average` methods on the struct.

```rust
impl AveragedCollection {
    pub fn add(&mut self, value: i32) {
        self.list.push(value);
        self.update_average();
    }

    pub fn remove(&mut self) -> Option<i32> {
        let result = self.list.pop();
        match result {
            Some(value) => {
                self.update_average();
                Some(value)
            }
            None => None,
        }
    }

    pub fn average(&self) -> f64 {
        self.average
    }

    fn update_average(&mut self) {
        let total: i32 = self.list.iter().sum();
        self.average = total as f64 / self.list.len() as f64;
    }
}
```

The public methods `add`, `remove`, and `average` are the only ways to access or modify data in an instance of `AveragedCollection`. When an item is added to list using the `add` method or removed using the `remove` method, the implementations of each call the private `update_average` method that handles updating the `average` field as well.

### 1.3. Inheritance and Polymorphism

>_Inheritance_ is a mechanism whereby an object can inherit from another object’s definition, thus gaining the parent object’s data and behavior without you having to define them again.

Considering inheritance, the Rust programming language has no way of specifying that a struct inherits attributes or functionality from another one. So in this sense, Rust is not an object oriented programming language. However there are other solutions that the Rust language offers that are similar to (but not quite) inheritance, for example, Traits.

>A trait tells the Rust compiler about functionality a particular type has and can share with other types. We can use traits to define shared behavior in an abstract way. We can use trait bounds to specify that a generic type can be any type that has certain behavior.

Note: Traits are similar to a feature often called interfaces in other languages, although with some differences.

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

This is an example of a Trait. Just like an interface, any class or structure in the case of Rust that uses this, MUST implement it somewhere in it's own definition. A trait can have multiple methods in it's body, and anyone using it must implement them all.

The implementation would look something like this:

```rust
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}
```

Although sometimes it's useful or necessary to have some default implementation for these. Which is possible.

```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}
```

If we want to use the default implementation, we would have an empty impl block.

To many people, polymorphism is synonymous with inheritance. But it’s actually a more general concept that refers to code that can work with data of multiple types. For inheritance, those types are generally subclasses.

Rust instead uses generics to abstract over different possible types and trait bounds to impose constraints on what those types must provide. This is sometimes called bounded parametric polymorphism.

To be able to explain thoroughly and well what these are, I'd have to first explain a few other topics more specific about Rust's syntax and way of doing things, so for now, know that it's possible to have some form of polymorphism in Rust, just not the way classical OOP languages do it.

**Overloading:** Rust does not support traditional overloading where the same method is defined with multiple signatures. But traits provide much of the benefit of overloading: if a method is defined generically over a trait, it can be called with any type implementing that trait. Compared to traditional overloading, this has two advantages. 
- First, it means the overloading is less ad hoc: once you understand a trait, you immediately understand the overloading pattern of any APIs using it. 
- Second, it is extensible: you can effectively provide new overloads downstream from a method by providing new trait implementations.

## 2. The Rust Standard Library
The Rust Standard Library is the foundation of portable Rust software, a set of minimal and battle-tested shared abstractions for the broader Rust ecosystem

`std` is available to all Rust crates by default. Therefore, the standard library can be accessed in `use` statements through the path `std`, as in use `std::env`.

First of all, The Rust Standard Library is divided into a number of focused modules. These modules are the bedrock upon which all of Rust is forged, and they have mighty names like `std::slice` and `std::cmp`. Modules' documentation typically includes an overview of the module along with examples, and are a smart place to start familiarizing yourself with the library.

| Module      | uses                                                                                                                                          |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| alloc 	     | Memory allocation APIs                                                                                                                        |
| any 	       | This module implements the Any trait, which enables dynamic typing of any 'static type through runtime reflection.                            |
| array 	     | Implementations of things like Eq for fixed-length arrays up to a certain length. Eventually, we should be able to generalize to all lengths. |
| ascii 	     | Operations on ASCII strings and characters.                                                                                                   |
| char        | A character type.                                                                                                                             |
| clone       | The Clone trait for types that cannot be 'implicitly copied'.                                                                                 |
| cmp         | Functionality for ordering and comparison.                                                                                                    |
| collections | Collection types.                                                                                                                             |
| convert     | Traits for conversions between types.                                                                                                         |
| default     | The `Default` trait for types which may have meaningful default values.                                                                       |
| error       | Traits for working with Errors.                                                                                                               |
| f32         | This module provides constants which are specific to the implementation of the f32 floating point data type.                                  |
| i32         | The 32-bit signed integer type.                                                                                                               |
| num         | Additional functionality for numerics.                                                                                                        |
| str         | Unicode string slices.                                                                                                                        |
| string      | A UTF-8 encoded, growable string.                                                                                                             |
| slice       | A dynamically-sized view into a contiguous sequence, `[T]`.                                                                                   |
| vec         | A contiguous growable array type with heap-allocated contents, written Vec<T>.                                                                |

Second, implicit methods on [primitive types](https://github.com/annguyen200011/CS308_Rust/blob/main/Names%2C%20Types%20and%20Binding.md). This can be a source of confusion for two reasons:

1. While primitives are implemented by the compiler, the standard library implements methods directly on the primitive types (and it is the only library that does so), which are documented in the section on primitives.
2. The standard library exports many modules with the same name as primitive types. These define additional items related to the primitive type, but not the all-important methods.

Examples:
```rust
// deprecated way
let min = std::i32::MIN;

// intended way
let min = i32::MIN;
```

_Trait std::string::ToString_
```rust
pub trait ToString {
  fn to_string(&self) -> String;
}
```
```rust
let i = 5;
let five = String::from("5");

assert_eq!(five, i.to_string());
```

Third, the standard library defines The Rust Prelude, a small collection of items - mostly traits - that are imported into every module of every crate. The traits in the prelude are pervasive, making the prelude documentation a good entry point to learning about the library.

And finally, the standard library exports a number of standard macros (technically, not all of the standard macros are defined by the standard library - some are defined by the compiler - but they are documented here the same). Like the prelude, the standard macros are imported by default into all crates.

| Macros          | uses                                                                              |
|-----------------|-----------------------------------------------------------------------------------|
| assert          | Asserts that a boolean expression is true at runtime.                             |
| assert_eq       | Asserts that two expressions are equal to each other (using PartialEq).           |
| assert_ne       | Asserts that two expressions are not equal to each other (using PartialEq).       |
| cfg             | Evaluates boolean combinations of configuration flags at compile-time.            |
| column          | Expands to the column number at which it was invoked.                             |
| compile_error   | Causes compilation to fail with the given error message when encountered.         |
| concat          | Concatenates literals into a static string slice.                                 |
| dbg             | Prints and returns the value of a given expression for quick and dirty debugging. |
| debug_assert    | Asserts that a boolean expression is true at runtime.                             |
| debug_assert_eq | Asserts that two expressions are equal to each other.                             |
| debug_assert_ne | Asserts that two expressions are not equal to each other.                         |
| env             | Inspects an environment variable at compile time.                                 |
| panic           | Panics the current thread.                                                        |
| print           | Prints to the standard output.                                                    |
| println         | Prints to the standard output, with a newline.                                    |
| todo            | Indicates unfinished code.                                                        |
| try 	           | `Deprecated` Unwraps a result or propagates its error.                            |
| include         | Parses a file as an expression or an item according to the context.               |
| include_bytes   | Includes a file as a reference to a byte array.                                   |
| include_str     | Includes a UTF-8 encoded file as a string.                                        |


## 3. Object Oriented

### 3.1. Structs
A struct, or structure, is a custom data type that lets you package together and name multiple related values that make up a meaningful group. 

Unlike with tuples, in a struct you’ll name each piece of data so it’s clear what the values mean. Adding these names means that structs are more flexible than tuples: you don’t have to rely on the order of the data to specify or access the values of an instance.

To define a struct, we enter the keyword `struct` and name the entire struct. A struct’s name should describe the significance of the pieces of data being grouped together. Then, inside curly brackets, we define the names and types of the pieces of data, which we call fields.

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

To use a struct after we’ve defined it, we create an instance of that struct by specifying concrete values for each of the fields. We create an instance by stating the name of the struct and then add curly brackets containing key: value pairs, where the keys are the names of the fields and the values are the data we want to store in those fields.

```rust
fn main() {
    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };
}
```

To get a specific value from a struct, we use dot notation. If we wanted just this user’s email address, we could use `user1.email` wherever we wanted to use this value. If the instance is mutable, we can change a value by using the dot notation and assigning into a particular field.

Note that the entire instance must be mutable; Rust doesn’t allow us to mark only certain fields as mutable. As with any expression, we can construct a new instance of the struct as the last expression in the function body to implicitly return that new instance.

```rust
fn build_user(email: String, username: String) -> User {
    User {
        email: email,
        username: username,
        active: true,
        sign_in_count: 1,
    }
}
```

Example

```rust
fn main() {
    let width1 = 30;
    let height1 = 50;

    println!(
        "The area of the rectangle is {} square pixels.",
        area(width1, height1)
    );
}

fn area(width: u32, height: u32) -> u32 {
    width * height
}
```

### 3.2. Method

Methods are similar to functions: we declare them with the `fn` keyword and a name, they can have parameters and a return value, and they contain some code that’s run when the method is called from somewhere else. Unlike functions, methods are defined within the context of a struct (or an enum or a trait object), and their first parameter is always `self`, which represents the instance of the struct the method is being called on.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

To define the function within the context of `Rectangle`, we start an `impl` (implementation) block for `Rectangle`. Everything within this `impl` block will be associated with the `Rectangle` type. Then we move the area function within the `impl` curly brackets and change the first (and in this case, only) parameter to be `self` in the signature and everywhere within the body. In `main`, where we called the `area` function and passed `rect1` as an argument, we can instead use method syntax to call the `area` method on our `Rectangle` instance. The method syntax goes after an instance: we add a dot followed by the method name, parentheses, and any arguments.

In the signature for `area`, we use `&self` instead of `rectangle  : &Rectangle`. The `&self` is actually short for `self: &Self`. Within an `impl` block, the type `Self` is an alias for the type that the `impl` block is for. Methods must have a parameter named `self` of type `Self` for their first parameter, so Rust lets you abbreviate this with only the name `self` in the first parameter spot. Note that we still need to use the `&` in front of the `self` shorthand to indicate this method borrows the `Self` instance, just as we did in `rectangle: &Rectangle`. Methods can take ownership of `self`, borrow `self` immutably as we’ve done here, or borrow `self` mutably, just as they can any other parameter.

The main reason for using methods instead of functions, in addition to providing method syntax and not having to repeat the type of `self` in every method’s signature, is for organization. We’ve put all the things we can do with an instance of a type in one `impl` block rather than making future users of our code search for capabilities of `Rectangle` in various places in the library we provide.

```rust
impl Rectangle {
    fn width(&self) -> bool {
        self.width > 0
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    if rect1.width() {
        println!("The rectangle has a nonzero width; it is {}", rect1.width);
    }
}
```

### 3.3. Enums

Enums are a way of defining custom data types in a different way than you do with structs.

Example: Say we need to work with IP addresses. Currently, two major standards are used for IP addresses: version four and version six. Because these are the only possibilities for an IP address that our program will come across, we can enumerate all possible variants, which is where enumeration gets its name.

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

We can create instances of each of the two variants of IpAddrKind like this:

```rust
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;
```

```rust
    enum IpAddrKind {
        V4,
        V6,
    }

    struct IpAddr {
        kind: IpAddrKind,
        address: String,
    }

    let home = IpAddr {
        kind: IpAddrKind::V4,
        address: String::from("127.0.0.1"),
    };

    let loopback = IpAddr {
        kind: IpAddrKind::V6,
        address: String::from("::1"),
    };
```
This one has a wide variety of types embedded in its variants.
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```
If we use struct:
```rust
struct QuitMessage; // unit struct
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // tuple struct
struct ChangeColorMessage(i32, i32, i32); // tuple struct
```

### 3.4. Trait

A type’s behavior consists of the methods we can call on that type. Different types share the same behavior if we can call the same methods on all of those types. Trait definitions are a way to group method signatures together to define a set of behaviors necessary to accomplish some purpose.

```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

Now that we’ve defined the desired signatures of the Summary trait’s methods, we can implement it on the types in our media aggregator.

```rust
pub struct NewsArticle {
    pub headline: String,
    pub location: String,
    pub author: String,
    pub content: String,
}

impl Summary for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}, by {} ({})", self.headline, self.author, self.location)
    }
}

pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summary for Tweet {
    fn summarize(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
```

Implementing a trait on a type is similar to implementing regular methods. The difference is that after `impl`, we put the trait name that we want to implement, then use the `for` keyword, and then specify the name of the type we want to implement the trait for. Within the `impl` block, we put the method signatures that the trait definition has defined. Instead of adding a semicolon after each signature, we use curly brackets and fill in the method body with the specific behavior that we want the methods of the trait to have for the particular type.

```rust
use aggregator::{Summary, Tweet};

fn main() {
    let tweet = Tweet {
        username: String::from("horse_ebooks"),
        content: String::from(
            "of course, as you probably already know, people",
        ),
        reply: false,
        retweet: false,
    };

    println!("1 new tweet: {}", tweet.summarize());
}
```

Sometimes it’s useful to have default behavior for some or all of the methods in a trait instead of requiring implementations for all methods on every type.

For example, we could define the `Summary` trait to have a `summarize_author` method whose implementation is required, and then define a `summarize` method that has a default implementation that calls the `summarize_author` method:

```rust
pub trait Summary {
    fn summarize_author(&self) -> String;

    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}
```
To use this version of Summary, we only need to define summarize_author when we implement the trait on a type:

```rust
impl Summary for Tweet {
    fn summarize_author(&self) -> String {
        format!("@{}", self.username)
    }
}
```

## 4. Project Management


### 4.1. Packages and Crates

The first parts of the module system we’ll cover are packages and crates. A crate is a binary or library. The crate root is a source file that the Rust compiler starts from and makes up the root module of your crate

A package is one or more crates that provide a set of functionality. A package contains a Cargo.toml file that describes how to build those crates.

Several rules determine what a package can contain. A package can contain at most one library crate. It can contain as many binary crates as you’d like, but it must contain at least one crate (either library or binary).

### 4.2. Module

Modules let us organize code within a crate into groups for readability and easy reuse. Modules also control the privacy of items, which is whether an item can be used by outside code (public) or is an internal implementation detail and not available for outside use (private).

Example: In the restaurant industry, some parts of a restaurant are referred to as front of house and others as back of house. Front of house is where customers are; this is where hosts seat customers, servers take orders and payment, and bartenders make drinks. Back of house is where the chefs and cooks work in the kitchen, dishwashers clean up, and managers do administrative work.

To structure our crate in the same way that a real restaurant works, we can organize the functions into nested modules. Create a new library named `restaurant` by running `cargo new --lib restaurant`; then put the code into src/lib.rs to define some modules and function signatures.

Filename: src/lib.rs

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}

        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}

        fn serve_order() {}

        fn take_payment() {}
    }
}
```

We define a module by starting with the `mod` keyword and then specify the name of the module (in this case, `front_of_house`) and place curly brackets around the body of the module. Inside modules, we can have other modules, as in this case with the modules `hosting` and `serving`. Modules can also hold definitions for other items, such as structs, enums, constants, traits

Earlier, we mentioned that src/main.rs and src/lib.rs are called crate roots. The reason for their name is that the contents of either of these two files form a module named crate at the root of the crate’s module structure, known as the module tree.

```
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```

The tree also shows that some modules are siblings to each other, meaning they’re defined in the same module (hosting and serving are defined within front_of_house). To continue the family metaphor, if module A is contained inside module B, we say that module A is the child of module B and that module B is the parent of module A. Notice that the entire module tree is rooted under the implicit module named crate.


### 4.3. Path

To show Rust where to find an item in a module tree, we use a path in the same way we use a path when navigating a filesystem. If we want to call a function, we need to know its path.

A path can take two forms:
- An absolute path starts from a crate root by using a `crate` name or a literal crate.
- A relative path starts from the current module and uses `self`, `super`, or an identifier in the current module.

Both absolute and relative paths are followed by one or more identifiers separated by double colons (::).

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    // Absolute path
    crate::front_of_house::hosting::add_to_waitlist();

    // Relative path
    front_of_house::hosting::add_to_waitlist();
}
```

We can also use pub to designate structs and enums as public, but there are a few extra details. If we use pub before a struct definition, we make the struct public, but the struct’s fields will still be private

Filename: src/lib.rs
```rust
mod back_of_house {
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String,
    }

    impl Breakfast {
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }
}

pub fn eat_at_restaurant() {
    // Order a breakfast in the summer with Rye toast
    let mut meal = back_of_house::Breakfast::summer("Rye");
    // Change our mind about what bread we'd like
    meal.toast = String::from("Wheat");
    println!("I'd like {} toast please", meal.toast);

    // The next line won't compile if we uncomment it; we're not allowed
    // to see or modify the seasonal fruit that comes with the meal
    // meal.s
```

In contrast, if we make an enum public, all of its variants are then public. We only need the `pub` before the `enum` keyword.

```rust
mod back_of_house {
    pub enum Appetizer {
        Soup,
        Salad,
    }
}

pub fn eat_at_restaurant() {
    let order1 = back_of_house::Appetizer::Soup;
    let order2 = back_of_house::Appetizer::Salad;
}
```

We can bring a path into a scope once and then call the items in that path as if they’re local items with the `use` keyword.

we bring the `crate::front_of_house::hosting` module into the scope of the `eat_at_restaurant` function so we only have to specify `hosting::add_to_waitlist` to call the `add_to_waitlist` function in `eat_at_restaurant`.

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
    hosting::add_to_waitlist();
}
```



Adding `use` and a path in a scope is similar to creating a symbolic link in the filesystem. 

By adding use `crate::front_of_house::hosting` in the crate root, `hosting` is now a valid name in that scope, just as though the `hosting` module had been defined in the crate root. Paths brought into scope with `use` also check privacy, like any other paths.

You can also bring an item into scope with use and a relative path.

```rust
use std::collections::HashMap;

fn main() {
    let mut map = HashMap::new();
    map.insert(1, 2);
}
```

```rust
use std::fmt::Result;
use std::io::Result as IoResult;

fn function1() -> Result {
  // --snip--
}

fn function2() -> IoResult<()> {
  // --snip--
}
```

### 4.4. Separating Modules into Different Files



## Reference
1. https://iq.opengenus.org/oop-in-rust/
2. https://doc.rust-lang.org/book/ch17-00-oop.html
3. https://docs.w3cub.com/rust/std/index
4. https://stackoverflow.com/questions/42236166/is-it-possible-to-overload-a-function-with-different-numbers-of-arguments-using


