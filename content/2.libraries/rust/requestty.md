---
title: 'requestty'
description: 'interactive command-line prompts'
icon: 'question'
---

<CardGroup cols={3}>
  <Card title="crates.io" icon="box-open" href="https://crates.io/crates/requestty">View the crate</Card>
  <Card title="docs.rs" icon="book" href="https://docs.rs/clap/latest/requestty">Read the docs</Card>
  <Card title="Repo" icon="inbox" href="https://github.com/clap-rs/clap">Visit the Repo</Card>
</CardGroup>

---

## Description

`requestty` (request-tty) is a Rust library designed to provide an easy-to-use collection of interactive command-line prompts, drawing inspiration from the popular JavaScript library Inquirer.js. It allows you to define various types of questions and prompt the user for input directly in the terminal.

Key features include:

*   **Multiple Question Types:** Supports various prompt types like input, password, confirm, list, checkbox, expand, etc.
*   **Flexible Question Definition:** Questions can be created using either a fluent builder pattern or convenient macros (requires the `macros` feature).
*   **Conditional Prompting:** Questions can be shown or skipped based on previous answers using the `.when()` method.
*   **Customizable Backends:** Supports different terminal backends (`crossterm` by default, `termion` available via feature flag).
*   **Extensibility:** Allows for the creation of custom prompt types by implementing the `Prompt` trait.

## Installation

Add `requestty` to your `Cargo.toml`:

```toml
[dependencies]
# Default features (crossterm backend)
requestty = "0.5.0"

# Example: Enable macros feature
requestty = { version = "0.5.0", features = ["macros"] }

# Example: Use termion backend instead of crossterm
# (disable default features first)
requestty = {
    version = "0.5.0",
    default-features = false,
    features = ["termion"]
}
```

The default features are `crossterm` (for the terminal backend) and `smallvec` (for some internal optimizations).

## Creating Questions

You can define questions using either the builder API or the `questions!` macro.

<Tabs>
  <Tab title="Builder API">
    ```rust
    use requestty::{Question, Answers, OnEsc};

    fn main() -> Result<(), requestty::ErrorKind> {
        let questions = vec![
            Question::input("name")
                .message("What is your name?")
                .build(),

            Question::expand("toppings")
                .message("What toppings do you want?")
                // Only ask if the name is not "Admin" (example condition)
                .when(|answers: &Answers| {
                    answers
                        .get("name")
                        .and_then(|ans| ans.as_string())
                        .map_or(true, |name| name != "Admin")
                })
                .choices(vec![
                    ('p', "Pepperoni and cheese"),
                    ('a', "All dressed"),
                    ('w', "Hawaiian"),
                ])
                .default_separator() // Add default separators between choices
                .on_esc(OnEsc::Terminate) // What to do if Esc is pressed
                .build(),

             Question::password("password")
                .message("Enter a password:")
                .mask('*') // Character to display instead of typed input
                .validate(|pass, _| {
                    if pass.len() < 8 {
                        Ok("Password must be at least 8 characters long".into())
                    } else {
                        Ok(None) // Indicates valid input
                    }
                })
                .build(),
        ];

        // We'll cover prompting in the next section
        // let answers = requestty::prompt(questions)?;
        // println!("{:?}", answers);

        Ok(())
    }
    ```
    **Explanation:**
    *   Each `Question::type()` function starts a builder chain.
    *   Methods like `.message()`, `.choices()`, `.when()`, `.validate()`, `.on_esc()`, `.mask()` configure the specific question.
    *   `.build()` finalizes the question definition.

  </Tab>
  <Tab title="Macros API (`macros` feature)">
    ```rust
    // Requires the `macros` feature:
    // requestty = { version = "0.5.0", features = ["macros"] }

    use requestty::{questions, Answers, OnEsc, Question}; // Need Question for type hints if needed

    fn main() -> Result<(), requestty::ErrorKind> {
        let questions = questions![
            Input {
                name: "name",
                message: "What is your name?"
            },
            Expand {
                name: "toppings",
                message: "What toppings do you want?",
                when: |answers: &Answers| {
                     answers
                        .get("name")
                        .and_then(|ans| ans.as_string())
                        .map_or(true, |name| name != "Admin")
                },
                choices: [
                    ('p', "Pepperoni and cheese"),
                    ('a', "All dressed"),
                    ('w', "Hawaiian"),
                ],
                on_esc: OnEsc::Terminate,
                // separator: // Can specify custom separators too
            },
            Password {
                 name: "password",
                 message: "Enter a password:",
                 mask: '*',
                 validate: |pass, _| {
                    if pass.len() < 8 {
                        Ok("Password must be at least 8 characters long".into())
                    } else {
                        Ok(None)
                    }
                 }
            }
        ];

        // We'll cover prompting in the next section
        // let answers = requestty::prompt(questions)?;
        // println!("{:?}", answers);

        Ok(())
    }
    ```
    **Explanation:**
    *   The `questions!` macro takes a list of question definitions.
    *   Each definition starts with the question type (`Input`, `Expand`, `Password`, etc.).
    *   Fields correspond to the builder methods (`name`, `message`, `choices`, `when`, etc.).
    *   Requires enabling the `macros` feature in `Cargo.toml`.
  </Tab>
