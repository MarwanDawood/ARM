This manual is created based on [Azeria labs](https://azeria-labs.com/arm-data-types-and-registers-part-2/)

## ARM processor vs. Intel processor
### Intel is a CISC (Complex Instruction Set Computing) processor
that has a larger and more feature-rich instruction set and allows many complex instructions to access memory. It therefore has more operations, addressing modes, but less registers than ARM. CISC processors are mainly used in normal PC’s, Workstations, and servers.

The Intel x86 and x86-64 series of processors use the **little-endian format**.

### ARM is a RISC (Reduced instruction set Computing) processor
therefore has a simplified instruction set (100 instructions or less) and more general purpose registers than CISC. Unlike Intel, ARM uses instructions that operate only on registers and uses a Load/Store memory model for memory access, which means that only Load/Store instructions can access memory. This means that incrementing a 32-bit value at a particular memory address on ARM would require three types of instructions (load, increment and store).

The reduced instruction set has its advantages and disadvantages. One of the **advantages is that instructions can be executed more quickly**, potentially allowing for greater speed (RISC systems shorten execution time by reducing the clock cycles per instruction). The **downside is that less instructions means a greater emphasis on the efficient writing of software** with the limited instructions that are available. Also important to note is that ARM has two modes, ARM mode and Thumb mode. Thumb instructions can be either 2 or 4 bytes.

In ARM, most instructions can be used for conditional execution.

The ARM architecture was little-endian before version 3. Since then ARM processors became **BI-endian and feature a setting which allows for switchable endianness**.


|                  ARM family                 | ARM architecture |
|:-------------------------------------------:|:----------------:|
| ARM7                                        | ARM v4           |
| ARM9                                        | ARM v5           |
| ARM11                                       | ARM v6           |
| Cortex-A                                    | ARM v7-A         |
| Cortex-R                                    | ARM v7-R         |
| Cortex-M                                    | ARM v7-M         |


|    ARM   |                 Description                 |           x86           |
|:--------:|:-------------------------------------------:|:-----------------------:|
| R0       | General Purpose                             | EAX                     |
| R1-R5    | General Purpose                             | EBX, ECX, EDX, ESI, EDI |
| R6-R10   | General Purpose                             | –                       |
| R11 (FP) | Frame Pointer                               | EBP                     |
| R12      | Intra Procedural Call                       | –                       |
| R13 (SP) | Stack Pointer                               | ESP                     |
| R14 (LR) | Link Register                               | –                       |
| R15 (PC) | <- Program Counter / Instruction Pointer -> | EIP                     |
| CPSR     | Current Program State Register/Flags        | EFLAGS                  |

### R0-R12:
can be used during common operations to store temporary values, pointers (locations to memory), etc.

**R0**, for example, can be referred as accumulator during the arithmetic operations or for storing the result of a previously called function.

**R7** becomes useful while working with syscalls as it **stores the syscall number** and

**R11** helps us to keep track of boundaries on the stack serving as the **frame pointer**. Moreover, the function calling convention on ARM specifies that the first four arguments of a function are stored in the registers r0-r3.

### R13:
SP (Stack Pointer). The Stack Pointer points to the top of the stack. The stack is an area of memory used for function-specific storage, which is reclaimed when the function returns. The stack pointer is therefore used for allocating space on the stack, by subtracting the value (in bytes) we want to allocate from the stack pointer. In other words, if we want to allocate a 32 bit value, we subtract 4 from the stack pointer.

### R14:
LR (Link Register). When a function call is made, the Link Register gets updated with a memory address referencing the next instruction where the function was initiated from. Doing this allows the program return to the “parent” function that initiated the “child” function call after the “child” function is finished.

### R15:
PC (Program Counter). The Program Counter is automatically incremented by the size of the instruction executed. This size is always 4 bytes in **ARM state** and 2 bytes in **THUMB mode**. When a branch instruction is being executed, the PC holds the destination address. During execution, PC stores the address of the current instruction plus 8 (two ARM instructions) in ARM state, and the current instruction plus 4 (two Thumb instructions) in Thumb(v1) state. This is different from x86 where PC always points to the next instruction to be executed.
