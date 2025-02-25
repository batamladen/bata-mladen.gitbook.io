# Control Structures

## Branching

(Conditional branching and jumps)

**Definition** Conditional branching is a fundamental concept in assembly language programming that allows the flow of execution to be changed, based on the evaluation of a condition.

In the context of the 8086 microprocessor, this involves making decisions to either continue executing the next instruction in sequence or to jump to a different part of the code based on the status of specific flags.

**Purpose** Enables implementation of control structures such as if-then-else conditions, loops, and switches which are essential for creating logical pathways in code.

The Intel 8086 microprocessor evaluates conditions using the flags in the flag register, particularly the zero flag (ZF), carry flag (CF), sign flag (SF), overflow flag (OF), and parity flag (PF). These flags are set or cleared by arithmetic, logical, and comparison operations.

***

### JMP (Jump)

The JMP instruction is used to transfer control to another location in the program unconditionally. It simply transfers execution to the specified target address.

For example:

```nasm
JMP label_1     ; go to the label1 and continue from there
;code
;code

label_1:
;more code
```

***

### JE (Jump If Equal)

The JE instruction is used to jump to a target address if the **Zero Flag (ZF)** is set (indicating that the result of a previous operation was zero or equal). It is often used in conditional branching for equality checks.

For example:

```nasm
JE label2     ; if ZF==1 go to the label2 and continue from there
… 
… 

label2:
…
```

***

JNE (Jump If Not Equal)

The JNE instruction is used to jump to a target address if the <mark style="color:red;">**Zero Flag (ZF)**</mark> is clear (indicating that the result of a previous operation was not zero or not equal). It is used for inequality checks.

For example:

```nasm
JNE label3     ; if ZF==0 go to the label3 and continue from there
…
…

label3:
…
```

***

### JG (Jump If Greater)

The JG instruction is used to jump to a target address if the <mark style="color:red;">**Zero Flag (ZF)**</mark> is clear, and the <mark style="color:red;">**Sign Flag (SF)**</mark> and <mark style="color:red;">**Overflow Flag (OF)**</mark> are the same (indicating a signed greater-than comparison).

For example:

```nasm
JG label4      ;if ZF==0 and SF==OF go to the label4 and continue from there
…
…

label4:
…
```

***

### JGE (Jump If Greater or Equal)

The JGE instruction is used to jump to a target address if the <mark style="color:red;">**Zero Flag (ZF)**</mark> is set, or the <mark style="color:red;">**Sign Flag (SF)**</mark> and <mark style="color:red;">**Overflow Flag (OF)**</mark> are the same (indicating a signed greaterthan-or-equal comparison).

For example:

```nasm
JGE label5    ; if ZF==1 (or SF==OF) go to the label5 and continue from there
…
…

label5:
…
```

***

### JL (Jump If Less)

The JL instruction is used to jump to a target address if the <mark style="color:red;">**Zero Flag (ZF)**</mark> is clear, and the <mark style="color:red;">**Sign Flag (SF)**</mark> and <mark style="color:red;">**Overflow Flag (OF)**</mark> are different (indicating a signed less-than comparison).

For example:

```nasm
JL label6    ; if ZF==0 (and SF!=OF) go to the label6 and continue from there
…
…

label6:
…
```

***

### JLE (Jump If Less or Equal)

The JLE instruction is used to jump to a target address if the <mark style="color:red;">**Zero Flag (ZF)**</mark> is set, or the <mark style="color:red;">**Sign Flag (SF)**</mark> and <mark style="color:red;">**Overflow Flag (OF)**</mark> are different (indicating a signed less-thanor-equal comparison).

For example:

```nasm
JLE label7     ; if ZF=1 (or SF!=OF) go to the label7 and continue from there
…
…

label7:
…
```

***

### Other Branching Instructions

▪ <mark style="color:orange;">**JE/JZ**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Equal/Zero)</mark>: Jump if the Zero Flag (ZF) is set (indicating that the operands are equal, or the result of the last operation was zero).

▪ <mark style="color:orange;">**JNE/JNZ**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Not Equal/Not Zero)</mark>: Jump if the Zero Flag (ZF) is clear (indicating that the operands are not equal, or the result of the last operation was not zero).

