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
* In Rust, literals (Number, Float, Character, String, Boolean) are expressions because they evaluate to a value.

#### Point - 2 :
* Function definitions is statements 
* Calling a function is expression
* Calling a macro is an expression

    For example:
    ```
    fn main() {
        let x =add(5,5);            // calling add() function here is a expression because it gives a value
        println!("result is {x}");  // println! macro is a expression because it gives () unit value
    }

    fn add(x: i32, y: i32) -> i32 {     // this whole thing (call function defination)
        x + y                           //  is a STATEMENT
    }                                   // it does not give you a value
    ```
* A new scope block created with curly brackets is an expression
    ```
    fn main() {
        let y = {
            let x = 3;
            x + 1
        };

        println!("The value of y is: {y}");
    }

    // this scope block is an expression
    {
        let x = 3;
        x + 1
    }
    ```
#### Point - 3 :
Expressions do not include ending semicolons. If you add a semicolon to the end of an expression, it becomes a statement and return no value.
```
fn main() {
    let x =add(5,5);   // Calling add() function is aa expression, ; at end makes it a statement
    println!("result is {x}")
}

fn add(x: i32, y: i32) -> i32 {     
    x + y                           
}
```