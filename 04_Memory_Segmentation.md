# What is memory segmentation ? 🚧 Work in progress I'm still learning about memory segmentation some information might be slightly incorect 🚧

<br />
<br />


Before reading this I highly sugest you watch theses videos : [simple overview](https://youtu.be/DmDJsQqafBE?list=PLm3B56ql_akNcvH8vvJRYOc7TbYhRs19M&t=188),
[Slightly more indepth overview](https://youtu.be/9t-SPC7Tczc?t=649), [indepth overview]()

<br />
<br />

In [02_How_To_Visualize_Data](https://github.com/JacobJohnson089/Learning-Assembly/blob/main/Learning_Assembly/notes/02_How_To_Visualize_Data.md) we talked about the address of a line
Well, you noticed that the address looks like this ```00000000``` which is a ``` 8 bytes ``` value ``` 64 bits ```
However in [03_Registers](https://github.com/JacobJohnson089/Learning-Assembly/blob/main/Learning_Assembly/notes/03_Registers.md#lets-quickly-analyze-the-data) 
We learn that the "address" was a ```4 bytes``` value. This is not magic, This is memory segmentation <br />

<br />
<br />

The address shown in this format ```00000000``` is actually this ```segment:offset``` where segment is ``` 4 bytes``` <br />
And offset is also ```4 bytes```. 

<br />
<br />

What is a segment <br />
A segment is a ``` 64 Kib ``` (``` 64 000 bytes```) and segments overlap every ``` 16 bytes ```

![alt_text](https://i.imgur.com/fPqe6oj.png)

<br />

Since they over lap every 16 bits we can find the ``` real address ``` like so ``` real_address = segment * 16 + offset ```

This means that there are multiple ways to get the same ``` real address ```
```

segment:offset        real address
 0x0000:0x7c00        0x7c00
 0x0001:0x7bf0        0x7c00
 0x0010:0x7b00        0x7c00
 0x00c0:0x7000        0x7c00
 0x07c0;0x0000        0x7c00
 
```

<br />

There are special registers used to specify currently active segments ( see in [03_Registers](https://github.com/JacobJohnson089/Learning-Assembly/blob/main/Learning_Assembly/notes/03_Registers.md) )
```
CS - currently running code segment
DS - data segment
SS - stack segment
ES,FS,GS - extra segment

```
\*The ``RIP`` only gives us the offset ( see [here](https://github.com/JacobJohnson089/Learning-Assembly/blob/main/Learning_Assembly/notes/03_Registers.md#status-registers) )


<br />
<br />

Well How do you  Refer a memory location ? <br />

You do this : ``` segment:[base + index * scale + displacement] ``` aka ```segment:offset```
```
segment : CS, DS, ES, FS, GS, SS (DS id unspecified)

base : ( 16 bits) BP/BX

index : (16 bits) SI/DI
        (32/64 bits) any general purpose register

scale : (32/64 bits only) 1, 2, 4, or 8 

displacement : a (signed) constant value
```

All the opperands in a memory reference expression are optional, so you only have to use what you need

by default the code segment register ```CS``` has been set up by the bios and points to segment 0

<br />
<br />

# This may seem complicated so here is an exemple 👾

<br />
<br />

```
; We want to read the 3th element in an array

array: dw 100, 200, 300

mov bx, array      ; we put the offset of the array into bx
mov si, 2 * 2      ; 2 * 2 ( array[2], words are 2 bytes wide ) 

mov ax, [bx + si] ; copy memory contents

```

  