▪ <mark style="color:orange;">**JG**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Greater)</mark>: Jump if the Zero Flag (ZF) is clear, and the Sign Flag (SF) and Overflow Flag (OF) are the same (indicating a signed greater-than comparison).

▪ <mark style="color:orange;">**JGE/JNL**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Greater or Equal/Not Less)</mark>: Jump if the Zero Flag (ZF) is set, or the Sign Flag (SF) and Overflow Flag (OF) are the same (indicating a signed greater-than-or-equal comparison).

▪ <mark style="color:orange;">**JL/JNGE**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Less/Not Greater or Equa</mark>l): Jump if the Zero Flag (ZF) is clear, and the Sign Flag (SF) and Overflow Flag (OF) are different (indicating a signed less-than comparison).

▪ <mark style="color:orange;">**JLE/JNG**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Less or Equal/Not Greater)</mark>: Jump if the Zero Flag (ZF) is set, or the Sign Flag (SF) and Overflow Flag (OF) are different (indicating a signed less-than-or-equal comparison).

▪ <mark style="color:orange;">**JC**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Carry)</mark>: Jump if the Carry Flag (CF) is set (indicating a carry or borrow occurred in an arithmetic operation).

▪ <mark style="color:orange;">**JNC/JNB**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if No Carry/Not Below):</mark> Jump if the Carry Flag (CF) is clear (indicating no carry or borrow occurred).

▪ <mark style="color:orange;">**JO**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Overflow)</mark>: Jump if the Overflow Flag (OF) is set (indicating signed overflow occurred in an arithmetic operation).

▪ <mark style="color:orange;">**JNO**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if No Overflow)</mark>: Jump if the Overflow Flag (OF) is clear (indicating no signed overflow occurred).

▪ <mark style="color:orange;">**JS**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Sign)</mark>: Jump if the Sign Flag (SF) is set (indicating the result is negative).

▪ <mark style="color:orange;">**JNS**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if No Sign)</mark>: Jump if the Sign Flag (SF) is clear (indicating the result is non-negative or positive).

▪ <mark style="color:orange;">**JP/JPE**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Parity/Parity Even)</mark>: Jump if the Parity Flag (PF) is set (indicating an even number of set bits in the result).

▪ <mark style="color:orange;">**JNP/JPO**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if No Parity/Parity Odd)</mark>: Jump if the Parity Flag (PF) is clear (indicating an odd number of set bits in the result).

▪ <mark style="color:orange;">**JB/JNAE**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Below/Not Above or Equal)</mark>: Jump if the Carry Flag (CF) is set (indicating an unsigned less-than comparison).

▪ <mark style="color:orange;">**JAE/JNB**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Above or Equal/Not Below)</mark>: Jump if the Carry Flag (CF) is clear (indicating an unsigned greater-than-or-equal comparison).

▪ <mark style="color:orange;">**JBE/JNA**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Below or Equal/Not Above)</mark>: Jump if the Carry Flag (CF) is set or the Zero Flag (ZF) is set (indicating an unsigned less-than-or-equal comparison).

▪ <mark style="color:orange;">**JA/JNBE**</mark> <mark style="color:orange;"></mark><mark style="color:orange;">(Jump if Above/Not Below or Equal)</mark>: Jump if the Carry Flag (CF) is clear and the Zero Flag (ZF) is clear (indicating an unsigned greater-than comparison).

***

## Looping

### For Loop

A for loop is typically implemented <mark style="color:yellow;">using a combination of instructions like MOV, CMP, JNZ, and loop control variables</mark>. The loop control variable <mark style="color:yellow;">is typically decremented or incremented within the loop body</mark> until a certain condition is met.

For loop repeats a block of code a fixed number of times.

In Intel 8086 assembly language, a for loop can be implemented using the CX register as a loop counter.

Here's an example of a simple for loop that counts from 1 to 5:

```nasm
mov cx, 5        ; Initialize CX with the loop count (5)
mov ax, 18 

for_loop: 
cmp cx, ax       ; Bitwise CX with itself – to check if zero
jz loop_end      ; Check if CX is zero (end of loop)

…                ; for body

inc cx           ; Increment CX
jmp for_loop     ;continue looping

loop_end:        ; Code after the loop 
int 0x21         ;terminate program
```



**The For Loop Block Diagram**

