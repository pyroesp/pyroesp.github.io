---
layout: post
title:  "Game Boy Oscilloscope"
date:   2019-03-23 06:00:00 +0200
categories: [electronics, PCB design, KiCad, Game Boy, Oscilloscope]
---

In the October/November magazine of Elektor 2000, there was an article from Steve Willis who made a cartridge for the Game Boy named GBDSO.  
This cartridge made the Game Boy into a 2 channel oscilloscope.  

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
<br/>

### Research

All the info I needed could be found in the article. The schematic and PCB layout were there, as well as a BOM list.  
Here you can find the article online : <a href="http://www.radanpro.com/Radan2400/TestShematics/GameBoyScope.pdf">http://www.radanpro.com/Radan2400/TestShematics/GameBoyScope.pdf</a>  

***************************  
<br/>

### Schematic Design

I first started by creating a BOM list on digikey. What I couldn't find I substitued with something similar.  
You can find the BOM list at the bottom of the page.  

I'm using KiCad as my EDA software. It's easy to use, open source and it's free.  
There's nothing much to say about the design. The schematic is essentially the same as the one in the Elektor magazine.

![Schematic]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/schematic-1.png" }})  

**************************  
<br/>

### PCB Design

The first PCB design I did was pretty simple. The size of the board matched a Game Boy cartridge.  
I did however forget that there's a notch in the top right corner of the board.  
![PCB v0]({{ "https://github.com/pyroesp/GBDSO/raw/3203120118c3032adb6ad858880712b54764b28c/pcb-front.png" }})  

When the PCB arrived, I tried putting it in the shell of a game just to realise it wouldn't fit.  
The other thing I realised is that this board was thicker than the PCB of the game I have.  
This could potentially damage the connector of the Game Boy, so I opted to redo the PCB.  

![PCB v0]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/pcb-v0.png" }})  

Before I started the new version, I made a technical drawing of the dimensions of the PCB of an actual game.  

<img src="https://github.com/pyroesp/GBDSO/raw/master/freecad/pcb dimensions/cartridge dimensions.svg?sanitize=true" style="max-width:100%;">

After that I changed the board size in KiCad and started moving components around.  
Then I got the idea of programming the EPROM directly onto the PCB.  
This prompted me to make a small extension to the PCB which would convert the cartridge to a DIP 28 pin version of the EPROM (AT27C256R).  

![PCB v1]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/pcb.png" }})  

The second board arrived and here are a couple of pictures:  

![PCB v1 front]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/pcb-v1-front.jpg" }})  

![PCB v1 back]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/pcb-v1-back.jpg" }})  

See part 2 for the assembly!  

***************************  
<br/>

### Programming

The AT27C256R EPROM on the board needs to be programmed with the GBDSO software.  
I wanted to make it as easy as possible, so I added two jumpers on the board and a small extension.  

Because the EPROM is in a PLCC32 package, you need an adapter board to program it as all universal programmers use some sort of wide DIP ZIF socket.

All the EPROM pins are accessible through the edge connector of the PCB, so the extra board converts the cartridge to a simple 28 pin AT27C256R EPROM.  

![Top - out]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/top%20-%20out.png" }})  

![Top - in]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/top%20-%20in.png" }})  

![Side - pinheader]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/side%20-%20pinheader.png" }}) 


To get the AT27C256R into program mode, VCC needs to rise to 6.5V and VPP to 13V.  
Jumper 1 switches between 5V or 13V going to VPP of the EPROM. While programming, the jumper needs to connect the middle and bottom pad.  
![JP1]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/programming-JP1.png" }})  

Jumper 2 isolates the whole board, except the EPROM, from the 6.5V.  
![JP2]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/programming-JP2.png" }})   

After you're done programming, bridge JP2 and change JP1 so that the middle and top pad are connected.

***************************  
<br/>

### Original GBDSO

![original - front]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/original/GBDSO_F.jpg" }})  

![original - shell]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/original/GBDSO_Shell_Side.jpg" }})  

![original - warning]({{ "https://github.com/pyroesp/GBDSO/raw/master/pictures/original/GBDSO_Warning.jpg" }})  

***************************  
<br/>

### BOM list

Here's the list with links: [Google Sheet](https://docs.google.com/spreadsheets/d/1vu9Xc1Mw5STfz_mAsAOrmmyRrHJMRS4_JKM7iif-KBE/edit?usp=sharing)  

Total Price: 31.33€  
Total Components: 70  

***************************  
