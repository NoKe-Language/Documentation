# Assembly

NoKe is compiled by Nolfaris to an optimized bytecode. Then, it is interpreted by the endiver virtual machine.

The structure of a `.nk` file is described here.

## Instruction set

There are four types of instruction

`Huge type instruction` refers to `INST ARG1 ARG2 DEST`.

`Medium type instruction` refers to `INST DEST ARG1`.

`Little type instruction` refers to `INST ARG1`.

`Tiny type instruction` refers to `INST`.

```c
// Add
// Huge type instruction
#define IADD 0x60 // Add int
#define FADD 0x62 // Add float

// Divide
// Huge type instruction
#define IDIV 0x6c // Div int 
#define FDIV 0x6e // Div float

// Load
// Huge type instruction
#define ILOAD 0x15 // Load immediate int
#define FLOAD 0x17 // Load immediate float

// Multiply
// Huge type instruction
#define IMUL 0x68 // Mul int
#define FMUL 0x6a // Mul float

// Jump
// Little type instruction
#define JUMP 0x00 // Jump
#define JAL 0x01 // Jump and link

// Log
// Little type instruction
#define LOG 0x02 // Print
```