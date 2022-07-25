# Ruby to Assembly

Are assembly  `.proc` lines the same as a Ruby `#proc`? Or perhaps a function/method?


## Opcodes and Operands

```assembly
; opcode | operand
LDA #$00
```

### Loading Data: `LDA`, `LDX`, and `LDY`.

The `LD*` opcodes load data into a register. Remember the 6502 processor has 3
registers - the accumulator and the X and Y.

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

### Storing Data:

`$2006` and `$2007`, PPUADDR and PPUDATA respectively. 

PPUDATA stores incrementally. so even without editing `$2006`(PPUADDR) it will
always increment the memory address.

`$2001` PPUMASK actually tells the PPU how to draw to the screen. This is the
eight bit flags.

## Vectors and RTI interruptors

The `.addr` directive in
```
.segment "VECTORS"
.addr nmi_handler, reset_handler, irq_handler
```
outputs the memory address of the `.proc` labels nmi_handler for example.

Does this mean the .proc methods are stored in a memory address ? like a bank of memory
for which the bytes are instructed? but how does it know where to go next if it stores
the function in memory addresses one byte at a time?
