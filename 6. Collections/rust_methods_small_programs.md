# Rust Standard Library: Small Programs for Common Methods

Beginner-friendly examples for `String`, `Vec`, `HashMap`, and
iterators.

Each example is a **small, separate program**. Copy one example into
`src/main.rs` and run it with:

``` bash
cargo run
```

The examples are independent. They are not meant to be pasted all
together because each one has its own `fn main()`.

------------------------------------------------------------------------

# 1. String

A `String` is growable, owned UTF-8 text.

## `String::new()`

Create an empty string, then add text to it.

``` rust
fn main() {
    let mut name = String::new();
    name.push_str("Rust");
    println!("{name}");
}
```

## `String::from()`

Create an owned `String` from a string slice.

``` rust
fn main() {
    let name = String::from("Rust");
    println!("{name}");
}
```

## `to_string()`

Convert a string slice into an owned `String`.

``` rust
fn main() {
    let word = "hello";
    let owned_word = word.to_string();
    println!("{owned_word}");
}
```

## `push()`

Add one character to the end of a string.

``` rust
fn main() {
    let mut word = String::from("hell");
    word.push('o');
    println!("{word}");
}
```

## `push_str()`

Add a string slice to the end of a string.

``` rust
fn main() {
    let mut message = String::from("Hello");
    message.push_str(" Rust");
    println!("{message}");
}
```

## `len()`

Get the number of bytes, not necessarily the number of characters.

``` rust
fn main() {
    let word = String::from("hello");
    println!("Bytes: {}", word.len());
}
```

## `is_empty()`

Check whether a string contains no bytes.

``` rust
fn main() {
    let name = String::new();
    println!("Empty? {}", name.is_empty());
}
```

## `contains()`

Check whether text contains a particular piece of text.

``` rust
fn main() {
    let sentence = "Rust is fun";
    println!("{}", sentence.contains("fun"));
}
```

## `starts_with()`

Check the beginning of a string.

``` rust
fn main() {
    let file = "report.pdf";
    println!("{}", file.starts_with("report"));
}
```

## `ends_with()`

Check the ending of a string.

``` rust
fn main() {
    let file = "report.pdf";
    println!("{}", file.ends_with(".pdf"));
}
```

## `trim()`

Remove whitespace from the beginning and end. It returns a string slice;
it does not change the original string.

``` rust
fn main() {
    let input = "   hello   ";
    let cleaned = input.trim();
    println!("[{cleaned}]");
}
```

## `to_lowercase()`

Create a lowercase `String`.

``` rust
fn main() {
    let word = "RUST";
    let lower = word.to_lowercase();
    println!("{lower}");
}
```

## `to_uppercase()`

Create an uppercase `String`.

``` rust
fn main() {
    let word = "rust";
    let upper = word.to_uppercase();
    println!("{upper}");
}
```

## `replace()`

Create a new string with matching text replaced.

``` rust
fn main() {
    let sentence = "I like Java";
    let updated = sentence.replace("Java", "Rust");
    println!("{updated}");
}
```

## `split_whitespace()`

Split text into words using whitespace. It returns an iterator.

``` rust
fn main() {
    let sentence = "Rust is easy to read";

    for word in sentence.split_whitespace() {
        println!("{word}");
    }
}
```

## `split()`

Split text using a chosen separator.

``` rust
fn main() {
    let csv = "red,green,blue";

    for color in csv.split(',') {
        println!("{color}");
    }
}
```

## `chars()`

Iterate over Unicode characters (more precisely, Unicode scalar values).

``` rust
fn main() {
    let word = "Rust";

    for character in word.chars() {
        println!("{character}");
    }
}
```

## `bytes()`

Iterate over the UTF-8 bytes that make up a string.

``` rust
fn main() {
    let word = "cat";

    for byte in word.bytes() {
        println!("{byte}");
    }
}
```

## `get()`

Safely get a string slice. It returns `None` if the range is invalid or
not on UTF-8 character boundaries.

``` rust
fn main() {
    let word = String::from("hello");

    match word.get(0..2) {
        Some(part) => println!("{part}"),
        None => println!("Invalid range"),
    }
}
```

### String reminder

-   `len()` counts **bytes**.
-   `chars().count()` counts Unicode scalar values.
-   String indexes are byte positions, so `word[0]` is not allowed.
-   Use `chars()` when you want to process characters.

