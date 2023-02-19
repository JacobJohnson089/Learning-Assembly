# Here we will store all of the instructions 📦

<br />
<br />

## Sumary
[Booting Related](https://github.com/JacobJohnson089/Learning-Assembly/edit/main/Learning_Assembly/notes/00_Instructions.md#booting-related)

[Moving in the code](https://github.com/JacobJohnson089/Learning-Assembly/edit/main/Learning_Assembly/notes/00_Instructions.md#moving-in-the-code)

[Interacting with data](https://github.com/JacobJohnson089/Learning-Assembly/edit/main/Learning_Assembly/notes/00_Instructions.md#interacting-with-data)

<br />
<br />

### Booting related

```

org 0x7c00                                        ; tells the assambler to calculate all memory offset starting at 0x7c00
bits 16                                           ; We are telling the computer that he is in 16 bits
$                                                 ; Special symbol which is equal to the memory offset of the current line
$$                                                ; Special symbol which is equal to the memory offset of the beginning of the current section (program)

```

<br />

### Moving in the code

```

hlt                                               ; Stops CPU from executing (it can ve resumed by an interupt)
jmp     label                                     ; Jumps to a given location, unconditionally (equivalent to goto in C)
cmp data1, data2                                  ; Verifies if data1 is equal to data2
je  label                                         ; Evaluates the result of the previous comparison instruction to determine whether to jump to the target label
jz                                                ; jz destination jumps to destination if zero flag is set

```
<br />

### Interacting with data

```

db data                                           ; Stands for "define byte". Writes given byte(s) to the assembled binary file. times number instructions ; Repeats given instruction or piece of data a number of time
dw data                                           ; Stands for "define word(s)". Writes given word(s) (2 byte value, encoded in little endian) to the assembled binary file.
lodsb,lodsw,lodsd                                 ; These instruction load a byte/word/double-word from DS:SI into AL/AX/EAX, then increment SI by the number of bytes loaded
or                                                ; Or destination, source perform bitwise OR between source and destination, stores result in destination

```




