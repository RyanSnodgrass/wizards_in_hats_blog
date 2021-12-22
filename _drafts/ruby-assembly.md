# Ruby to Assembly

Are assembly  `.proc` lines the same as a Ruby `.proc()`? Or perhaps a function/method?


## Opcodes

### `LDA`, `LDX`, and `LDY`.

`LDA` loads contents of the memory address specified into the "accumulator" which
can perform math.

```assembly
LDA $3f00  ; load contents of memory address $3f00
           ; into the accumulator
LDA #$3f   ; load the value $3f into the accumulator
```

Operands:
The `$` symbol means it's a memory address at that hex value.

While the `#` means the literal hex value.

The different operand formats are called "addressing modes". Referring to a memory
address is known as "absolute" mode, while the literal hex value is known as
"immediate"
