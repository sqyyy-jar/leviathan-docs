# Assembler reference

## Dialect declaration

To use the assembler mode start your file with the following line:

```clj
(mod assembly)
```

## Static variables

Static variables are variables that are directly stored inside the binary.

The assembler supports static variables of the following types:

* `int` (64-bit)
* `uint` (64-bit)
* `float` (64-bit)
* `str`
* `buffer` (zero-initialized)

### `str`

Static strings (`str`) consist of a `uint` representing the size of the string
in bytes followed by the bytes of the string terminated by a zero-byte.

Strings are aligned to 4 bytes by appending zero-bytes to the end.

Referencing a string returns the address to the bytes of the string.

### Syntax

Static variables are declared like this:

```clj
(static a 10) ; int
(static b 10u) ; uint
(static c 10.0) ; float
(static d "A string") ; str
(static e (buffer 1024u)) ; buffer of size 1024
```

## Labels

Labels are like functions though they do not have a signature and may interact
with the state of the runtime in any way possible.

Labels may be public (`+label`) or private (`-label`).

Public modules can be accessed by other modules while private modules can only
be accessed from the module they were declared in.

### Syntax

A label is declared like this:

```clojure
(+label add (do
  (add r0 r0 r1)
))
```

### Instructions

* `add Xdst Xlhs u17`
  * Add `u17` to `Xlhs` and store the result in `Xdst`
  * Supports signed and unsigned integers
* `add Xdst Xlhs Xrhs`
  * Add `Xrhs` to `Xlhs` and store the result in `Xdst`
  * Supports signed and unsigned integers
* `sub Xdst Xlhs u17`
  * Subtract `u17` from `Xlhs` and store the result in `Xdst`
  * Supports signed and unsigned integers
* `sub Xdst Xlhs Xrhs`
  * Subtract `Xrhs` from `Xlhs` and store the result in `Xdst`
  * Supports signed and unsigned integers
* `mul Xdst Xlhs u17`
  * Multiply `Xlhs` by `u17` and store the result in `Xdst`
  * Supports signed and unsigned integers
* `mul Xdst Xlhs Xrhs`
  * Multiply `Xlhs` by `Xrhs` and store the result in `Xdst`
  * Supports signed and unsigned integers
* `div Xdst Xlhs u17`
  * Divide `Xlhs` by `u17` and store the result in `Xdst`
  * Supports unsigned integers only
* `div Xdst Xlhs Xrhs`
  * Divide `Xlhs` by `Xrhs` and store the result in `Xdst`
  * Supports unsigned integers only
* `rem Xdst Xlhs u17`
  * Store the remainder of `Xlhs` and `u17` in `Xdst`
  * Supports unsigned integers only
* `rem Xdst Xlhs Xrhs`
  * Store the remainder of `Xlhs` and `Xrhs` in `Xdst`
  * Supports unsigned integers only
* `divs Xdst Xlhs i17`
  * Divide `Xlhs` by `i17` and store the result in `Xdst`
  * Supports signed integers only
* `divs Xdst Xlhs Xrhs`
  * Divide `Xlhs` by `Xrhs` and store the result in `Xdst`
  * Supports signed integers only
* `rems Xdst Xlhs i17`
  * Store the remainder of `Xlhs` and `i17` in `Xdst`
  * Supports signed integers only
* `rems Xdst Xlhs Xrhs`
  * Store the remainder of `Xlhs` and `Xrhs` in `Xdst`
  * Supports signed integers only
* `mov Xdst u22`
  * Move `u22` into `Xdst`
* `mov Xdst Xsrc`
  * Move `Xsrc` into `Xdst`
* `movs Xdst i22`
  * Move `i22` into `Xdst` with sign extension
* `shl Xdst Xlhs u11`
  * Move `Xlhs` shifted left by `u11` into `Xdst`
* `shl Xdst Xlhs Xrhs`
  * Move `Xlhs` shifted left by `Xrhs` into `Xdst`
* `shr Xdst Xlhs u11`
  * Move `Xlhs` shifted right by `u11` into `Xdst`
