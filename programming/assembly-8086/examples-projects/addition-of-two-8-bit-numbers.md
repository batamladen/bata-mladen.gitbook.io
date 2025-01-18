# Addition of two 8-bit numbers

{% tabs %}
{% tab title="First Tab" %}
```
data segment
a db 09h
b db 0fh 
c dw ?
data ends


code segment
assume cs:code, ds:data
start:

mov ax, data
mov ds, ax

mov al, a
mov bl, b
add al, bl
mov c, ax

mov ah,4ch
int 21h

code ends 
end start 
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

## Understand the code

### Data Segment:

data segment: This section defines the variables.&#x20;

<mark style="color:yellow;">**a db 09h**</mark>**:** Declares an 8-bit byte variable a initialized to 09h (hexadecimal).&#x20;

<mark style="color:yellow;">**b db 02h**</mark>**:** Declares an 8-bit byte variable b initialized to 02h.&#x20;

<mark style="color:yellow;">**c dw ?**</mark>**:** Declares a 16-bit word variable c to store the sum. We use a 16-bit variable to handle potential overflow if the sum exceeds 255 (FFh).

### Code Segment:

<mark style="color:yellow;">**code segment**</mark>**:** This section contains the program instructions. ; assume cs:code,ds:data: Tells the assembler that the cs register points to the code segment and the ds register points to the data segment.&#x20;

<mark style="color:yellow;">**start**</mark>**:** The label marking the program’s entry point.&#x20;

<mark style="color:yellow;">**mov ax,data**</mark>**:** Loads the address of the data segment into the ax register.&#x20;

<mark style="color:yellow;">**mov ds,ax**</mark>**:** Sets the ds register to point to the data segment. This is crucial for accessing the variables a and b.&#x20;

<mark style="color:yellow;">**mov al,a**</mark>**:** Loads the value of a (09h) into the al register (the lower 8 bits of ax).&#x20;

<mark style="color:yellow;">**mov bl,b**</mark>**:** Loads the value of b (02h) into the bl register (the lower 8 bits of bx).&#x20;

<mark style="color:yellow;">**add al,bl**</mark>**:** Adds the contents of bl to al. The result (0Bh) is stored in al.&#x20;

<mark style="color:yellow;">**mov c,ax**</mark>**:** Moves the contents of ax (which now contains the sum in its lower byte, al) into the variable c.&#x20;

<mark style="color:yellow;">**int 3**</mark>**:** The breakpoint instruction halts execution for debugging purposes.