------------------------------------------------------------------------

# 2. Vec

A `Vec<T>` is a growable list of values stored on the heap.

## `Vec::new()`

Create an empty vector. Rust infers its element type when you add
values.

``` rust
fn main() {
    let mut numbers = Vec::new();
    numbers.push(10);
    numbers.push(20);
    println!("{numbers:?}");
}
```

## `vec![]`

Create a vector with initial values.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];
    println!("{numbers:?}");
}
```

## `push()`

Add an item to the end. The vector must be mutable.

``` rust
fn main() {
    let mut numbers = vec![10, 20];
    numbers.push(30);
    println!("{numbers:?}");
}
```

## `pop()`

Remove and return the last item. The result is an `Option<T>` because
the vector might be empty.

``` rust
fn main() {
    let mut numbers = vec![10, 20, 30];
    let last = numbers.pop();

    println!("Removed: {last:?}");
    println!("Now: {numbers:?}");
}
```

## `len()`

Get the number of items.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];
    println!("Items: {}", numbers.len());
}
```

## `is_empty()`

Check whether the vector has no items.

``` rust
fn main() {
    let numbers: Vec<i32> = Vec::new();
    println!("Empty? {}", numbers.is_empty());
}
```

## `capacity()`

Get how many items the vector can hold before it needs to reallocate.
Capacity is not the same as length.

``` rust
fn main() {
    let mut numbers = Vec::with_capacity(5);
    numbers.push(10);
    numbers.push(20);

    println!("Length: {}", numbers.len());
    println!("Capacity: {}", numbers.capacity());
}
```

## `get()`

Safely access an item by index. It returns `Option<&T>`.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];

    match numbers.get(1) {
        Some(value) => println!("Found: {value}"),
        None => println!("No item at that index"),
    }
}
```

## `contains()`

Check whether a value exists in the vector.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];
    println!("Has 20? {}", numbers.contains(&20));
}
```

## `insert()`

Insert an item at an index. Existing items at that index and after it
move right.

``` rust
fn main() {
    let mut numbers = vec![10, 30];
    numbers.insert(1, 20);
    println!("{numbers:?}");
}
```

## `remove()`

Remove and return the item at an index. The index must be valid or the
program will panic.

``` rust
fn main() {
    let mut numbers = vec![10, 20, 30];
    let removed = numbers.remove(1);

    println!("Removed: {removed}");
    println!("Now: {numbers:?}");
}
```

## `sort()`

Sort values in ascending order. This changes the original vector.

``` rust
fn main() {
    let mut numbers = vec![30, 10, 20];
    numbers.sort();
    println!("{numbers:?}");
}
```

## `sort_unstable()`

Sort without preserving the relative order of equal elements. For simple
integers, it looks like `sort()`.

``` rust
fn main() {
    let mut numbers = vec![30, 10, 20];
    numbers.sort_unstable();
    println!("{numbers:?}");
}
```

## `reverse()`

Reverse the current order of items.

``` rust
fn main() {
    let mut numbers = vec![10, 20, 30];
    numbers.reverse();
    println!("{numbers:?}");
}
```

## `dedup()`

Remove consecutive duplicate values. It does not remove duplicates that
are separated.

``` rust
fn main() {
    let mut numbers = vec![1, 1, 2, 1, 3, 3];
    numbers.dedup();
    println!("{numbers:?}"); // [1, 2, 1, 3]
}
```

To remove all duplicates, sort first:

``` rust
fn main() {
    let mut numbers = vec![1, 2, 1, 3, 2];
    numbers.sort();
    numbers.dedup();
    println!("{numbers:?}");
}
```

## `iter()`

Borrow each item for reading. The vector remains usable.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];

    for number in numbers.iter() {
        println!("{number}");
    }

    println!("Vector still exists: {numbers:?}");
}
```

## `iter_mut()`

Mutably borrow each item so it can be changed.

``` rust
fn main() {
    let mut numbers = vec![10, 20, 30];

    for number in numbers.iter_mut() {
        *number += 1; // Dereference &mut i32 to change the actual value.
    }

    println!("{numbers:?}");
}
```

## `into_iter()`

Create an iterator that takes ownership of the items. For a vector, this
consumes the vector.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];

    for number in numbers.into_iter() {
        println!("{number}");
    }

    // `numbers` cannot be used here because it was moved.
}
```

## `first()`