* `shr Xdst Xlhs Xrhs`
  * Move `Xlhs` shifted right by `Xrhs` into `Xdst`
* `shrs Xdst Xlhs u11`
  * Move `Xlhs` arithmetically shifted right by `u11` into `Xdst`
* `shrs Xdst Xlhs Xrhs`
  * Move `Xlhs` arithmetically shifted right by `Xrhs` into `Xdst`
* `ldrb Xdst Xsrc i11`
  * Load 8 bits from address `Xsrc` with offset `i11` into `Xdst`
* `ldrh Xdst Xsrc i11`
  * Load 16 bits from address `Xsrc` with offset `i11` into `Xdst`
* `ldrw Xdst Xsrc i11`
  * Load 32 bits from address `Xsrc` with offset `i11` into `Xdst`
* `ldr Xdst Xsrc i11`
  * Load 64 bits from address `Xsrc` with offset `i11` into `Xdst`
* `strb Xdst Xsrc i11`
  * Store 8 bits from `Xsrc` to address `Xdst` with offset `i11`
* `strh Xdst Xsrc i11`
  * Store 16 bits from `Xsrc` to address `Xdst` with offset `i11`
* `strw Xdst Xsrc i11`
  * Store 32 bits from `Xsrc` to address `Xdst` with offset `i11`
* `str Xdst Xsrc i11`
  * Store 64 bits from `Xsrc` to address `Xdst` with offset `i11`
* `int 16u`
  * Interrupt the runtime with code `u16`
* `ncall u21`
  * Call a native function with id `u21`
* `ncall Xid`
  * Call a native function with id `Xid`
* `vcall u21`
  * Call a virtual function with id `u21`
* `vcall Xid`
  * Call a virtual function with id `Xid`
* `addf Xdst Xlhs Xrhs`
  * Add the float `Xrhs` to the float `Xlhs` and store the result in `Xdst`
* `subf Xdst Xlhs Xrhs`
  * Subtract the float `Xrhs` from the float `Xlhs` and store the result in `Xdst`
* `mulf Xdst Xlhs Xrhs`
  * Multiply the float `Xrhs` by the float `Xlhs` and store the result in `Xdst`
* `divf Xdst Xlhs Xrhs`
  * Divide the float `Xrhs` by the float `Xlhs` and store the result in `Xdst`
* `and Xdst Xlhs Xrhs`
  * Move `Xlhs` & `Xrhs` into `Xdst` (bitwise and)
* `or Xdst Xlhs Xrhs`
  * Move `Xlhs` | `Xrhs` into `Xdst` (bitwise or)
* `xor Xdst Xlhs Xrhs`
  * Move `Xlhs` ^ `Xrhs` into `Xdst` (bitwise xor)
* `cmp Xdst Xlhs Xrhs`
  * Compare `Xlhs` and `Xrhs` and store the result in `Xdst`
  * Supports unsigned integers only
* `cmps Xdst Xlhs Xrhs`
  * Compare `Xlhs` and `Xrhs` and store the result in `Xdst`
  * Supports signed integers only
* `cmpf Xdst Xlhs Xrhs`
  * Compare the float `Xlhs` and the float `Xrhs` and store the result in `Xdst`
* `not Xdst Xsrc`
  * Move !`Xsrc` into `Xdst` (bitwise not)
* `fti Xdst Xsrc`
  * Move the float `Xsrc` converted to an integer into `Xdst`
* `itf Xdst Xsrc`
  * Move the integer `Xsrc` converted to a float into `Xdst`
* `ldbo Xdst`
  * Load the address of the first byte after the binary header into `Xdst`
* `ldpc Xdst`
  * Load the program counter into `Xdst`
* `zero Xdst`
  * Move zero into `Xdst`
* `dbg Xreg`
  * Debug print the integer `Xreg`
* `inc Xreg`
  * Increment the integer `Xreg`
* `nop`
  * Do nothing
* `halt`
  * Halt the runtime
* `ret`
  * Return from a call
* `crash`
  * Insert an invalid opcode and crash the runtime dumping the registers contents
* `ref Xdst <static variable>` or `lea Xdst <static variable>`
  * Load the effective address of a static variable into `Xdst`
