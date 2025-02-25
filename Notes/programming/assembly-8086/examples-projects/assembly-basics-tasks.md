# Lesson 03 Tasks

{% tabs %}
{% tab title="Task 01.asm" %}
```nasm
; Task 01: add two numbers

; Direct addressing
MOV AL, 0x10
MOV BL, 0x20

ADD AL, BL


; With Variables
X DB 0x10
Y DB 0x20
RESULT DB ?

MOV AL, X
MOV BL, Y

ADD AL, BL

MOV DI OFFSET[RESULT]
MOV DI AL
```
{% endtab %}

{% tab title="Task 02.asm" %}
```nasm
; Task 02: Subtract Two Numbers

; Direct addressing
MOV AL, 0x10
MOV BL, 0x20

SUB BL, AL


; With Variables
X DB 0x10
Y DB 0x20
RESULT DB ?

MOV AL, X
MOV BL, Y

SUB BL, AL

MOV DI, OFFSET RESULT
MOV DI BL
```
{% endtab %}

{% tab title="Task 03.asm" %}
```nasm
; Task 03: Multiply Two Numbers

MOV AL 10
MOV BL 3
MUL BL


; With Variables
DATA SEGMENT
X DB 10
Y DB 3
RESULT DB ?
DATA ENDS

CODE SEGMENT
ASSUME CS:CODE, DS:DATA

START:
    MOV AX, DATA        
    MOV DS, AX           

    MOV AL, X
    MOV BL, Y

    MUL BL

    MOV DI, OFFSET RESULT
    MOV [DI], AL 

    ; Terminate program (for DOS emulator)
    MOV AX, 4C00H
    INT 21H

END START
CODE ENDS
```
{% endtab %}

{% tab title="Task 04.asm" %}
```nasm
; Task 04: Divide Two Numbers

; Direct addressing
MOV AL, 10
MOV BL, 5

DIV AL,BL


;With Variables
DATA SEGMENT
X DB 10
Y DB 5
RESULT DB ?
DATA ENDS

CODE SEGMENT
ASSUME CS:CODE. DS:DATA

START:
    MOV AX, DATA
    MOV DS, AX

    MOV AL, X
    MOV BL, Y

    DIV AL, BL

    MOV DI, OFFSET RESULT
    MOV [DI], AL


CODE ENDS  
END START
```
{% endtab %}

{% tab title="Task 05.asm" %}
```nasm
; Task 05: Swap Two Numbers

DATA SEGMENT
NUM1 DW 0x1001
NUM2 DW 0x2002
DATA ENDS

CODE SEGMENT
ASSUME CS:CODE, DS:DATA

START: 
    MOV AX, DATA
    MOV DS, AX

    MOV AX, NUM1
    MOV BX, NUM2

    MOV DX, AX
    MOV AX, BX
    MOV BX, DX

    MOV NUM1, AX
    MOV NUM2, BX

    MOV AX, 4C00H
    INT 21H

CODE ENDS
END START
```
{% endtab %}
{% endtabs %}

