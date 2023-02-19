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

# What is Little-Endian

<br />
<br />



https://user-images.githubusercontent.com/112841868/219923499-e82ed0ba-5e2a-4502-800e-8b2b92c2df27.mp4



<br />
<br />

# Congradulations, you now understand the basics of how data is handled in assembly ! 🥳













