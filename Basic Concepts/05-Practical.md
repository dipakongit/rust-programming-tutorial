# Convert temperatures between Fahrenheit and Celsius
```
fn main() {
    // Convert fahrenheit to celsius
    let f_temp = 30.0;
    let c_temp = fahrenheit_celsius(f_temp);
    println!("{f_temp}F = {c_temp:.2}C"); //.2 means "show only 2 decimal places"

    // Convert celsius to fahrenheit
    let c_temp = 30.0;
    let f_temp = celsius_fahrenheit(c_temp);
    println!("{c_temp}C = {f_temp:.2}F");
}

fn fahrenheit_celsius(f_temp: f32) -> f32 {
    (f_temp - 32.0) * (5.0 / 9.0)
}

fn celsius_fahrenheit(c_temp: f32) -> f32 {
    (c_temp * 9.0 / 5.0) + 32.0
}
```

# Generate the nth Fibonacci number.
```
fn main() {
    for n in 0..=10 {
        let fibonacci_number = fibonacci(n);
        println!("F({n}) = {fibonacci_number}")
    }
}

fn fibonacci(n: u32) -> u32 {
    let sqrt5 = 5.0_f32.sqrt();
    let phi = (1.0 + sqrt5) / 2.0;
    let psi = (1.0 - sqrt5) / 2.0;
    let result = (phi.powf(n as f32) - psi.powf(n as f32)) / sqrt5;
    result.round() as u32
}
```

# Print the lyrics to the Christmas carol “The Twelve Days of Christmas,” taking advantage of the repetition in the song.
```
fn main() {
    let days = [
        "first", "second", "third", "fourth", "fifth", "sixth", "seventh", "eighth", "ninth",
        "tenth", "eleventh", "twelfth",
    ];
    let gifts = [
        "A partridge in a pear tree",
        "Two turtle doves",
        "Three french hens",
        "Four calling birds",
        "Five golden rings",
        "Six geese a-laying",
        "Seven swans a-swimming",
        "Eight maids a-milking",
        "Nine ladies dancing",
        "Ten lords a-leaping",
        "Eleven pipers piping",
        "Twelve drummers drumming",
    ];

    for d in 0..=11 {
        println!(
            "On the {} day of Christmas, my true love sent to me",
            days[d]
        );
        for g in 0..=d {
            println!("{}", gifts[g]);
        }
        println!();
    }
}
```
