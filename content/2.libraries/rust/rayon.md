---
title: 'rayon'
description: 'data parallelism'
icon: 'fa6-solid:train'
---

<CardGroup cols={3}>
  <Card title="crates.io" icon="fa6-solid:box-open" href="https://crates.io/crates/rayon">View the crate</Card>
  <Card title="docs.rs" icon="fa6-solid:book" href="https://docs.rs/clap/latest/rayon">Read the docs</Card>
  <Card title="Repo" icon="fa6-solid:inbox" href="https://github.com/clap-rs/clap">Visit the Repo</Card>
</CardGroup>

---

## Description

Rayon is a data-parallelism library for Rust that aims to make it easy and efficient to convert sequential computations into parallel ones. It provides high-level constructs like parallel iterators and lower-level control through custom task spawning (`join`, `scope`).

Key features include:

*   **Ease of Use:** Designed to integrate parallelism into existing sequential code with minimal changes, often just by changing iterator methods (e.g., `.iter()` to `.par_iter()`).
*   **Performance:** Uses a work-stealing thread pool to efficiently distribute work among available CPU cores. It dynamically adapts to the workload, avoiding overhead for small tasks.
*   **Data Race Safety:** Guarantees data-race-free execution, leveraging Rust's ownership and borrowing rules.
*   **Rich API:** Offers parallel equivalents for many standard library iterator methods, parallel sorting, and ways to extend collections in parallel.
*   **Custom Task Management:** Provides functions like `rayon::join` and `rayon::scope` for more fine-grained control over task subdivision and execution.

## Basic Usage: The Rayon Prelude

To easily use parallel iterators and other high-level methods, Rayon provides a prelude module. Import it using `use rayon::prelude::*;` at the top of your Rust files where you use Rayon.

```rust
use rayon::prelude::*; // Import the prelude
```

This brings traits like `ParallelIterator`, `ParallelExtend`, `ParallelSliceMut`, etc., into scope, allowing you to call methods like `.par_iter()`, `.par_sort()`, `.par_extend()`.

## Core Concepts & Examples

Rayon offers several ways to introduce parallelism:

