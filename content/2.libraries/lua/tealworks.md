---
title: 'teal: How it Works'
description: 'Understand the workflow of writing, compiling, and running teal code.'
---

## Overview

Teal introduces static typing to Lua through a distinct workflow involving writing code in the teal dialect, compiling it using the `tl` tool, and then running the resulting standard Lua code. This process allows developers to catch type errors early while producing compatible Lua output.

The typical workflow involves these steps:

1.  **Write Code:** Create `.tl` files using Teal syntax, including type annotations.
2.  **Compile/Check:** Use the `tl` command-line tool to type-check the code (`tl check`) and/or generate Lua code (`tl gen`).
3.  **Compilation Process:** The compiler parses Teal code, performs type checking, and generates equivalent Lua code, potentially embedding compatibility layers.
4.  **Output:** The compiler produces standard `.lua` files.
5.  **Load/Run:** Execute the generated `.lua` files using any standard Lua interpreter, or load `.tl` files directly from Lua using `tl.loader()`.

## Step 1: Writing Teal Code

You write your programs in files with a `.tl` extension. This code looks very similar to Lua but includes type annotations for variables, function parameters, return values, and type definitions (records, enums, interfaces, etc.).

```lua
-- example.tl
local record Point
  x: number
  y: number
end

local function distance(p1: Point, p2: Point): number
  local dx = p1.x - p2.x
  local dy = p1.y - p2.y
  return math.sqrt(dx*dx + dy*dy)
end

local origin: Point = { x = 0, y = 0 }
local p: Point = { x = 3, y = 4 }

print(distance(origin, p))
```

## Step 2: Compilation / Type Checking

The `tl` command-line tool is used to process `.tl` files.

*   **`tl check <files...>`**: Parses and type-checks the specified `.tl` (or `.lua`) files, reporting any type errors or warnings found. It does *not* generate output files.
*   **`tl gen <files...>`**: Parses, type-checks, and generates corresponding `.lua` files for each input `.tl` file. Errors will halt generation.

## Step 3: The Compilation Process

When you run `tl gen` or `tl check`, the compiler performs several actions:

1.  **Parsing:** Reads the `.tl` file and understands its structure according to Teal's grammar.
2.  **Type Checking:** This is the core step. The compiler analyzes the code, verifies that all type annotations are consistent, infers types where possible, and ensures that operations are performed on compatible types according to the rules defined in the Teal language reference. It reports errors if inconsistencies are found.
3.  **Lua Code Generation (`tl gen` only):** If type checking passes, the compiler translates the Teal code into standard Lua code. This involves:
    *   Removing type annotations.
    *   Potentially adding compatibility code based on target settings (see below).
    *   Expanding `macroexp` calls inline.
    *   Ensuring the generated code reflects the logic of the original Teal program.

### Compiler Options

The compilation process can be configured via command-line flags or a `tlconfig.lua` file in the project root.

<ParamField body="--gen-target" type="string" default="auto">
  (`gen_target` in config) Minimum targeted Lua version for generated code. Affects generated operators (`//`, bitwise) and compatibility features.
  **Values:** `"5.1"` (for Lua 5.1, 5.2, LuaJIT), `"5.3"` (for Lua 5.3+), `"5.4"` (like 5.3, but enables `<close>` attribute, requires `--gen-compat=off`).
  **Default:** Inferred based on the Lua version running `tl`. Standalone binaries default to `5.3`.
</ParamField>

<ParamField body="--gen-compat" type="string" default="optional">
  (`gen_compat` in config) Controls generation of code using the `lua-compat-5.3` library for consistent standard library behavior on older Lua versions (5.1/5.2/JIT).
  **Values:**
  *   `"off"`: No compatibility code generated. Relies solely on the target Lua VM's standard library.
  *   `"optional"`: Generates code that tries to `pcall(require, "compat53")`. Uses compat library if available, otherwise falls back to native behavior.
  *   `"required"`: Generates code that directly `require("compat53")`. Fails if the library isn't installed on Lua < 5.3.
</ParamField>

<ParamField body="-I --include-dir" type="string[]">
  (`include_dir` in config) Directory to prepend to the module search path (`package.path`). Can be specified multiple times.
</ParamField>

<ParamField body="--global-env-def" type="string">
  (`global_env_def` in config) Path to a Teal declaration file (`.d.tl`) defining custom global variables available in your execution environment.
</ParamField>

<ParamField body="--wdisable" type="string[]">
  (`disable_warnings` in config) List of warning codes to suppress.
</ParamField>

<ParamField body="--werror" type="string[]">
  (`warning_error` in config) List of warning codes to treat as errors.
</ParamField>

<ParamField body="--keep-hashbang" type="boolean">
  (`gen` command only) Preserve the `#!` line from the input `.tl` file in the output `.lua` file.
</ParamField>

<ParamField body="-p --pretend" type="boolean">
  (`gen` command only) Perform type checking and report which files *would* be generated, but don't actually write any `.lua` files.
</ParamField>

<ParamField body="-l --require" type="string[]">
  (`run` command only) Require the specified Lua module before executing the script (similar to `lua -l`). Can be specified multiple times.
</ParamField>

## Step 4: Output

Running `tl gen example.tl` successfully produces an `example.lua` file containing standard Lua code.

```lua
-- example.lua (Generated from example.tl above)
local function distance(p1, p2)
  local dx = p1.x - p2.x
  local dy = p1.y - p2.y
  return math.sqrt(dx * dx + dy * dy)
end
local origin = {x = 0, y = 0}
local p = {x = 3, y = 4}
print(distance(origin, p))
```
Notice how the type annotations and the `record` definition are gone, leaving standard Lua.

## Step 5: Loading and Running

You have several options for running your Teal code:

1.  **Run Compiled Lua:** Execute the generated `.lua` file with any standard Lua interpreter: `lua example.lua`.
2.  **Load `.tl` Directly:** Use the Teal loader within your Lua application. This allows `require` to load `.tl` files on the fly (compiling them in memory):
    ```lua
    -- main.lua
    require("tl").loader()
    local my_module = require("example") -- Loads and compiles example.tl
    -- Use my_module...
    ```
3.  **Run Directly with `tl`:** Use `tl run example.tl`. This type-checks and executes the Teal file directly using the Lua interpreter `tl` itself is running on.

This workflow allows you to benefit from static type checking during development while deploying standard, compatible Lua code.
