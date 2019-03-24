---
layout: post
title:  "Game Boy Oscilloscope"
date:   2018-09-11 05:00:00 +0200
categories: [electronics, PCB design, KiCad]
---

In the November magazine of Elektor 2000, there's an article from Steve Willis who made a cartridge for the Game Boy named GBDSO.  
This cartridge converted the Game Boy into a 2 channel oscilloscope.  

These are it's key features :
- Dual trace display
- Sampling Rate: DC to 1 Msps
- Time Base: 100 s to 5 µs/Div
- Inputs: AC/DC 1 MegOhm
- Input gain: 50 mV to 10 V/Div
- Line or chart recorder trace modes
- Real-time FFT mode with dB scale
- Variable persistence XY mode
- PC link for screen or data transfer
- 5 hrs operation from NiMH batteries
- Averaging and Auto trigger functions
- Reference trace storage

As you can see from the features, this thing is pretty damn cool.  
Who knew the Game Boy could do even half of it, let alone real-time FFT?  

The GBDSO was a kit sold by Elektor, but because this project is old, they don't seem to be selling it anymore.  
I'm interested in having one of these, so I thought: Why don't I make one?  

***************************  

### Research

All the info I needed could be found in the article. The schematic and PCB layout were there, as well as a BOM list.  
Here you can find the article online : <a href="http://www.radanpro.com/Radan2400/TestShematics/GameBoyScope.pdf">http://www.radanpro.com/Radan2400/TestShematics/GameBoyScope.pdf</a>  

***************************  

### Schematic Design

I first started by creating a BOM list on digikey. What I couldn't find I substitued with something similar.  
You can find the BOM list at the bottom of the page.  

I'm using KiCad as my EDA software. It's easy to use, open source and it's free.  
There's nothing much to say about the design. The schematic is essentially the same as the one in the Elektor magazine.

![Schematic]({{ "https://github.com/pyroesp/GBDSO/blob/master/pictures/schematic-1.png" }})  

**************************  

### PCB Design

The first PCB design I did was pretty simple. The size of the board matched a Game Boy cartridge.  
I did however forget that there's a notch in the top right corner of the board.  
![PCB v0]({{ "https://github.com/pyroesp/GBDSO/blob/3203120118c3032adb6ad858880712b54764b28c/pcb-front.png" }})  

When the PCB arrived, I tried putting it in the shell of a game just to realise it wouldn't fit.  
The other thing I realised is that this board was thicker than the PCB of the game I have.  
This could potentially damage the connector of the Game Boy, so I opted to redo the PCB.  

![PCB v0]({{ "https://github.com/pyroesp/GBDSO/blob/master/pictures/pcb-v0.png" }})  

Before I started the new version, I made a technical drawing of the dimensions of the PCB of an actual game.  

![Dimensions]({{ "https://github.com/pyroesp/GBDSO/blob/master/freecad/pcb%20dimensions/cartridge%20dimensions.svg" }})

After that I changed the board size in KiCad and started moving components around.  
Then I got the idea of programming the EPROM directly onto the PCB.  
This prompted me to make a small extension to the PCB which would convert the cartridge to a DIP 28 pin version of the EPROM (AT27C256R).  

![PCB v1]({{ "https://github.com/pyroesp/GBDSO/blob/master/pictures/pcb.png" }})  

The second board has yet to arrive, but when it does, I'll update this post with pictures and tests.

***************************  

### Programming

The AT27C256R EPROM on the board needs to be programmed with the GBDSO software.  
I wanted to make it as easy as possible, so I added two jumpers on the board and a small extension.  

Because the EPROM is in a PLCC32 package, you need an adapter board to program it as all universal programmers use some sort of wide DIP ZIF socket.

All the EPROM pins are accessible through the edge connector of the PCB, so the extra board converts the cartridge to a simple 28 pin AT27C256R EPROM.  

![Top - out]({{ "https://github.com/pyroesp/GBDSO/blob/master/pictures/top%20-%20out.png" }})  
![Top - in]({{ "https://github.com/pyroesp/GBDSO/blob/master/pictures/top%20-%20in.png" }})  
![Side - pinheader]({{ "https://github.com/pyroesp/GBDSO/blob/master/pictures/side%20-%20pinheader.png" }}) 

To get the AT27C256R into program mode, VCC needs to rise to 6.5V and VPP to 13V.  
Jumper 1 switches between 5V or 13V going to VPP of the EPROM. While programming, the jumper needs to connect the middle and bottom pad.  
![JP1]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/programming-JP1.png" }})  

Jumper 2 isolates the whole board, except the EPROM, from the 6.5V.  
![JP2]({{ "https://github.com/pyroesp/GBDSO/blob/master/pictures/programming-JP2.png" }})   

After you're done programming, close JP2 and change JP1 so that the middle and top pad are connected.

***************************

### Original GBDSO

![original - front]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/original/GBDSO_F.jpg" }})  

![original - shell]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/original/GBDSO_Shell_Side.jpg" }})  

![original - warning]({{ "https://github.com/pyroesp/GBDSO/blob/master/pictures/original/GBDSO_Warning.jpg" }})  

***************************

### BOM list

