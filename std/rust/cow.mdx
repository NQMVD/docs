---
title: "Cow"
description: "cow - copy on write"
---

## Rust Tutorial: Understanding and Using `Cow<'a, T>`

### 1. What is `Cow`?

`Cow<'a, T>` stands for "Clone-on-Write". It's an enum defined in `std::borrow` that can hold either borrowed data (`Borrowed`) or owned data (`Owned`).

The key idea is **lazy cloning**: `Cow` allows immutable access to data without caring whether it's borrowed or owned. It only performs a clone *if and when* you request mutable access or explicit ownership of data that was originally borrowed.

It's defined roughly like this (simplified):

```rust
enum Cow<'a, T>
where
    T: ?Sized + ToOwned,
{
    Borrowed(&'a T),
    Owned(<T as ToOwned>::Owned),
}
```

*   `'a`: This is the lifetime associated with the borrowed data, if any.
*   `T: ?Sized + ToOwned`: The type `T` can be dynamically sized (like `str` or `[u8]`) and must implement the `ToOwned` trait. The `ToOwned` trait is crucial because it defines how to create an owned version of `T` (e.g., `String` is the owned version of `str`, `Vec<u8>` is the owned version of `[u8]`).

### 2. Why Use `Cow`?

The primary benefit is **performance optimization by avoiding unnecessary allocations and copies**.

Consider a function that processes a string. Sometimes the string might need modification (like trimming whitespace), but other times it might be perfectly fine as is.

*   **Without `Cow`:** You might always take `String` as input, forcing the caller to clone even if no modification is needed. Or, you might take `&str` and *always* clone it inside the function if modification *might* be needed.
*   **With `Cow`:** You can accept input that *could* be borrowed (`&str`) or owned (`String`). The function only clones the string to an owned `String` if modification is actually required.

### 3. How to Use `Cow`

#### a) Creating a `Cow`

You can create `Cow` instances from borrowed or owned data. The `From` trait is often the most convenient way.

```rust
use std::borrow::Cow;

fn main() {
    // From borrowed data
    let borrowed_str: &str = "Hello";
    let cow_borrowed: Cow<'_, str> = Cow::Borrowed(borrowed_str);
    let cow_borrowed_from: Cow<'_, str> = Cow::from(borrowed_str); // Using From

    println!("Cow from borrowed: {:?}", cow_borrowed); // Prints: Borrowed("Hello")

    // From owned data
    let owned_string: String = String::from("World");
    // Need to specify the type T for Cow if using From with owned data sometimes
    let cow_owned: Cow<'_, str> = Cow::Owned(owned_string.clone()); // Clone needed if owned_string is used later
    let cow_owned_from: Cow<'_, str> = Cow::from(owned_string); // Using From (consumes owned_string)

    println!("Cow from owned: {:?}", cow_owned_from); // Prints: Owned("World")

    // Example with Vec<u8> and &[u8]
    let borrowed_slice: &[u8] = &[1, 2, 3];
    let cow_slice_borrowed: Cow<'_, [u8]> = Cow::from(borrowed_slice);

    let owned_vec: Vec<u8> = vec![4, 5, 6];
    let cow_slice_owned: Cow<'_, [u8]> = Cow::from(owned_vec);

    println!("Cow slice borrowed: {:?}", cow_slice_borrowed); // Prints: Borrowed([1, 2, 3])
    println!("Cow slice owned: {:?}", cow_slice_owned); // Prints: Owned([4, 5, 6])
}
```

#### b) Accessing Data (Immutable)

`Cow` implements the `Deref` trait, meaning you can use the `*` operator or automatic deref coercion to get an immutable reference (`&T`) to the underlying data, regardless of whether it's `Borrowed` or `Owned`.

```rust
use std::borrow::Cow;

fn print_length(data: &Cow<'_, str>) {
    // Deref coercion allows calling &str methods directly
    println!("Length: {}", data.len());
}

fn main() {
    let cow_borrowed: Cow<'_, str> = Cow::from("Immutable");
    let cow_owned: Cow<'_, str> = Cow::from(String::from("Data"));

    print_length(&cow_borrowed); // Output: Length: 9
    print_length(&cow_owned);   // Output: Length: 4

    // Explicit deref:
    let borrowed_ref: &str = &*cow_borrowed;
    println!("Explicit deref: {}", borrowed_ref); // Output: Explicit deref: Immutable
}
```

#### c) Getting Mutable Access (`to_mut`)

This is where the "Clone-on-Write" happens. The `to_mut(&mut self) -> &mut T::Owned` method provides mutable access.

*   If the `Cow` is `Owned`, it returns a mutable reference to the existing owned data.
*   If the `Cow` is `Borrowed`, it **clones** the borrowed data into a new owned instance, updates the `Cow` to `Owned`, and then returns a mutable reference to the *newly owned* data.

