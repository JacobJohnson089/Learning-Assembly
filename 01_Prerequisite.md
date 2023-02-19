# Let's start with the basics 💽    ([source](https://www.youtube.com/watch?v=9t-SPC7Tczc&t=489s&ab_channel=nanobyte))

<br />
<br />

### How does the BIOS find your operating system

<br />

Legacy Booting
- BIOS loads the first sector of each bootable device into the memory at address ```0x7c00```
- BIOS check for the ```0xAA55``` signature
- If found, it starts executing the code


EFI
- BIOS looks into special EFI partitions
- OS must be compiled as an EFI program

<br />

We however will be using Legacy Booting

<br />

<br />
<br />

### Structure of an assembly instruction

<br />

An assembly instruction is made up of an ```mnemonic operand1, operand2, ect...``` e.g ``` mov ax, bx ```, or  ``` db 1 , 2, 3, 4 ```

<br />
<br />

### Some instruction needed to understand exemples given in later "notes"


```
org 0x7c00                                        ; tells the assambler to calculate all memory offset starting at 0x7c00

bits 16                                           ; We are telling the computer that he is in 16 bits

db byte1,...                                      ; Stands for "define byte". Writes given byte(s) to the assembled binary file.

times number instructions                         ; Repeats given instruction or piece of data a number of time

$                                                 ; Special symbol which is equal to the memory offset of the current line

$$                                                ; Special symbol which is equal to the memory offset of the beginning of the current section (program)

dw                                                ; Stands for "define word(s)". Writes given word(s) (2 byte value, encoded in little endian) to the assembled binary                                                     file
```

<br />
<br />

### The bare minimum you need to know about registers

<br />

All CPUs have registers, which is a very small pieces of memory read from and written to very fast. <br />
For now you only need to know about general purpose registers, which are you guessed it are general purpouses <br />

<br />

``ax``  ``bx`` ``cx`` ``dx`` These registers can be broken down into smaller parts, ``ax`` --> ``al`` (a low) and ``ah`` (a high) these will come handy later

<br />

# Congradulation you can start to learn assembly now ! 🥳









