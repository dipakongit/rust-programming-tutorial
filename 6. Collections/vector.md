# Collections
Collections are data structures that store multiple values on the heap. Unlike arrays, they can grow or shrink in size while the program runs. Each collection has different uses.

## Vector
* Vectors allow you to store more than one value in a single data structure that puts all the values next to each other in memory.
* Vectors can only store values of the same type.
* They are useful when you have a list of items, such as the lines of text in a file or the prices of items in a shopping cart.
* `Vec<T>` is a vector type provided by Rust's standard library can hold any type. Here, T represents the element type, such as i32, String, or char.

### Creating a New Vector
#### 1. Create an empty vector
```
let v: Vec<i32> = Vec::new();
```
* `Vec<i32>` means the vector stores i32 integers.
* `Vec::new()` creates an empty vector.
* We specify the type because Rust cannot infer it when the vector is empty.

#### 2. Create a vector with values
```
let v = vec![1, 2, 3];
```
* `vec![]` creates a vector with the given values.
* Rust automatically infers the type as `Vec<i32>`.

**Remember:** Use `Vec::new()` for an empty vector and `vec![]` when you already have values.

### Updating a Vector
* Use the `mut` keyword to make the vector mutable so you can add elements to it.
* Use the `push()` method to add elements to a vector.
* Rust automatically infers the element type from the values added.
```
let mut v = Vec::new();

v.push(5);
v.push(6);
v.push(7);
v.push(8);
```
Here, `v` is a mutable vector that stores `i32` integers.

### Reading Elements of Vectors
There are two ways to access elements in a vector:
1) indexing (`[]`)
2) `get()` method

#### Using indexing (`[]`)
```
let v = vec![10, 20, 30];
let third = &v[2];
```
* Vector indexes start at 0, so index 2 refers to the third element.
* `&v[2]` returns a reference to the element.
* If the index does not exist, the program panics.

#### Using the `get()` method
```
let v = vec![10, 20, 30];
let third = v.get(100);
match third {
    Some(value) => println!("{value}"),   // if the element exists
    None => println!("Element not found") // if the index is out of range
}
```
* `get()` returns `Option<&T>`, meaning it returns either a reference to an element or no value (`None`).
* Unlike indexing, `get()` does not panic when the index is invalid.

**Remember:** 
* Use `[]` when the index is expected to be valid. 
* Use `get()` when the index might be invalid and you want to handle that safely.

#### Modifying a vector while holding a reference
```
let mut v = vec![10, 20, 30];

// Borrow the first element without taking ownership
let first = &v[0];

v.push(40);          // Error
println!("{first}");
```
**Why does this program give a compile-time error?**  
First, understand how a vector stores elements in memory.  

Sometimes, a vector has enough capacity to add a new element. If it runs out of capacity, Rust may:
1. Allocate a larger memory area.
2. Move the existing elements to the new memory area.
3. Add the new element (`40`).
4. Release the old memory allocation.

If reallocation happens, `first` could point to the old memory location, making the reference invalid. Because `first` is still being used, Rust does not allow us to modify the vector with `push()`, as the elements might move to a new memory location.  

**How can we fix it?**  
**Option 1: Finish using the reference before modifying the vector.**
```
let mut v = vec![10, 20, 30];
    
let first = &v[0];
println!("{first}"); // Use the reference first

v.push(40); // Now allowed
```
After `println!("{first}")`, you don't use `first` again. So Rust knows it no longer needs the reference, and `push()` can safely modify the vector.  

**Remember:** Once a reference is no longer used, Rust can end its borrow automatically.  

**Option 2: Copy the value instead of borrowing it.**  
```
let mut v = vec![10, 20, 30];

// Copy the value because i32 implements the Copy trait.
let first = v[0];

v.push(40); 
println!("{first}");
```
Here, `first` stores its own copy of the `i32` value `10`. It does not borrow from the vector or take ownership of the vector. Therefore, modifying the vector does not affect `first`.

### Iterating Over the Values in a Vector
#### 1. Immutable references (`&v`)
Use a `for` loop to access and print each element without modifying the vector.
```
let v = vec![10, 20, 30];

// You can read the elements but 
//  cannot modify them through this reference &v. 
for i in &v{
    println!("{i}");
}
```
#### 2. Mutable references (`&mut v`)
Use `&mut v` to modify each element in a mutable vector.
```
let mut v = vec![10, 20, 30];

for i in &mut v{
    *i += 10;
}

println!("{:?}", v);
```
* `&mut v` allows you to modify the elements.
* `*i` (* is dereference operator) accesses the value that i refers to.
* `*i += 10` adds `10` to each element.

#### You cannot add or remove elements during the loop because that would modify the vector while it is already borrowed.
```
let mut v = vec![10, 20, 30];
    
for i in &v {
    v.push(40); // Error
    println!("{i}");
}
```
**Why does this give an error?** 
1) for `i` in `&v` borrows the vector so Rust can read its elements.
2) `v.push(40)` tries to modify the same vector while that borrow is active.
3) Rust rejects this because adding an element may move the existing elements in memory, making the reference `i` invalid. We discussed this briefly in the previous example:  [Modifying a vector while holding a reference](#Modifying-a-vector-while-holding-a-reference)

### Using an Enum to Store Multiple Types
* A vector normally stores elements of the same type.
* An `enum` lets you represent different types of values under a single `enum` type, allowing them to be stored in the same vector.
```
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```
**Use case:** Useful when a list needs to store different kinds of values, such as spreadsheet cells containing integers, decimal numbers, or text.

### Dropping a Vector
```
{
    let v = vec![1, 2, 3, 4];

    // do stuff with v
} // <- v goes out of scope and is freed here
```
* A vector is freed when it goes out of scope.
* When the vector is dropped, its elements are also dropped.
* Rust's borrow checker ensures that references to vector elements are not used after the vector is dropped.