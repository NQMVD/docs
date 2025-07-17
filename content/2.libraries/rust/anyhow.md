---
title: "anyhow"
description: "easier application-level error handling"
icon: "fa6-solid:xmark"
---

<CardGroup cols={3}>
  <Card title="crates.io" icon="fa6-solid:box-open" href="https://crates.io/crates/anyhow">View the crate</Card>
  <Card title="docs.rs" icon="fa6-solid:book" href="https://docs.rs/clap/latest/anyhow">Read the docs</Card>
  <Card title="Repo" icon="fa6-solid:inbox" href="https://github.com/clap-rs/clap">Visit the Repo</Card>
</CardGroup>

---

## Description

`anyhow` is designed to make error handling in applications significantly easier and more ergonomic. It provides a concrete error type, `anyhow::Error`, which can wrap any type that implements the standard `std::error::Error` trait.

Key features include:

1.  **`anyhow::Result<T>`:** A convenient type alias for `Result<T, anyhow::Error>`.
2.  **Easy Error Wrapping:** The `?` operator seamlessly converts standard library or other library errors into `anyhow::Error`.
3.  **Adding Context:** Easily attach custom messages (context) to errors as they propagate up the call stack, making debugging much simpler.
4.  **Backtraces:** Automatically captures a backtrace when an error is created (requires environment variable `RUST_BACKTRACE=1` or `RUST_BACKTRACE=full` at runtime).
5.  **Simple Error Creation:** Macros like `anyhow!` and `bail!` allow for quick creation of new errors.

`anyhow` is primarily intended for use in **applications**. For libraries, it's often better to define custom error types to provide more specific information to downstream users. A similar library often compared to `anyhow` is `eyre`.

## When to use `anyhow`

*   **In Applications (`main.rs`, binaries):** When you need to handle various error types from different libraries without defining complex custom error enums for every combination.
*   **Simplifying `main`:** Allows `main` to return `anyhow::Result<()>` for concise top-level error handling.
*   **Adding Context:** When you want to add meaningful messages to errors as they propagate (e.g., "Failed to read configuration file" -> "Failed to parse user settings").
*   **Prototypes and Scripts:** Quick development where detailed custom error types are overkill.

## Examples

Here are some common patterns for using `anyhow`.

```rust
// Add anyhow to your Cargo.toml:
// anyhow = "1.0"

use anyhow::{anyhow, bail, Context, Result}; // Import common items
use std::fs;
use std::path::Path;

// Function that returns anyhow::Result
// It can use '?' on functions returning std::io::Result, std::num::ParseIntError, etc.
fn read_and_parse_number(path: &Path) -> Result<i32> {
    let content = fs::read_to_string(path)
        .with_context(|| format!("Failed to read file content from: {}", path.display()))?;

    let number = content
        .trim()
        .parse::<i32>()
        .context("Failed to parse content into an integer")?; // Add context to the parse error

    Ok(number)
}

// Example of creating a new error or bailing early
fn check_positive(number: i32) -> Result<()> {
    if number <= 0 {
        // bail! creates an error and returns immediately
        bail!("Number must be positive, got {}", number);
    }
    Ok(())
}

// Using anyhow! to create a generic error
fn might_fail(should_fail: bool) -> Result<()> {
    if should_fail {
        // anyhow! creates an error that can be returned
        return Err(anyhow!("Something went wrong intentionally"));
    }
    Ok(())
}

// main can return anyhow::Result for easy top-level error handling
fn main() -> Result<()> {
    let path = Path::new("number.txt");

    // Create a dummy file for the example
    fs::write(path, "42")?;

    // Use '?' to propagate errors easily
    let number = read_and_parse_number(path)?;
    println!("Read number: {}", number);

    check_positive(number)?;
    println!("Number is positive.");

    might_fail(false)?; // This one succeeds
    println!("might_fail(false) succeeded.");

    // Example of handling a potential error from might_fail
    match might_fail(true) {
        Ok(_) => println!("This won't print."),
        Err(e) => {
            println!("Caught expected error from might_fail(true): {:?}", e);
            // Example of printing the error chain with context and backtrace (if enabled)
            // eprintln!("Error details: {:#}", e);
        }
    }

    // Clean up the dummy file
    fs::remove_file(path)?;

    // Example with a non-existent file to show context
    match read_and_parse_number(Path::new("non_existent.txt")) {
        Ok(_) => println!("This won't print."),
        Err(e) => {
            println!("\nDemonstrating error context:");
            // The output will show the chain of errors and context messages
            // Use {:#} for detailed output including causes (and backtrace if enabled)
            eprintln!("Error: {:#}", e);
        }
    }


    Ok(()) // Indicate success
}

```

**Running the Example:**

1.  Save the code as `main.rs`.
2.  Run `cargo run`. You'll see the successful operations and the caught error from `might_fail(true)`.
3.  You'll also see the detailed error output for the non-existent file, showing the context added by `.with_context()` and `.context()`.
4.  To see backtraces (if the error originated in your code or dependencies compiled with debug symbols), run: `RUST_BACKTRACE=1 cargo run`.
