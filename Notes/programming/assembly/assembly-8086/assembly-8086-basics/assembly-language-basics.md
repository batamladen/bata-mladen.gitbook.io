# Assembly Language Basics

## Assembly Language Basics

In Assembly Language:&#x20;

* syntax
* data types&#x20;
* addressing modes&#x20;

are essential for writing efficient and correct programs.

***

### Syntax

* Assembly language syntax is mnemonic and symbolic, making it readable and understandable.&#x20;
* Instructions are represented by mnemonics like MOV, ADD, SUB, JMP, etc. ▪ Instructions can have operands that specify the data to be operated on.&#x20;
* Operands can be <mark style="color:yellow;">registers, memory locations, or immediate values</mark>.&#x20;
* Labels are used for defining addresses in the program and creating jump targets.&#x20;
* Comments start with a semicolon ( ; ) and are used for documentation.

<figure><img src="../../../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

***

### Data Types

In 8086 Assembly, there is no strict type system as in high-level languages, but data is classified based on size and purpose.\


Common data types include:&#x20;

* Byte (8 bits): Represented as DB (Define Byte).&#x20;
* Word (16 bits): Represented as DW (Define Word).&#x20;
* Doubleword (32 bits): Represented as DD (Define Doubleword).&#x20;
* Quadword (64 bits): Represented as DQ (Define Quadword).

Data types are used for defining **variables, constants, and memory allocation**.

***

### Addressing Modes

Addressing modes determine how an operand's address is calculated.

Common addressing modes include:

* **Immediate:** Operand is a constant value ( <mark style="color:yellow;">MOV AX, 5</mark> ).&#x20;
* **Register:** Operand is a value stored in a register ( <mark style="color:yellow;">MOV AX, BX</mark> ).&#x20;
* **Direct:** Operand is the address of a memory location ( <mark style="color:yellow;">MOV AL, \[1000h]</mark> ).&#x20;
* **Indirect:** Operand is a register that holds the address of the memory location ( <mark style="color:yellow;">MOV AL, \[BX]</mark> ).&#x20;
* **Indexed:** Operand is an indexed memory location using a base register and an index register ( <mark style="color:yellow;">MOV AL, \[BX+SI]</mark> ).&#x20;
* **Base-Indexed:** Operand is a combination of base and index registers ( <mark style="color:yellow;">MOV AL, \[BX+SI+10h]</mark> ).
* **Scaled Indexed:** Operand is an indexed memory location with a scaling factor ( <mark style="color:yellow;">MOV AL, \[BX+SI \* 2]</mark> ).

***

### Rules for writing code

The code consists of:

* **Mnemonics**
  * Mnemonics are Machine Instructions&#x20;
  * Mnemonics are abbreviations&#x20;
  * Example: MOV, ADD, SUB, IMUL
* **Operands**
  * Operands are Data or Addresses&#x20;
  * Types of Operands: Registers, Memory, Constants&#x20;
  * Example: MOV AX, \[BX]
* **Comments**
  * &#x20;Enhance Code Readability&#x20;
  * Used for documenting important parts of the code&#x20;
  * Ignored by the Assembler Everything from the character ' ; ' till the end of the line is ignored by Assembler&#x20;
  * Example: ; This is a Comment

***

### Syntax Rules

Assembly is case **insensitive** (big or small letters, its the same)

* **Rule:** If an identifier is present in the label field, always start that identifier in column one of the source line.&#x20;
* **Rule:** All operands should start in the same column. Generally, this should be column 17 (the second tab stop) or some other convenient position&#x20;
* **Rule:** All mnemonics should start in the same column. Generally, this should be column 25 (the third tab stop) or some other convenient position.

