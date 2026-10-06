# Rust Module System
Rust uses several features to organize code:

## Package
A package is a collection of crates managed by **Cargo**. It contains a `Cargo.toml` file that tells **Cargo** how to build the project.
#### Example:
```
my-project/        ← Package
├── Cargo.toml     
└── src/
    └── main.rs    ← root of a binary crate
```

### Important Rules
A package:
* Must contain at least 1 crate
* Can contain many binary crates
* Can contain at most 1 library crate

## Crate
A crate is the complete unit of code that Rust compiles. There are two types:
* **Binary crate →** Creates an executable program. It must have `main()`.
* **Library crate →** Provides reusable code. It does not have `main()`.

#### Crate Root
The **crate root** is the starting source file of a crate. With Cargo:
```
src/main.rs → crate root of a binary crate
src/lib.rs  → crate root of a library crate
```

## Module
A way to group related code inside a crate.

#### Example:
```
// create module
mod resturent {
    pub fn add_customer() {
        println!("Customer added");
    }
}

fn main() {
    resturent::add_customer();
}
```
How it is organized?
```
Binary Crate
│
├── main()               ← function
│
└── restaurant           ← module
    └── add_customer()   ← function
```

### Separating Modules into Different Files
When a module becomes large, we can move it into a separate file to keep the project organized.

#### Example: Restaurant
We can organize restaurant code like this:
```
my-project/
├── Cargo.toml     
└── src/
    └── main.rs
    └── resturent.rs     ← parent module
    └── resturent/
        ├── customer.rs  ← submodule
        └── serving.rs   ← submodule
```
* if you use the `restaurant.rs` + `restaurant/` folder layout, they must have the same name.  

`src/resturent.rs`
```
pub mod customer;
pub mod serving;
```
* load `customer` and `serving` as child modules, and make them **public**.

`src/resturent/customer.rs`
```
pub fn add_customer() {
    println!("Customer added");
}
```

`src/resturent/serving.rs`
```
pub fn take_order() {
    println!("Taking customer order");
}

pub fn serve_order() {
    println!("Serving the order");
}
```

`src/main.rs`
```
mod resturent;

fn main() {
    resturent::customer::add_customer();
    resturent::serving::take_order();
    resturent::serving::serve_order();
}
```
* `mod resturent;` - load the `resturent` module, but keep the module **private**.

## Path
A path tells Rust where an item (function, struct, module, etc.) is located.
```
crate::resturent::add_customer();
│      │           │
│      │           └── Function
│      └──────── Module
└─────────────── Crate
```
Think of it like a **file path**.
#### Absolute Path
Starts from the crate root using `crate::`
```
crate::resturent::add_customer();
```
#### Relative Path
Starts from the current module.
```
resturent::add_customer();
```

## `pub`
### Exposing Paths with the `pub` Keyword
Items are private by default. `pub` makes an item accessible from outside its module.
```
fn add_customer() {}      // private

pub fn add_customer() {}  // public
```

Making a module public does not automatically make the items inside it public. Each item has its own visibility.  

Example:
```
pub mod customer {           // public
    fn add_customer() {      // private
        println!("Customer added");
    }
}
```
We can reach **customer**, but not `add_customer()`. Because each level has its own visibility:
```
pub mod customer {           // public
  pub fn add_customer() {    // public
        println!("Customer added");
    }
}
```
* `pub mod customer` → allows access to the **customer** module
* `pub fn add_customer` → allows access to the function inside **customer**

### Making Structs and Enums Public
#### Public Struct
Making the struct `pub` does not make its fields public.
```
use crate::resturent::Breakfast;

mod resturent{
    pub struct Breakfast {
        pub toast: String,      // public
        seasonal_fruit: String, // private
    }

}

fn main() {
    let meal = Breakfast{
        toast: String::from("egg"),
        // seasonal_fruit: ... ❌ cannot set this here
    }
}
```
* you can access `toast` because it is public. But you can't access `seasonal_fruit` because it is private.
* Each struct field needs its own `pub`.

#### Public Enum
For an enum, making the enum `pub` automatically makes all variants public.
```
use crate::resturent::Appetizer;

mod resturent {
    // Enum variants become public automatically.
    pub enum Appetizer {
        Soup, 
        Salad,   
    }
}

fn main() {
    let order1 = Appetizer::Salad;
    let order2 = Appetizer::Soup;
}
```

