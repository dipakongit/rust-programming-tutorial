## if Expressions
If condition is true then execute if block code, otherwise execute else block code.
```
fn main() {
   let number = 3;
   if number < 5 {
    println!("condition was true");
   }
   else {
    println!("condition was false")
   }
}
```

#### Why does this code give an error? 
```
fn main() {
    let number = 3;

    if number {
        println!("number was three");
    }
}
```
In Rust, the `if` condition must be a `bool` (true or false). But here `number` is an **integer**, not a bool.   
Rust thinks: "I need true/false here, but you gave me a number — I don't know what to do!"