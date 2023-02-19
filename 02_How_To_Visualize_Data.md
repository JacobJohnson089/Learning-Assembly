# Exploring the raw data (machine code) of a asm program 👨‍💻
<br />
<br />

### Recap of this Chapter (Typo, the last address is 1f0 which is equal to 496 in decimal)

https://user-images.githubusercontent.com/112841868/219788378-ba974bac-95b9-4b03-9189-60806837a5a2.mp4

<br />
<br />

### Our code exemple for this section

<br />
<br />

```
; Assembly code

org 0x7c00
bits 16

label:
        db "AAAAAAAAAAA"



times 510-($-$$) db 0
dw 0xaa55

```

```
; Hex data of the .img simplified

00000000  41 41 41 41 41 41 41 41  41 41 41 00 00 00 00 00  
000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa  
00000200  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  

```

<br />
<br />

# How to read the Hex data


<br />
<br />

🔵 Address of "line" : 🔴 "line" of memory

![alt text](https://i.imgur.com/JSJNbQG.png)

<br />

### Reading the Address

<br />

The Last bit of the address ```0000000[0]``` is the "index", Think of the line in memory as an array that is 16 bytes <br />
Long. This means that the last byte of that line is ```0000000F``` , ```F``` is equal to ```15``` in decimal. If you don't understand <br />
Why 15 is equal to the 16th byte you need to know that for an array of lenght 5 these are all the indexes <br /> ```[ 0 , 1 , 2 , 3 , 4 ]```
This is why the index 15 is the 16th byte

<br />

Notice that in our case even if the address shown is ```00000000``` it is showing the content of ```0000000[0-15]```. <br />
This is because you need to think of it as a pointer to the origin of that line. And the line as the next 16 bytes <br />
In memory, This might seem benign but will help you better understand the way memory is handled

<br />

Secondly you also need keep in mine that the address is shown in Hexadecimal meaning that if you where to put all the lines into one <br />
The address would be the location of where the line starts this means that ```00000010``` and ```00000020``` aren't the first and second lines <br />
But are in fact the ``` 00000010 (hex) ```th byte or in decimal the ```16```th byte, and the ```00000020 (hex) ``` would be the ```32```th byte <br />
Obiously you can simply consider everything beside the last digit of the address as the line number, but it never hurts to know whats behind the scene 


<br />

⚠ in our case the line is 16 bytes long because we are viewing the data in Hexadecimal  Which is base 16 




<br />

### Analysing the Hex data

<br />

Well now that you better understand How to read the Address reading the lines should be way easier <br />
But you still need to know something else to be able to read the HEX data, and well it's kinda obvious <br />
You need to know how to convert numbers from Base 10 (decimal) to base 16 (hexadecimal) luckely <br />
For you I will be doing all the conversion for now.

<br />

Let's go line by line

<br /> 

```00000000  41 41 41 41 41 41 41 41  41 41 41 00 00 00 00 00```  <br />
&emsp; &emsp; Here we have ```41 41 41 41 41 41 41 41  41 41 41``` If you translate from Hex to Letters you would get ```AAAAAAAAAAA``` <br />
&emsp; &emsp; The remaining ``` 00 00 00 00 00 ``` is simply the empty space left in the line

<br />

``` 000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa ``` <br />
&emsp; &emsp; Firstly we are at at the address ``` 000001f0 ``` we can also see the signature 0xaa55 you might question why 55 aa <br />
&emsp; &emsp; Is at the address ```000001f0``` and not ```000001fe```  This is because the Address is pointing to the beginning <br />
&emsp; &emsp; Of the line, ``` 55 ``` as the index ```14 (dec)``` or ```e (hex)```, if you add that to the addres you get ```000001fe``` <br />
&emsp; &emsp; It is also important to note that ```000001fe (hex)``` is ```510 (dec)``` and that the next byte ``aa`` is the ```511```th byte <br />
&emsp; &emsp; So our signature is located at the end of the .img file put the last byte is ```0```


<br />

```00000200  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00 ``` <br />
&emsp; &emsp; Althought this line is empty the interesting part here is the address ```00000200``` Which in decimal is ```512```, <br /> 
&emsp; &emsp; And if you remember the BIOS looks for a 512 bytes bootable device with the signature 0xaa55

<br />
<br />

# Having fun with the data 🎉
<br />
<br />

### Weird things happen when we define data in the wrong way... 👻

<br />

### First code exemple : db 123456789

<br />
<br />

```
; Assembly code

org 0x7c00
bits 16

db 123456789


times 510-($-$$) db 0
dw 0xaa55:
```
```
; Hex data of the .img simplified

00000000  15 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa
00000200  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00

```
<br />
<br />

### Second code exemple : db 123456789h

<br />
<br />

```
; Assembly code

org 0x7c00
bits 16

db 123456789h


times 510-($-$$) db 0
dw 0xaa55:
```
```
; Hex data of the .img simplified

00000000  89 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00
000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa
00000200  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00

```
<br />
<br />

### Third code exemple : db 1, 2, 3, 4, 5, 6, 7, 8, 9

<br />
<br />

```
; Assembly code

org 0x7c00
bits 16

db 1,2,3,4,5,6,7,8,9


times 510-($-$$) db 0
dw 0xaa55:
```
```
; Hex data of the .img simplified

00000000  01 02 03 04 06 06 07 08  09 00 00 00 00 00 00 00
000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa
00000200  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00

```
<br />
<br />

### Now you might be confused, about why this is happening so let's go over each exemple and explain  🤯

<br />
<br />

### Starting with db 123456789
<br />

Firstly we need to talk about **little-endian byte order** , little-endian byte order is how x86 stores data <br />
What it does is you start with the least significant byte, then you move to the next most significant byte and so on <br />
For exemple with ```1234 (hex)``` would be stored as `34` `12`. 

<br />

Secondly, we stored the ```123456789``` as  an integer, and we are viewing the data as HEX therefore to understand <br />
Why we see ```15``` in the memory we first need to convert ```123456789 (dec) ``` to ``` 75BCD15 (hex) ```, Well <br />
Now every thing makes sense, ```15``` is the least significant byte, which means that it will be the first <br />
To be define, and since we are using db and trying to define ```75BCD15 (hex)``` as a single byte it can only store <br />
```15``` and discards the rest

<br />
<br />

### db 123456789h

<br />

Now that you understand why  ```db 123456789```  was stored as ```15``` you can easily guess why <br />
```db 123456789h ``` is stored as 89, (if you didn't know h at the end is telling the computer) <br />
That ```123456789``` is a Hexadecimal number. Well if you still don't understand why it's ```89``` <br />
Here's the explaination, ```89``` is the least significant byte, and we can't define more than one <br />
byte so, it only stores ```89```, for more indepth explanation look at the first exemple

<br />
<br />

### db 1,2,3,4,5,6,7,8,9

<br />

Well Finally we have data that makes sense, here we are seperating each bytes that we want to define <br />
With ``` , ```. When you store the value db ```1``` in memory, it only occupies a single byte of memory,<br />
And there is no "next most significant byte" to store in little-endian byte ordering. In this case, <br />
The single byte ```1``` represents both the least significant byte and the most significant byte. <br />
And this applies to every byte we are defining, this is why despite the little-endian system <br />
All of our bytes are stores in the correct order.

<br />
<br />

### But whats happen when we define as string ? db "Hello World"

<br />

Well when we use the ```"text"``` while defining the byte, It already tells the computer that each ```char``` is a single byte <br />
Therefore, it stores the string in the correct order, using the same logic as for the ```db 1,2,3,4,5,6,7,8,9``` exemple <br />
This is also why when we `mov bx, Label` the value stored in  ```bx``` is the pointer to the first defined byte


<br />
<br />

### But there's still more to learn about data 😔

<br />

### And yes we will need one last code exemple

<br />
<br />

### Code exemple  

<br />
<br />

```
; Assembly code

org 0x7c00
bits 16

db 0xaa
string:
        db "Hello World"

db 0xaa
mov bx, string
db 0xaa
mov ax, [bx]


times 510-($-$$) db 0
dw 0xaa55:

```
```
; Hex data of the .img simplified

00000000  aa 48 65 6c 6c 6f 20 57  6f 72 6c 64 aa bb 01 7c  
00000010  aa 8b 07 00 00 00 00 00  00 00 00 00 00 00 00 00  
000001f0  00 00 00 00 00 00 00 00  00 00 00 00 00 00 55 aa  
00000200  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  

```

<br />
<br />

Firstly, you can noticed that we are defining ```aa``` before every action in our code, <br />
The reason why is so that we can see where each element of the code is stored in the data <br />
by using ```aa``` as a "marker". Obiously the ```aa``` from ```0x55aa``` is not a marker <br />

<br />

Now let's convert ```Hello World``` to ```48 65 6c 6c 6f 20 57  6f 72 6c 64 (hex)``` Well <br />
As you probably noticed, this is the exact value stored after our ```first marker``` <br />
Then after our ```second marker``` and ```third marker ``` we have simply have our code <br />
So for now at least, if there's something in the data that you don't understand, <br />
it's either code, or data that was badly stored such as e.g ``` db 123456789 ```, ```db 123456789h```

<br />
<br />

# Congradulations, you now understand the basics of how data is handled in assembly ! 🥳













