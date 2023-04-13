# Code reference

> **Warning**  
> Work in progress

## Dialect declaration

To use the code dialect start the module file with this line:

```clojure
(mod code)
```

## Imports

To import other modules use the `use` keyword:

```clojure
(use module-name)
```

You can also import multiple modules at once:

```clojure
(use module-a module-b)
```

## Static variables

Static variables are variables that belong to the global state and are
independent from function calls.

The following types are supportet for static variables:

* `int`: 64-bit signed integer
* `uint`: 64-bit unsigned integer
* `float`: 64-bit floating point number
* `str`: a string of text
* `buffer`: a fixed size buffer filled with a specific byte
* `int-array`: an array of 64-bit signed integers
* `uint-array`: an array of 64-bit unsigned integers

## Syntax

Static variables are declared at module-level like this:

```clojure
(static my-variable 42)
```

Multiple variables can also be declared in one statement:

```clojure
(static
  a "Hello world!"
  b 500
)
```

### Buffers

Buffers have the following syntax:

```clojure
{16} ; length = 16, fill-byte = 0
{-7 10} ; length = 10, fill-byte = -7
```

### Arrays

Arrays are represented by its elements enclosed by square brackets:

```clojure
[1 2 3 4 5] ; signed
[1u 2 3 4 5] ; unsigned
```
