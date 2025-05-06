---
title: "Box"
description: "box - values on the heap"
---

## Rust Tutorial: Understanding and Using `Box<T>`

### 1. What is `Box<T>`?

`Box<T>`, often just called a "box", is the simplest smart pointer in Rust's standard library (`std::boxed::Box`). Its main purpose is to **allocate data on the heap** instead of the stack.

Think of it like this:

*   Normally, when you create a variable like `let x: i32 = 5;`, that `5` is stored directly on the **stack**.
*   When you create `let b: Box<i32> = Box::new(5);`, the `5` is stored on the **heap**, and the `Box` itself (which is essentially a pointer to that heap location) is stored on the stack.

`Box<T>` guarantees:

*   **Heap Allocation:** The value of type `T` lives on the heap.
*   **Unique Ownership:** A `Box<T>` uniquely owns the data it points to. When the `Box` goes out of scope, it is `drop`ped, and the heap memory it manages is automatically deallocated. This is Rust's RAII (Resource Acquisition Is Initialization) principle in action – no manual memory management (`malloc`/`free` or `new`/`delete`) is needed.
*   **Known Size:** A `Box<T>` itself has a known size at compile time (it's just a pointer), even if the `T` it points to does *not* have a known size at compile time (like a trait object).

### 2. Why Use `Box<T>`?

You typically reach for `Box<T>` in specific scenarios:

1.  **Recursive Types:** When defining types that could theoretically be infinitely large, like a linked list or a tree node that contains itself. Storing parts of the structure inside a `Box` breaks the infinite recursion for the compiler's size calculation.
2.  **Trait Objects:** When you want to store different concrete types that implement the same trait, but you don't know the exact type at compile time (dynamic dispatch). `Box<dyn MyTrait>` allows this because the `Box` provides a pointer with a known size, hiding the unknown size of the actual underlying type.
3.  **Large Data Transfer:** To transfer ownership of large amounts of data without copying it on the stack. Since the `Box` (a pointer) is small, moving the `Box` is cheap, even if the data it points to on the heap is huge.
4.  **Explicit Heap Allocation:** When you specifically *want* data on the heap, perhaps to ensure it has a stable memory address even if the owner moves.

### 3. How to Use `Box<T>`

#### a) Creating a `Box`

The primary way to create a box is using `Box::new()`:

```rust
fn main() {
    // Allocate an i32 on the heap
    let five: Box<i32> = Box::new(5);
    println!("Box value: {}", five); // Prints: Box value: 5

    // Allocate a struct on the heap
    #[derive(Debug)]
    struct Point {
        x: f64,
        y: f64,
    }
    let point_on_heap: Box<Point> = Box::new(Point { x: 0.0, y: 1.0 });
    println!("Boxed point: {:?}", point_on_heap); // Prints: Boxed point: Point { x: 0.0, y: 1.0 }
}
```

#### b) Accessing Data (Dereferencing)

`Box<T>` implements the `Deref` and `DerefMut` traits. This means you can access the data inside the box using the dereference operator (`*`) or rely on deref coercion for method calls and field access.

```rust
fn main() {
    let mut b = Box::new(10);

    // Using the dereference operator *
    let value: i32 = *b; // Copies the value out (if T is Copy)
    println!("Value: {}", value); // Value: 10

    // Modifying the value through * (requires mut Box)
    *b = 20;
    println!("New value via *: {}", *b); // New value via *: 20

    // Using deref coercion for methods/fields
    #[derive(Debug)]
    struct Data {
        value: String,
    }
    impl Data {
        fn print_value(&self) {
            println!("Data value: {}", self.value);
        }
    }

    let mut boxed_data = Box::new(Data {
        value: String::from("Hello"),
    });

    // No need for * here due to deref coercion
    boxed_data.print_value(); // Data value: Hello
    println!("Accessing field: {}", boxed_data.value); // Accessing field: Hello

    // Modifying via deref coercion (requires mut Box)
    boxed_data.value.push_str(" Box!");
    boxed_data.print_value(); // Data value: Hello Box!
}
```

#### c) Dropping and Memory Deallocation

You don't manually free the memory. When the `Box<T>` variable goes out of scope, Rust automatically calls its `drop` implementation, which deallocates the heap memory.

```rust
fn main() {
    println!("Entering scope...");
    {
        // Allocate data on the heap
        let my_box = Box::new(vec![1, 2, 3]);
        println!("Box created with data: {:?}", *my_box);
        // my_box goes out of scope here
        // Rust automatically calls drop on my_box
        // The heap memory for the Vec is deallocated
    }
    println!("...Exited scope. Memory freed.");

    // Example showing Drop order
    struct MyStruct(i32);
    impl Drop for MyStruct {
        fn drop(&mut self) {
            println!("Dropping MyStruct({})!", self.0);
        }
    }

    println!("Creating boxed MyStruct...");
    let boxed_struct = Box::new(MyStruct(100));
    println!("Boxed MyStruct created.");
    // boxed_struct goes out of scope here
    // First, MyStruct's drop is called
    // Then, the Box's memory is freed
}
// Output:
// Entering scope...
// Box created with data: [1, 2, 3]
// ...Exited scope. Memory freed.
// Creating boxed MyStruct...
// Boxed MyStruct created.
// Dropping MyStruct(100)!
```

### 4. Common Use Cases Examples

#### a) Recursive Data Structures

Without `Box`, this definition would be infinitely large:

```rust
// Error: recursive type `List` has infinite size
// enum List {
//     Cons(i32, List),
//     Nil,
// }
```

Using `Box` breaks the cycle by adding indirection:

```rust
#[derive(Debug)]
enum List {
    Cons(i32, Box<List>), // Box<List> has a known size (pointer size)
    Nil,
}

use List::{Cons, Nil};

fn main() {
    let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
    println!("My recursive list: {:?}", list);
    // Output: My recursive list: Cons(1, Cons(2, Cons(3, Nil)))
} // list goes out of scope, all heap nodes are recursively dropped
```

#### b) Trait Objects

Store different shapes (which have different sizes) in one vector using `Box<dyn Trait>`.

```rust
trait Draw {
    fn draw(&self);
}

struct Circle {
    radius: f64,
}
impl Draw for Circle {
    fn draw(&self) {
        println!("Drawing a circle with radius {}", self.radius);
    }
}

struct Square {
    side: f64,
}
impl Draw for Square {
    fn draw(&self) {
        println!("Drawing a square with side {}", self.side);
    }
}

fn main() {
    // We need Box because Circle and Square have different sizes.
    // Vec<Box<dyn Draw>> stores pointers (all same size) on the stack,
    // pointing to the actual Circle/Square objects on the heap.
    let shapes: Vec<Box<dyn Draw>> = vec![
        Box::new(Circle { radius: 1.0 }),
        Box::new(Square { side: 2.0 }),
        Box::new(Circle { radius: 0.5 }),
    ];

    for shape in shapes {
        // Calls the correct draw() method via dynamic dispatch
        shape.draw();
    }
    // shapes vector goes out of scope
    // Each Box is dropped, deallocating the Circle/Square on the heap
}
// Output:
// Drawing a circle with radius 1
// Drawing a square with side 2
// Drawing a circle with radius 0.5
```

### 5. When *Not* to Use `Box`

*   **Default to Stack:** Prefer stack allocation when possible. It's generally faster (no allocator overhead, better cache locality). Use `Box` only when you *need* heap allocation for one of the reasons above.
*   **Shared Ownership:** If multiple owners need access to the same heap data, `Box` is not suitable because it enforces unique ownership. Use `Rc<T>` (Reference Counting) for single-threaded shared ownership or `Arc<T>` (Atomic Reference Counting) for multi-threaded shared ownership.
*   **Borrowing:** If you only need temporary access to data owned by someone else, use references (`&T` or `&mut T`) instead of transferring ownership with `Box`.
