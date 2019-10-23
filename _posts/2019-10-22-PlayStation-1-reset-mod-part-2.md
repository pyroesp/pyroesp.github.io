---
layout: post
title:  "PlayStation 1 reset mod, part 2"
date:   2019-10-22 14:00:00 +0200
categories: [electronics, reverse engineering, playstation, modification]
---

### Recap

A few months ago I bought a PSIO from Cybdyn System.  
This device let's you play games from an SD card on a PlayStation 1, a bit like krikzz EverDrives for older cartridge based consoles.  

After playing around with it for a bit I saw a user on the PSIO forums ask for a way to reset the PlayStation 1 through a button combination.  
Unfortunately the PSIO has no access to the status of the buttons on a controller, so this can't be done from the PSIO.  

I had messed around with the PlayStation 1 communication before and I was sure I could build something that would do just that.  
From my previous post you can see I managed to make something that worked, but was not perfect.  

In this article I'm going to explain what I did afterwards.  

************************
<br/>
### Video

But first here's a video of the latest version working:  
<p align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/dBLZW0fIh68" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></p>

************************
<br/>
### Disclaimer

This mod is a piece of hardware you add to the PlayStation. Although you don't have to modify the motherboard, it still requires a bit of soldering.  
I am not responsible if the PlayStation doesn't work after you've installed or removed my mod.  
If you don't know what you're doing, go to someone that does.  

**You do this at your own risk.**  

************************
<br/>
### Caution

It goes without saying that resetting the PlayStation whilst you're saving data can corrupt the save data and/or the memory card.  
Do **NOT** reset the PlayStation when it is writing data to the memory card.  

You have been warned.  

************************
<br/>
### What's new

Now that we're done with all the legal bullshit, let's talk about the differences with the previous version.  

The previous mod used an Arduino Nano (ATMEGA328P) at 16MHz, powered with the PlayStation 3.5VDC. The program written for it uses the polling method to read the command and data between the PS1 and controller.  
Although this works there's two issues:  
- The Arduino Nano is underpowered for the 16MHz at which it's running, so unexpected behaviour can happen. The datasheet specifies that anything above 8MHz needs to be powered from 5VDC.  
- The polling method is unreliable as it does miss data, which means that the reset is not instantenious and takes a second. I also don't like how the polling loop has to be programmed.  

The communication from the PS1 with the controller is basically SPI, which means that I needed a microcontroller with two SPI modules to be able to read the command and data signals.  

I've looked at what microchip has available and the 16F18325 was the cheapest mcu with two SPI modules they had.  
The 16F18325 also can be powered by the 3.5VDC and use it's internal oscillator with PLL to go up to 32MHz.

So that's what I'm using in the new mod.  

************************
<br/>

### Schematic and PCB

![Schematic]({{ "https://raw.githubusercontent.com/pyroesp/PlayStation-1-Reset-Mod/master/pictures/mod/schematic.png" }})

![pcb front]({{ "https://raw.githubusercontent.com/pyroesp/PlayStation-1-Reset-Mod/master/pictures/mod/pcb%20-%20front.png" }})

![pcb bottom]({{ "https://raw.githubusercontent.com/pyroesp/PlayStation-1-Reset-Mod/master/pictures/mod/pcb%20-%20bottom.png" }})


**TODO**