Get a reference to the first item. Returns `None` if empty.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];
    println!("{:?}", numbers.first());
}
```

## `last()`

Get a reference to the last item. Returns `None` if empty.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];
    println!("{:?}", numbers.last());
}
```

## `extend()`

Add items from another iterator.

``` rust
fn main() {
    let mut numbers = vec![1, 2];
    numbers.extend([3, 4]);
    println!("{numbers:?}");
}
```

## `truncate()`

Keep only the first `len` items.

``` rust
fn main() {
    let mut numbers = vec![10, 20, 30, 40];
    numbers.truncate(2);
    println!("{numbers:?}");
}
```

## `clear()`

Remove all items but keep the vector itself.

``` rust
fn main() {
    let mut numbers = vec![10, 20, 30];
    numbers.clear();
    println!("{numbers:?}");
}
```

------------------------------------------------------------------------

# 3. HashMap

A `HashMap<K, V>` stores key-value pairs. Each key is unique; different
keys can have the same value.

Add this import at the top of each HashMap program:

``` rust
use std::collections::HashMap;
```

## `HashMap::new()`

Create an empty HashMap.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);
    println!("{scores:?}");
}
```

## `insert()`

Add a key-value pair. If the key already exists, its old value is
replaced.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();

    scores.insert("Alice", 90);
    scores.insert("Alice", 95);

    println!("{scores:?}");
}
```

## `get()`

Look up a key. It returns `Option<&V>` because the key might not exist.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);

    match scores.get("Alice") {
        Some(score) => println!("Score: {score}"),
        None => println!("No score found"),
    }
}
```

## `get_mut()`

Get a mutable reference to a value so you can change it.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);

    if let Some(score) = scores.get_mut("Alice") {
        *score += 5; // Change the value stored in the map.
    }

    println!("{scores:?}");
}
```

## `entry()`

Get access to a key's entry. It is useful when you want to insert a
value only if the key is missing.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.entry("Alice").or_insert(90);

    println!("{scores:?}");
}
```

## `or_insert()`

Insert a default value if the key does not exist. It returns a mutable
reference to the value.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();

    let score = scores.entry("Alice").or_insert(0);
    *score = 90; // Dereference the mutable reference to update the map value.

    println!("{scores:?}");
}
```

## `or_insert_with()`

Create the default value by calling a closure, but only if the key is
missing.

``` rust
use std::collections::HashMap;

fn main() {
    let mut names = HashMap::new();

    names.entry("first_name").or_insert_with(|| String::from("Alice"));

    println!("{names:?}");
}
```

## `contains_key()`

Check whether a key exists.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);

    println!("Alice exists? {}", scores.contains_key("Alice"));
}
```

## `remove()`

Remove a key and return its value, if it existed.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);

    let removed = scores.remove("Alice");

    println!("Removed: {removed:?}");
    println!("Map: {scores:?}");
}
```

## `len()`

Get the number of key-value pairs.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);
    scores.insert("Bob", 80);

    println!("Pairs: {}", scores.len());
}
```

## `is_empty()`

Check whether the map contains no pairs.

``` rust
use std::collections::HashMap;

fn main() {
    let scores: HashMap<&str, i32> = HashMap::new();
    println!("Empty? {}", scores.is_empty());
}
```

## `keys()`

Iterate over references to all keys.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);
    scores.insert("Bob", 80);

    for key in scores.keys() {
        println!("{key}");
    }
}
```

## `values()`

Iterate over references to all values.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);
    scores.insert("Bob", 80);

    for value in scores.values() {
        println!("{value}");
    }
}
```

## `values_mut()`

Mutably iterate over values and change them.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);
    scores.insert("Bob", 80);

    for value in scores.values_mut() {
        *value += 5;
    }

    println!("{scores:?}");
}
```

## `iter()`

Borrow each key-value pair. The key and value are references.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);
    scores.insert("Bob", 80);

    for (name, score) in scores.iter() {
        println!("{name}: {score}");
    }
}
```

## `iter_mut()`

Borrow each pair, allowing the values to be changed.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);
    scores.insert("Bob", 80);

    for (name, score) in scores.iter_mut() {
        *score += 1;
        println!("{name}: {score}");
    }
}
```

## `clear()`

Remove every key-value pair.

``` rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert("Alice", 90);
    scores.insert("Bob", 80);

    scores.clear();
    println!("{scores:?}");
}
```

