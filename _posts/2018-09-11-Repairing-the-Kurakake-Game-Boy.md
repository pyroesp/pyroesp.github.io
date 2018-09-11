---
layout: post
title:  "Repairing the Kurakake Game Boy"
date:   2018-09-11 05:00:00 +0200
categories: [electronics, repair, Nintendo]
---
A couple of weeks ago I bought myself a red Nintendo Game Boy of off eBay.<br/>
This one was listed as "JUNK", so a non-working unit.

I've dubbed it the Kurakake Game Boy, because it has a sticker on the back with the name of the previous owner: N. Kurakake.

![Game Boy - Front]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/front.jpg" }})

![Game Boy - Back]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/back.jpg" }})

***************************
<br/>

### Inspecting the Game Boy

Upon opening the battery compartiment, I saw this :

![Game Boy - battery compartiment]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/battery compartiment.jpeg" }})

That's really bad...<br/>
I tried putting in batteries, but there's just too much corrosion so the Game Boy doesn't turn on.

Looking at the motherboard of the Game Boy I can see corrosion there too.

![Game Boy - Corrosion PCB 1]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion pcb (1).jpg" }})

![Game Boy - Corrosion PCB 2]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion pcb (2).jpg" }})

The audio jack PCB is completely corroded.<br/>
It's so bad that I'm thinking of redoing the PCB entirely.

![Game Boy - Corrosion Audio]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion audio.jpg" }})

*********************
<br/>

### Cleaning battery contact

I started with the battery contacts first.<br/>
Removing them is fairly easy. These have a little tab on the back.<br/>
You just have to push the tabs in and then you can push the contacts out.

![Game Boy - Corrosion Battery]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion battery.jpg" }})

I cleaned the contacts first with water and soap, then Isopropyl Alcohol.<br/>
Some spots were very corroded so I had to remove it by scratching it off with a small flathead screwdriver (I didn't have anything else).

![Game Boy - Battery Contact cleaned 1]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion battery cleaned 1.jpeg" }})

![Game Boy - Battery Contact cleaned 2]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion battery cleaned 2.jpeg" }})

I put the contacts back in and they look good now.

![Game Boy - Battery Contact replaced]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion battery replaced.jpg" }})

I put in some fresh batteries and it started!<br/>
This tells me a few things:
- The motherboard is fine, no corroded traces to the point they're broken.
- The audio works, but cracks a bit.
- The screen is impeccable, no lines or dead pixels.

<p align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/Aa2UDvbr94U" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></p>

*********************
<br/>

### Removing corrosion from the motherboard

As mentioned before, when I opened the Game Boy you could see some corrosion on the motherboard.

This was worse than I had thought. With the point of my tweezers I could very easily scrape off the solder mask.

![Game Boy - Corrosion PCB 3]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion pcb (3).jpg" }})

Trying to desolder components from the motherboard, peels the solder mask off the board too.<br/>
I've never desoldered anything that looked as bad as those battery contacts through holes.

I could litteraly peel of the solder mask that was behind the cartridge connector!

![Game Boy - Corrosion PCB 6]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion pcb (6).jpg" }})

<p align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/Hu7WHmsKPR0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></p>

The good thing here is that the copper underneath wasn't corroded at all.<br/>
At this point I had removed the audio jack PCB, see top right corner of the next picture. Keep this in mind for the next chapter.

![Game Boy - Corrosion PCB 7]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion pcb (7).jpg" }})

I was afraid that the copper traces would start to corrode just from the moisture in the air.<br/>
Not having any solder mask paint or coating on hand I applied a thin layer of solder.

![Game Boy - Corrosion PCB 4]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion pcb (4).jpg" }})

![Game Boy - Corrosion PCB 8]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/corrosion pcb (8).jpg" }})

I admit it doesn't look great, but it should protect the copper traces.

After putting it all back together, I turned it on and... no sound.

*****************
<br/>

### An afternoon lost

Why did the audio stop working? Broken trace due to corrosion?<br/>
The speaker is still fine, so that's not it.<br/>
The ribbon cable between the LCD board and the CPU board is fine.<br/>
The audio amp is not outputting audio... That's weird.

I have to say it wasn't easy to follow traces due to them jumping from top layer to bottom layer.<br/>
Plus, this version of the CPU board has epoxy blobs instead of ICs.

You can see the epoxy blobs on the last picture in the previous chapter.

Using my oscilloscope I measured PWM out of the CPU, so that's fine. The potentiometer on the side for volume control is fine too.<br/>
The PWM goes to the amplifier, but nothing comes out of it.<br/>
I was a bit worried I blew the amplifier up at this point.

Not knowing what to do I opened my other Game Boy which had the same CPU board, with the same epoxy blobs.<br/>
Using my multimeter I found a discrepency on one or two traces going to the amplifier.<br/>
Following these traces I found out that they go to the audio jack board.

Could the audio jack board have some feedback to the amplifier?

I soldered the bad audio jack board to the main board and what do you know, the sound is back.

I'm glad the audio is back, but I'm frustrated it took me an afternoon to figure that out.

![Game Boy - Fixed]({{ "/assets/2018-09-11-Repairing-the-Kurakake-Game-Boy/fixed.jpg" }})

<p align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/rj2O1cerSGA" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></p>

*************