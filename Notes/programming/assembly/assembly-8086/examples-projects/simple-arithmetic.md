# Simple Arithmetic

{% tabs %}
{% tab title="code.asm" %}
Takes two 8-bit numbers as input from the user\
Adds the numbers and stores the result in the AX register. \
Multiplies the numbers and stores the result in the BX register. \
Displays the results.

```nasm
DATA SEGMENT 
    MSG1 DB "Enter first number (0-9): $"
    MSG2 DB "Enter second number (0-9): $"
    RESULT_SUM DB "Sum is: $"
    Result_MUL DB "Product is: $"
DATA ENDS

CODE SEGMENT
    ASSUME CS:CODE, DS:DATA
    START:
        MOV AX, DATA
        MOV DS, AX

        ; Prompt for the first number
        MOV AH, 09H
        LEA DX, MSG1
        INT 21H

        ; Read the first number
        MOV AH, 01H
        INT 21H
        SUB AL, '0'
        MOV BL,AL

        ; Prompt for the second number
        MOV AH, 09H
        LEA DX, MSG2
        INT 21H

        ; Read the first number
        MOV AH, 01H
        INT 21H
        SUB AL, '0'
        MOV BH,AL

        ; Calculate sum
        MOV AL,BL
        ADD AL, BH
        ADD AL, '0'

        ; Display sum
        MOV AH, 09H
        LEA DX, RESULT_SUM
        INT 21H
        MOV DL, AL
        MOV AH,02H
        INT 21H

        ; Calculate product
        MOV AL, BL
        MUL BH
        ADD AL, '0'

        ; Display product
        MOV AH, 09H
        LEA DX, RESULT_MUL
        INT 21H
        MOV DL, AL
        MOV AH, 02H
        INT 21H

        ; Exit program
        MOV AX, 4C00H
        INT 21H
CODE ENDS
END START
```
{% endtab %}
{% endtabs %}





