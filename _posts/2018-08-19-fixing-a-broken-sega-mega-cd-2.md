---
layout: post
title:  "Fixing a broken Sega Mega-CD 2"
date:   2018-08-19 10:00:00 +0200
categories: [electronics, repair, Sega]
---
In December of 2016 I bought myself a broken Sega Mega-CD 2 for $50 shipped, with the intention of fixing it and giving it to my brother as a gift.<br/>
The seller said he couldn't get it to boot.

Now, when I got it, I didn't have the correct power supply and ended up blowing up the fuse on the motherboard.

Who knew Sega would use a center pin negative barrel jack power supply when the Sega Mega Drive 2 (MD-2) uses a center pin positive power supply!<br/>
Who at Sega thought this was a good idea !?


Anyways, I was left with an even more broken Sega Mega-CD 2.<br/>
I looked a bit for shorts and faulty components on the board, but couldn't find anything, and so the Mega-CD sat in a corner for 1,5 years.


For some reason, this past Friday, I really wanted to try to fix it again.<br/>
And so I got home and opened up the Mega-CD and started working on it.


Full service manuals for different Sega consoles can be found here : <br/>
[http://www.sega-16.com/forum/showthread.php?23228-Sega-Service-manuals-16bit](http://www.sega-16.com/forum/showthread.php?23228-Sega-Service-manuals-16bit)


Below are the steps I followed to get it up and running again.

****************
<br/>
### Rule 1: Thou shalt measure voltage:

Unless like an idiot (=me) you blow up the fuse...

Looking at the schematic of the Mega-CD 2, the 10VDC external power supply gets converted to 5VDC with a NEC2405 linear voltage regulator.

![Sega Mega-CD 2 - Voltage Reg Schematic]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2/mega-cd-2_voltage-reg.png" }})

The schematic isn't that great, but the voltage regulation used in the Mega-CD 2 is identical to the one used in the Mega-CD 1, so I'll be using that.

![Sega Mega-CD 1 - Voltage Reg Schematic]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2/mega-cd-1_voltage-reg.png" }})

I measured the resistors and transistors to see if they were still ok, and they looked fine.<br/>
Because of that, I wanted to send power to it with my lab power supply and it's overcurrent protection (OCP).<br/>
I'm sure I can bridge the fuse with a wire. What can possibly go wrong? <br/>
<b>Note:</b> Don't do this, I'm just too impatient to wait for a new fuse.

So I did just that. I used the thinnest copper wire I had to bridge the fuse.<br/>
I set my lab power supply to 10VDC with a small OCP.<br/>
I'm not really expecting this to blow up if there's a high current going through it.

![Sega Mega-CD 1 - Bridge]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2/mega-cd-2_bridge.jpeg" }})

Before applying power I disconnected the CD player from the main motherboard. I didn't want it to interfere with the measurements.

The voltage was stable and the current drawn was ok, but no voltage on the NEC2405.<br/>
Looking at the schematic, the pass transistor TR3 (Q301), only lets current through when TR4 (Q303) is on.

TR4 (Q303) gets a 9VDC signal from the MD-2, to start the Mega-CD 2.<br/>
At this point I didn't want to connect the MD-2 to the Mega-CD so I soldered a wire to the correct pin of the connector and applied 9V to it.

![Sega Mega-CD 1 - Power]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2/mega-cd-2_power_and_MD_signal.jpeg" }})

The lab power supply went into OCP and I quickly turned it off.

At this point I desoldered and tested the pass transistor with one of those cheap component testers from eBay, and it was ok.<br/>
Next I desoldered the NEC2405 and tested it out of circuit, 6.5VDC for a 5V regulator... Not great.<br/>
I applied power to the Mega-CD 2 again, with the MD-2 turn on signal and the power supply stayed at 10V.<br/>

Fortunately enough I have a few 7805, which are pin compatible with the NEC2405, and soldered it in place.<br/>
I powered it back up again and now I have 5VDC!

I measured the voltage on most of the ICs, just to be sure everything got 5V, and this was ok.

***************************
<br/>
### Fixing the CPU:

Most if not every IC on the Mega-CD 2 had previously been reflowed. The board looked like shit with old flux everywhere.

![Sega Mega-CD 2 - Motherboard]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2/mega-cd-2_motherboard.jpeg" }})

I cleaned it as much as I could with Kontact IPA Isopropanol, but some of it just didn't want to come off.

![Sega Mega-CD 1 - Motherboard 2]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2/mega-cd-2_motherboard-2.jpeg" }})

At this point I connected the MD-2 to it, but still nothing.

I measured the 2 crystal oscillators I saw on the board with a scope.<br/>
There's a 50MHz oscillator and a 16.9344MHz one, and they both measured fine.

I measured a few of the signals of the boot ROM IC, IC111, but it didn't look like it was doing much.

I'm not familiar with the 68000 CPU, but it looked like the reset and halt pins stayed low.<br/>
Unfortunately this was coming from the big Sega ASIC, IC101, the game processor. If there was an issue with that IC then I could scrap the board.

Looking at the CPU more closely, it is not alligned properly to the pads. I checked each pin and added I tiny bit of solder to them to make sure there was good contact.

I plugged the MD-2 back in and measured the clock of the 68000 CPU. It measures fine at 12.5MHz, which I didn't have previously.<br/>
I soldered a wire to the reset pin of the CPU and it changed when I powered one the MD-2.

Yet I still didn't have an image on the TV.

***************************
<br/>
### Cleaning the contacts:

I'm plugging the MD-2 on and off the Mega-CD 2 and measuring the boot ROM address and data lines.

At first I didn't get anything so I continued probing the ROM, removing the MD-2, probing the CPU again, connecting the MD-2,...<br/>
After a little bit it seemed to be doing something as the addresses and data lines changed.

And then it happened, I saw a white flash on the TV next to me, just when I powered the MD-2 off.<br/>
I had a black screen until now, so the white flash was a surprise.

I turned it back on, and there it was, in all it's glory:

<p align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/mwROd91qzWs" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></p>

What had happened is that by plugging the MD-2 on and off the Mega-CD 2, the connector on both consoles broke through a layer of dust and oxidation.<br/>
This was something I didn't think about as the MD-2 was in working condition and still had the protective cap over the PCB contacts. The Mega-CD 2 connector though, it's always open so dust can easily get in there.

It didn't cross my mind to check the connectors to be honest.

This thought me an important lesson and I'm definitely going to be looking at this in the future if I ever attempt a repair again.

I cleaned everything with Kontakt 60 contact cleaner and q-tips, and it worked fine after that.<br/>

******************************
<br/>
### Testing the CD player:

Until now I have tested the Mega-CD 2, without disc.<br/>
I don't have a game, so for those of you who don't know, the Mega-CD 2 has a built-in music disk player.<br/>

After putting everything back in the Mega-CD 2 case I tried playing a music disk.<br/>
It worked!

After getting the console to boot, it would have been all a waste of time if the CD player was broken. So I'm happy it works fine.

The only issue I have is that my power supply can't provide enough current to drive the CD player continuesly and because of this, the Mega-CD 2 locks up from time to time.

*****************************
<br/>
### TODO:

The Mega-CD 2 has a 3V rechargeable battery for the RAM.<br/>
Unfortunately, this one is completely dead and can't hold a charge anymore.

I'll have to replace that with a new one.


The fuse still needs to be replaced. I'm probably going to buy a fuse holder and put in a glass fuse instead of an SMD one.<br/>
That's going to be easier to replace if it ever blows up again.

I still need to find a 10VDC 1.2A positive barrel negative center barrel jack power supply as I'm still using my lab power supply.<br/>
I have a 12V 1A negative barrel positive center PS I could probably use if I rewire the barrel jack, but the increase in voltage means an increase in dropout voltage over the voltage regulator, which in turn increases the power dissipated by the 7805. That's something I don't want.

I also have to clean and apply new thermal paste to the 7805 and heat sink. That will help with it not overheating.

******************************
<br/>
### Mods:

The only mod I'll be doing will be the fuse mod.<br/>
As this is a gift for my brother, he told me he wanted to keep it all original, so no region free, rgb, ... mods.

******************************
