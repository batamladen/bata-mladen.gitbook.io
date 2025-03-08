# Data Transfer & Algoritmic Operations

## Data Transfer

### Instruction MOV

The MOV (Move) instruction in Intel 8086 assembly language is used to transfer data between registers, memory locations, and immediate values.

**The syntax:** <mark style="color:yellow;">MOV destination, source</mark>

destination - This is where the data will be moved to. It can be a register or a memory location. \
source - This is the source of the data. It can be another register, a memory location, or an immediate value.

```nasm
MOV AX, BX         ; Move the contents of register BX into register AX
MOV CX, 10         ; Move the immediate value 10 into register CX
MOV DX, [SI]       ; Move the contents of the memory location addressed by                          ; the contents of SI into register DX

MOV [DI], AX       ; Move the contents of register AX into the memory location ;                      addressed by the contents of DI 

MOV [BX], 20       ; Move the immediate value 20 into the memory location                             addressed by the contents of BX

```

***

### Instruction XCHG

The XCHG (Exchange) instruction in Intel 8086 assembly language is used to swap the contents of two operands. This operation is often used for tasks such as sorting elements in an array or managing data in a program.

**The syntax:** <mark style="color:yellow;">XCHG destination, source</mark>

destination - This is one of the operands involved in the exchange operation. It can be a register or a memory location. \
source - This is the other operand involved in the exchange operation. It can be another register or a memory location.

```nasm
XCHG AX, BX          ; Swap the contents of registers AX and BX 
XCHG CX, [SI]        ; Swap the contents of register CX and the memory location                         addressed by the contents of SI

XCHG [DI], [BX]     ; Swap the contents of the memory locations addressed by                           the contents of DI and BX

```

***

### Instruction PUSH & POP

The PUSH and POP instructions in Intel 8086 assembly language are used to push data onto the stack and pop data from the stack, respectively.

**The syntax:** <mark style="color:yellow;">PUSH source</mark>

source - This is the operand whose value is to be pushed onto the stack. It can be a register or a memory location

**The syntax:** <mark style="color:yellow;">POP destination</mark>

destination - This is where the value popped from the stack will be stored. It can be a register or a memory location

```nasm
PUSH AX             ; Push the contents of register AX onto the stack
POP BX              ; Pop the value from the stack and store it in register BX
; MOV BX, AX 

PUSH [SI]           ; Push the contents of the memory location addressed by SI ;                       onto the stack

POP [DI]            ; Pop the value from the stack and store it in the memory                          location addressed by DI

PUSH 10             ; Push the immediate value 10 onto the stack 
POP CX              ; Pop the value from the stack and store it in register CX
;MOV CX, 10
```

The PUSH instruction pushes the specified value onto the stack. The POP instruction retrieves the top value from the stack and stores it in the specified destination.

These instructions are fundamental for managing the stack, especially in procedures and subroutine calls, where they are used to save and restore register values.

***

### Instruction LEA

LEA (Load Effective Address) instruction in Intel 8086 assembly language is used to load a memory address into a register.

The syntax: <mark style="color:yellow;">LEA destination, source</mark>

destination - This is the register that will receive the effective address. It cannot be a memory location. \
source - This is the operand whose address will be loaded into the destination register. It can be a memory location specified by an offset or an indexed address.

```nasm
LEA SI, [BX + 10]   ; SI is the destination register that will receive the                             effective address. [BX + 10] is the source, representing a                       memory location calculated as the sum of the contents of                         register BX and the immediate value 10.
```

The LEA instruction is often used to perform address calculations without accessing memory, making it useful for scenarios where you need to calculate addresses for array indexing or accessing data structures. It does not actually fetch data from memory; it only loads the calculated address into the specified register.

***

### String instruction MOVS

The MOVS (Move String) instruction in Intel 8086 assembly language is used for block memory transfers, commonly used for copying a string of bytes from one location to another.

The syntax: <mark style="color:yellow;">MOVS destination, source</mark>

destination - This is the destination memory address where the string will be copied. \
source - This is the source memory address from where the string will be copied.

```nasm
MOV SI, 1000h       ; Set the source index to memory address 1000h 
MOV DI, 2000h       ; Set the destination index to memory address 2000h 
MOV CX, 10          ; Set the count of bytes to copy 

MOVS                ; Copy the string of 10 bytes from [SI] to [DI]
```

***

### String instruction LODS

The LODS (Load String) instruction in Intel 8086 assembly language is used for loading a byte or word from the source string into the accumulator (AL or AX).

**The syntax:**

For loading a byte: <mark style="color:yellow;">LODSB</mark>

For loading a word: <mark style="color:yellow;">LODSW</mark>

The LODS instruction automatically increments or decrements the source index (SI register) based on the direction flag in the flags register. If the direction flag is set, the index is decremented; If the flag is clear, the index is incremented.

```nasm
MOV SI, 1000h      ; Set the source index to memory address 1000h
MOV CX, 10         ; Set the count of bytes to load
LODSB              ; Load a byte from [SI] into AL
```