```rust
use std::borrow::Cow;

fn main() {
    let mut cow_borrowed: Cow<'_, str> = Cow::from("initial");
    let mut cow_owned: Cow<'_, str> = Cow::from(String::from("owned"));

    // --- Case 1: Borrowed ---
    println!("Before to_mut (borrowed): {:?}", cow_borrowed); // Borrowed("initial")
    // This clones the data because it was Borrowed
    let mutable_ref1: &mut String = cow_borrowed.to_mut();
    mutable_ref1.push_str(" modified");
    println!("After to_mut (borrowed): {:?}", cow_borrowed); // Owned("initial modified")
    println!("Modified value: {}", mutable_ref1); // initial modified

    // --- Case 2: Owned ---
    println!("Before to_mut (owned): {:?}", cow_owned); // Owned("owned")
    // This does *not* clone, just returns a mutable ref to the existing String
    let mutable_ref2: &mut String = cow_owned.to_mut();
    mutable_ref2.make_ascii_uppercase();
    println!("After to_mut (owned): {:?}", cow_owned); // Owned("OWNED")
    println!("Modified value: {}", mutable_ref2); // OWNED
}
```

#### d) Getting Owned Data (`into_owned`)

The `into_owned(self) -> T::Owned` method consumes the `Cow` and returns the owned version of the data.

*   If the `Cow` is `Owned`, it returns the owned data directly (no clone).
*   If the `Cow` is `Borrowed`, it clones the borrowed data and returns the new owned instance.

```rust
use std::borrow::Cow;

fn main() {
    let cow_borrowed: Cow<'_, str> = Cow::from("borrowed data");
    let cow_owned: Cow<'_, str> = Cow::from(String::from("owned data"));

    // Clones the borrowed data into a new String
    let owned1: String = cow_borrowed.into_owned();
    println!("From borrowed: {}", owned1); // borrowed data

    // Moves the existing String out of the Cow (no clone)
    let owned2: String = cow_owned.into_owned();
    println!("From owned: {}", owned2); // owned data

    // cow_borrowed and cow_owned are moved here and cannot be used anymore
    // println!("{:?}", cow_borrowed); // Error: use of moved value
}
```

### 4. Common Use Cases

#### a) Function Arguments

Accepting `Cow` allows flexibility for the caller.

```rust
use std::borrow::Cow;

// This function might add a prefix, requiring ownership.
// It takes Cow to avoid forcing clones on the caller if no prefix is needed.
fn ensure_prefix<'a>(s: Cow<'a, str>, prefix: &str) -> Cow<'a, str> {
    if s.starts_with(prefix) {
        s // No change needed, return the original Cow (Borrowed or Owned)
    } else {
        // Need to modify, so we ensure it's owned and modify it.
        let mut owned_s = s.into_owned(); // Clones if s was Borrowed
        owned_s.insert_str(0, prefix);
        Cow::Owned(owned_s) // Return the new Owned Cow
    }
}

fn main() {
    let user_input1 = "data";
    let user_input2 = String::from("PREFIX_data");

    // Pass a borrowed string slice
    let result1 = ensure_prefix(Cow::from(user_input1), "PREFIX_");
    println!("Result 1: {:?}", result1); // Owned("PREFIX_data") - Cloned here

    // Pass an owned string
    let result2 = ensure_prefix(Cow::from(user_input2), "PREFIX_");
    println!("Result 2: {:?}", result2); // Owned("PREFIX_data") - No clone inside ensure_prefix
                                         // (but was already owned)

    // Example where no modification is needed
    let user_input3 = "PREFIX_already";
    let result3 = ensure_prefix(Cow::from(user_input3), "PREFIX_");
    println!("Result 3: {:?}", result3); // Borrowed("PREFIX_already") - No clone
}
```

#### b) Function Return Values

Returning `Cow` allows a function to return borrowed data (if no modifications occurred) or owned data (if modifications were necessary). This is often seen in parsing or sanitization functions.

```rust
use std::borrow::Cow;

// Removes leading/trailing whitespace. Returns owned only if changes were made.
fn trim_whitespace(input: &str) -> Cow<'_, str> {
    let trimmed = input.trim();
    if trimmed.len() == input.len() {
        // No whitespace was removed, return borrowed
        Cow::Borrowed(input)
    } else {
        // Whitespace removed, return owned
        Cow::Owned(trimmed.to_string())
    }
}

fn main() {
    let s1 = "  needs trimming  ";
    let s2 = "no trimming needed";

    let result1 = trim_whitespace(s1);
    let result2 = trim_whitespace(s2);

    println!("Result 1: {:?}", result1); // Owned("needs trimming")
    println!("Result 2: {:?}", result2); // Borrowed("no trimming needed")

    // We can use the results uniformly
    println!("Processed 1: {}", result1);
    println!("Processed 2: {}", result2);
}
```

### 5. When *Not* to Use `Cow`

*   **Always Need Ownership:** If your function *always* needs to modify the data or store it beyond the input lifetime, just take an owned type (`String`, `Vec<T>`) directly.
*   **Always Borrowed:** If you only ever need read-only access, just use a reference (`&str`, `&[T]`).
*   **Cheap Clones:** For types that are `Copy` or very cheap to clone (like small arrays or simple structs), the overhead of `Cow` (enum discriminant check, potential indirection) might outweigh the benefit of avoiding the cheap clone. `Cow` shines with heap-allocated types like `String` and `Vec`.
*   **Performance Criticality:** In *very* hot loops, the branching logic inside `to_mut` or `into_owned` *might* have a measurable impact, though this is rare. Profile first!
