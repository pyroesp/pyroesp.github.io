---
layout: post
title:  "Game Boy Oscilloscope, part 2"
date:   2019-04-24 05:00:00 +0200
categories: [electronics, PCB design, KiCad]
---

*Recap:* 
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
<br/>

### Elektor saves the day  

I've been in contact with Elektor, trying to get the binary file for the EPROM.  
After a couple of weeks of "I've forwarded your question to the tech. department", they told me they had *one* EPROM left for the GBDSO.  
They were willing to sell it to me for 10€, so of course I bought it!  

I tried to read the EPROM with an Arduino, but something was wrong as the bin file I read didn't work in an emulator.  

***************************  
<br/> 

### PCB v1 arrives  

Oh, how I loathe customs. 1,5 week after the PCB was supposed to arrive I get a letter in the mail from our wonderfule Belgian customs telling me I need to provide proof of purchase for the package.  
I ordered a few extra things from JLCPCB this time, because I thought the PCB was done:
- Color red
- Gold plated
- ...  

Total price comes around to 30$, which is more than the allowed 22EUR before import tax/customs checks.  

Anyways, once I proved the GBDSO PCB wasn't a bomb, I got the board in the mail another 1,5 week later. Got to love our postal services.  

***************************  
<br/> 

### Assembling the GBDSO  

Soldering the parts on the GBDSO was going fine. I started with discrete stuff: resistors, capacitors, ...  

![Assembly 1]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (1).jpg" }})  

Then I started adding the diodes, opamps and other stuff.  
Unfortunately I didn't check the footprint of some of the ICs I'm using.  

The digital potentiometer DS1267 is in a wide SOIC16 package. I have a normal SOIC16 footprint.  
To try and fix this without having to make a new board, I bent the pins inwards:

![Assembly 2]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (2).jpg" }})  

![Assembly 3]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (3).jpg" }}) 

It looks like it's going to fit. I added some flux to help and here's the result :

![Assembly 4]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (4).jpg" }})   

![Assembly 5]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (5).jpg" }})  

As you can see, the pins are bent inwards and soldered to the normal SOIC16 footrpint.  

****************************
<br/>

### Missing IC

Everything was soldered onto the board, except for the 74HC138 and the MAX114CAG.  
Where did I put those? I can't find them anywhere.  
I checked my digikey BOM list online and they weren't in there!  
I had to order them from another supplier as digikey charges 18€ for shipping less than 50€.  
That seemed like a lot for two ICs.  

Once I got them I soldered the 74HC138 without issue, but then I messed up again.  
The ADC is in a wide TSSOP24 package and I used a normal TSSOP24 footprint.

![Assembly 6]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (6).jpg" }})  

![Assembly 7]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (7).jpg" }})  

![Assembly 8]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (8).jpg" }})  

![Assembly 9]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (9).jpeg" }})  

![Assembly 10]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (10).jpeg" }})  