**SI** is the source index pointing to the memory address of the source string. **CX** is the count of bytes to be loaded. **LODSB** is the instruction that loads a byte from the source string into the AL register. After execution, SI is incremented or decremented based on the direction flag, and CX is decremented.

**Similarly, you can use LODSW to load a word (two bytes) into the AX register.**

***

### String instruction STOS

The STOS (Store String) instruction in Intel 8086 assembly language is used for storing a byte or word from the accumulator (AL or AX) into the destination string.

**The syntax:**

For loading a byte: <mark style="color:yellow;">STOSB</mark>

For loading a word: <mark style="color:yellow;">STOSW</mark>

The STOS instruction automatically increments or decrements the destination index (DI register) based on the direction flag in the flags register. If the direction flag is set, the index is decremented; If the flag is clear, the index is incremented.

```nasm
MOV DI, 2000h      ; Set the destination index to memory address 2000h 
MOV CX, 10         ; Set the count of bytes to store 
STOSB              ; Store the byte in AL into [DI] 
```

**DI** is the destination index pointing to the memory address of the destination string. **CX** is the count of bytes to be stored. **STOSB** is the instruction that stores a byte from the accumulator (AL) into the destination string. After execution, DI is incremented or decremented based on the direction flag, and CX is decremented.

**Similarly, you can use STOSW to store a word (two bytes) from the accumulator (AX) into the destination string.**

***

## Arithmetic Operations

When working with these operations, it's important to consider operand sizes, handle overflow conditions, and interpret the flags in the flags register for conditional branching.

### Instruction ADD

Adds the source operand to the destination operand and stores the result in the destination.

The syntax: <mark style="color:yellow;">ADD destination, source</mark>

destination - This is the operand to which the source operand will be added. It can be a register or a memory location. \
source - This is the operand that will be added to the destination operand. It can be another register, a memory location, or an immediate value.

```nasm
ADD AX, BX      ; Add the contents of register BX to register AX 
ADD CX, 10      ; Add the immediate value 10 to register CX
ADD [SI], BX    ; Add the contents of register BX to the memory location                           addressed by SI 
ADD [DI], 20    ; Add the immediate value 20 to the memory location                                addressed by DI
```



<figure><img src="../../../../.gitbook/assets/Pasted image 20241215105233.png" alt=""><figcaption></figcaption></figure>

***

### Instruction SUB

Subtracts the source operand from the destination operand and stores the result in the destination.

The syntax: <mark style="color:yellow;">SUB destination, source</mark>

destination - This is the operand from which the source operand will be subtracted. It can be a register or a memory location. \
source - This is the operand that will be subtracted from the destination operand. It can be another register, a memory location, or an immediate value.

```nasm
SUB AX, BX  
SUB CX, 10  
SUB [SI], BX 
SUB [DI], 20
```

***

### Instruction INC

Increments the value of the destination operand by 1.

The syntax: <mark style="color:yellow;">INC destination</mark>

destination - This is the operand (register or memory location) whose value will be incremented.

```nasm
INC AX       ; Increment the value in register AX by 1
INC [SI]     ; Increment the value in the memory location addressed by SI by 1
INC CX       ; Increment the value in register CX by 1 (16-bit mode)
INC AL 
INC BH
```

***

### Instruction DEC

Decrements the value of the destination operand by 1.

The syntax: <mark style="color:yellow;">DEC destination</mark>

destination - This is the operand (register or memory location) whose value will be decremented.

***

### Instruction MUL

Multiplies the accumulator (AL for byte multiplication, AX for word multiplication) by the source operand, storing the result in the double-length register pair (AX:DX for word multiplication).

The syntax: <mark style="color:yellow;">MUL source</mark>

source - This is the operand that will be multiplied with the accumulator (AL for byte multiplication, AX for word multiplication).

The MUL instruction performs multiplication, and the result is stored in the specified destination register (AX for byte multiplication, DX:AX for word multiplication). If the result is larger than the destination size, the overflow is stored in the overflow flag (OF), and the carry flag (CF) is also affected.

Keep in mind that MUL is an unsigned multiplication instruction. If you need to perform signed multiplication, you should use the IMUL instruction. Additionally, care should be taken to handle overflow conditions appropriately, especially when dealing with large values.

```nasm
MOV AX, 1000h ; Load AX with a value 
MOV BX, 5     ; Load BX with a value 
MUL BX        ; Multiply AX by BX, result in AX (and DX if needed) 
MOV AX, 2000h ; Load AX with a value
MOV SI, 2     ; Load SI with an index
MUL [SI]      ; Multiply AX by the contents of the memory ; location addressed                   by SI
```

***

### Instruction DIV

Divides the double-length register pair (AX:DX for word division) by the divisor operand, storing the quotient in the accumulator and the remainder in the doublelength register pair.

The syntax: <mark style="color:yellow;">DIV divisor</mark>

