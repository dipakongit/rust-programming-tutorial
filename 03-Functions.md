# Functions
A function is a block of code that performs a specific task. It can take parameters as input, run some logic, and return a value. For example:
```
fn main() {
   println!("hello world")
}
```
* **fn** - which allows you to declare new functions
* **main()** - which is the entry point of rust programs
* Rust’s naming convention for functions is to use  all letters are lowercase and underscores separate words

## Parameters & arguments
A function can have **parameters**, which are special variables used inside it.  

When you call the function, you give values to those parameters.
These values are called **arguments**.
```
fn main() {
    print_value(5); // 5 is the argument
}

fn print_value(x: u8) { // x is the parameter
    println!("value of x: {x}");
}

```
* `x` is a parameter because it is defined in the function declaration.
* `5` is an argument because it is passed to the function when calling it.

#### Always remember: when you declare a parameter, always specify its type.
```
fn main() {
    print_labeled_measurement(5, 'h');
}

fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

## Statements and Expressions
#### Point - 1 :
* Statements are instructions that perform some action and do not return a value. 
* An expression evaluates and gives you a value

For example:
```
let x = 5;    // Statement — just stores 5, gives you nothing back
```
```
5 + 6        // Expression — gives you value 11
```
```
6           // Any literal value is expression — gives you value 6
```
* In Rust, any literal (Number, Float, Character, String, Boolean) value is an expression

#### Point - 2 :
* Function definitions is statements 
* Calling a function is expression

For example:
```
fn main() {
    let x =add(5,5);                 // calling add() function here is a expression
    println!("result is {x}")
}

fn add(x: i32, y: i32) -> i32 {     // this whole thing
    x + y                           //  is a STATEMENT
}                                   // it does not give you a value
```