---
title: 'Teal Language Reference'
description: 'A reference guide for Teal, a typed dialect of Lua.'
---

## Introduction

Teal is a typed dialect of Lua. It adds static type checking to Lua, allowing developers to catch a class of errors at compile time rather than runtime. Teal code compiles directly to Lua code. This document serves as a reference guide to Teal's syntax and features, assuming familiarity with Lua.

**Key Benefits:**

*   **Compile-Time Safety:** Catches type mismatches, typos in field names, incorrect argument counts, etc., before running the code.
*   **Improved Explicitness:** Requires clear declaration of data types, making code easier to understand and maintain.
*   **Documentation:** Type annotations serve as checked documentation.
*   **Compatibility:** Compiles to standard Lua and aims for compatibility with existing Lua code and object models.

## Installation

Teal requires a Lua environment (Lua + LuaRocks). Install the Teal compiler (`tl`) using LuaRocks:

```bash
luarocks install tl
```

This provides the `tl` command-line tool.

## Core `tl` Commands

*   `tl check <file...>`: Type-checks Teal (`.tl`) or Lua (`.lua`) files.
*   `tl gen <file...>`: Generates corresponding Lua (`.lua`) files from Teal (`.tl`) files.
*   `tl run <file.tl>`: Type-checks and runs a Teal file directly.

## Loading Teal Modules

1.  **Generate Lua:** Use `tl gen your_module.tl` to create `your_module.lua`, then `require("your_module")` in Lua as usual.
2.  **Teal Loader:** Use the Teal package loader to `require` `.tl` files directly from Lua (type checking only occurs via `tl check` or `tl run`, not during Lua runtime loading):
    ```lua
    -- In your Lua script
    local tl = require("tl")
    tl.loader()
    local my_module = require("your_module") -- Loads your_module.tl directly
    ```

## Basic Types

Teal uses Lua's runtime types but allows for more specific static declarations.

