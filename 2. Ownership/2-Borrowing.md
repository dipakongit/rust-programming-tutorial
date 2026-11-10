## References and Borrowing
Sometimes you want to use a value without taking ownership of it. Rust lets you do this using a **Reference** - this is called **Borrowing**. You create a reference using the `&` symbol. For example:
```
fn main() {
    let s1 = String::from("hello");
    let s2 = &s1;
    println!("{s2}");
    println!("{s1}");
}
```
**s2** only accesses (borrowing) the value, but **s1** still owns it.

**Borrow** → you can use the data without taking ownership.

### Passing References to Functions
```
fn calculate_length(s: &String) -> usize {      // s is a reference to a String
    s.len()                                     // With references, a function can use a value without taking ownership,
                                                // so there is no need to return the value to give ownership back.
} // s goes out of scope, but the String is not dropped
  // because s does not own the String.

fn main() {
    let s1 = String::from("hello");

    // &s1 creates a reference that refers to the value of s1 but does not own it.
    // When the reference is no longer needed, Rust does not delete the original
    // value because the reference does not own it.
    let len = calculate_length(&s1);   // reference used here
                                       // reference is no longer needed

    println!("The length of {s1} is {len}");  // but s1 is still alive

} // The "hello" value is dropped when s1 (the owner)
  // goes out of scope, not when the reference stops being used.
```
Simple rule:
* A reference can use a value.
* A reference does not own or delete the value.
* The owner is responsible for dropping it.

### Mutable References
So, what happens if we try to modify something we’re borrowing? 
```
fn main() {
    let s = String::from("hello");  // immutable variable
    change(&s);                     // &s creates an immutable reference
}

fn change(some_string: &String) {    // take an immutable reference
    some_string.push_str(", world");  
}
```
This code gives a compile-time error because **&String** is an immutable reference by default. An immutable reference can read a value, but it cannot modify the value. If you want to modify a borrowed value, you need to use a mutable reference `(**&mut)`.  

To make this example work, both need to be mutable:  
```
fn change(s_ref: &mut String) {        // takes a mutable reference
    s_ref.push_str(" world");          // mutable reference allows us to change the value
}

fn main() {
    let mut s = String::from("hello"); // mutable variable
    change(&mut s);                    // &mut creates a mutable reference
}
```

#### What happens if we create two mutable references to the same value?
```
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;          // Error

println!("{r1}, {r2}");
```
This code gives an error because, in Rust, a value can have only one mutable reference at a time.

#### What problem does this restriction prevent?
It helps prevent data races, and the Rust compiler prevents them at compile time.

#### What is a Data Race?
* A data race can happen when multiple references access the same data at the same time and at least one of them is changing the data without proper synchronization.  
* Data races cause undefined behavior and can be difficult to diagnose and fix when you’re trying to track them down at runtime. Rust prevent this problem at compile time.

#### How Can We Use Multiple Mutable References?
we can use curly brackets to create a new scope, allowing for multiple mutable references, but not at the same time. For example:
```
let mut s = String::from("hello");

{
    let r1 = &mut s;
}  // r1 goes out of scope here, it's borrow ends, and it's no longer usable

let r2 = &mut s;  // we can create a new mutable reference
```

#### Can We Use Mutable and Immutable References at the same Time?
Rust does not allow to use mutable and immutable references at the same time. This code give an error:
```
let mut s = String::from("hello");

let r1 = &s;      //  Multiple immutable references 
let r2 = &s;      //    are allowed

let r3 = &mut s;  // Rust cannot create a mutable reference while
                  // immutable references are still being used

println!("{r1}, {r2}, and {r3}");
```

#### Can We Use Multiple immutable references?
Multiple immutable references are allowed because someone who is only reading the data cannot affect anyone who is also reading it.
```
let s = String::from("hello");
let r1 = &s;
let r2 = &s;
```

#### When Does Rust Allow a Mutable Reference After Immutable References?
A reference starts when it is created and ends when it is last used. This code compiles because the immutable references are last used before the mutable reference is introduced.
```
let mut s = String::from("hello");

let r1 = &s;                    // Multiple immutable references
let r2 = &s;                   //   are allowed
println!("{r1} and {r2}");    // r1 and r2 are last used here
                             //   After that, they are not used anymore.

let r3 = &mut s;             // So Rust allows r3 to create a mutable reference. 
println!("{r3}");
```

### Dangling References
In languages where you manually work with pointers, it is easy to accidentally create a dangling pointer. A pointer that points to memory that has already been freed this is called **Dangling Pointer**. This can cause crashes or incorrect program behavior.  

But Rust is different. Rust has **references** and Rust compiler guarantees that references will never be dangling references.  

Let’s try to create a dangling reference to see how Rust prevents it:
```
fn main() {
    let r = dangle();
}

fn dangle() -> &String {            // returns a reference to a String
    let s = String::from("hello");  // s is a new String
    &s                              // return a reference to s
}   // Here, s goes out of scope and is dropped, so its memory will be freed.
```
This code give an error:  
Because `s` is created inside `dangle`, when the code of `dangle` is finished, `s` will be dropped. But we tried to return a reference to it. That means the reference `&s` would point to data that no longer exists. Rust detects this at compile time and gives an error instead of allowing us to create a dangling reference.   

The solution is:
```
fn main() {
    let r = dangle(); // Ownership of the String is moved to `r`.
}

fn dangle() -> String {
    let s = String::from("hello"); // `s` owns the String.
    s // Ownership of the String moves from `s` to the caller.
      // So the String is not dropped when `dangle()` ends.
}
```
### The Rules of References
1) You can have many immutable references, but only one mutable reference at a time.
2) A reference must always point to data that still exists