### Useful HashMap program: count word occurrences

This combines `split_whitespace()`, `entry()`, and `or_insert()`.

``` rust
use std::collections::HashMap;

fn main() {
    let sentence = "red blue red green blue red";
    let mut counts = HashMap::new();

    for word in sentence.split_whitespace() {
        // Get the word's count, or insert 0 if this word is new.
        let count = counts.entry(word).or_insert(0);

        // `count` is &mut i32, so *count changes the stored number.
        *count += 1;
    }

    println!("{counts:?}");
}
```

------------------------------------------------------------------------

# 4. Iterators

An iterator produces items one at a time. Methods such as `map()` and
`filter()` usually create another iterator; `collect()` can turn it into
a collection.

## `iter()`

Borrow items without consuming the collection.

``` rust
fn main() {
    let numbers = vec![1, 2, 3];

    let iterator = numbers.iter();

    for number in iterator {
        println!("{number}");
    }

    println!("{numbers:?}"); // The vector is still available.
}
```

## `iter_mut()`

Borrow items mutably.

``` rust
fn main() {
    let mut numbers = vec![1, 2, 3];

    for number in numbers.iter_mut() {
        *number *= 10;
    }

    println!("{numbers:?}");
}
```

## `into_iter()`

Consume a collection and take ownership of its items.

``` rust
fn main() {
    let numbers = vec![1, 2, 3];

    for number in numbers.into_iter() {
        println!("{number}");
    }

    // The vector has been consumed.
}
```

## `map()`

Transform every item into a new value.

``` rust
fn main() {
    let numbers = vec![1, 2, 3];

    let doubled: Vec<i32> = numbers.iter()
        .map(|n| *n * 2)
        .collect();

    println!("{doubled:?}");
}
```

## `filter()`

Keep only items that match a condition.

``` rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5, 6];

    let even: Vec<i32> = numbers.iter()
        .filter(|n| **n % 2 == 0)
        .copied()
        .collect();

    println!("{even:?}");
}
```

## `filter_map()`

Keep items where a conversion returns `Some`; skip items where it
returns `None`.

``` rust
fn main() {
    let words = vec!["10", "hello", "30"];

    let numbers: Vec<i32> = words.iter()
        .filter_map(|word| word.parse::<i32>().ok())
        .collect();

    println!("{numbers:?}");
}
```

## `collect()`

Consume an iterator and build a collection such as a `Vec` or `HashMap`.

``` rust
fn main() {
    let numbers = vec![1, 2, 3];

    let doubled: Vec<i32> = numbers.iter()
        .map(|n| *n * 2)
        .collect();

    println!("{doubled:?}");
}
```

## `enumerate()`

Add an index to each item. The index starts at zero.

``` rust
fn main() {
    let names = vec!["Alice", "Bob", "Charlie"];

    for (index, name) in names.iter().enumerate() {
        println!("{index}: {name}");
    }
}
```

## `count()`

Count how many items an iterator produces. It consumes the iterator.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];
    let total = numbers.iter().count();

    println!("{total}");
}
```

## `sum()`

Add all items together.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];
    let total: i32 = numbers.iter().sum();

    println!("{total}");
}
```

## `product()`

Multiply all items together.

``` rust
fn main() {
    let numbers = vec![2, 3, 4];
    let result: i32 = numbers.iter().product();

    println!("{result}");
}
```

## `min()` and `max()`

Find the smallest or largest item.

``` rust
fn main() {
    let numbers = vec![8, 3, 10, 5];

    println!("Min: {:?}", numbers.iter().min());
    println!("Max: {:?}", numbers.iter().max());
}
```

## `min_by_key()`

Find the item with the smallest calculated key.

``` rust
fn main() {
    let words = vec!["pear", "watermelon", "fig"];

    let shortest = words.iter().min_by_key(|word| word.len());

    println!("{shortest:?}");
}
```

## `max_by_key()`

Find the item with the largest calculated key.

``` rust
fn main() {
    let words = vec!["pear", "watermelon", "fig"];

    let longest = words.iter().max_by_key(|word| word.len());

    println!("{longest:?}");
}
```

## `fold()`

Process all items while carrying an accumulated result.

``` rust
fn main() {
    let numbers = vec![1, 2, 3, 4];

    let total = numbers.iter().fold(0, |sum, number| sum + number);

    println!("{total}");
}
```

