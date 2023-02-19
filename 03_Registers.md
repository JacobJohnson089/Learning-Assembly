# How are register structured 🧱

<br />

The original un-edited diagram is from Wikipedia and can be found [here](https://en.wikipedia.org/wiki/X86#x86_registers)

![alt_text](https://i.imgur.com/exgYQf7.png)

![alt text](https://i.imgur.com/nu2tkml.png)

![alt text](https://i.imgur.com/ybkNSSj.png)

![alt text](https://i.imgur.com/Mb1HUHw.png)

![alt_text](https://i.imgur.com/Q2rIkPV.png)

![alt text](https://i.imgur.com/oSi2iD2.png)

![alt text](https://i.imgur.com/LKh9yIw.png)

![alt text](https://i.imgur.com/B19oNUq.png)

![alt text](https://i.imgur.com/qdz5Wym.png)

<br />
<br />

# Register type purpose ⛳

<br />

### General purpose registers

General purpose registers can be used for almost any purpose, however we consider the ```RBP RDI RSI RSP ``` as index registers but they can also be used for other purposes


<br />

### Status registers

This type contains the ```RIP``` register (also called program counter) is a special register which keeps track of the memory location at which the current instructions begins 

It also contains a ```RFLAGS``` register (aslo called Flag) which contains special ```flags``` set by various instructions

<br />

### Segment registers

The registors in this type are used to keep track of the currently active memory segments

<br />

### We will introduce the purpose of the other types of registers when needed

<br />
<br />

# Lets look into some data ! 👨‍💻

<br />
<br />

### Firstly lets look at how registers are divided 

<br />

Let's say we run this code ``` mov ah, 0x12 ``` and ``` mov al, 0x34 ``` the content of ``` ax ``` would be ```12 34``` <br />
This is because ``` 00 (hex) ``` is  ``` 0000 0000 (bin) ``` or in other words ``` 00 (hex) ``` is ```8 bits``` <br />
We also know that ``` ax ``` is a ``` 16 bits ``` register made out of ```ah``` the first ``` 8 bits ``` and ```al``` the last ```8 bits``` <br />

<br />
<br />

### some exemples 

<br />

```
; Assembly code

org 0x7c00
bits 16

jmp start

hello: db "HELLO WORLD"

start:
        mov bx, hello
        mov ah, [bx]

times 510-($-$$) db 0
dw 0xaa55:


```
```
; Hex data of the .img simplified

00000000  eb 0b 48 45 4c 4c 4f 20  57 4f 52 4c 44 bb 02 7c
00000010  8a 27 00 00 00 00 00 00  00 00 00 00 00 00 00 00
000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa
00000200  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00

```
```
; Simplified Value stored in our registers

bx : 00 02
ah : 48

```

<br />

### Let's quickly analyze the data

``` eb 0b ``` is our jump instructions and is taking up ```0000``` to ```0001```

``` 48 45 4c 4c 4f 20  57 4f 52 4c 44 ``` is our ``` "HELLO WORLD" ``` and is taking up ```0002``` to ```000C```

``` bb 02 7c  8a 27 ``` is the rest of our code and is taking up ```000D ``` to ``` 0011```


<br />

Notice that we cannot store the ```hello``` label into ```ah```, the reason is that `` ah `` is a ``` 8 bits ``` register <br />
And the value of ```hello``` is ```16 bits```, This is why we store ``` hello ``` in a ``` 16 bits ``` register

Yet we can store ```[bx]``` into a ``` 8 bits ``` register in this case ``` ah ```. This is because when we are using ``` [ ] ``` <br />
we are not reforing to the data stored in ``` bx ``` but where would that data point to if it was a pointer.

<br />
<br />

# Congradulations you know understand the basics of what register are and how to use them 🥳