## `use`
With `use`, we can make the path shorter:
```
use crate::resturent::customer;

mod resturent {
    pub mod customer {
        pub fn add_customer() {
            println!("Customer added");
        }
    }
}

fn main() {
    customer::add_customer();
}
```
#### Important: `use` has a scope
The shortcut only works where the `use` is written.
```
use crate::resturent::customer;

mod resturent {

    pub mod customer {
        pub fn add_customer() {
            println!("Customer added");
        }
    }

    pub mod serving {
        pub fn serve_order() {
        
            // here short path not working 
            // because `use` is outside `serving` module
            customer::add_customer();
            
            println!("Serving the order");
        }
    }
}

fn main() {
    customer::add_customer();
}
```
Use it inside `serving` module:
```
pub mod serving {
    use crate::resturent::customer;
    
    pub fn serve_order() {
        customer::add_customer();

        println!("Serving the order");
    }
}
```
**`use` creates a shortcut for a path, and that shortcut works only in its scope.**

### Idiomatic `use` Paths
#### For functions -> import the parent module
Instead of:
```
use crate::restaurant::customer::add_customer;

add_to_waitlist();
```
Prefer:
```
use crate::restaurant::customer;

customer::add_customer();
```
It makes it clear that `add_customer()` comes from the `customer` module.

#### For structs, enums, etc. → import the item directly
For example:
```
// Here we import an Enum `Appetizer` directly.
use crate::resturent::Appetizer;

mod resturent {
    pub enum Appetizer {
        Soup,
        Salad,
    }
}

fn main() {
    let order = Appetizer::Soup;
}
```

#### Same name → keep the parent module
Suppose two modules both have `order()` function:
```
mod resturent {
    pub mod kitchen {
        pub fn order() {
            println!("Kitchen order");
        }
    }

    pub mod customer {
        pub fn order() {
            println!("Customer order");
        }
    }
}
```
We cannot do this:
```
use crate::resturent::customer::order;
use crate::resturent::kitchen::order;
```
Both functions are named `order`, so there would be a name conflict. Instead, import the parent modules:
```
use crate::resturent::customer;  // import the parent modules
use crate::resturent::kitchen;   // import the parent modules

mod resturent {
    pub mod kitchen {
        pub fn order() {
            println!("Kitchen order");
        }
    }

    pub mod customer {
        pub fn order() {
            println!("Customer order");
        }
    }
}

fn main() {
    customer::order();
    kitchen::order();
}
```

### Providing New Names with the `as` Keyword
`as` is used to give another name (alias) to an imported item. This is useful when two items have the same name.  

For example, imagine a project uses two libraries that both have a type called `Config`:
```
use library_a::Config;
use library_b::Config as DatabaseConfig;
```
Now you can clearly use:
```
let config = Config::new();
let db_config = DatabaseConfig::new();
```
Without `as`, both would be called `Config`, causing a conflict.

### Nested `use` Paths
When multiple use statements have the same beginning, we can combine them into one line.   

Instead of:
```
use crate::resturent::customer;
use crate::resturent::kitchen;
```
We can write:
```
use crate::resturent::{customer, kitchen};
```
The common part is:
```
crate::resturent::
```

### Using External Packages
Rust lets you use packages made by other developers.  

#### 1. Add the package to `Cargo.toml`
For example, `rand`:
```
[dependencies]
rand = "0.10.1"
```
Cargo downloads it and makes it available to your project.
#### 2. Use it in your code
```
use rand::RngExt;

fn main() {
    let number = rand::rng().random_range(1..=100);
    println!("{number}")
}
```

# How to Create and Use a Library
We can create a library crate and use it from another Rust project.

### Create the library
#### 1. Creates a new Rust library package name `restaurant`
```
cargo new restaurant --lib
```
This creates:
```
restaurant/
├── Cargo.toml
└── src/
    └── lib.rs
```
#### 2. Create the module files
Final structure:
```
restaurant/
├── Cargo.toml
└── src/
    ├── lib.rs
    ├── restaurant.rs    ← parent module
    └── restaurant/
        └── customer.rs  ← submodule
        └── serving.rs   ← submodule
```
`src/lib.rs`
```
pub mod resturent;
```
`src/restaurant.rs`
```
pub mod customer;
pub mod serving;
```
`src/restaurant/customer.rs`
```
pub fn add_customer() {
    println!("Customer added");
}
```
`src/restaurant/serving.rs`
```
pub fn take_order() {
    println!("Taking customer order");
}

pub fn serve_order() {
    println!("Serving the order");
}
```

### Use this library to another project
#### 1. Create new Project
```
cargo new my_app
```
This creates:
```
my_app/
├── Cargo.toml
└── src/
    └── main.rs
```
#### 2. Add the library as a dependency
In `my_app/Cargo.toml`:
```
[dependencies]
restaurant = { path = "../restaurant" }
```
* `path` tells Cargo where the local library is located.
#### 3. Use the library
In `my_app/src/main.rs`:
```
use restaurant::resturent::{customer, serving};

fn main() {
    customer::add_customer();
    serving::take_order();
    serving::serve_order();
}
```