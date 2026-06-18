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

## break and continue
```
fn main() {
    let mut count = 0;
    loop {
        count += 1;

        if count == 2 {
            continue;     // skip 2
        }

        if count == 4 {
            break;        // stop the loop
        }

        println!("{count}");
    }
}
```
* **break** - stopes the loop
* **continue** - skips the current iteration and moves to the next one

### Returning Values from Loops
If you want to return a value from a loop, you can add the value after the break expression. 
```
fn main() {
    let result = loop {
        let x = 5;

        if x == 5 {
            break x; // Stop the loop and return 5
        }
    };

    println!("{result}");
}
```

## while
while is just a cleaner way to write loop + break
#### Using loop + break:
```
fn main() {
    let mut count = 4;
    loop {
        if count == 0 {
            break;
        }
        println!("count={count}");
        count -= 1
    }
}
```
#### Using while (cleaner way)
```
fn main() {
    let mut count = 4;
    while count != 0 {
        println!("count={count}");
        count -= 1
    }
}
```
While a condition evaluates to true, the code runs; otherwise, it exits the loop.

## for
for loop is used to iterate a collection or a range of values, executing the same block of code for each item.
#### Example on collection of values
```
fn main() {
    let colors = ["red", "green", "yellow", "blue", "white"];
    for item in colors {
        println!("colour name - {item}")
    }
}
```
#### Example on range of values
```
fn main() {
    for number in (1..5).rev() {
        println!("{number}")
    }
}
```
  * `1..5` this is range that prnt 1 to 4 but if want to print 1 to 5 use `1..=5`
  * `rev()` is used to iterate a range or collection in reverse order
