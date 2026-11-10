# Slice
A slice is a reference to a contiguous sequence of the elements of a **collection**. A slice is a kind of reference, so it does not have ownership.

#### What is collection?
Collection is a group of values stored together. For example:
* **String** - collection of characters
* **Array** - collection of values
* **Vec** - collection of values

## String Slices
A string slice is a reference to a contiguous sequence of the elements of a **String**. Example:
```
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```
Syntax of a string slice:
```
&String[starting_index..ending_index]
```
* **starting_index** is the first position in the slice 
* **ending_index** is one position after the last position (last position + 1) in the slice

Internally, the slice data structure stores the memory address of the first element of the slice and its length (ending_index - starting_index). For example:  
`world` is a slice that contains a pointer to its first element (`w`) and a length of 5.
```
         s                            
┌─────────────────┐
│ pointer = 0x1000│────────────┐
│ length = 11     │            │  points to address 0x1000
│ capacity = 11   │            │
└─────────────────┘            │       
                               │      ┌─────────────────┐
                               │      │ Address → Value │
                               │      ├─────────────────┤
                               └────> │ 0x1000 → h      │
                                      │ 0x1001 → e      │
                                      │ 0x1002 → l      │
                                      │ 0x1003 → l      │
                                      │ 0x1004 → o      │
                                      │ 0x1005 → space  │
                               ┌────> │ 0x1006 → w      │
                               │      │ 0x1007 → o      │
                               │      │ 0x1008 → r      │
                               │      │ 0x1009 → l      │
                               │      │ 0x100A → d      │
                               │      └─────────────────┘  
                               │
                               │
                               │
      world                    │   points to address 0x1005
┌─────────────────┐            │ 
│ pointer = 0x1000│────────────┘
│ length = 5      │   
└─────────────────┘
```
#### What does "contiguous" mean here?
contiguous means the elements in the slice are next to each other without skipping any elements. For example:
```
let s = String::from("hello world");
let hello = &s[0..5];
```
`&s[0..5]` refers to:
```
h e l l o
↑       ↑
0       5
```
These characters are next to each other:
```
h → e → l → l → o
```
So this is a **contiguous sequence**.  

**We don't skip any elements.**   
For example, if we want `h l o` from `h e l l o`, we skip elements between them, so they are not contiguous.

### Slice Example
1) If you want to start at index 0, you can drop the starting index.
    ```
    let s = String::from("hello");
    let slice = &s[..2];
    ```
2) if your slice includes the last byte of the String, you can drop the ending index.
    ```
    let s = String::from("hello");
    let slice = &s[2..];
    ```
3) You can also drop both values to take a slice of the entire string.
    ```
    let s = String::from("hello");
    let slice = &s[..];
    ```

#### Write a function that takes a string of words separated by spaces and returns the first word. If there is no space, return the entire string.
```
fn main() {
    // Create a String
    let s = String::from("hello world");

    // Pass a reference to the String
    let word = first_word(&s);

    // Print the first word
    println!("the first word is: {word}");


    // String literals have the type &str
    let string_literal = "hello world";

    // Pass the string slice directly
    let word = first_word(string_literal);

    // Print the first word
    println!("the first word of string literal is: {word}");
}


// &str can accept both &String and &str
fn first_word(s: &str) -> &str {

    // Convert the string into bytes
    let bytes = s.as_bytes();

    // Loop through each byte with its index
    for (i, &item) in bytes.iter().enumerate() {

        // Check if the byte is a space
        if item == b' ' {

            // Return the slice from the start to the space
            return &s[0..i];
        }
    }

    // If no space is found, return the whole string
    return &s[..];
}
```

### Array Slices
```
let a = ['a', 'e', 'i', 'o', 'u'];  // array
let slice = &a[..3];                // slice of the array

// Access elements of the slice
for i in slice {
    println!("{i}");
}
```
#### Why use a slice with an array?
Suppose you have:
```
let a = ['a', 'e', 'i', 'o', 'u']; 
```
You only want the first 3 elements:
```
let slice = &a[..3];
```
