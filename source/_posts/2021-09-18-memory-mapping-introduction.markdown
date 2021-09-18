---
layout: post
title: "Memory Mapping Introduction"
date: 2021-09-18 16:40:05 +0530
comments: true
categories: ASM  
---

### **Memory mapping**

Memory of computer has addresses for its smooth working. These addresses are same for everyone and does not change from use to use. The first byte is at address 0, the second byte is at address 1, and so on until the last byte of the computer’s memory. Basically if I want to explain memory mapping in short there are memory registers that map memory. First a “logical” address is given to these processes which are currently running on the computer by CPU. This “logical” address is completely temporary and is not same for all users, its completely virtual. This process when becomes a permanent process, is assigned a “physical” address on the memory itself.


How exciting huh ? Well for me these things are very exciting 😁

**These mapping registers can only map data of 2 different size – 4096 bytes and 2 megabytes. Linux uses 2MB for the kernel and 4Kb for most other uses.** Recent CPUs can support up to 1GB !!

To understand these concepts more clearly, you all can read this [book](http://library.bagrintsev.me/ASM/Introduction%20to%2064bit%20Intel%20Assembly%20Language%20Programming%20for%20Linux.2011.pdf) and watch this Youtube [video](https://www.youtube.com/watch?v=p3q5zWCw8J4)

I have ended memory mapping in very short and has just given you a little overview because I can’t explain everything here but you all can look the links that I have given you all.

So now comes the more exciting part. Ever heard of stack, heap etc. Well in this article you will be explained everything. That’s why I just love assembly, It completely builds all the concepts clear related to working of computers.

### **Process Memory Model** 

For the smooth running of a process linux memory is divided into 4 logical segments- text, data, heap and stack. A process is mapped from lowest address i.e. **text** to the highest address i.e. **stack.**

Now before moving ahead I would like to discuss about 2 types of memory – **logical and physical. The segmentation is done in the logical part** of the memory because it can vary but the **pages(blocks) are the part of physical part** of the memory as a block size remains constant.

<img align="left" src="/images/process-memory-dia.png" HSPACE=”50” VSPACE=”50”>

1. **Text** – In assembly text segment is indicated by `.text` , It contains the machine instructions of a program. It basically tells the story of what program does.

2. **Data** - Data segment contains all the static data. It is represented by `.data` .Which means it contains all variables that have been initialized in the program. 

3. **.bss** – Above data segment, there is `.bss` segment which stands for “block started by symbol”. Ths segment contains data which is statistically allocated in a process, but is not stored in the executable file. Instead this data is allocated when the process is loaded into memory.

4. **Heap** - Heap is basically some space reserved by the program so that it can use it for future purposes when calling functions like `malloc.`

The size of segments vary according to usage but the ultimate size of the page will remain same. for example Sometimes, you will see that processor is processing a lot of information in that case we can assume that `.text` part is large but other parts is small.Sometimes processing done is less but still `.data` is large but others are small. Sometimes a process needs more temporary storage for later use so stack will be more in size.

#### **Hello world program in assembly.**

I do not want you to understand the whole code below. Just observe how segmentation has been performed in the code and keep this code in the back of your mind so that you can understand and write codes yourself in the upcoming articles.

```as
section .data
    msg db "Hello world!",10      ; 10 is the ASCII code for a new line (LF)

section .text
    global _start
    _start:
  
        mov rax, 1
        mov rdi, 1
        mov rsi, msg
        mov rdx, 13
        syscall ; write syscall 

        mov rax, 60
        mov rdi, 0
        syscall ; exit syscall 
```

To run this: 

```sh 
nasm -felf64 helloworld.asm
ld helloworld.o -o helloworld
./helloworld
```
I have not added a lot of things in this article. So please read and view the following sources.


Sources:

Book link given in the previous article.

http://www.cs.uwm.edu/classes/cs315/Bacon/Lecture/HTML/ch10s04.html </br>
https://jameshfisher.com/2018/03/10/linux-assembly-hello-world/ </br>
https://web.stanford.edu/class/archive/cs/cs107/cs107.1212/schedule.html </br>
https://youtu.be/HWwNTWY1rxo


