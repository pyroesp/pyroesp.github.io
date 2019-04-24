---
layout: post
title:  "Game Boy Oscilloscope, part 2"
date:   2019-04-24 05:00:00 +0200
categories: [electronics, PCB design, KiCad]
---

Recap: 
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
They were willing to sell it to me for 10EUR, so of course I bought it!  

I tried to read the EPROM with an Arduino, but something did't work as the file I read didn't work in an emulator.  

***************************  
<br/> 

### PCB v1 arrives  

Oh, how I loathe customs. 1,5 week after the PCB was supposed to arrive I get a letter in the mail from Belgian customs telling me I need to provide proof of purchase for package X.  
I ordered a few extra things from JLCPCB this time, because I thought the PCB was done:
- Color red
- Gold plated
- ...  

Total price comes around to 30$, which is more than the allowed 22EUR before import tax.  

Anyways, once I proved the GBDSO PCB wasn't a bomb, I got the board in the mail another 1,5 week later. Got to love our postal services.  


TODO
...
