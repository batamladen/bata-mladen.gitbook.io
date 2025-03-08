# Flags Practice

## CF (Carry Flag)

When we have two 8-bit numbers in a calculation, and the result is more than 8 bits (9 for example) than the carry flag is 1\
If we have two 16-bit numbers in a calculation, and the result is more than 16 bits (17) than the carry flag is 1

<figure><img src="../../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

***

## ZF (Zero Flag)&#x20;

When the result is 0, zero flag is 1

***

## SF (Sign Flag)

If the result in a calculation has the MSB (most significant bit) 1, than the sign flag is 1. \
If we have a carry flag up, a bigger result than 8-bit, the carry flag does not determine the sign flag, the sign flag is 1 only if the msb of the 8-bit is 1.

<figure><img src="../../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

***

## Overflow Flag

This flag is up if in a calculation, the MSB of the first number is different than the result MSB.

<figure><img src="../../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

***

## Parity Flag

If the number of 1s in the result is paired (2, 4, 6, 8) the flag is up \
If we have 16-bit situation, the Least significant byte is looked (last 8 bits)

<figure><img src="../../../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>
