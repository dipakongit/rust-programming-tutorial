# Data Types
Rust is a **statically typed language**. Rust data types are divided into two main groups: 
1) scalar
2) compound

## Scalar
Scalar = single value  
In rust scalar type represnt single value. Rust has four scalar type: 
1) Integer
2) Floating-point
3) Boolean
4) Character

### Integer
An integer is a number without a decimal point. Integer has two types:
#### 1) Signed
Represent both positive and negative numbers
Length|Types
-|-
8-bit|i8
16-bit|i16
32-bit|i32
64-bit|i64
128-bit|i128
Depend on the architecture |isize
**isize** depend on the architecture of the computer where your program is running. It will be 64-bits if you’re on a 64-bit architecture and will be 32-bits if you’re on a 32-bit architecture

**Formula to calculate the range of a signed integer**  
$$\text{ from } -2^{n-1} \text{ to } 2^{n-1} - 1$$
* **n** is the number of bits.  

So, an **i8** can store from -128 to 127    
$$\text{ from }-2^{8-1} = -2^7 = -128\text{ to }2^{8-1} - 1 = 2^7 - 1 = 128 - 1 = 127$$
#### 2) Unsigned
Represent only non-negative numbers (0 and positive)
Length|Types
-|-
8-bit|u8
16-bit|u16
32-bit|u32
64-bit|u64
128-bit|u128
Depend on the architecture |usize
**usize** depend on the architecture of the computer where your program is running. It will be 64-bits if you’re on a 64-bit architecture and will be 32-bits if you’re on a 32-bit architecture

**Formula to calculate the range of a unsigned integer**  
$$\text{ from } 0 \text{ to } 2^n - 1$$
* **n** is the number of bits.  

So, an **u8** can store from 0 to 255  
$$2^8 - 1 = 256 - 1 = 255$$

#### Example
```
let x: i32 = 5;
```

### Floating-point
Floating-point is number with decimal point
Length|Types
-|-
32-bit|f32
64-bit|f64

#### Example
```
let x: f32 = 5.2;
```

### Boolean
In rust boolean type has two possible values: **true** and **false**. Booleans are one byte in size. 
#### Example
```
let f: bool = false;
```
Boolean values are mainly used in conditionals, like an if statement

### Character
In Rust, A char is written using single quotes **' '**
```
let a: char = 'a';
let heart_eyed_cat: char = '😻';
```
Rust **char** type is 4 bytes in size and represents a Unicode scalar value, means 
* You see an English letter: 'a'
* But rust sees a Unicode number: U+0061 (unicode number of a)

That’s why Rust char can represent not only English letters, but also accented letters, Chinese, Japanese, and Korean characters, emojis, and even zero-width spaces.

## Compound
Rust has two compound type: 
1) Tuple
2) Array

### Tuple
Using a tuple, we can group multiple values into a single compound type, where each value may have a different type. For example:
```
let tup: (i32, f64, u8) = (500, 6.4, 1);
```
* **(i32, f64, u8)** is one type. It is a tuple type
* Inside that single tuple type have multiple values. Each value can have a different type

Tuples have a fixed length: Once declared at compile time, they cannot grow or shrink in size. For example:
```
let tup: (i32, f64, u8) = (500, 6.4, 1);
```
* The tuple **tup** has 3 elements
* Its type is **(i32, f64, u8)**
* You cannot add or remove elements after it is created

you can get values from a tuple in two ways:
#### 1) Using destructuring (pattern matching)
Unpack the tuple into individual variables
```
let tup: (i32, f64, u8) = (500, 6.4, 1);

let (x, y, z) = tup; // destructuring

println!("x = {x}");
println!("y = {y}");
println!("z = {z}");
```
Each variable gets the corresponding value from the tuple

#### 2) Using index
You can also access tuple values using their index
```
let tup: (i32, f64, u8) = (500, 6.4, 1);

println!("First value: {}", tup.0);
println!("Second value: {}", tup.1);
println!("Third value: {}", tup.2);
```

### Array
* An array is a collection of multiple values.
* Every element of an array must have the same type.
* In Rust, arrays have a fixed length, which means once an array is initialized, you cannot add or remove elements.

Example:
```
let a: [i32; 5] = [1, 2, 3, 4, 5];
```
* **i32** is the type of each element.
* **5** is the length of the array, which means this array can contain 5 elements.

You can initialize an array to cantaint same value like this:
```
let a = [3;5]
```
This is equivalent to:
```
let a = [3, 3, 3, 3, 3];
```
#### How to access array element:
You can access elements of an array using indexing, like this:
```
let a: [i32; 5] = [1, 2, 3, 4, 5];
println!("1st element = {}",a[0]);
println!("2nd element = {}",a[1]);
```

## What is Type inference
The compiler needs the type. If you don’t define the type, the compiler infers(figures it out) it from the value. This is caled type inference.  

### For Example:
#### Integer
```
let i = 5;
```
The compiler takes it as an integer and infers the type as `i32`.
#### Float
```
let f = 5.0;
```
The compiler takes it as a float and infers the type as `f64`.
#### Bool
```
let b = true;
```
The compiler takes it as a bool.
#### Char
```
let c = 'a';
```
The compiler takes it as a character.
#### Tuple
```
let tup = (5,5.0,true,'a');
```
The compiler takes it as a tuple with different types.
#### Array
```
let arr = [1,2,3,4];
```
The compiler treats it as an array of integers.