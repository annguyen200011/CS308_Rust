# Objects and Classes

### Table of content
- [1. Characteristics of Object-Oriented Languages]()
  - [1.1. Objects Contain Data and Behavior]()
  - [1.2. Encapsulation that Hides Implementation Details]()
  - [1.3. Inheritance and Polymorphism]()
  - [1.4. Using Trait Objects That Allow for Values of Different Types]()
- [2. Standard methods]()
- 
- [Reference]()


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

## Reference
1. https://iq.opengenus.org/oop-in-rust/
2. https://doc.rust-lang.org/book/ch17-00-oop.html
3. 


