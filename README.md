# RCPU

_RCPU_ is a virtual machine emulator and assembler written in Ruby.

Very much work in progress.

## Usage

To assemble a file, use _rcpu-assemble_, passing the input filename (with the _rcs_ extension). This will generate a corresponding _rcb_ file. For example:

	% ./bin/rcpu-assemble samples/counter.rcs

To run a file, use _rcpu-emulate_, passing the input filename (with the _rcb_ extension). For example:

	% ./bin/rcpu-emulate samples/counter.rcb

The _rcs_ extension is used for **RC**PU **s**ource files, while _rcb_ is for **RC**PU **b**inary (i.e. compiled) files.

## Registers

| Name   | Code | Purpose
| ------ | ---- | -------
| r0     | 0    | general-purpose
| r1     | 1    | general-purpose
| r2     | 2    | general-purpose
| r3     | 3    | general-purpose
| r4     | 4    | general-purpose
| r5     | 5    | general-purpose
| r6     | 6    | general-purpose
| r7     | 7    | general-purpose
| rpc    | 8    | program counter (a.k.a instruction pointer)
| rflags | 9    | contains result of `cmp(i)`
| rsp    | a    | stack pointer
| rbp    | b    | base pointer

## Instruction format

Instructions are of variable length. The first byte is the opcode.

| Mnemonic  | Opcode | Arguments             | Effect
| --------- | ------ | --------------------- | ------
| `nop`     | 0x00   | 0                     | nothing
| `halt`    | 0xff   | 0                     | stops emulation
| `pop`     |        | 1 (reg)               | pop into a0 (reg)
| `prn`     |        | 1 (reg)               | print a0 (reg)
| `jmp`     |        | 1 (label)             | pc ← a0
| `je`      |        | 1 (label)             | if == then pc ← a0
| `jne`     |        | 1 (label)             | if != then pc ← a0
| `jg`      |        | 1 (label)             | if >  then pc ← a0
| `jge`     |        | 1 (label)             | if >= then pc ← a0
| `jl`      |        | 1 (label)             | if <  then pc ← a0
| `jle`     |        | 1 (label)             | if <= then pc ← a0
| `push(i)` |        | 1 (reg/imm)           | push a0 onto stack
| `not`     |        | 2 (reg, reg)          | a0 ← ~a1
| `mov(i)`  |        | 2 (reg, reg/imm)      | a0 ← a1
| `cmp(i)`  |        | 2 (reg, reg/imm)      | (see below)
| `mod(i)`  |        | 2 (reg, reg, reg/imm) | a0 ← a1 % a2
| `add(i)`  |        | 2 (reg, reg, reg/imm) | a0 ← a1 + a2
| `sub(i)`  |        | 2 (reg, reg, reg/imm) | a0 ← a1 - a2
| `mul(i)`  |        | 2 (reg, reg, reg/imm) | a0 ← a1 * a2
| `div(i)`  |        | 2 (reg, reg, reg/imm) | a0 ← a1 / a2
| `xor(i)`  |        | 2 (reg, reg, reg/imm) | a0 ← a1 ^ a2
| `or(i)`   |        | 2 (reg, reg, reg/imm) | a0 ← a1 \| a2
| `and(i)`  |        | 2 (reg, reg, reg/imm) | a0 ← a1 & a2
| `shl(i)`  |        | 2 (reg, reg, reg/imm) | a0 ← a1 << a2
| `shr(i)`  |        | 2 (reg, reg, reg/imm) | a0 ← a1 >> a2

`cmp(i)` updates the `flags` register and sets the 0x01 bit to true if the arguments are equal, and the 0x02 bit to true if the first argument is greater than the second.

Several opcodes have an `(i)` variant. These variants take a four-byte immediate argument (meaning the data is encoded in the instruction) rather than a register name. For opcodes that have immediate variants, the _Opcode_ column contains the non-immediate variant followed by the immediate variant.

## To do

* Finish implementing all opcodes
