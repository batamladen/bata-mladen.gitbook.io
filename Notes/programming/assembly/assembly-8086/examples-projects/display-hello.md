# Display "Hello"

{% tabs %}
{% tab title="code.asm" %}
```
data segment
string db "Hello"
data ends

code segment
assume cs:code, ds:data
start:

mov ax, data
mov ds, ax

mov sp, 0d000h
mov bx, offset string
mov cx, 0005h

move:
mov ah, 02h
mov dl,[bx]
int 21h
inc bx
dec cx 
jnz move

mov ax, 4c00h
int 21h

code ends
end start
```
{% endtab %}
{% endtabs %}