Here's the list with links:  
|                       Label                        |               Value / Name               | Price per unit | Number | Total Price |                                                     Link                                                     |
|----------------------------------------------------|------------------------------------------|----------------|--------|-------------|--------------------------------------------------------------------------------------------------------------|
| R1, R2, R4, R8, R9, R11                                 | 1M                                       |           0.09 |      6 |        0.54 | https://www.digikey.be/product-detail/en/bourns-inc/CR0805-FX-1004ELF/CR0805-FX-1004ELFCT-ND/3740924         |
| R15                                                | 15k                                      |           0.09 |      1 |        0.09 | https://www.digikey.be/product-detail/en/bourns-inc/CR0805-FX-1502ELF/CR0805-FX-1502ELFCT-ND/3740926         |
| R5, R12                                             | 470k                                     |           0.09 |      2 |        0.18 | https://www.digikey.be/product-detail/en/RC0805FR-07470KL/311-470KCRCT-ND/730924/?itemSeq=284203171          |
| R3, R6, R7, R10, R13, R14                               | 4k7                                      |           0.09 |      6 |        0.54 | https://www.digikey.be/product-detail/en/CR0805-FX-4701ELF/CR0805-FX-4701ELFCT-ND/3925402/?itemSeq=284203192 |
| R16, R17                                            | 100k                                     |           0.09 |      2 |        0.18 | https://www.digikey.be/product-detail/en/yageo/RC0805FR-07100KL/311-100KCRCT-ND/730491                       |
| P1, P2                                              | 100k                                     |           0.24 |      2 |        0.48 | https://www.digikey.be/product-detail/en/bourns-inc/TC33X-2-104E/TC33X-104ECT-ND/612912                      |
| C1, C10                                             | 18p                                      |           0.12 |      2 |        0.24 | https://www.digikey.be/product-detail/en/yageo/CC0805JRNPO9BN180/311-1102-1-ND/303012                        |
| C2, C4, C5, C8, C9, C11, C12, C14, C17-C21, C23, C27, C29, C30 | 100n                                     |           0.09 |     17 |        1.53 | https://www.digikey.be/product-detail/en/CL21B104KBCNNNC/1276-1003-1-ND/3889089/?itemSeq=284204783           |
| C3, C13                                             | 1p8                                      |           0.09 |      2 |        0.18 | https://www.digikey.be/product-detail/en/samsung-electro-mechanics/CL21C1R8CBANNNC/1276-2599-1-ND/3890685    |
| C6, C15                                             | 15p                                      |            0.1 |      2 |         0.2 | https://www.digikey.be/product-detail/en/wurth-electronics-inc/885012007052/732-7847-1-ND/5454474            |
| C7, C16                                             | 220p                                     |           0.09 |      2 |        0.18 | https://www.digikey.be/product-detail/en/CL21B221KBANNNC/1276-2474-1-ND/3890560/?itemSeq=284205074           |
| C22, C24, C25, C26, C28, C31, C33                        | 10µ 16V                                  |           0.31 |      7 |        2.17 | https://www.digikey.be/product-detail/en/vishay-sprague/293D106X0016A2TE3/718-1956-1-ND/3985805              |
| L1, L2, L3                                           | 100µ                                     |           0.18 |      3 |        0.54 | https://www.digikey.be/product-detail/en/taiyo-yuden/LBM2016T101J/587-1817-1-ND/1465287                      |
| D1, D2                                              | BAV199                                   |           0.17 |      2 |        0.34 | https://www.digikey.be/product-detail/en/diodes-incorporated/BAV199-7-F/BAV199-FDICT-ND/1033647              |
| D3                                                 | ZR25D01 -> ZR40402F25TA                  |           0.61 |      1 |        0.61 | https://www.digikey.be/product-detail/en/diodes-incorporated/ZR40402F25TA/ZR40402F25TACT-ND/243209           |
| IC1, IC3                                            | MC33182D -> TL062IDT                     |           0.43 |      2 |        0.86 | https://www.digikey.be/product-detail/en/stmicroelectronics/TL062IDT/497-6763-1-ND/1865381                   |
| IC2                                                | DS1267S100 -> DS1267BS-100+              |           5.52 |      1 |        5.52 | https://www.digikey.be/product-detail/en/maxim-integrated/DS1267BS-100/DS1267BS-100-ND/5049982               |
| IC4                                                | MAX114CAG                                |            6.9 |      1 |         6.9 | https://www.digikey.be/product-detail/en/maxim-integrated/MAX114CAG/MAX114CAG-ND/1428095                     |
| IC5                                                | 74HC175D                                 |           0.41 |      1 |        0.41 | https://www.digikey.be/product-detail/en/texas-instruments/SN74HC175DR/296-14840-1-ND/562676                 |
| IC6                                                | 74HC138D                                 |           0.34 |      1 |        0.34 | https://www.digikey.be/product-detail/en/texas-instruments/SN74HC138D/296-1193-5-ND/277220                   |
| IC7                                                | AT27C256R-12JC -> AT27C256R-70JU         |            1.2 |      1 |         1.2 | https://www.digikey.be/product-detail/en/microchip-technology/AT27C256R-70JU/AT27C256R-70JU-ND/1008583       |
| IC8                                                | TLC27L2CD                                |           0.86 |      1 |        0.86 | https://www.digikey.be/product-detail/en/texas-instruments/TLC27L2CDR/296-1316-1-ND/404950                   |
| IC9                                                | MAX828EUK                                |            4.5 |      1 |         4.5 | https://www.digikey.be/product-detail/en/MAX828EUK%2bT/MAX828EUK%2bTCT-ND/2699369/?itemSeq=284207452         |
| S1, S2                                              | Slide switch 1 change-over contact |           0.49 |      2 |        0.98 | https://www.digikey.be/product-detail/en/c-k/JS102011SAQN/401-1999-1-ND/1640114                              |
| K1, K2                                              | 3.5mm stereo socket -> mono smd          |           0.88 |      2 |        1.76 | https://www.digikey.be/product-detail/en/cui-inc/MJ-3523-SMT-TR/CP-3523MJCT-ND/669691                        |


Total Price: 31.33€  
Total Components: 70  

***************************  