**Exception:** If a mnemonic (typically a macro) is longer than seven characters and requires an operand, you have no choice but to start the operand field beyond column 25 (this is an exception assuming you've chosen columns 17 and 25 for your mnemonic and operand fields, respectively).&#x20;

**Guideline:** Try to always start the comment fields on adjacent source lines in the same column&#x20;

**Guideline:** Use blank lines to separate special blocks of code from the surrounding code. Use an aesthetic looking row of asterisks or dashes if you need a stronger separation between two blocks of code (do not overdo this, however)

***

### Example Code:

```
GetFileRecords: mov    dx, offset DTA                          ;Set up DTA
                DOS    SetDTA
		mov    dx, FileSpec
		mov    cl, 37h
		DOS    FindFirstFile
		jc     FileNotFound

;              ********************************************* 
		mov    di, offset fileRecords  ;DI -> storage for file names 
		mov    bx, offset files        ;BX -> array of files 
		sub    bx, 2                   ;Special case for 1st iteration
				
```

***

## Error Handling

Assembly Errors:&#x20;

* **Undefined symbol:** Program refers to a label that does not exist&#x20;
  * **How to fix:** check spelling of both the definition and access
* **Undefined opcode or pseudo-op**&#x20;
  * **How to fix:** check the spelling/availability of the instruction
* **Addressing mode not available**&#x20;
  * **How to fix:** look up the addressing modes available for the instruction
* **Expression error**&#x20;
  * **How to fix:** check parentheses, start with a simpler expression
* **Phasing Error:** occurs when the value of a symbol changes from pass1 to pass2&#x20;
  * **How to fix:** first remove any undefined symbols, then remove forward references
* **Address error**&#x20;
  * **How to fix:** use org pseudo-op’s to match available memory.

***

### Data Types - Byte, Word, Doubleword, Quadword

**Byte Data Type** \
Represents 8 Bits (1 Byte) \
Example: DB 65 ; (Decimal Byte Value)

**Word Data Type** \
Represents 16 Bits (2 Bytes) \
Example: DW 1234 ; (Decimal Word Value)

**Doubleword Data Type** \
Represents 32 Bits (4 Bytes) \
Example: DD 12345678h ; (Hex Doubleword Value)

**Quadword Data Type** (no native instructions or directives to directly declare Quadword (64-bit) data types) \
Represents 64 Bits (8 Bytes) \
Example: DQ 123456789ABCDEFh ; (Hex Quadword Value)

***

### Data Type Conversion

Maintaining Data Integrity

CBW - convert byte to word extends the sign bit of AL into the AH register. This preserves the number 's sign

```nasm
byte_val SBYTE -101 
mov al, byte_val                 ; AL = 9Bh
cbw                              ; AX = FF9Bh
```

CWD - convert word to doubleword extends the sign bit of AX into the DX register:

```nasm
word_val SWORD -101 
mov ax, word_val                    ; FF9Bh ; AX = FF9Bh
cwd                                 ; DX:AX = FFFFh:FF9Bh
```

***

## Addressing Modes

Different Ways to Specify Operand Locations -> Immediate, Register, Direct, Indirect, Indexed, Based Indexed

**Immediate Addressing Mode** - Operand as Constants \
MOV AX, 42

**Register Addressing Mode** - Operand is a Register \
MOV BX, AX

**Direct Addressing Mode** - Operand is a Memory Location \
MOV AL, \[SI]

**Indirect Addressing Mode** - Register Holds Memory Address \
MOV DL, \[BX]

**Indexed Addressing Mode** - Operand as Indexed Memory Location \
MOV AL, \[BX+SI]

**Based Indexed Addressing Mode** - Combination of Base and Index Registers \
MOV AH, \[BX+SI+10h]

***

### Calculate the Sum of Two Numbers

Example 1:

```nasm
MOV AX, 2340h
MOV BX, 4008h 
ADD AX, BX
```

Example 2:

```nasm
Number1 DW 2340h          ; h -> hex number
Number2 DW 4008h          ; h -> hex number
Result DW ?

MOV AX, Number1 
MOV BX, Number2 
ADD AX, BX  ;  
MOV Result, AX
```

***

Few registers – additional info

**Register SP - stack pointer** (16 bits) It points to the topmost item of the stack. If the stack is empty the stack pointer will be (FFFE)h. Its offset address is relative to the stack segment.

**Register BP- base pointer** (16 bits) It is primarily used in accessing parameters passed by the stack. Its offset address is relative to the stack segment.

**Register SI - source index** (16bits) It is used in the pointer addressing of data and as a source in some string-related operations. Its offset is relative to the data segment.

**Register DI - destination index** (16 bits) It is used in the pointer addressing of data and as a destination in some stringrelated operations. Its offset is relative to the extra segment.

***

### Example – Accessing array elements



```
Array DW 10, 20, 30, 40, 50 ; Define an array of 16-bit integers 
Index DW 2                  ; Index for element access (e.g., access the third element) 
Result DW ?                 ; Storage for the accessed element
```

```
; Load the array index into a register 
MOV BX, Index
```

```
; Calculate the offset in the array based on the index 
MOV CX, 2                   ; Each element in the array is 2 bytes (16 bits) 
MUL CX                      ; Multiply the index by 2 to get the offset 
MOV SI, AX                  ; Store the offset in SI
```

```
; Access the array element and store it in Result 
MOV AX, Array[SI] ; Load the element at the calculated offset 
MOV Result, AX
```



***

## Microsoft Macro Assembler (MASM)

The assembler contains as its front-end a macro processor.

The macro processor scans the source file for macro definitions and macro calls written in Macro Processor Language (MPL).

**Macro calls are expanded** according to macro definitions, and the resulting source assembly language is assembled by the assembler.

By using MPL, you can create macros specific to your application that can generate sequences of assembly language instructions or directives.

The macro processor is a very powerful string replacement facility that can help to simplify a programming task. Repeatedly used code sequences can be replaced by a simple macro call. Also, frequently used assembler directive statements can be replaced by macro calls.

***

### MASM Directives

MASM uses Directives – instructions to the Assembler Indicate how an operand or section of a program is to be processed by the assembler.

**DB (Define Byte)** \
**DW (Define Word)** \
**DD (Define Double Word)**\
very often used to define and store memory data. With these directives, we give a symbolic name of a memory location and at the same time we define its size.

**ARRAY DW 100 DUP(0**) \
reserve 100 words of storage in memory, give it the name “ARRAY”, and initialize those 100 words with 0000.

**ARRAY DW 100 DUP(0)** \
reserve 100 words of storage in memory, give it the name “ARRAY”, and initialize those 100 words with 0000.

**ARRAY DW 100 DUP(?)** \
reserve 100 words of storage in memory, give it the name “ARRAY”, and the Assembled does not initialize them with anything.

**ORG** \
ORG (Origin) directive is an indication on where to put the next piece of code/data, related to the current segment. changes the starting offset address of the data in the data segment to a desired location the origin of data or the code must be assigned to an absolute offset address with the ORG statement

```
ORG 0000h 
DataX DB  28 
DataY DB 45
ORG 0020h 
DataZ DB 86 

// 0000 -> 1Ch
// 0001 -> 2Dh
// 0020 -> 56h
```



**ASSUME** the name of the logical segment it should use for a specified segment. It instructs the Assembler what names are chosen for the code, data, extra, stack segments

```
ASSUME DS:data , CS:code, SS:stack
```



**EQU**\
equates a numeric, ASCII, or label to another label Each time the assembler finds the given name in the program, it will replace the name with the value or symbol we equated with that name.

```
CONTROL_WORD EQU 11001001               ; replacement 
MOV AX, CONTROL_WORD                    ; assignment

COUNT EQU 10 
CONST EQU 20H 
MOV AH, COUNT 
MOV AL, CONST
```

\
**PROC** \
indicate start and end of a procedure (subroutine)

```
PROCONVERT PROC FAR 

; identifies the start of a procedure named PROCONVERT and tells the Assembler that the procedure is far (in a segment with a different name from the one that contains the instruction that calls the procedure.)
```

The PROC directive, which indicates the start of a procedure, must also be followed with a NEAR or FAR

A NEAR procedure is one that resides in the same code segment as the program, often considered to be local A FAR procedure may reside at any location in the memory system, considered global

The term global denotes a procedure that can be used by any program. Local defines a procedure that is only used by the current program.

***

### Model Types:

Tiny – All data and code fit in one segment. Tiny programs are written in .COM, which means the program must be originated at location 100h.

Small – Contains two segments – One DS (64KB) and one CS (64KB)

Medium – Contains one DS segment (64KB) and any number of CS for large programs.

Compact – Contains one CS (64KB) for the program and any number of DS contain the data

Large – Allows any number of CS and DS

Huge – same as Large, but the DS segments may contain more than 64KB each

Tiny programs are written in **.COM**, which means <mark style="background-color:yellow;">**the program must be originated at location 100h**</mark>**.**

COM programs in DOS are set up: \
The **first 256 bytes contain management data** for the operating system and the command line (which can be re-used as 128-byte data transfer buffer), and **the program code starts after it.**

So ORG 100h forces the toolchain to build a program that fits the memory layout of COM programs.

***

### MASM – structure of a main module with full segment directives

```
assume ds:data, cs:code 
data segment 

data ends 

code segment 
START: 
    nop          ; code here

code ends 
end START
```

***

### Assembler Phases

Phase 1 - Analysis The Analysis Phase validates the syntax of the code, checks for errors, and creates a symbol table.

Phase 2 – Synthesis The Synthesis Phase converts the assembly language instructions into machine code, using the information from the Analysis Phase.

These two phases work together to produce the final machine code that can be executed by the computer. The combination of these two phases makes the Assembler an essential tool for transforming assembly language into machine code, ensuring high-quality and error-free software.