*   `any`: Represents any Lua value. Type checking is bypassed; use with caution, often requires `as` casts.
*   `nil`: The `nil` type and value. Any type in Teal can hold `nil`.
*   `boolean`: Represents `true` or `false`.
*   `number`: Represents floating-point numbers (Lua's default number type).
*   `integer`: Represents integer numbers. An `integer` is a subtype of `number`. Precision depends on the underlying Lua VM.
*   `string`: Represents sequences of bytes.
*   `thread`: Represents a Lua coroutine.

## Local Variables

Variables declared with `local` must have their type determined either by explicit annotation or initialization.

```lua
-- Explicit type annotation
local name: string
local count: integer
local is_ready: boolean

-- Type inferred from initialization
local message = "Hello" -- inferred as string
local value = 10.5      -- inferred as number
local items = {}        -- Initially ambiguous, type inferred on first use (see Arrays/Maps)

-- Initialization with nil requires annotation
local price: number = nil
local user: UserRecord = nil -- UserRecord is a hypothetical record type

-- Multiple variables
local x, y: number, number = 1, 2
local file, err: File, string = io.open("data.txt") -- File is hypothetical
```

Declaring without annotation or initialization is an error: `local x -- Error!`

## Composite Types

### Arrays (`{T}`)

Ordered sequences where all values have the same type `T`, corresponding to Lua sequences (integer keys starting from 1).

```lua
-- Declaration
local scores: {number}
local names: {string} = {"Alice", "Bob"}

-- Initialization
local empty_scores: {number} = {}

-- Type inference for empty tables
local lengths = {} -- Type initially unknown
table.insert(lengths, 5) -- lengths is now inferred as {number}
-- table.insert(lengths, "abc") -- Error: trying to insert string into {number}
```

*   Use `as T` to insert values of different types (bypasses type check): `scores[#scores+1] = "invalid" as number -- Runtime error likely!`

### Tuples (`{T1, T2, ...}`)

Ordered sequences with a fixed number of elements, where each position has a potentially different, known type.

```lua
-- Declaration and Initialization
local point: {number, number} = {10, 20}
local person: {string, integer} = {"Charlie", 30}

-- Indexing
local name = person[1] -- name is string
local age = person[2]  -- age is integer
-- local invalid = person[3] -- Error: index out of range

-- Indexing with a variable results in a union type
local idx = math.random(1, 2)
local value = person[idx] -- value is (string | integer) union
if value is string then
  print("Name:", value)
else
  print("Age:", value)
end

-- Length checking (if type is annotated)
-- local wrong: {string, integer} = {"David", 40, true} -- Error: expected length 2, got 3
```

*   **Inference:** A table literal like `{1, "two"}` is inferred as a tuple `{number, string}`. A literal like `{1, 2}` is inferred as an array `{number}`. Annotate explicitly if you need `{number, number}` (tuple) or `{number | string}` (array of union).

### Maps (`{K: V}`)

Tables where all keys are of type `K` and all values are of type `V`.

```lua
-- Declaration
local populations: {string: number}
local settings: {string: boolean} = {}

-- Initialization
local config = {
  host = "localhost", -- inferred as {string: string}
  port = 8080,        -- Error: cannot mix value types without union/any
}

-- Explicit map type
local options: {string: string | number} = {
  host = "localhost",
  port = 8080,
}

-- Non-string keys
local states: {boolean: string} = {
  [true] = "active",
  [false] = "inactive",
}
```

*   An array `{T}` is equivalent to a map `{integer: T}`.
*   Annotate map types explicitly, especially with string keys, to avoid confusion with Records and improve error messages.

## Named Types

### Records

Struct-like tables with a predefined set of named fields, each with a specific type. Records use *nominal typing* (types must match by name, not just structure).

**Declaration:**

```lua
-- Long form
local type Point = record
   x: number
   y: number
end

-- Short form
local record Vector
   x: number
   y: number
end

-- Nested record
local record Graphics
   record Color -- Nested type Graphics.Color
      r: integer
      g: integer
      b: integer
   end
   default_color: Color
end
```

**Usage:**

```lua
local p1: Point = { x = 10, y = 20 }
-- local p2: Point = { x = 5, y = 15, z = 0 } -- Error: field 'z' not in Point
-- local p3: Point = { x = 5 } -- Error: missing field 'y' (unless using <total>)

local v1: Vector = { x = 1, y = 1 }
-- local p4: Point = v1 -- Error: Type mismatch (Vector is not Point)
local p5: Point = v1 as Point -- OK (type cast)

local g: Graphics
g.default_color = { r = 255, g = 0, b = 0 } -- Access nested type
local c: Graphics.Color = g.default_color
```

**Methods:** Functions associated with a record, defined using `.` or `:` syntax within the same scope block as the record definition.

```lua
function Point:translate(dx: number, dy: number)
   self.x = self.x + dx
   self.y = self.y + dy
end

function Point.origin(): Point
   return { x = 0, y = 0 }
end

local p: Point = Point.origin()
p:translate(5, 5)
```

**Function Fields:** Declare function types directly within the record for callbacks or late binding.

```lua
local record Button
   label: string
   on_click: function(Button) -- Function field
end

local my_button: Button = { label = "OK" }
my_button.on_click = function(b: Button) print(b.label .. " clicked") end
```

**Array Interface:** Allow a record to also act as an array.

```lua
local record Node is {Node} -- Node acts as a record AND an array of Node
   name: string
   weight: number
end

local root: Node = { name = "root", weight = 10 }
table.insert(root, { name = "child1", weight = 5 }) -- Use as array
print(root.name) -- Use as record
print(root[1].name)
```

### Interfaces

Abstract record types. They define a structure (fields and method signatures) but cannot be instantiated directly or hold implementations themselves (except via `self` type resolution). Records can implement interfaces using `is`.

```lua
local interface Drawable
   x: number
   y: number
   draw: function(self) -- 'self' refers to the implementing type
end

local interface Movable
   move: function(self, number, number)
end

-- Record implementing multiple interfaces
local record Player is Drawable, Movable
   sprite: Sprite -- Sprite is a hypothetical type
   health: integer

   -- Implementation (typically via metatables in Lua)
   -- function Player:draw() ... end
   -- function Player:move(dx, dy) ... end
end

local p: Player = { x=0, y=0, health=100, sprite=some_sprite }
-- p:draw() -- Call interface method
-- p:move(1, 0)
```

*   Interfaces support multiple inheritance (`is Interface1, Interface2`). Field names must not conflict, or types must be identical.
*   `self` in an interface method signature resolves to the concrete implementing record type.
*   This is *subtyping*, not implementation inheritance. Teal doesn't enforce a specific OOP model; use Lua metatables for implementation sharing.

### Enums

A restricted set of named string constants.

**Declaration:**

```lua
-- Long form
local type Status = enum
   "pending"
   "running"
   "completed"
   "failed"
end

-- Short form
local enum Color
   "red"
   "green"
   "blue"
end
```

**Usage:**

```lua
local current_status: Status = "running"
-- local invalid_status: Status = "error" -- Error: "error" not in enum Status

local function process_status(s: Status)
   if s == "running" then -- Compare directly with string literal
      -- ...
   end
   local s_str: string = s -- OK: Enum converts to string
end

local input_string = "pending"
-- local input_status: Status = input_string -- Error: string does not convert to Enum
local input_status: Status = input_string as Status -- OK (type cast)
```

## Functions

Functions are first-class values with type annotations for parameters and return values.

**Syntax:**

```lua
-- Basic function
local function add(a: number, b: number): number
   return a + b
end

-- Function type declaration
local type Callback = function(string): boolean

local function process(data: string, cb: Callback)
   if cb(data) then print("Processed") end
end

-- Optional parameters (use '?')
local function greet(name: string, title?: string)
   local msg = "Hello, " .. (title or "") .. " " .. name
   print(msg)
end
greet("Alice")
greet("Bob", "Dr.")

-- Multiple return values (parenthesize if needed for clarity)
local function divide(a: number, b: number): (boolean, number | string)
   if b == 0 then return false, "division by zero" end
   return true, a / b
end

-- Iterator function (returns an iterator function)
local function count_to(n: number): (function(): number)
   local i = 0
   return function(): number
      i = i + 1
      if i <= n then return i end
      return nil -- Return nil to signal end
   end
end

for i in count_to(3) do print(i) end -- 1, 2, 3
```

**Variadic Functions:** Use `...` for variable arguments and `T...` for variable return types.

```lua
-- Variadic arguments
local function sum(...: number): number
  local total = 0
  for _, n in ipairs({...}) do -- Access varargs via table constructor
    total = total + n
  end
  return total
end
print(sum(1, 2, 3, 4)) -- 10

-- Variadic returns
local function get_values(): (string, number...) -- Returns string, then zero or more numbers
  return "data", 10, 20
end
local name, v1, v2 = get_values()

-- Multi-value 'as' cast for known return types from dynamic functions (like table.unpack)
local data = { "hello", 123, true }
local s, n, b = table.unpack(data) as (string, number, boolean)
print(s:upper(), n + 1, not b)
```

## Metamethods

Declare metamethods within record types using the `metamethod` keyword for static checking. Implement using standard Lua `setmetatable`.

```lua
local record Vector2
   x: number
   y: number
   metamethod __add: function(Vector2, Vector2): Vector2
   metamethod __tostring: function(Vector2): string
end

local mt: metatable<Vector2> -- Declare metatable type
mt = {
   __add = function(a: Vector2, b: Vector2): Vector2
      return setmetatable({ x = a.x + b.x, y = a.y + b.y }, mt) -- Return a new Vector2
   end,
   __tostring = function(v: Vector2): string
      return "(" .. v.x .. ", " .. v.y .. ")"
   end
}

local v1: Vector2 = setmetatable({ x = 1, y = 2 }, mt)
local v2: Vector2 = setmetatable({ x = 3, y = 4 }, mt)

local v3 = v1 + v2 -- Uses __add metamethod
print(v3)          -- Uses __tostring metamethod: prints (4, 6)
```

*   Teal supports Lua 5.3+ operators (`//`, `&`, `|`, `~`, `<<`, `>>`) via `compat-5.3` even on older Lua versions.

## Generics

Parameterize types and functions with type variables (typically uppercase letters like `T`, `K`, `V`).

**Generic Functions:**

```lua
-- <K, V> declares type variables
local function get_keys<K, V>(map: {K: V}): {K}
   local keys: {K} = {}
   for k, _ in pairs(map) do
      table.insert(keys, k)
   end
   return keys
end

local names: {string: number} = { Alice = 1, Bob = 2 }
local name_keys: {string} = get_keys(names)
```

**Generic Records:**

```lua
local record Optional<T>
   value: T
   present: boolean
end

local opt_num: Optional<number> = { value = 10, present = true }
local opt_str: Optional<string> = { value = nil, present = false }
```

**Generic Constraints:** Restrict type variables using `is`.

```lua
local interface Comparable -- Hypothetical interface
   compare: function(self, Comparable): integer
end

-- T must implement Comparable
local function find_max<T is Comparable>(items: {T}): T
   if #items == 0 then return nil end -- Assuming T can be nil
   local max_item = items[1]
   for i = 2, #items do
      if items[i]:compare(max_item) > 0 then
         max_item = items[i]
      end
   end
   return max_item
end
```

*   Type variables are inferred at the call site based on the arguments.

## Union Types (`|`)

Represent a value that can be one of several types. Use the `is` operator for type discrimination and narrowing.

```lua
local result: string | number

-- Assigning values
result = "Success"
result = 100

-- Discriminating
if result is string then
   print("Message: " .. result:upper()) -- result is known to be string here
elseif result is number then
   print("Value: " .. result + 1) -- result is known to be number here
end

-- Use in function signatures
local function format(value: string | number): string
   if value is string then return value end
   return tostring(value)
end
```

**Limitations:**

1.  `is` operator works on variables, not arbitrary expressions.
2.  Unions can contain at most one *undiscriminable* table type (arrays, maps, basic records).
3.  Multiple record/interface types *can* be used in a union if they are discriminable via `where` clauses:
    ```lua
    local interface Node where self.kind == "node" kind: string end
    local record TextNode is Node where self.kind == "text" text: string end
    local record ElementNode is Node where self.kind == "element" children: {Node} end

    local node: TextNode | ElementNode -- Valid union
    if node is TextNode then print(node.text) end
    ```
4.  Unions involving `string` and `enum`, or multiple `enum` types, are currently disallowed due to runtime ambiguity.

## Variable Attributes

Annotations placed after a variable name using `<...>`.

<ParamField body="<const>" type="attribute">
  Declares that the variable cannot be reassigned after initialization. The value itself (if a table/userdata) can still be mutated. Works like Lua 5.4 const.
  ```lua
  local PI <const> = 3.14159
  -- PI = 3 -- Error!
  local CONFIG <const> = { host = "localhost" }
  CONFIG.host = "remote" -- OK: Mutating the table is allowed
  -- CONFIG = {} -- Error!
  ```
</ParamField>

<ParamField body="<close>" type="attribute">
  Declares a to-be-closed variable. Requires code generation target Lua 5.4. Behaves like Lua 5.4 closeable variables, automatically calling the `__close` metamethod when the variable goes out of scope.
  ```lua
  -- Requires --target=lua54
  local function read_file(name: string): string
     local f <close> = assert(io.open(name, "r"))
     return f:read("*a")
     -- f:close() called automatically here
  end
  ```
</ParamField>

<ParamField body="<total>" type="attribute">
  Teal-specific. Declares a `const` variable assigned to a *literal table* (`{...}`) where all possible keys for the table's type *must* be explicitly listed in the literal. Useful for ensuring exhaustive handling of enum cases or record fields.
  ```lua
  local enum State "on" "off" "error" end
  local handlers <total>: {State: function()} = {
     ["on"] = function() print("ON") end,
     ["off"] = function() print("OFF") end,
     ["error"] = function() print("ERROR") end,
     -- If a new state is added to State enum, this causes a compile error
  }

  local record Point3D x:number y:number z:number end
  local origin <total>: Point3D = { x=0, y=0, z=0 }
  -- If a 'w' field is added to Point3D, this causes a compile error

  -- Explicitly setting a key to nil satisfies totality
  local partial_handlers <total>: {State: function() | nil} = {
     ["on"] = function() print("ON") end,
     ["off"] = nil, -- Explicitly handled
     ["error"] = nil, -- Explicitly handled
  }
  ```
</ParamField>

## Global Variables and Types

Globals must be declared using `global` before use to inform the type checker.

```lua
-- Global variable declaration/assignment
global AppName: string = "My Teal App"
global Config: {string: any}

-- Global function
global function log_message(level: string, msg: string)
   print("[" .. level .. "] " .. msg)
end

-- Global type declaration
global type UserID = integer

global record UserData
   id: UserID
   name: string
end

-- Forward declaration for circular dependencies across files
-- file_a.tl
global type B -- Forward declare B
global record A b_ref: B end

-- file_b.tl
global type A -- Forward declare A
global record B a_ref: A end
```

## Standard Library & Lua Compatibility

*   Teal provides type definitions for a subset of the Lua standard library (mostly based on Lua 5.3, using `compat-5.3` for compatibility on older versions).
*   Standard library functions/tables are treated as `<const>`.
*   Supports Lua 5.3+ operators (`//`, `&`, `|`, `~`, `<<`, `>>`) via `compat-5.3`.
*   Use `--skip-compat53` flag to disable `compat-5.3` (may lead to inconsistencies across Lua versions).

## Using Teal with Lua Files (`.lua`)

*   `tl check your_file.lua` can type-check existing Lua code.
*   In `.lua` files, undeclared/unannotated variables have the implicit type `unknown`.
*   Operations involving `unknown` generally bypass type checking.
*   `tl check` reports `unknown` variables separately, helping identify areas for potential annotation.
*   You can add Teal type annotations directly to `.lua` files (making them invalid standard Lua). These files can be loaded using `tl.loader()` in Lua. This allows for incremental typing of Lua projects.
