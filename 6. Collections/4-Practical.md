# Given a list of integers, use a vector and return the median (when sorted, the value in the middle position) and mode (the value that occurs most often; a hash map will be helpful here) of the list.
```rust
use std::collections::HashMap;

fn main() {
    let mut nums = vec![2, 3, 5, 6, 9, 10, 11, 13];
    let med = median(&mut nums);
    println!("median={med}");

    let mut nums = vec![1, 2, 3, 3, 2];
    let mode = mode(&mut nums);
    println!("mode={mode:?}");
}

fn median(nums: &mut Vec<i32>) -> f64 {
    nums.sort();
    let n = nums.len();
    (nums[(n - 1) / 2] as f64 + nums[n / 2] as f64) / 2.0
}

fn mode(nums: &Vec<i32>) -> Vec<i32> {
    let mut map = HashMap::new();

    // Count how many times each number appears
    for n in nums {
        let c = map.entry(*n).or_insert(0);
        *c += 1;
    }

    // Find the highest frequency
    let mut count = 0;
    for (_, v) in &map {
        if *v > count {
            count = *v;
        }
    }

    // Collect numbers that have the highest frequency
    let mut modes = Vec::new();
    for (k, v) in &map {
        if count == *v && count > 1 {
            modes.push(*k);
        }
    }

    // Return all numbers with the highest frequency
    modes
}
```

# Convert strings to Pig Latin. The first consonant of each word is moved to the end of the word and ay is added, so first becomes irst-fay. Words that start with a vowel have hay added to the end instead (apple becomes apple-hay). Keep in mind the details about UTF-8 encoding!
```rust
fn main() {
    let str = String::from("first");
    let mut chars = str.chars();

    // get first element
    match chars.nth(0) {
        None => (),
        Some(c) => {
            if c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' {
                println!("{str}-hay")
            } else {
                let rest = chars.as_str(); // returns the remaining slice after the first character
                println!("{rest}-{c}ay")
            }
        }
    }
}
```

