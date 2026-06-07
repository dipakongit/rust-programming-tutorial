## if
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

Fix it with proper condition:
```
fn main() {
    let number = 3;

    if number == 3 {
        println!("number was three");
    }
}
```

## else if
Handling Multiple Conditions with else if
```
fn main() {
   let age = 32;
   if age < 13 {
      println!("You are a Child");
   } else if age < 18 {
      println!("You are a Teenager")
   } else if age < 60 {
       println!("You are an Adult")
   } else {
       println!("You are a Senior Citizen")
   }
}
```

## if in a let statement
```
fn main() {
   let age = 32;
   let status = if age >= 18 {"Adult"} else {"Minor"};
   println!("{status}");
}
```
#### Why does this code give an error? 
```
fn main() {
   let age = 32;
   let status = if age >= 18 {"Adult"} else {5};
   println!("{status}");
}
```
The expression in the `if` block evaluates to an string, and the expression in the `else` block evaluates to a integer. This won’t work, because variables must have a single type.

# Loops
The loop keyword in Rust repeats a block of code continuously until you stop it
```
fn main() {
   loop{
      println!("hello world");
   }
}
```