---
layout: post
title:  "Fixing a broken Sega Mega-CD 2, part 2"
date:   2018-08-26 05:00:00 +0200
categories: [electronics, repair, Sega]
---
### Final repairs:

I bought two new batteries for the RAM, I've bought a glass fuse holder with cap and 1.25A glass fuses.
I also bought a 9VDC 1.33A power supply for the Mega-CD 2, but couldn't use it, see below why.

I installed the battery without issues. No pictures taken for this, sorry.

I've also installed the fuse holder. I decided to put it in front of the CD driver, as there's an opening in the shielding right by it.<br/>
I didn't want to cut into the metal shielding case, nor remove it, so I could place it some other place.<br/>
The wires I used are pretty thick and hard to bend, probably overkill, but better safe than sorry.

![Fuse Holder]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2-part-2/fuse-holder.jpeg" }})

That's the only place I've used hot glue.<br/>
I've seen some horrible mods with hot glue everywhere.

I've taped the spare battery and two spare 1.25A fuses inside the Mega-CD 2.<br/>

![Spare Battery]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2-part-2/spare-battery.jpeg" }})

![Spare Fuses]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2-part-2/spare-fuses.jpeg" }})

***********************
<br/>
### Mods

Except for the fuse holder mod, there's one more thing I did.<br/>
Some of you might have seen an inductor next to the battery. That's the input filter inductor.

I've replaced this inductor with a full bridge rectifier, making it possible to use both center pin positive and center pin negative power supplies.

This mod isn't something I've seen done before and the only downside to this is that it requires a power supply of at least 10VDC, like the original power supply.<br/>
A 9V power supply didn't work as the voltage after the bridge rectifier is only around 8.2V, which is a bit low for the CD drive.<br/>

Before:
![Sega Mega-CD 1 - Voltage Reg Schematic]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2/mega-cd-1_voltage-reg.png" }})

After:
![Full Bridge Rectifier Schematic]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2-part-2/full-bridge-rectifier-schematic.png" }})

![Full Bridge Rectifier]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2-part-2/full-bridge-rectifier-1.jpeg" }})

![Full Bridge Rectifier]({{ "/assets/2018-08-19-fixing-a-broken-sega-mega-cd-2-part-2/full-bridge-rectifier-2.jpeg" }})

This was the only one I had that's reasonably small and can take 1A continuously.<br/>
As you can see I had to put it upside down and bend the leads so that it's height is the same as the caps.

I'm now using a 12VDC 1A power supply. The voltage drop over the 7805 might be a bit too high still, but that's all I had.<br/>
If it heats up too much or breaks, then I'll change the power supply.

***************************
<br/>
### Not 100% fixed

The Mega-CD 2 isn't 100% fixed.<br/>
I burned Sonic CD to a CD-R, just to test the system.

When playing Sonic CD, I wait on the start menu so the intro movie can start playing.<br/>
This intro doesn't load all the time, and when that happens the CD drive makes noises like it's trying to read or find the correct spot on the CD but then just stops spinning the disk.

I can get it to load by opening and closing the port of the CD drive. That resets/homes the laser head and then the CD drive starts spinning and reading the disk again.

I've tried another game and this one doesn't want to load the levels unless I open and close the CD port.

Anyone knows why this happens and/or knows how to fix this ?

******************************