<Tabs>
  <Tab title="Parallel Iterators">
    Parallel iterators are the most common way to use Rayon. You can often convert a sequential iterator chain to a parallel one by changing `.iter()` or `.iter_mut()` to `.par_iter()` or `.par_iter_mut()`.

    ```rust
    use rayon::prelude::*;

    fn main() {
        let mut data = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

        // Sequential sum
        let seq_sum: i32 = data.iter().map(|x| x * x).sum();
        println!("Sequential sum of squares: {}", seq_sum);

        // Parallel sum using par_iter()
        let par_sum: i32 = data.par_iter() // Change iter() to par_iter()
                             .map(|x| x * x)
                             .sum(); // Many standard iterator methods have parallel versions
        println!("Parallel sum of squares: {}", par_sum);

        // Modify data in parallel using par_iter_mut()
        data.par_iter_mut().for_each(|x| *x *= 2);
        println!("Data doubled in parallel: {:?}", data);

        // Collect results into a new collection
        let squares: Vec<i32> = data.par_iter().map(|x| x * x).collect();
        println!("Parallel collection of squares: {:?}", squares);
    }
    ```
    **Explanation:**
    *   Import the `rayon::prelude::*`.
    *   Call `.par_iter()` or `.par_iter_mut()` on slices, `Vec`, and other collections that support it.
    *   Chain parallel iterator methods like `.map()`, `.filter()`, `.fold()`, `.reduce()`, `.sum()`, `.collect()`, etc. Rayon provides parallel implementations for these.

  </Tab>
  <Tab title="Parallel Sorting">
    Rayon provides `par_sort` and `par_sort_unstable` for sorting slices in parallel, which can be significantly faster for large datasets than the standard library's sequential sort.

    ```rust
    use rayon::prelude::*;

    fn main() {
        let mut large_vec: Vec<i32> = (0..1_000_000).rev().collect(); // Large reversed vector

        // Sort the vector in parallel
        large_vec.par_sort_unstable(); // Use par_sort for stable sorting

        println!("First 10 elements after parallel sort: {:?}", &large_vec[0..10]);
        // Verify the sort (optional)
        assert!(large_vec.windows(2).all(|w| w[0] <= w[1]));
        println!("Vector successfully sorted in parallel.");
    }
    ```
     **Explanation:**
    *   Import the `rayon::prelude::*`.
    *   Call `.par_sort()` or `.par_sort_unstable()` directly on a mutable slice or `Vec`.

  </Tab>
  <Tab title="`rayon::join`">
    `rayon::join` is used to split a task into two sub-tasks that *may* run in parallel. It's useful for recursive divide-and-conquer algorithms.

    ```rust
    use rayon::join;

    // Example: Parallel recursive sum (demonstration, not the most efficient way)
    fn parallel_sum(slice: &[i32]) -> i32 {
        if slice.len() < 1024 { // Base case: Use sequential sum for small slices
            slice.iter().sum()
        } else {
            let mid = slice.len() / 2;
            let (left, right) = slice.split_at(mid);

            // Recursively call parallel_sum on both halves, potentially in parallel
            let (sum_left, sum_right) = join(|| parallel_sum(left),
                                             || parallel_sum(right));

            sum_left + sum_right
        }
    }

    fn main() {
        let data: Vec<i32> = (1..=10_000).collect();
        let sum = parallel_sum(&data);
        println!("Parallel sum using join: {}", sum);
        assert_eq!(sum, data.iter().sum());
    }
    ```
    **Explanation:**
    *   `rayon::join(closure_a, closure_b)` takes two closures.
    *   Rayon's scheduler may run `closure_b` on another thread if one is available and idle (work-stealing). `closure_a` typically continues on the current thread.
    *   It waits for both closures to complete and returns their results as a tuple.

  </Tab>
  <Tab title="`rayon::scope` and `rayon::scope_fifo`">
    `rayon::scope` allows you to spawn multiple tasks that can borrow data from the current stack frame. The scope waits for all spawned tasks to complete before returning. `scope_fifo` provides First-In, First-Out ordering for tasks spawned from the *same* thread.

    ```rust
    use rayon::scope;
    use std::sync::atomic::{AtomicUsize, Ordering};

    fn main() {
        let mut data = vec![0; 10];
        let counter = AtomicUsize::new(0);

        // Create a scope to spawn tasks that can borrow `data` and `counter`
        scope(|s| {
            for (i, item) in data.iter_mut().enumerate() {
                // Spawn a task for each item
                s.spawn(move |_| { // The closure gets a Scope argument, often unused (_)
                    *item = i * 2; // Modify data borrowed from the outer scope
                    counter.fetch_add(1, Ordering::Relaxed); // Access shared atomic
                });
            }
        }); // Scope ends here, waits for all spawned tasks

        println!("Data after scope: {:?}", data);
        println!("Counter after scope: {}", counter.load(Ordering::Relaxed));
        assert_eq!(counter.load(Ordering::Relaxed), data.len());
    }
    ```
     **Explanation:**
    *   `rayon::scope(|s| { ... })` creates a scope `s`.
    *   Inside the closure, `s.spawn(|_| { ... })` spawns tasks.
    *   These tasks can safely borrow local variables from the function calling `scope`.
    *   The `scope` function blocks until all tasks spawned within it (and any tasks they spawn) are finished.

  </Tab>
</Tabs>

## Thread Pool Configuration

While Rayon manages a global thread pool by default (usually matching the number of logical CPU cores), you can customize it or create separate pools using `ThreadPoolBuilder`.

```rust
use rayon::ThreadPoolBuilder;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    // Configure the global thread pool (must be done before any Rayon operation)
    ThreadPoolBuilder::new()
        .num_threads(4) // Set the number of threads
        .build_global()?; // Apply to the global pool

    println!("Global pool configured to use {} threads.", rayon::current_num_threads());

    // Or create a custom pool
    let pool = ThreadPoolBuilder::new().num_threads(2).build()?;
    pool.install(|| {
        // Code run within this closure will use the custom pool
        println!("Running inside custom pool with {} threads.", rayon::current_num_threads());
        let sum: i32 = (0..1000).into_par_iter().sum();
        println!("Sum calculated in custom pool: {}", sum);
    });

     println!("Back to global pool with {} threads.", rayon::current_num_threads());


    Ok(())
}

```

Rayon provides a powerful yet accessible way to introduce parallelism into Rust applications, often leading to significant performance improvements on multi-core processors.