</Tabs>

## Prompting Questions

Once you have defined your questions, you can prompt the user for answers.

<Tabs>
  <Tab title="Prompting Multiple Questions">
    ```rust
    use requestty::{Question, Answers};

    fn main() -> Result<(), requestty::ErrorKind> {
        let questions = vec![
            Question::input("first_name")
                .message("First name?")
                .build(),
            Question::input("last_name")
                .message("Last name?")
                .build(),
        ];

        // Prompt all questions in the iterator
        let answers: Answers = requestty::prompt(questions)?;

        // Access answers by name
        let first = answers.get("first_name").unwrap().as_string().unwrap();
        let last = answers.get("last_name").unwrap().as_string().unwrap();

        println!("Hello, {} {}!", first, last);

        Ok(())
    }
    ```
    **Explanation:**
    *   `requestty::prompt()` takes an iterator of `Question`s.
    *   It prompts them sequentially, respecting `when` conditions.
    *   Returns a `Result<Answers, ErrorKind>`.
    *   `Answers` is a map-like structure holding the results.

  </Tab>
  <Tab title="Prompting a Single Question">
    ```rust
    use requestty::{Question, Answer};

    fn main() -> Result<(), requestty::ErrorKind> {
        let question = Question::confirm("proceed")
            .message("Do you want to proceed?")
            .default(true) // Default value if user just presses Enter
            .build();

        // Prompt just one question
        let answer: Answer = requestty::prompt_one(question)?;

        if answer.as_bool().unwrap_or(false) {
             println!("Proceeding...");
        } else {
             println!("Aborting.");
        }

        Ok(())
    }
    ```
    **Explanation:**
    *   `requestty::prompt_one()` takes a single `Question`.
    *   Returns a `Result<Answer, ErrorKind>`.
    *   `Answer` is an enum representing the value for that single question.

  </Tab>
   <Tab title="Using PromptModule">
    ```rust
    use requestty::{PromptModule, Question, Answers};

    fn main() -> Result<(), requestty::ErrorKind> {
        // Can be created with `PromptModule::new()` or `prompt_module!` macro
        let mut prompt_module = PromptModule::new(vec![
             Question::input("part1").message("Enter part 1:").build(),
             Question::input("part2").message("Enter part 2:").build(),
        ]);

        // Prompt all remaining questions
        let answers1: Answers = prompt_module.prompt_all()?;
        println!("First run answers: {:?}", answers1);

        // You could potentially add more questions or re-prompt
        // prompt_module.add_question(...)

        // Prompt again, potentially using existing answers if questions have `when` clauses
        // Note: This example doesn't reuse answers effectively, but shows the pattern.
        // In a real scenario, you might prompt specific questions based on prior answers.
        // let answers2 = prompt_module.prompt_all()?;
        // println!("Second run answers: {:?}", answers2);


        Ok(())
    }
    ```
    **Explanation:**
    *   `PromptModule` allows more control over the prompting process.
    *   You can prompt all questions (`prompt_all`), specific questions (`prompt`), or add questions dynamically.
    *   It maintains the state of answers internally, which is useful for complex `when` conditions across multiple prompt calls.
  </Tab>
</Tabs>

## Optional Features

*   `macros`: Enables the `questions!` and `prompt_module!` macros.
*   `smallvec` (default): Uses `SmallVec` for potentially better performance with auto-completions.
*   `crossterm` (default): Uses the `crossterm` library for terminal interaction (drawing, events).
*   `termion`: Uses the `termion` library for terminal interaction instead of `crossterm`. You must disable default features to use this (`default-features = false`).

For more detailed examples and advanced usage, refer to the official `requestty` documentation and examples repository.
