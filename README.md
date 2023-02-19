# Setting up an environment to learn assembly x86 💻
<br />
<br />

 ### **HARDWARE NEEDED**

Any CPU that can run x86 which should be almost all of them for computers at least

<br />
<br />

### **THE OS**

Any linux distro with the apt-get packet manager will work for this guide or you can use Windwos Sub System For Linux



<br />
<br />

### **TOOLS NEEDED** 

- Any text editor ( I use NeoVim )
- Make
- Nasm - assambler
- qemu

```
sudo apt install neovim
sudo apt install make
sudo apt install nasm
sudo apt install qemu
```

To learn how to set up NeoVim to turn it into a proper text editor you can watch this [video](https://www.youtube.com/watch?v=JWReY93Vl6g&t=807s&ab_channel=NeuralNine)

[My neovim config](https://github.com/JacobJohnson089/Nvim-config) (you will need to watch the video first

<br />

![alt text](https://i.imgur.com/OOxZN0t.png)


<br />
<br />

# Setting up your files  📁

<br />
<br />

 
### Firstly you will need to create a few files as such


```
Assembly                                             [ This is a directory ] 
├──src                                               [ This is a directory ]                                             
│  └──main.asm                                       [ This is a file      ]
└──build                                             [ This is a directory ]                                                   
└──Makefile                                          [ This is a file      ]
```
the Assembly directory can be called anything but I will refer to is as the Assembly directory <br /> <br />

### The code for the Makefile | not mine made by nanobyte original code [here](https://github.com/nanobyte-dev/nanobyte_os/blob/Part1/Makefile)

```
ASM=nasm

SRC_DIR=src
BUILD_DIR=build

$(BUILD_DIR)/main_floppy.img: $(BUILD_DIR)/main.bin
    cp $(BUILD_DIR)/main.bin $(BUILD_DIR)/main_floppy.img
    truncate -s 1440k $(BUILD_DIR)/main_floppy.img
    
$(BUILD_DIR)/main.bin: $(SRC_DIR)/main.asm
    $(ASM) $(SRC_DIR)/main.asm -f bin -o $(BUILD_DIR)/main.bin
```

<br />
<br />

 
# Compiling and running your assembly code 🏃‍♂️

<br />
<br />

(you need to be in the Assembly directory and you need to re compile your code every time you modify it)
```
make
qemu-system-i386 -fda build/main_floppy.img
```
<br />

When you'll run these commands you will get an error saying "No bootable device" This is normal
When the BIOS starts it looks for the first sector of each bootable device, a bootable device is 512 bytes with the signature OxAA55 located in the last 2 bytes of the boot sector
The BIOS then loads the bootable device at the memory address 0x7c00

<br />
<br />

![alt text](https://i.imgur.com/2tAtw5s.png)

<br />
<br />

To fix this you will need to turn your asm code into a bootable device 

- Set origin in memory to 0x7c00
- Make your .img file 512 bytes long  ( your .img file is the compiled version of your .asm file)
- Make it end with 0xAA55

<br />
<br />

Here is the code if you're Interested I will explain how exactly the code works further down

```

org 0x7c00   ; at the beginning of your code



times 510-($-$$) db 0   ; at the end of your code
dw 0xaa55     

```

<br />

Now you should have when you compile and run your code you should have a window that looks like this (its says fail to boot from hard disk because our program is considerd a floppy)

<br />

![alt text](https://i.imgur.com/JgsTL5t.png)

<br />
<br />

### How did that work ?

<br />
<br />

``` org 0x7c00 ``` This simply offset to origin of where we are in memory to 0x7c00 which is the place where the bios moves the code of bootable devices, so we need to be here to access any data that we are storing without issues 

<br />
<br />

``` times 510 ``` This will repeat an operation n times in this case 510

<br />
<br />

``` $ ```    is equal to the memory offset of the current line 

<br />
<br />

```$$``` gives us the beginning od the current section

<br />
<br />

``` 510 - ($-$$) ``` gives us the number of bytes that we have left before having 510 bytes

<br />
<br />

``` db 0 ``` db means define byte, and db 0 simply means that we are defining the byte to be 0 at the current location in memory

<br />
<br />

``` times 510-($-$$) db 0 ``` This will fill the rest of or img file with 0 until it is 510 bytes

<br />
<br />

``` dw 0x55aa ```  dw means define word, "word" is a 2 byte value, meaning that we have a 512 byte img file that ends with 0x55aa which makes it bootable


# Congradulation you're done ! 🎉














