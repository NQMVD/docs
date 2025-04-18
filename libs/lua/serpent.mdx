---
title: 'serpent'
description: 'A Lua library for serializing Lua data structures and pretty-printing them.'
icon: 'brackets-curly'
---

<CardGroup cols={2}>
  <Card title="Luarocks" icon="moon-stars" href="https://luarocks.org/modules/paulclinger/serpent">View the Rock</Card>
  <Card title="Repo" icon="inbox" href="https://github.com/pkulchenko/serpent">Visit the Repo</Card>
</CardGroup>

---

## Description

`serpent` is a Lua library designed for serializing Lua data structures into a string format that can be reliably deserialized using `loadstring()` or `serpent.load()`. It also functions as a powerful pretty printer, offering human-readable output for complex tables.

It handles nested tables, self-references, shared references, and can even serialize functions (as bytecode using `string.dump()`).

## Features

*   **Human Readable Output:**
    *   Single-line (`line`) and multi-line indented (`block`) formats.
    *   Proper indentation for nested tables.
    *   Numerical keys listed first.
    *   Optional alphanumeric key sorting.
    *   Optimized array representation (`{'a', 'b'}` vs `{[1]='a', [2]='b'}`).
    *   Correct handling of `nil` values in arrays (`{1, nil, 3}`).
    *   Short key notation (`{foo = 'foo'}` vs `{['foo'] = 'foo'}`).
    *   Marks shared and self-references.
*   **Machine Readable Output:**
    *   Generates Lua code loadable via `loadstring()` or `serpent.load()`.
    *   Handles deeply nested and self-referential tables correctly.
    *   Preserves shared references for tables and functions.
*   **Function Serialization:** Supports serializing functions using `string.dump()` (can be disabled).
*   **Metamethod Support:** Respects `__tostring` and `__serialize` metamethods.
*   **String Safety:** Escapes potentially problematic control characters (`\010`, `\026`).
*   **Configurable:** Offers numerous options to control output format and serialization behavior.
*   **Safe Loading:** Provides `serpent.load()` for safer deserialization compared to raw `loadstring()`.

## Installation / Setup

Require the library in your Lua script:

```lua
local serpent = require("serpent")
```

## Usage Example

```lua
local serpent = require("serpent")

-- Sample table with various features
local a = {1, nil, 3, x=1, ['true'] = 2, [false]=3}
a.self = a -- self-reference
a[a] = true -- table as key

-- Full serialization (includes reference tracking)
local serialized_data = serpent.dump(a)
print("Serialized (dump):\n", serialized_data)

-- Pretty print (single line)
print("Pretty (line):", serpent.line(a))

-- Pretty print (multi-line indented)
print("Pretty (block):\n", serpent.block(a))

-- Deserialization using serpent.load (safer)
local ok, copy = serpent.load(serialized_data)
if ok then
    print("Deserialized successfully:", copy[3] == a[3]) -- true
    print("Self reference preserved:", copy.self == copy) -- true
else
    print("Deserialization failed:", copy) -- error message
end

-- Deserialization using loadstring (less safe)
-- Note: serpent.dump output includes necessary 'local _=' for references
local func, err = loadstring(serialized_data)
if err then error(err) end
local copy_loadstring = func()
print("Loadstring self reference preserved:", copy_loadstring.self == copy_loadstring) -- true

-- Loading pretty-printed output requires adding 'return'
local ok_line, copy_line = serpent.load('return ' .. serpent.line({msg = "hello"}))
print("Loaded line output:", ok_line and copy_line.msg == "hello") -- true
```

## Core Functions

### `serpent.dump(value [, options])`

Performs full serialization, including tracking for shared and self-references. Outputs a string containing Lua code that, when executed, reconstructs the original value.

<ParamField body="value" type="any" required>
  The Lua value (table, string, number, boolean, nil, function) to serialize.
</ParamField>
<ParamField body="options" type="table">
  An optional table containing configuration options (see Options section below).
  Default options set by `dump`: `name` (internal), `compact=true`, `sparse=true`.
</ParamField>

### `serpent.line(value [, options])`

Pretty-prints the value on a single line. Does *not* include the self-reference tracking section, making it primarily for display.

<ParamField body="value" type="any" required>
  The Lua value to pretty-print.
</ParamField>
<ParamField body="options" type="table">
  An optional table containing configuration options (see Options section below).
  Default options set by `line`: `sortkeys=true`, `comment=true`.
</ParamField>

### `serpent.block(value [, options])`

Pretty-prints the value over multiple lines with indentation. Does *not* include the self-reference tracking section.

<ParamField body="value" type="any" required>
  The Lua value to pretty-print.
</ParamField>
<ParamField body="options" type="table">
  An optional table containing configuration options (see Options section below).
  Default options set by `block`: `indent='  '`, `sortkeys=true`, `comment=true`.
</ParamField>

### `serpent.load(str [, options])`

Loads a serialized string generated by `serpent` (or compatible Lua code). Returns `ok, result` similar to `pcall`. Remember to prepend `return ` if loading output from `line` or `block`.

<ParamField body="str" type="string" required>
  The string containing the serialized Lua data.
</ParamField>
<ParamField body="options" type="table" default="{safe = true}">
  An optional table containing load options. Currently only supports `safe`.
  Set `safe = false` to disable safety checks preventing execution of arbitrary code (use with caution on untrusted input).
</ParamField>

## Options

