---
title: Linux上課筆記-Introduction
date: 2018-09-20 19:54:26
tags: 
   - Linux
   - 上課
   - OS
categories:
	- 學校課程
---
就是個甚麼都說一說，~~甚麼都說不清楚~~，大概讓你知道接下來要幹嘛的章節，其實感覺也沒有特別要記的，反正加減note一下。
<!-- more -->
## Architecture
**x86**:32bit，= IA32
**x64**:64bit，=IA32e，=/=IA64
x64的Instruction set延伸自x86，因此能向下相容32-bit aplliction，AMD先生出來的
IA64是intel自己搞得，沒辦法向下相容，~~難怪大失敗~~


### Interl 64 vs. IA-64
Instruction set不一樣


## x86 Registers
列一下各個暫存器，現在也不知道各個在幹嘛，反正之後會把重要的跟做甚麼用寫一下，~~搞不好會刪一堆也不一定~~
+ **General Purpose Registers**
ESP,EBP
+ **Instruction Pointer**
IP:指向接下來要執行的指令的位置
+ **EFLAG**
+ **Segment Registers**
+ **Table Registers(System Address Registers)**
+ **Control Registers**
+ **Debug Registers**
好像是intel特有的

64bit暫存器就是ROO RXX之類的


## x86-64
又稱x64、x86_64、AMD64、IA32e、EM64T，各家都想自己取一個名字，~~你們搞得我好亂R~~，不管叫哪個反正都是x86 Instruction set的64-bit版
AMD先搞出了這個東西後，現在Intel、VIA都跟著做
由於是原本x86的擴充，所以能完全相容16bit和32bit的x86 code，跑起來不會有penalty


## IA32-Real Mode vs. Protected Mode
~~來了，當初OS要是有提到肯定沒聽懂的部分~~
其實，在電腦還沒灌系統時，就有一些code在裡面，那個東西就叫做BIOS。
當IA32 processor啟動或重開時，IP會指到BIOS那部分，此時電腦為16bit且是在`Real mode`中，之後才在real mode中切換至32bit，變為`Protected mode`，OS是運行在Protected mode下。

Registers切換時好像會不一樣吧?IP=>EIP之類的，Instruction Set似乎也是…?介紹講很快很表面，還沒很懂

### Addressing in Real mode
Real mode中，CPU存取的位置實際上是：Segment register x 16 + offset (=physical address)，也就是shift 4 bit後再加上offset
PPT上寫用16-bit offset限制CPU to 64k segment sizes，沒看很懂，以後再研究吧
Real mode下program可以load任何東西到segment register

### Addressing in Protected mode
PPT 36頁有圖

### Interrupts in Real mode
Real-mode Interrupt Vector Table(IVT)~~新朋友~~
內含256個real-mode pointers，提供所有real-mode Interrupt Service Routines(ISRs)~~又是新朋友~~
Real-mode pointers長32bit(?)，由16bit的segment offset(在後面)和16-bit segment address組成(在前面)
0	0x0000 [[offset][segment]]
1	0x0004[[offset][segment]]
2	0x0008[[offset][segment]] etc…


### Interrupts in Protected Mode
Real mode 和protected mode 發生interrupt時，找到interrupt service routine的方式是不一樣的，介紹說不清楚，就先這樣吧。


### How to Switch to Protected Mode
簡而言之，把CR0(斜體)或MSW(斜體)暫存器設成PE-bit就行，整個流程看PPT，不過也沒記的必要。


## IA-32e Mode/Long mode
聽說後面就不會這樣32和64bit的都講
### IA-32e mode
兩種模式，compatibility mode和64-bit mode，前面那個大概就是偶爾會看到的相容模式？
64-bit有64-bit的linear addressing和最多支援64GB RAM
相容模式跑32-bit以下的code

### IA32_EFFR
IA32_Extended Feature Enable Register，這個model指定(記載?)IA32e下的control activation和operation

### Protected mode to IA-32e mode
Enabling paging和setting the LME bit(在IA32_EFFR中)

### Little Endian and Big Endian
Little從low-order byte存起，big相反，以0x123456要從記憶體位置0x0000寫入就會是
	     Big	    Little
0x0000   0x12   0x56
0x0001   0x34   0x34
0x0002   0x56   0x12


## Linux	Source Code Tree overview
~~太多了先跳過~~


~~