## `any()`

Check whether at least one item matches a condition.

``` rust
fn main() {
    let numbers = vec![1, 3, 5, 8];

    let has_even = numbers.iter().any(|n| n % 2 == 0);

    println!("{has_even}");
}
```

## `all()`

Check whether every item matches a condition.

``` rust
fn main() {
    let numbers = vec![2, 4, 6];

    let all_even = numbers.iter().all(|n| n % 2 == 0);

    println!("{all_even}");
}
```

## `find()`

Return the first item that matches a condition.

``` rust
fn main() {
    let numbers = vec![1, 3, 4, 6];

    let found = numbers.iter().find(|n| **n % 2 == 0);

    println!("{found:?}");
}
```

## `position()`

Return the index of the first item that matches a condition.

``` rust
fn main() {
    let numbers = vec![10, 20, 30, 40];

    let index = numbers.iter().position(|n| *n == 30);

    println!("{index:?}");
}
```

## `take()`

Take only the first specified number of items.

``` rust
fn main() {
    let numbers = vec![10, 20, 30, 40, 50];

    for number in numbers.iter().take(3) {
        println!("{number}");
    }
}
```

## `skip()`

Skip the first specified number of items.

``` rust
fn main() {
    let numbers = vec![10, 20, 30, 40, 50];

    for number in numbers.iter().skip(2) {
        println!("{number}");
    }
}
```

## `chain()`

Join two iterators one after another.

``` rust
fn main() {
    let first = vec![1, 2];
    let second = vec![3, 4];

    let combined: Vec<i32> = first.iter()
        .chain(second.iter())
        .copied()
        .collect();

    println!("{combined:?}");
}
```

## `zip()`

Combine items from two iterators into pairs. It stops when the shorter
iterator ends.

``` rust
fn main() {
    let names = vec!["Alice", "Bob"];
    let scores = vec![90, 80];

    for (name, score) in names.iter().zip(scores.iter()) {
        println!("{name}: {score}");
    }
}
```

## `copied()`

Copy values out of an iterator of references when the values implement
`Copy`, such as integers.

``` rust
fn main() {
    let numbers = vec![10, 20, 30];

    let copied: Vec<i32> = numbers.iter().copied().collect();

    println!("{copied:?}");
}
```

## `cloned()`

Clone values out of an iterator of references. This also works for types
such as `String` that are not `Copy`.

``` rust
fn main() {
    let names = vec![String::from("Alice"), String::from("Bob")];

    let copied_names: Vec<String> = names.iter().cloned().collect();

    println!("{copied_names:?}");
}
```

------------------------------------------------------------------------

# 5. Quick guide: references and iteration

These three forms behave differently:

``` rust
fn main() {
    let numbers = vec![10, 20, 30];

    // Borrow each item. `number` is &i32.
    for number in &numbers {
        println!("{number}");
    }

    // Borrow each item mutably. `number` is &mut i32.
    let mut other_numbers = vec![10, 20, 30];
    for number in &mut other_numbers {
        *number += 1;
    }

    // Take ownership of each item. The vector is consumed.
    for number in numbers {
        println!("{number}");
    }
}
```

  -----------------------------------------------------------------------
  Syntax                  What the loop gets      Effect on collection
  ----------------------- ----------------------- -----------------------
  `for x in &items`       `&T`                    Keeps collection
                                                  available

  `for x in &mut items`   `&mut T`                Keeps collection
                                                  available; items can be
                                                  changed

  `for x in items`        `T`                     Moves items out;
                                                  collection is consumed
  -----------------------------------------------------------------------

## What does `*` mean?

When a variable is a reference, `*` dereferences it to access the value
it points to.

``` rust
fn main() {
    let number = 10;
    let reference = &number; // reference: &i32

    println!("{reference}"); // Print through the reference.
    println!("{}", *reference); // Dereference to access the i32 value.
}
```

For mutable references, dereferencing lets you update the original
value:

``` rust
fn main() {
    let mut number = 10;
    let reference = &mut number;

    *reference += 5;

    println!("{number}");
}
```

## A useful rule

-   `&T`: shared reference; read the value.
-   `&mut T`: mutable reference; read or change the value.
-   `*reference`: access the value through a reference.
-   `.iter()`: shared references.
-   `.iter_mut()`: mutable references.
-   `.into_iter()`: takes ownership of the items.
