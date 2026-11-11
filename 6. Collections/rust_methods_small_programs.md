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