<figure><img src="../../../.gitbook/assets/Pasted image 20241222124744.png" alt=""><figcaption></figcaption></figure>

***

### While Loop

A while loop can be implemented using a combination of instructions such as <mark style="color:yellow;">CMP, JZ, and loop control variables</mark>. The loop continues **as long as a specified condition is true**.

A while loop is used to repeat a specific block of code an **unknown number of times**, until a condition is met.

```nasm
count db 5            ; Define a byte variable 'count' with an initial value of 5
mov cx, 20            ; Load 20 into the CX register

while_loop:
mov ax, [count]
test cx, ax           ; bitwise check -> CX AND AX
jz while_loop_end     ; Jump to 'while_loop_end' if CX is zero

…                     ; loop body

dec cx 
jmp while_loop

while_loop_end:       ; If (CX AND AX) is zero, the loop terminates                                    ; Continue with the rest of your program here

int 21h               ; Terminate program
```



**The While-Loop block diagram**

<figure><img src="../../../.gitbook/assets/Pasted image 20241222124830 (1).png" alt=""><figcaption></figcaption></figure>

***

### Do-While Loop

A do-while loop can be implemented using a combination of instructions like **JMP, CMP, and JZ**. The loop body is executed at least once, and then the condition is checked to determine if the loop should continue.

```nasm
mov cx, 1             ; Initialize CX with the loop count (5)

do_while_loop: 
...                   ; Loop body

dec cx                
jz loop_end           ; Check if CX is zero (end of loop)

jmp do_while_loop     ; Continue looping

loop_end:             ; Code after the loop
int 21h               ; Terminate the program
```



**Do-While Loop block diagram**

<figure><img src="../../../.gitbook/assets/Pasted image 20241222124950.png" alt=""><figcaption></figcaption></figure>

***

### Infinite Loop

An infinite loop is created using an <mark style="color:yellow;">unconditional jump instruction</mark> (e.g., JMP), causing the program to continuously execute a block of code <mark style="color:yellow;">until manually interrupted or terminated</mark>.

```nasm
loop_start: 
mov ah, 9                        ; AH = 9 indicates print string function
mov dx, hello_msg                ; DX points to the address of the message
int 21h                          ; Call the DOS interrupt to print the message

jmp loop_start                   ; Infinite loop: Jump back to 'loop_start’
hello_msg db "Hello, World!", 0  ; Null-terminated message

int 20h                          ; Terminate the program
```

***

### Nested Loops

Nested loops can <mark style="color:yellow;">be implemented by using multiple loop control variables and appropriately placed labels and jump instructions</mark>. This allows creation of more complex looping structures.

***

### Looping with Counters

Looping can also be controlled using loop counters. <mark style="color:yellow;">The CX register is commonly used as a loop counter in Intel 8086 assembly language</mark>. You can decrement or increment the counter in each iteration and use conditional jumps (e.g., JNZ or JZ) to control the loop.

```nasm
mov cx, 10            ; Initialize CX with the loop count (10) 

loop_start: 
dec cx                ; Decrement CX

jz loop_end           ; Check if CX is zero (end of loop)
 
 
jmp loop_start        ; Continue looping  
 
loop_end:             ; Code after the loop 
int 20h               ; Terminate the program
```

***

### Break Statement

A break statement can be implemented using conditional jumps and labels to exit a loop prematurely based on a certain condition.

<figure><img src="../../../.gitbook/assets/Pasted image 20241222124442.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Pasted image 20241222124513.png" alt=""><figcaption></figcaption></figure>

***

### Continue Statement

A continue statement can be implemented using conditional jumps and labels to skip the current iteration of a loop and proceed to the next iteration based on a certain condition.

<figure><img src="../../../.gitbook/assets/Pasted image 20241222124538.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/Pasted image 20241222124551.png" alt=""><figcaption></figcaption></figure>



***

### Looping with Arrays

Use loops to iterate over arrays or data structures, performing operations on each element or item in the array.



<figure><img src="../../../.gitbook/assets/Pasted image 20241222124623.png" alt=""><figcaption></figcaption></figure>

***

### Looping with Strings

Loops can be used for string manipulation, iterating over characters in a string, and performing string-related operations.



<figure><img src="../../../.gitbook/assets/Pasted image 20241222124648.png" alt=""><figcaption></figcaption></figure>
