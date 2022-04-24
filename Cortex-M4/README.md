This manual is created based on [Azeria labs](https://azeria-labs.com/arm-data-types-and-registers-part-2/)

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
can be used during common operations to store temporary values, pointers (locations to memory), etc. **R0**, for example, can be referred as accumulator during the arithmetic operations or for storing the result of a previously called function. **R7** becomes useful while working with syscalls as it **stores the syscall number** and **R11** helps us to keep track of boundaries on the stack serving as the **frame pointer**. Moreover, the function calling convention on ARM specifies that the first four arguments of a function are stored in the registers r0-r3.

### R13:
SP (Stack Pointer). The Stack Pointer points to the top of the stack. The stack is an area of memory used for function-specific storage, which is reclaimed when the function returns. The stack pointer is therefore used for allocating space on the stack, by subtracting the value (in bytes) we want to allocate from the stack pointer. In other words, if we want to allocate a 32 bit value, we subtract 4 from the stack pointer.

### R14:
LR (Link Register). When a function call is made, the Link Register gets updated with a memory address referencing the next instruction where the function was initiated from. Doing this allows the program return to the “parent” function that initiated the “child” function call after the “child” function is finished.

###R15:
PC (Program Counter). The Program Counter is automatically incremented by the size of the instruction executed. This size is always 4 bytes in ARM state and 2 bytes in THUMB mode. When a branch instruction is being executed, the PC holds the destination address. During execution, PC stores the address of the current instruction plus 8 (two ARM instructions) in ARM state, and the current instruction plus 4 (two Thumb instructions) in Thumb(v1) state. This is different from x86 where PC always points to the next instruction to be executed.