divisor - This is the operand by which the double-length register pair (AX:DX for word division) is divided. The result is stored in the accumulator (AL for quotient) and the double-length register pair (DX:AX for remainder).

The DIV instruction performs unsigned division, and the result is stored in the specified destination registers (AL for quotient, DX for remainder). If the quotient is too large to fit in the destination register (AL), an overflow condition occurs, and the program should be designed to handle it.

It's essential to ensure that the divisor is not zero before executing the DIV instruction to avoid a divide-by-zero error.

```nasm
MOV AX, 2000h   ; Load AX with a value 
MOV BX, 10      ; Load BX with a value (divisor) 
DIV BX          ; Divide AX (and DX) by BX, result in AX (quotient) and DX                       (remainder) 

MOV AX, 3000h   ; Load AX with a value 
MOV SI, 3       ; Load SI with an index
DIV [SI]        ; Divide AX (and DX) by the contents of the memory location                        addressed by SI
```

***

### Instruction IMUL

Performs signed multiplication of the source operand and the destination operand, storing the result in the destination.

The syntax: <mark style="color:yellow;">IMUL destination, source</mark>

destination - This is the operand to be multiplied. It can be a register or a memory location. source - This is the multiplier. It can be another register, a memory location, or an immediate value.

The IMUL instruction performs signed multiplication, and the result is stored in the destination register (AX for byte multiplication, DX:AX for word multiplication). If the result is larger than the destination size, the overflow is stored in the overflow flag (OF), and the carry flag (CF) is also affected.

Unlike MUL, which performs unsigned multiplication, IMUL is used for signed multiplication. Additionally, care should be taken to handle overflow conditions appropriately, especially when dealing with large values.

***

### Instruction IDIV

Performs signed division of the double-length register pair (AX:DX for word division) by the signed divisor operand, storing the quotient in the accumulator and the remainder in the double-length register pair.

The syntax: <mark style="color:yellow;">IDIV divisor</mark>

divisor - This is the operand by which the double-length register pair (AX:DX for word division) is divided. The result is stored in the accumulator (AL for quotient) and the double-length register pair (DX:AX for remainder).

The IDIV instruction performs signed division, and the result is stored in the specified destination registers (AL for quotient, DX for remainder). If the quotient is too large to fit in the destination register (AL), an overflow condition occurs, and the program should be designed to handle it.

It's essential to ensure that the divisor is not zero before executing the IDIV instruction to avoid a divide-by-zero error. Additionally, like with IMUL, care should be taken to handle overflow conditions appropriately, especially when dealing with large values.

***

### Instruction NEG

The NEG (Negate) instruction in Intel 8086 assembly language is used to change the sign of a number. It essentially performs two's complement negation on the specified operand.

The syntax: <mark style="color:yellow;">NEG destination</mark>

destinations - This is the operand (register or memory location) whose value will be negated.

The NEG instruction changes the sign of the specified operand by performing two's complement negation. If the operand is initially positive, NEG makes it negative, and vice versa. The flags in the flags register are affected, including the overflow flag (OF), which is set if the original value was the most negative representable number.

The NEG instruction is commonly used in scenarios where sign reversal is needed, such as in arithmetic calculations or data processing.

```nasm
MOV AX, 1000h      ; Load AX with a value 
NEG AX             ; Negate the value in register AX

MOV [SI], 2000h    ; Load the memory location addressed by SI with a value 
NEG [SI]           ; Negate the value in the memory location addressed by SI 
MOV CX, -500   
NEG CX             ; Load CX with a signed value ; Negate the value in register                       CX (16-bit mode)
```

1000(16) => => F000h

1000(16) => 0001 0000 0000 0000 (2) = 4096(10) 2(16) = 65536-4096 = F000h

***

### Instruction CMP

Compares two operands by subtracting operand2 from operand1, setting the flags without storing the result. The CMP (Compare) instruction in Intel 8086 assembly language is used to compare two operands. It subtracts the second operand from the first operand but does not store the result; instead, it updates the flags in the flags register based on the result of the subtraction.

The syntax: <mark style="color:yellow;">CMP operand1, operand2</mark>

operand1 - The first operand in the comparison. \
operand2 - The second operand in the comparison.

After the CMP instruction is executed, the flags register is updated, providing information about the relationship between the two operands. Commonly used flags include the zero flag (ZF), sign flag (SF), and overflow flag (OF), among others.

The CMP instruction is often followed by conditional jump instructions (e.g., JE, JG, JL) based on the flags' state to control program flow.

```nasm
MOV AX, 1000h   ; Load AX with a value 
MOV BX, 2000h   ; Load BX with another value 
CMP AX, BX      ; Compare AX and BX 

MOV CX, 500     ; Load CX with a value 
CMP CX, 300     ; Compare CX and the immediate value 300

MOV SI, 1000h   ; Load SI with an index 
MOV AX, 2000h   ; Load AX with a value 
CMP AX, [SI]    ; Compare AX and the contents of the memory ; location addressed                   by SI
```
