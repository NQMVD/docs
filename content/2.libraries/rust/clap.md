# clap

parse command-line arguments

::card-group
---
cols: 3
---
::card
---
title: crates.io
icon: fa6-solid:box-open
to: https://crates.io/crates/clap
---
View the crate
::

::card
---
title: docs.rs
icon: fa6-solid:book
to: https://docs.rs/clap/latest/clap
---
Read the docs
::

::card
---
title: Repo
icon: fa6-solid:inbox
to: https://github.com/clap-rs/clap
---
Visit the Repo
::
::

---

## Description

`clap` (Command Line Argument Parser) allows you to define your application's CLI interface declaratively or programmatically, handling argument parsing, validation, help message generation (`--help`), and version information (`--version`) automatically.

It offers two main ways to define your CLI:

1.  **Derive API:** Uses procedural macros (`#[derive(Parser)]`) on structs to define arguments based on struct fields and attributes. This is often the quickest and most idiomatic way for simpler to moderately complex CLIs.
2.  **Builder API:** Provides a programmatic, fluent interface to build the argument parser step-by-step. This offers more flexibility and control, especially for complex or dynamically generated CLIs.

## Derive vs. Builder API

Choosing between the derive and builder APIs depends on your project's needs and complexity. Here's a quick comparison:

| Feature         | Derive API                     | Builder API                      |
| --------------- | ------------------------------ | -------------------------------- |
| **Ease of Use** | Generally easier for simple cases | More verbose, steeper learning curve |
| **Boilerplate** | Less boilerplate code          | More explicit code required      |
| **Flexibility** | Less flexible for dynamic CLIs | Highly flexible, good for complex logic |
| **Compile Time**| Can increase compile times     | Generally faster compile times   |
| **Readability** | Often very clear and concise   | Can become verbose               |
| **IDE Support** | Relies on macro expansion      | Standard Rust code analysis      |

## Examples

Here are basic examples demonstrating how to achieve the same simple CLI structure using both approaches. Let's create a tool that takes a name and an optional count.

::code-group
```rust [Derive API]
// Add clap to your Cargo.toml:
// clap = { version = "4.5", features = ["derive"] }

use clap::Parser;

/// Simple program to greet a person
#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct Args {
    /// Name of the person to greet
    #[arg(short, long)]
    name: String,

    /// Number of times to greet
    #[arg(short, long, default_value_t = 1)]
    count: u8,
}

fn main() {
    let args = Args::parse();

    for _ in 0..args.count {
        println!("Hello {}!", args.name);
    }
}
```

```rust [Builder API]
// Add clap to your Cargo.toml:
// clap = "4.5"

use clap::{Arg, Command}; // Note: Command is used instead of Parser

fn main() {
    let matches = Command::new("Greeter")
        .version("1.0")
        .author("Noah <noah@example.com>")
        .about("Simple program to greet a person")
        .arg(
            Arg::new("name")
                .short('n')
                .long("name")
                .value_name("NAME")
                .help("Name of the person to greet")
                .required(true), // Explicitly mark as required
        )
        .arg(
            Arg::new("count")
                .short('c')
                .long("count")
                .value_name("COUNT")
                .help("Number of times to greet")
                .value_parser(clap::value_parser!(u8)) // Specify the type parser
                .default_value("1"), // Default values are strings here
        )
        .get_matches();

    // Retrieve the values using the names defined in Arg::new()
    // We need to unwrap because get_one returns an Option<&T>
    let name = matches
        .get_one::<String>("name")
        .expect("`name` is required");
    let count = matches
        .get_one::<u8>("count")
        .expect("`count` has a default value");

    for _ in 0..*count {
        println!("Hello {}!", name);
    }
}
```
::

**Explanation:**
*   We derive `Parser` for our `Args` struct.
*   Doc comments (`///`) on the struct and fields generate help messages.
*   `#[command(...)]` sets top-level app information (author, version, about).
*   `#[arg(...)]` configures individual arguments (short/long flags, default values).
*   `Args::parse()` handles the actual parsing based on the struct definition.

Choose the API that best suits the complexity and maintainability requirements of your specific project! For many common cases, the `derive` API offers a great balance of power and conciseness.