# Loops and Subroutines

### Table of content

- [1. Loop](#1-loop)
   - [1.1. loop](#11-loop)
     - [1.1.1. Nesting and labels](#111-nesting-and-labels)
     - [1.1.2. Returning from loops](#112-returning-from-loops)
   - [1.2. while](#12-while)
   - [1.3. for](#13-for)
     - [1.3.1. for and range](#131-for-and-range)
     - [1.3.2. for and iterators](#132-for-and-iterators)
     - [1.3.3. Range expression](#133-range-expression)
- [2. Function](#2-function)
   - [2.1. Syntax](#21-syntax)
   - [2.2. Methods](#22-methods)
   - [2.3. Closures](#23-closures)
     - [2.3.1. Capturing](#231-capturing)
     - [2.3.2. As input parameters](#232-as-input-parameters)
     - [2.3.3. Type anonymity](#233-type-anonymity)
     - [2.3.4. Input functions](#234-input-functions)
     - [2.3.5. As output parameters](#235-as-output-parameters)
     - [2.3.6. Examples in std](#236-examples-in-std)
       - [2.3.6.1. Iterator::any](#2361-iteratorany)
       - [2.3.6.2. Searching through iterators](#2362-searching-through-iterators)
   - [2.4. Higher Order Functions](#24-higher-order-functions)
   - [2.5. Diverging functions](#25-diverging-functions)
   - [2.6. Generic Functions](#26-generic-functions)
   - [2.7. Recursion Functions](#27-recursion-functions)
   - [2.8. Multiple Parameters](#28-multiple-parameters)
- [3. Return](#3-return) 
- [4. Pass by value]() 
- [Reference]

## 1. Loop

### 1.1. loop
Rust provides a `loop` keyword to indicate an infinite loop.

The `break` statement can be used to exit a loop at anytime, whereas the `continue` statement can be used to skip the rest of the iteration and start a new one.

```rust
fn main() {
    let mut count = 0u32;

    println!("Let's count until infinity!");

    // Infinite loop
    loop {
        count += 1;

        if count == 3 {
            println!("three");

            // Skip the rest of this iteration
            continue;
        }

        println!("{}", count);

        if count == 5 {
            println!("OK, that's enough");

            // Exit this loop
            break;
        }
    }
}
```

#### 1.1.1. Nesting and labels
It's possible to `break` or `continue` outer loops when dealing with nested loops. In these cases, the loops must be annotated with some 'label, and the label must be passed to the break/continue statement.

```rust
fn main() {
    'outer: loop {
        println!("Entered the outer loop");

        'inner: loop {
            println!("Entered the inner loop");

            // This would break only the inner loop
            //break;

            // This breaks the outer loop
            break 'outer;
        }

        println!("This point will never be reached");
    }

    println!("Exited the outer loop");
}
```

#### 1.1.2. Returning from loops

One of the uses of a `loop` is to retry an operation until it succeeds. If the operation returns a value though, you might need to pass it to the rest of the code: put it after the `break`, and it will be returned by the `loop` expression.

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    assert_eq!(result, 20);
}
```

### 1.2. while

The `while` keyword can be used to run a loop while a condition is true.

```rust
fn main() {
    // A counter variable
    let mut n = 1;

    // Loop while `n` is less than 101
    while n < 101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }

        // Increment counter
        n += 1;
    }
}
```

### 1.3. for

#### 1.3.1. for and range

The `for in` construct can be used to iterate through an `Iterator`. One of the easiest ways to create an iterator is to use the range notation `a..b`. This yields values from `a` (inclusive) to `b` (exclusive) in steps of one.

```rust
fn main() {
    // `n` will take the values: 1, 2, ..., 100 in each iteration
    for n in 1..101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }
    }
}
```

Alternatively, `a..=b` can be used for a range that is inclusive on both ends. The above can be written as:

```rust
fn main() {
    // `n` will take the values: 1, 2, ..., 100 in each iteration
    for n in 1..=100 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }
    }
}
```


#### 1.3.2. for and iterators

The `for` in construct is able to interact with an `Iterator` in several ways. As discussed in the section on the Iterator trait, by default the `for` loop will apply the `into_iter` function to the collection. However, this is not the only means of converting collections into iterators.

`into_iter`, `iter` and `iter_mut` all handle the conversion of a collection into an iterator in different ways, by providing different views on the data within.

- `iter` - This borrows each element of the collection through each iteration. Thus leaving the collection untouched and available for reuse after the loop.

```rust
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter() {
        match name {
            &"Ferris" => println!("There is a rustacean among us!"),
            // TODO ^ Try deleting the & and matching just "Ferris"
            _ => println!("Hello {}", name),
        }
    }
    
    println!("names: {:?}", names);
}   

    /* print:
    Hello Bob
    Hello Frank
    There is a rustacean among us!
    names: ["Bob", "Frank", "Ferris"]
    */
```

- `into_iter` - This consumes the collection so that on each iteration the exact data is provided. Once the collection has been consumed it is no longer available for reuse as it has been 'moved' within the loop.
```rust
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.into_iter() {
        match name {
            "Ferris" => println!("There is a rustacean among us!"),
            _ => println!("Hello {}", name),
        }
    }
    println!("names: {:?}", names);
    // if we use: println!("names: {:?}", names); -> error because the `names` moved due to .into_iter
}
```

- `iter_mut` - This mutably borrows each element of the collection, allowing for the collection to be modified in place.
```rust
fn main() {
    let mut names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter_mut() {
        *name = match name {
            &mut "Ferris" => "There is a rustacean among us!",
            _ => "Hello",
        }
    }

    println!("names: {:?}", names);
}
//print: names: ["Hello", "Hello", "There is a rustacean among us!"]
```


#### 1.3.3. Range expression

| Production	                                                             | Syntax                                | Type                       | Range           |
|-------------------------------------------------------------------------|---------------------------------------|----------------------------|-----------------|
| RangeExpr                                                               | 	start`..`end	                          | std::ops::Range            | start ≤ x < end |
| RangeFromExpr                                                           | 	start`..`	                             | std::ops::RangeFrom	       | start ≤ x       |
| RangeToExpr	| `..`end                                 | 	std::ops::RangeTo         | 	x < end        |
| RangeFullExpr	| `..`	                                   | std::ops::RangeFull	       | -               |
| RangeInclusiveExpr	| start`..=`end| 	std::ops::RangeInclusive	 | start ≤ x ≤ end            |
| RangeToInclusiveExpr	| `..=`end	| std::ops::RangeToInclusive | 	x ≤ end                              |


## 2. Function

### 2.1. Syntax
Functions are declared using the `fn` keyword. Its arguments are type annotated, just like variables, and, if the function returns a value, the return type must be specified after an arrow `->`.

The final expression in the function will be used as `return` value. Alternatively, the return statement can be used to return a value earlier from within the function, even from inside loops or `if` statements.

```rust
// Unlike C/C++, there's no restriction on the order of function definitions
fn main() {
    // We can use this function here, and define it somewhere later
    fizzbuzz_to(100);
}

// Function that returns a boolean value
fn is_divisible_by(lhs: u32, rhs: u32) -> bool {
    // Corner case, early return
    if rhs == 0 {
        return false;
    }

    // This is an expression, the `return` keyword is not necessary here
    lhs % rhs == 0
}


// Functions that "don't" return a value, actually return the unit type `()`
fn fizzbuzz(n: u32) -> () {
    if is_divisible_by(n, 15) {
        println!("fizzbuzz");
    } else if is_divisible_by(n, 3) {
        println!("fizz");
    } else if is_divisible_by(n, 5) {
        println!("buzz");
    } else {
        println!("{}", n);
    }
}

// When a function returns `()`, the return type can be omitted from the
// signature
fn fizzbuzz_to(n: u32) {
    for n in 1..=n {
        fizzbuzz(n);
    }
}
```

### 2.2. Methods

Some functions are connected to a particular type. These come in two forms: associated functions, and methods. Associated functions are functions that are defined on a type generally, while methods are associated functions that are called on a particular instance of a type.

```rust
struct Point {
    x: f64,
    y: f64,
}

// Implementation block, all `Point` associated functions & methods go in here
impl Point {
    // This is an "associated function" because this function is associated with
    // a particular type, that is, Point.
    //
    // Associated functions don't need to be called with an instance.
    // These functions are generally used like constructors.
    fn origin() -> Point {
        Point { x: 0.0, y: 0.0 }
    }

    // Another associated function, taking two arguments:
    fn new(x: f64, y: f64) -> Point {
        Point { x: x, y: y }
    }
}

struct Rectangle {
    p1: Point,
    p2: Point,
}

impl Rectangle {
    // This is a method
    // `&self` is sugar for `self: &Self`, where `Self` is the type of the
    // caller object. In this case `Self` = `Rectangle`
    fn area(&self) -> f64 {
        // `self` gives access to the struct fields via the dot operator
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;

        // `abs` is a `f64` method that returns the absolute value of the
        // caller
        ((x1 - x2) * (y1 - y2)).abs()
    }

    fn perimeter(&self) -> f64 {
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;

        2.0 * ((x1 - x2).abs() + (y1 - y2).abs())
    }

    // This method requires the caller object to be mutable
    // `&mut self` desugars to `self: &mut Self`
    fn translate(&mut self, x: f64, y: f64) {
        self.p1.x += x;
        self.p2.x += x;

        self.p1.y += y;
        self.p2.y += y;
    }
}

// `Pair` owns resources: two heap allocated integers
struct Pair(Box<i32>, Box<i32>);

impl Pair {
    // This method "consumes" the resources of the caller object
    // `self` desugars to `self: Self`
    fn destroy(self) {
        // Destructure `self`
        let Pair(first, second) = self;

        println!("Destroying Pair({}, {})", first, second);

        // `first` and `second` go out of scope and get freed
    }
}

fn main() {
    let rectangle = Rectangle {
        // Associated functions are called using double colons
        p1: Point::origin(),
        p2: Point::new(3.0, 4.0),
    };

    // Methods are called using the dot operator
    // Note that the first argument `&self` is implicitly passed, i.e.
    // `rectangle.perimeter()` === `Rectangle::perimeter(&rectangle)`
    println!("Rectangle perimeter: {}", rectangle.perimeter());
    println!("Rectangle area: {}", rectangle.area());

    let mut square = Rectangle {
        p1: Point::origin(),
        p2: Point::new(1.0, 1.0),
    };

    // Error! `rectangle` is immutable, but this method requires a mutable
    // object
    //rectangle.translate(1.0, 0.0);
    // TODO ^ Try uncommenting this line

    // Okay! Mutable objects can call mutable methods
    square.translate(1.0, 1.0);

    let pair = Pair(Box::new(1), Box::new(2));

    pair.destroy();

    // Error! Previous `destroy` call "consumed" `pair`
    //pair.destroy();
    // TODO ^ Try uncommenting this line
}
```

### 2.3. Closures

Closures are functions that can capture the enclosing environment. For example, a closure that captures the x variable:

```rust
|val| val + x
```

The syntax and capabilities of closures make them very convenient for on the fly usage. Calling a closure is exactly like calling a function. However, both input and return types can be inferred and input variable names must be specified.

Other characteristics of closures include:

- using `||` instead of `()` around input variables.
- optional body delimination (`{}`) for a single expression (mandatory otherwise).
- the ability to capture the outer environment variables.

```rust
fn main() {
    // Increment via closures and functions.
    fn function(i: i32) -> i32 { i + 1 }

    // Closures are anonymous, here we are binding them to references
    // Annotation is identical to function annotation but is optional
    // as are the `{}` wrapping the body. These nameless functions
    // are assigned to appropriately named variables.
    let closure_annotated = |i: i32| -> i32 { i + 1 };
    let closure_inferred  = |i     |          i + 1  ;

    let i = 1;
    // Call the function and closures.
    println!("function: {}", function(i));
    println!("closure_annotated: {}", closure_annotated(i));
    println!("closure_inferred: {}", closure_inferred(i));

    // A closure taking no arguments which returns an `i32`.
    // The return type is inferred.
    let one = || 1;
    println!("closure returning one: {}", one());

}

/* print: 
function: 2
closure_annotated: 2
closure_inferred: 2
closure returning one: 1 
 */
```

#### 2.3.1. Capturing

Closures are inherently flexible and will do what the functionality requires to make the closure work without annotation. This allows capturing to flexibly adapt to the use case, sometimes moving and sometimes borrowing. Closures can capture variables:

- **by reference:** `&T`
- **by mutable reference:** `&mut T`
- **by value:** `T`

They preferentially capture variables by reference and only go lower when required.

```rust
fn main() {
    use std::mem;

    let color = String::from("green");

    // A closure to print `color` which immediately borrows (`&`) `color` and
    // stores the borrow and closure in the `print` variable. It will remain
    // borrowed until `print` is used the last time. 
    //
    // `println!` only requires arguments by immutable reference so it doesn't
    // impose anything more restrictive.
    let print = || println!("`color`: {}", color);

    // Call the closure using the borrow.
    print();

    // `color` can be borrowed immutably again, because the closure only holds
    // an immutable reference to `color`. 
    let _reborrow = &color;
    print();


    let mut count = 0;
    // A closure to increment `count` could take either `&mut count` or `count`
    // but `&mut count` is less restrictive so it takes that. Immediately
    // borrows `count`.
    //
    // A `mut` is required on `inc` because a `&mut` is stored inside. Thus,
    // calling the closure mutates the closure which requires a `mut`.
    let mut inc = || {
        count += 1;
        println!("`count`: {}", count);
    };

    // Call the closure using a mutable borrow.
    inc();

    // The closure still mutably borrows `count` because it is called later.
    // An attempt to reborrow will lead to an error.
    // let _reborrow = &count; 
    // ^ TODO: try uncommenting this line.
    inc();

    // The closure no longer needs to borrow `&mut count`. Therefore, it is
    // possible to reborrow without an error



    // A non-copy type.
    let movable = Box::new(3);

    // `mem::drop` requires `T` so this must take by value. A copy type
    // would copy into the closure leaving the original untouched.
    // A non-copy must move and so `movable` immediately moves into
    // the closure.
    let consume = || {
        println!("`movable`: {:?}", movable);
        mem::drop(movable);
    };

    // `consume` consumes the variable so this can only be called once.
    consume();
}

/* 
print:
`color`: green
`color`: green
`count`: 1
`count`: 2
`movable`: 3
*/

```

Using `move` before vertical pipes forces closure to take ownership of captured variables:

```rust
fn main() {
    // `Vec` has non-copy semantics.
    let haystack = vec![1, 2, 3];

    let contains = move |needle| haystack.contains(needle);

    println!("{}", contains(&1));
    println!("{}", contains(&4));

    // println!("There're {} elements in vec", haystack.len());
    // ^ Uncommenting above line will result in compile-time error
    // because borrow checker doesn't allow re-using variable after it
    // has been moved.
    
    // Removing `move` from closure's signature will cause closure
    // to borrow _haystack_ variable immutably, hence _haystack_ is still
    // available and uncommenting above line will not cause an error.
}
```

#### 2.3.2. As input parameters

While Rust chooses how to capture variables on the fly mostly without type annotation, this ambiguity is not allowed when writing functions. When taking a closure as an input parameter, the closure's complete type must be annotated using one of a few `traits`, and they're determined by what the closure does with captured value. In order of decreasing restriction, they are:

- `Fn`: the closure uses the captured value by reference (&T)
- `FnMut`: the closure uses the captured value by mutable reference (&mut T)
- `FnOnce`: the closure uses the captured value by value (T)

On a variable-by-variable basis, the compiler will capture variables in the least restrictive manner possible.

For instance, consider a parameter annotated as `FnOnce`. This specifies that the closure may capture by `&T`, `&mut T`, or `T`, but the compiler will ultimately choose based on how the captured variables are used in the closure.

This is because if a move is possible, then any type of borrow should also be possible. Note that the reverse is not true. If the parameter is annotated as `Fn`, then capturing variables by `&mut T` or `T` are not allowed.

In the following example, try swapping the usage of Fn, FnMut, and FnOnce to see what happens:

```rust
// A function which takes a closure as an argument and calls it.
// <F> denotes that F is a "Generic type parameter"
fn apply<F>(f: F) where
    // The closure takes no input and returns nothing.
    F: FnOnce() {
    // ^ TODO: Try changing this to `Fn` or `FnMut`.

    f();
}

// A function which takes a closure and returns an `i32`.
fn apply_to_3<F>(f: F) -> i32 where
    // The closure takes an `i32` and returns an `i32`.
    F: Fn(i32) -> i32 {

    f(3)
}

fn main() {
    use std::mem;

    let greeting = "hello";
    // A non-copy type.
    // `to_owned` creates owned data from borrowed one
    let mut farewell = "goodbye".to_owned();

    // Capture 2 variables: `greeting` by reference and
    // `farewell` by value.
    let diary = || {
        // `greeting` is by reference: requires `Fn`.
        println!("I said {}.", greeting);

        // Mutation forces `farewell` to be captured by
        // mutable reference. Now requires `FnMut`.
        farewell.push_str("!!!");
        println!("Then I screamed {}.", farewell);
        println!("Now I can sleep. zzzzz");

        // Manually calling drop forces `farewell` to
        // be captured by value. Now requires `FnOnce`.
        mem::drop(farewell);
    };

    // Call the function which applies the closure.
    apply(diary);

    // `double` satisfies `apply_to_3`'s trait bound
    let double = |x| 2 * x;

    println!("3 doubled: {}", apply_to_3(double));
}

/* 
if F:FnOnce, print: 
I said hello.
Then I screamed goodbye!!!.
Now I can sleep. zzzzz
3 doubled: 6

if F:Fn, error: 
mem::drop(farewell);
   |                   -------- closure is `FnOnce` because it moves the variable `farewell` out of its environment

if F:FnMut, error:
error[E0525]: expected a closure that implements the `FnMut` trait, but this closure only implements `FnOnce`

*/
```

#### 2.3.3. Type anonymity

Closures succinctly capture variables from enclosing scopes. Does this have any consequences? It surely does. Observe how using a closure as a function parameter requires [generics](https://doc.rust-lang.org/rust-by-example/generics.html), which is necessary because of how they are defined:

```rust
// `F` must be generic.
fn apply<F>(f: F) where
    F: FnOnce() {
    f();
}
```

When a closure is defined, the compiler implicitly creates a new anonymous structure to store the captured variables inside, meanwhile implementing the functionality via one of the `traits`: `Fn`, `FnMut`, or `FnOnce` for this unknown type. This type is assigned to the variable which is stored until calling.

Since this new type is of unknown type, any usage in a function will require generics. However, an unbounded type parameter `<T>` would still be ambiguous and not be allowed. Thus, bounding by one of the `traits`: `Fn`, `FnMut`, or `FnOnce` (which it implements) is sufficient to specify its type.

```rust
// `F` must implement `Fn` for a closure which takes no
// inputs and returns nothing - exactly what is required
// for `print`.
fn apply<F>(f: F) where
    F: Fn() {
    f();
}

fn main() {
    let x = 7;

    // Capture `x` into an anonymous type and implement
    // `Fn` for it. Store it in `print`.
    let print = || println!("{}", x);

    apply(print);
}
// print: 7
```

#### 2.3.4. Input functions
Since closures may be used as arguments, you might wonder if the same can be said about functions. And indeed they can! If you declare a function that takes a closure as parameter, then any function that satisfies the trait bound of that closure can be passed as a parameter.
```rust
// Define a function which takes a generic `F` argument
// bounded by `Fn`, and calls it
fn call_me<F: Fn()>(f: F) {
    f();
}

// Define a wrapper function satisfying the `Fn` bound
fn function() {
    println!("Hi, I'm a function!");
}

fn main() {
    // Define a closure satisfying the `Fn` bound
    let closure = || println!("I'm a closure!");

    call_me(closure);
    call_me(function);
}
//print:
//I'm a closure!
//Hi, I'm a function!
```

As an additional note, the `Fn`, `FnMut`, and `FnOnce` `traits` dictate how a closure captures variables from the enclosing scope.
#### 2.3.5. As output parameters

Closures as input parameters are possible, so returning closures as output parameters should also be possible. However, anonymous closure types are, by definition, unknown, so we have to use `impl` `Trait` to return them.

The valid traits for returning a closure are:

- `Fn`
- `FnMut`
- `FnOnce`

Beyond this, the `move` keyword must be used, which signals that all captures occur by value. This is required because any captures by reference would be dropped as soon as the function exited, leaving invalid references in the closure.

```rust
fn create_fn() -> impl Fn() {
    let text = "Fn".to_owned();

    move || println!("This is a: {}", text)
}

fn create_fnmut() -> impl FnMut() {
    let text = "FnMut".to_owned();

    move || println!("This is a: {}", text)
}

fn create_fnonce() -> impl FnOnce() {
    let text = "FnOnce".to_owned();

    move || println!("This is a: {}", text)
}

fn main() {
    let fn_plain = create_fn();
    let mut fn_mut = create_fnmut();
    let fn_once = create_fnonce();

    fn_plain();
    fn_mut();
    fn_once();
}
/* 
print:
This is a: Fn
This is a: FnMut
This is a: FnOnce
*/
```
#### 2.3.6. Examples in std
This section contains a few examples of using closures from the `std` library.
##### 2.3.6.1. Iterator::any
`Iterator::any` is a function which when passed an iterator, will return `true` if any element satisfies the predicate. Otherwise `false`. Its signature:

```rust
pub trait Iterator {
    // The type being iterated over.
    type Item;

    // `any` takes `&mut self` meaning the caller may be borrowed
    // and modified, but not consumed.
    fn any<F>(&mut self, f: F) -> bool where
        // `FnMut` meaning any captured variable may at most be
        // modified, not consumed. `Self::Item` states it takes
        // arguments to the closure by value.
        F: FnMut(Self::Item) -> bool {}
}
```

```rust
fn main() {
    let vec1 = vec![1, 2, 3];
    let vec2 = vec![4, 5, 6];

    // `iter()` for vecs yields `&i32`. Destructure to `i32`.
    println!("2 in vec1: {}", vec1.iter()     .any(|&x| x == 2));
    // `into_iter()` for vecs yields `i32`. No destructuring required.
    println!("2 in vec2: {}", vec2.into_iter().any(| x| x == 2));

    let array1 = [1, 2, 3];
    let array2 = [4, 5, 6];

    // `iter()` for arrays yields `&i32`.
    println!("2 in array1: {}", array1.iter()     .any(|&x| x == 2));
    // `into_iter()` for arrays unusually yields `&i32`.
    println!("2 in array2: {}", array2.iter().any(|&x| x == 2));
}

/* print
2 in vec1: true
2 in vec2: false
2 in array1: true
2 in array2: false
 */
```
##### 2.3.6.2. Searching through iterators

`Iterator::find` is a function which iterates over an iterator and searches for the first value which satisfies some condition. If none of the values satisfy the condition, it returns `None`. Its signature:

```rust
pub trait Iterator {
    // The type being iterated over.
    type Item;

    // `find` takes `&mut self` meaning the caller may be borrowed
    // and modified, but not consumed.
    fn find<P>(&mut self, predicate: P) -> Option<Self::Item> where
        // `FnMut` meaning any captured variable may at most be
        // modified, not consumed. `&Self::Item` states it takes
        // arguments to the closure by reference.
        P: FnMut(&Self::Item) -> bool {}
}
```

```rust
fn main() {
    let vec1 = vec![1, 2, 3];
    let vec2 = vec![4, 5, 6];

    // `iter()` for vecs yields `&i32`.
    let mut iter = vec1.iter();
    // `into_iter()` for vecs yields `i32`.
    let mut into_iter = vec2.into_iter();

    // `iter()` for vecs yields `&i32`, and we want to reference one of its
    // items, so we have to destructure `&&i32` to `i32`
    println!("Find 2 in vec1: {:?}", iter     .find(|&&x| x == 2));
    // `into_iter()` for vecs yields `i32`, and we want to reference one of
    // its items, so we have to destructure `&i32` to `i32`
    println!("Find 2 in vec2: {:?}", into_iter.find(| &x| x == 2));

    let array1 = [1, 2, 3];
    let array2 = [4, 5, 6];

    // `iter()` for arrays yields `&i32`
    println!("Find 2 in array1: {:?}", array1.iter()     .find(|&&x| x == 2));
    // `into_iter()` for arrays yields `i32`
    println!("Find 2 in array2: {:?}", array2.into_iter().find(|&x| x == 2));
}
/*
print:
Find 2 in vec1: Some(2)
Find 2 in vec2: None
Find 2 in array1: Some(2)
Find 2 in array2: None
*/
```

`Iterator::find` gives you a reference to the item. But if you want the _index_ of the item, use `Iterator::position`.

```rust
fn main() {
    let vec = vec![1, 9, 3, 3, 13, 2];

    let index_of_first_even_number = vec.iter().position(|x| x % 2 == 0);
    assert_eq!(index_of_first_even_number, Some(5));
    
    
    let index_of_first_negative_number = vec.iter().position(|x| x < &0);
    assert_eq!(index_of_first_negative_number, None);
}
```
### 2.4. Higher Order Functions

Rust provides Higher Order Functions (HOF). These are functions that take one or more functions and/or produce a more useful function. HOFs and lazy iterators give Rust its functional flavor.

```rust
fn is_odd(n: u32) -> bool {
    n % 2 == 1
}

fn main() {
    println!("Find the sum of all the squared odd numbers under 1000");
    let upper = 1000;

    // Imperative approach
    // Declare accumulator variable
    let mut acc = 0;
    // Iterate: 0, 1, 2, ... to infinity
    for n in 0.. {
        // Square the number
        let n_squared = n * n;

        if n_squared >= upper {
            // Break loop if exceeded the upper limit
            break;
        } else if is_odd(n_squared) {
            // Accumulate value, if it's odd
            acc += n_squared;
        }
    }
    println!("imperative style: {}", acc);

    // Functional approach
    let sum_of_squared_odd_numbers: u32 =
        (0..).map(|n| n * n)                             // All natural numbers squared
             .take_while(|&n_squared| n_squared < upper) // Below upper limit
             .filter(|&n_squared| is_odd(n_squared))     // That are odd
             .fold(0, |acc, n_squared| acc + n_squared); // Sum them
    println!("functional style: {}", sum_of_squared_odd_numbers);
}
/*
print:
Find the sum of all the squared odd numbers under 1000
imperative style: 5456
functional style: 5456
 */
```

### 2.5. Diverging functions

Diverging functions never return. They are marked using `!`, which is an empty type.

```rust
fn foo() -> ! {
    panic!("This call never returns.");
}
```
As opposed to all the other types, this one cannot be instantiated, because the set of all possible values this type can have is empty. Note that, it is different from the `()` type, which has exactly one possible value.

For example, this function returns as usual, although there is no information in the return value.

```rust
fn some_fn() {
    ()
}

fn main() {
    let a: () = some_fn();
    println!("This function returns and you can see this line.")
}
```

As opposed to this function, which will never return the control back to the caller.

```rust
fn main() {
    let x: ! = panic!("This call never returns.");
    println!("You will never see this line!");
}
```

Although this might seem like an abstract concept, it is in fact very useful and often handy. The main advantage of this type is that it can be cast to any other one and therefore used at places where an exact type is required, for instance in match branches. This allows us to write code like this:

```rust
fn main() {
    fn sum_odd_numbers(up_to: u32) -> u32 {
        let mut acc = 0;
        for i in 0..up_to {
            // Notice that the return type of this match expression must be u32
            // because of the type of the "addition" variable.
            let addition: u32 = match i%2 == 1 {
                // The "i" variable is of type u32, which is perfectly fine.
                true => i,
                // On the other hand, the "continue" expression does not return
                // u32, but it is still fine, because it never returns and therefore
                // does not violate the type requirements of the match expression.
                false => continue,
            };
            acc += addition;
        }
        acc
    }
    println!("Sum of odd numbers up to 9 (excluding): {}", sum_odd_numbers(9));
}
```

It is also the return type of functions that loop forever (e.g. `loop {}`) like network servers or functions that terminate the process (e.g. `exit()`).

### 2.6. Generic Functions

A generic function allows one or more parameterized types to appear in its signature. Each type parameter must be explicitly declared in an angle-bracket-enclosed and comma-separated list, following the function name.

```rust
// foo is generic over A and B

fn foo<A, B>(x: A, y: B) {
```

Inside the function signature and body, the name of the type parameter can be used as a type name. [Trait](https://doc.rust-lang.org/reference/items/traits.html) bounds can be specified for type parameters to allow methods with that trait to be called on values of that type. This is specified using the `where` syntax:

```rust
fn foo<T>(x: T) where T: Debug {
```

When a generic function is referenced, its type is instantiated based on the context of the reference. For example, calling the foo function here:

```rust
use std::fmt::Debug;

fn foo<T>(x: &[T]) where T: Debug {
    // details elided
}

foo(&[1, 2]);
```

will instantiate type parameter `T` with `i32`.

The type parameters can also be explicitly supplied in a trailing path component after the function name. This might be necessary if there is not sufficient context to determine the type parameters. For example, `mem::size_of::<u32>() == 4`.

### 2.7. Recursion Functions
The final form of loop to consider is known as a recursive function. This is a function that calls itself until a condition is met.

```rust
fn factorial(num: u64) -> u64 {
    match num {
        0 | 1 => 1,
        _ => factorial(num - 1) * num,
    }
}

fn main() {
    println!("{}", factorial(10)); //print: 3628800
}
```

### 2.8. Multiple Parameters
Generics types can have more than one type parameters, eg. Result is defined like this:

```rust
pub enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

or simply
```rust
fn test(a: u8, b: u8, c: u8) {}
```

There is no short-cut syntax available if you want to name each individually.

If you do not care for individual names:

```rust
fn test(a: &[u8; 3]) {}
```

And if the number of items ought to be dynamic:

```rust
fn test(a: &[u8]) {}
```

## 3. Return 
A `return` marks the end of an execution path in a function:

```rust
fn foo() -> i32 {
    return 3;
}
assert_eq!(foo(), 3);
```

`return` is not needed when the returned value is the last expression in the function. In this case the `;` is omitted:

```rust
fn foo() -> i32 {
    3
}
assert_eq!(foo(), 3);
```

`return` returns from the function immediately (an “early return”):

```rust
use std::fs::File;
use std::io::{Error, ErrorKind, Read, Result};

fn main() -> Result<()> {
    let mut file = match File::open("foo.txt") {
        Ok(f) => f,
        Err(e) => return Err(e),
    };

    let mut contents = String::new();
    let size = match file.read_to_string(&mut contents) {
        Ok(s) => s,
        Err(e) => return Err(e),
    };

    if contents.contains("impossible!") {
        return Err(Error::new(ErrorKind::Other, "oh no!"));
    }

    if size > 9000 {
        return Err(Error::new(ErrorKind::Other, "over 9000!"));
    }

    assert_eq!(contents, "Hello, world!");
    Ok(())
}
```

## 4. Pass by value
 > **Rust is strictly a pass-by-value language.**

Example:

```rust
fn main() {
  let n_main: usize = 100;
  println!("{}", inc(n_main));
}

fn inc(n_inc: usize) -> usize {
  n_inc + 1
}
```
In the above example, `n_main` is a number allocated on the stack of the `main` function. When we pass `n_main` to the `inc` function it is copied to the stack of the `inc` function.`n_main` and `n_inc` are two separate values at two places in memory.

```rust
fn main() {
  let n_main: usize = 100;
  let n_main_ref: &usize = &n_main;
  println!("{}", inc_ref(n_main_ref));
}

fn inc_ref(n_inc_ref: &usize) -> usize {
  n_inc_ref + 1
}
```

In this example, the function `inc_ref` works the same way as `inc` did before just now with the value being a reference instead of a number of type `usize`. When we pass `n_main_ref` to `inc_ref`, the reference value `n_main_ref` (not the value that it’s pointing to) is copied to the stack of `inc_ref`. The reference is a value and it behaves with pass-by-value semantics just like the value of type `usize` did.

Let’s take a look at another example to drive the case home:

```rust
fn main() {
  let array_main: [Vec<u8>; 3] = [vec![1], vec![2, 4], vec![]];
  print(array_main);
}

fn print(array: [Vec<u8>; 3]) {
  for e in array.iter() {
    println!("{:?}", e)
  }
}
```

This example is the exact same as the above examples except the underlying value we’re looking at is a fixed length array with elements that are `Vec`s. In the case of the `print` function, the array will be copied from the stack of the main function to the stack of the `print` function. This means that each of the three elements (the `Vec`s) will be copied to the new stack frame.

It’s important to be clear what “copying” means here. While logically we might think of a `Vec` as a growable array on the heap, in reality is is just a struct that contains three values: a length, a capacity and a pointer to the actual data on the heap. When we say the `Vec` is copied - we mean this struct is copied. The data on the heap, however, is not copied.

Now if Rust didn’t have the borrow checker this could present a problem. Why? Copying one of the `Vec`s would lead to two pointers (a.k.a. aliased pointers) to the same data on the heap. If we then “dropped” (a.k.a freed, a.k.a. deallocated, a.k.a. destroyed) one of the `Vec`s (and in turn free the heap allocated memory), the pointer in other `Vec` would now point to junk data. Luckily, the borrow checker makes it impossible to use `array_main` after it’s been “moved” to the print function.

Ok, now let’s look again at passing a reference by value:

```rust
fn main() {
  let array_main: [Vec<u8>; 3] = [vec![1], vec![2, 4], vec![]];
  let array_main_ref: &[Vec<u8>] = &array_main;
  print(array_main_ref);
}

fn print(array_ref: &[Vec<u8>]) {
  for e in array.iter() {
    println!("{:?}", e)
  }
}
```

This example again is pass-by-value semantics. The value in question is the reference to our stack allocated array. The reference `array_main_ref` will be copied from `main`’s stack to the stack of the print function, but of course, the array itself will not be copied and will stay on the stack of the `main` function.

## Reference
- https://doc.rust-lang.org/rust-by-example/flow_control/loop.html
- https://doc.rust-lang.org/rust-by-example/fn.html
- https://mockstacks.com/Rust_Recursive_Function
- https://stackoverflow.com/questions/47893783/how-to-give-multiple-function-parameters-the-same-type
- https://riptutorial.com/rust/example/5882/multiple-type-parameters
- https://blog.ryanlevick.com/rust-pass-value-or-reference