These options can be provided as key-value pairs in the `options` table passed as the second argument to `serpent.dump`, `serpent.line`, or `serpent.block`.

<ParamField body="indent" type="string">
  String used for indentation (e.g., `'  '`, `'\t'`). Setting this enables multi-line output.
  Default for `block`: `'  '`. Default for `dump`/`line`: `nil` (no indentation).
</ParamField>

<ParamField body="comment" type="boolean | number">
  Add comments showing stringified values (e.g., `--[[true]]`). If a number is provided, it limits the maximum depth for comments.
  Default for `line`/`block`: `true`. Default for `dump`: `false`.
</ParamField>

<ParamField body="sortkeys" type="boolean | function">
  Sort table keys alphanumerically before serialization. If a function is provided, it's used for custom sorting (receives `(keys_array, original_table)`, should sort `keys_array` in-place).
  Default for `line`/`block`: `true`. Default for `dump`: `false`.
</ParamField>

<ParamField body="sparse" type="boolean">
  Force sparse table encoding (e.g., `{[1]=1, [3]=3}` instead of `{1, nil, 3}`), ignoring the array part optimization based on `#t`.
  Default for `dump`: `true`. Default for `line`/`block`: `false`.
</ParamField>

<ParamField body="compact" type="boolean">
  Remove extra spaces between elements for more compact output.
  Default for `dump`: `true`. Default for `line`/`block`: `false`.
</ParamField>

<ParamField body="fatal" type="boolean" default="false">
  Raise a fatal error if a non-serializable value is encountered (e.g., a function when `nocode=true`, or unsupported userdata).
</ParamField>

<ParamField body="fixradix" type="boolean" default="false">
  (Likely legacy) Change radix character set depending on locale to ensure a decimal dot is used for numbers.
</ParamField>

<ParamField body="nocode" type="boolean" default="false">
  Disable bytecode serialization for functions. Outputs `function() --[[skipped]] end` instead. Useful for comparing structures without code differences.
</ParamField>

<ParamField body="nohuge" type="boolean" default="false">
  Disable checking numbers against `math.huge` (or `1/0`, `-1/0`). May slightly improve performance if huge numbers are not expected.
</ParamField>

<ParamField body="maxlevel" type="number">
  Specify the maximum level up to which nested tables should be expanded. Deeper levels will be represented as a comment like `--[[...]]`. No default limit.
</ParamField>

<ParamField body="maxnum" type="number">
  Specify the maximum number of elements (key-value pairs) to include when serializing a table. No default limit.
</ParamField>

<ParamField body="maxlength" type="number">
  Specify the maximum total string length for all serialized elements within a single table. No default limit.
</ParamField>

<ParamField body="metatostring" type="boolean" default="true">
  Use the `__tostring` metamethod when serializing tables or userdata if present. Set to `false` to disable this behavior and serialize the raw table/userdata even if `__tostring` exists.
</ParamField>

<ParamField body="numformat" type="string" default="%.17g">
  Specify the `string.format` pattern for numeric values. "%.17g" is the default, aiming for the shortest possible round-trippable double precision. Use "%.16g" for potentially better readability at the cost of potential minor precision loss.
</ParamField>

<ParamField body="keyallow" type="table">
  A table used as a set to specify a whitelist of keys (strings or numbers) that should be included in the serialization. Any keys *not* present in this table will be skipped. Example: `{ ['name'] = true, ['age'] = true }`. Default is inactive (all keys allowed).
</ParamField>

<ParamField body="keyignore" type="table">
  A table used as a set to specify a blacklist of keys (strings or numbers) that should be ignored during serialization. Example: `{ ['password'] = true, ['internal_id'] = true }`. Default is inactive (no keys ignored).
</ParamField>

<ParamField body="valignore" type="table">
  A table used as a set to specify a list of *values* to ignore. If a table's value is present as a key in this `valignore` table, that key-value pair is skipped. Example: `{ [some_function] = true, [some_userdata] = true }`. Default is inactive.
</ParamField>

<ParamField body="valtypeignore" type="table">
  A table used as a set to specify a list of value *types* (as strings: `"function"`, `"userdata"`, `"table"`, etc.) to ignore. If a table entry's value has a type listed as a key in this table, that key-value pair is skipped. Example: `{ ['function'] = true }`. Default is inactive.
</ParamField>

<ParamField body="custom" type="function">
  Provide a custom output formatter function. This function receives `(tag, head, body, tail, level)` and should return the formatted string representation for a table. See the original Serpent documentation for details. Default is inactive.
</ParamField>

<ParamField body="name" type="string">
  Internal option used by `serpent.dump` to enable the self-reference tracking mechanism. Not typically set manually.
</ParamField>

## Advanced Features

*   **Metatables:** If a table/userdata has a `__serialize` metamethod, it's called with the value, and its return value is serialized instead. If only `__tostring` exists (and `metatostring` option is true), `tostring(value)` is called, and the resulting string is included.
*   **Custom Sorting:** Provide a function to the `sortkeys` option for custom key ordering logic. The function receives `(keys_array, original_table)` and should sort `keys_array` in-place.
*   **Custom Formatters:** Provide a function to the `custom` option to completely override how tables are formatted into strings.

## Limitations

*   Doesn't handle most `userdata` types (except file handles like `io.stdout`).
*   Threads, function upvalues/environments, and most metatables are not serialized (only `__tostring`/`__serialize` affect output).
