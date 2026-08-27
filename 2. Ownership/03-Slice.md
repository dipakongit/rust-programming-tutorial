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

