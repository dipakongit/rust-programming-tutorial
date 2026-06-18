# Variables

## What is immutable variable
* Immutable means unchangeable. Once a value is defined, it can't be changed.
* In Rust, variables are immutable by default.
```
let x = 5;  // immutable
x = 6;      // compile time error
```
Rust checks this at compile time, so your code won’t run until you fix it.

## What is mutable variable
* Mutable means changeable. You can change its value anytime after it is defined.
* In Rust, a variable is immutable by default. You can make it mutable by adding the **mut** keyword
```
let mut x = 5; // mutable variable
x = 10;        // no compile time error
```

## Constants
Constants are like immutable variables — their value cannot change.
### Differences between constants and variables
1) You can't use **mut** with constants
2) Constants are not just immutable by default, they are always immutable
3) You declare constants using the **const** keyword instead of the **let** keyword
4) You must have to provide type for constant
```
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```
* Rust’s naming convention for constants is to use all uppercase with underscores between words

## Shadowing
In rust, You can declare a new variable with the same name as a previous variable. The new variable shadows (hide) the old one.
```
let x = 5;

// create new variable with same name that shadows the previous one
let x = x + 1;

// compiler will see second variable. so it print 6
println!("The value of x is: {x}");
```
### Benifits of Shadowing
1) In shadowing, we can perform a few transformations on a value but have the variable be immutable after those transformations have completed. For example:
```
let x = 5;
let x = x + 1;
// new x shadows old x
// x is now immutable again
```
2) In shadowing, we can change the type of the value, because we create variable everytime with same name
```
// int type
let x = 5;

// string type
let x = "hello";
```

##
## Q&A
### Can one immutable variable copy the value of another?
Yes
```
let x = 5;
let y = x;
```
### Can one mutable variable copy the value of a immutable variable?
Yes 
```
let x = 5;
let mut y = x;
```
### Can one immutable variable copy the value of a mutable variable?
Yes 
```
let mut x = 5;
let y = x;
```
### Does this code compile? If not, which rule of Rust is being violated?
```
let x = 5;
let mut y = x;
x = 7;
println!("The value of x is: {x}");
```
No, the code does not compile, because immutable variable **x** cannot be reassigned
### Is constants can mutable in rust?
No, Constants are always immutable.
### How does variable shadowing work across scopes (block of code) in Rust?
```
// creates a new variable
let x = 5;

// scope start
{
    // creates a new shadowed x using the outer x (5)
    let x = x + 1;

    // prints 6
    println!("The value of x in the inner scope is: {x}");
}
// scope end
// When that scope is over,
// the inner x is dropped; the outer x (still 5) is visible again
println!("The value of x is: {x}");

```
* We can’t use a variable declared in an inner scope outside that scope, because once the scope finishes executing, the variable is destroyed.
