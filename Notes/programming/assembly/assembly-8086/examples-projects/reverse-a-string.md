# Reverse a String

{% tabs %}
{% tab title="code.asm" %}
```nasm
DATA SEGMENT
	InputString db "Hello00!", 0, '$'  ; Null-terminated string to be reversed
	
DATA ENDS

CODE SEGMENT
	ASSUME CS:CODE, DS:DATA

	START:

	MOV AX, DATA        		    ; Load data segment address
	MOV DS, AX

	LEA SI, InputString  		    ; Load address of the input string (SI points to the beginning)


	; Calculate the length
	LEA SI, InputString
	MOV CX, -1
DO:
    	LODSB
    	INC CX
    	CMP AL, 0
    	JNE DO


    	MOV DI, OFFSET InputString
    	ADD DI, CX
	DEC DI

	; Reverse the string
	MOV SI, OFFSET InputString	    ; Reset SI to the beginning

reverseLoop:
	CMP SI, DI             		    ; Compare SI and DI to check if we've reached the middle
	JAE done               		    ; If SI >= DI, the string is reversed

	MOV AL, [SI]           		    ; Load character from the beginning
	MOV AH, [DI]           		    ; Load character from the end
	MOV [SI], AH           		    ; Swap characters
	MOV [DI], AL
	INC SI                 		    ; Move SI forward
	DEC DI                 		    ; Move DI backward
	JMP reverseLoop

done:
	; Print the reversed string
	MOV AH, 09h            		    ; Function code for string output
	MOV DX, OFFSET InputString  	    ; DS:DX points to the reversed string; The string must ends with '$'
	INT 21h                		    ; Display the reversed string


    	MOV AX, 4C00h           	    ; Function code for program termination
    	INT 21h                 	    ; Trigger software interrupt 21h (DOS)

CODE ENDS
END START	
```


{% endtab %}
{% endtabs %}

