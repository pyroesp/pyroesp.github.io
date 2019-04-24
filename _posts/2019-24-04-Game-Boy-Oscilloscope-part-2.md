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

Oh, how I loathe customs. 1,5 week after the PCB was supposed to arrive I get a letter in the mail from our wonderful Belgian customs telling me I need to provide proof of purchase for the package.  
I ordered a few extra things from JLCPCB this time, because I thought the PCB was done:
- Color red
- Gold plated
- ...  

Total price comes around to 30$. If you buy something that's 22€ or more then customs can check your packages and you'll be charged an import tax...

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

It looks like it's going to fit. I added some flux to help and then reflow soldered it with my hot air station.  
Here's the result :

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

Again, I had to bend the pins inwards to make it fit the footprint. Soldering this with the soldering iron was impossible, so I had to reflow solder it again.  

********************************
<br/>

### Assembly done

The board is done. I found a 3$ delivered Game Boy game and used it's shell for the GBDSO.  
I was a bit rough cutting the shell, I should have used the dremel for this.

![Assembly 8]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (8).jpeg" }})  

That small hole is too low. I checked my technical drawing of the cart and I messed up the distance from top to center of the hole...  
I broke off the pin from the shell that should go in that hole and fixed the PCB in KiCad and the tech. drawing, but now the PCB can move a bit so I hope it's still going to make good contact in the Game Boy.  

![Assembly 9]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (9).jpeg" }})  

Unfortunately once I plugged it in, it didn't work.  

*********************************
<br/>

### Troubleshooting, part 1

When powering the GBDSO up into a Game Boy, I had this Nintendo logo no matter which Game Boy I used:  

![Assembly 10]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/troubleshooting-1.png" }})  

After a bit of back and forth with the guys on the gbdev discord and looking at the difference between a normal logo and the one I have, it came apparent that D7 is held low for some reason.  

As you can see from the image below, those red pixels are missing if D7 is grounded.  

![Assembly 10]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/troubleshooting-2.png" }})  

I'm thinking I must have damaged the EPROM when reflow soldering it, as it was difficult to solder and wouldn't get into place.  
But after some probing I found I had D7 shorted to GND on the ADC.  
Due to the pin pitch being so small on a TSSOP I didn't see the shorts, but managed to fix them with desoldering braid.  

I only hope that I didn't break anything on the PCB because of the shorts I had...  

I also found out that the digital potentiometer symbol in KiCad, had it's pin 12 and 13 swapped.  
Unfortunately my board was already done, so I have to somehow swap those two pins.
I submitted an issue and fixed the symbol in a pull request to the official KiCad symbols github repo.

The fix isn't pretty, but it should work:

![Assembly 10]({{ "/assets/2019-04-24-Game-Boy-Oscilloscope-part-2/assembly (10).jpg" }})  

**********************************
<br/>

### First boot

After all this troubleshooting was done I checked short between power and GND, and then turned on the Game Boy:

<p align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/ue8mhNeEI1Q" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></p>  

As cool as this is, I wasn't able to get any waveform on either channels so I'll have to troubleshoot some more.  

**********************************
<br/>

### Troubleshooting, part 2

TODO