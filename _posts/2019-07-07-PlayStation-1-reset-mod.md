---
layout: post
title:  "PlayStation 1 reset mod"
date:   2019-07-07 04:00:00 +0200
categories: [electronics, reverse engineering, playstation, modification]
---

I've recently acquired a PSIO from Cybdyn System.  
It's a device that's plugged into the parallel port of a PlayStation 1.  
It allows you to play games from an SD card instead of a CD.  

If you've ever had or heard about Krikzz EverDrive SD cartridges for older consoles, well it's similar to those.  


When you first start the PlayStation with the PSIO plugged in, a menu pops up where you can choose your game.  
The only thing missing to the PSIO is a way to get back to this menu once a game has been launched.  

Here's where my mod comes into play.  
The purpose of the mod is to be able to reset the PlayStation with a button combination on controller 1, so that you don't have to get up to reset the PS1.  

*******************
<br/>
### Disclaimer

I'm not responsible for anything you with this mod.  
You do this at your own risk.  

If something blows up... You probably fucked up somewhere.  

*******************
<br/>
### Parts needed

This mod uses an Arduino Nano with 4x 1k Ohm resistors and 1x 500 Ohm resistor or 2x 1k Ohm in parallel.  
You'll also need some wire.

Don't forget the program. You can find it here: [https://github.com/pyroesp/PlayStation-1-Reset-Mod](https://github.com/pyroesp/PlayStation-1-Reset-Mod).

*******************
<br/>
### Installing the mod

First of all, flash your Arduino Nano. If you don't know how to do this, google it.  

The next steps are just describing how I did it on my PU-18 motherboard and where I placed the Arduino.  
You don't have to follow this. If you think you've found a better place to hide the Arduino then go for it.  

The Arduino Nano will use the 3.5V from the PlayStation so I removed the AMS1117 voltage regulator.  
I also removed the CH430 chip as I won't use the serial port anymore, and some misc caps/diode.  
This way only the ATMEGA328P will be powered by the PlayStation, plus it makes the bottom of the Arduino Nano flat.  

![Arduino Nano Bottom]({{ "https://github.com/pyroesp/PlayStation-1-Reset-Mod/raw/master/Pictures/arduino nano bottom.jpg" }})  

If you didn't change the I/O from the code then these are the pins you need the resistors to.  
* PB5 - SCK (input, connect to PS1 clock)
* PB4 - CMD (input, connect to PS1 TX)
* PB3 - DATA (input, connect to PS1 RX)
* PB2 - /SS (input, connect to controller 1 select)
* PB1 - playstation reset (output, connect to reset of parallel port (pin 2))

![Arduino Nano Top]({{ "https://github.com/pyroesp/PlayStation-1-Reset-Mod/raw/master/Pictures/arduino nano top.jpg" }})  

PB1 has a 500 Ohm resistor. 1k was too much to pull the reset low.  
You could use 2x 1k Ohm resistors in parallel here instead.

Next you'll connect the Arduino Nano to the PlayStation.  

![Controller Connection]({{ "https://github.com/pyroesp/PlayStation-1-Reset-Mod/raw/master/Pictures/controller connection.jpg" }})  

![Controller Connection 2]({{ "https://github.com/pyroesp/PlayStation-1-Reset-Mod/raw/master/Pictures/controller connection 2.jpg }})  

![Controller Connection Location]({{ "https://github.com/pyroesp/PlayStation-1-Reset-Mod/raw/master/Pictures/controller connection location.jpg }})  

The PlayStation reset can be connected here:  

![Reset From Parallel Port]({{ "https://github.com/pyroesp/PlayStation-1-Reset-Mod/raw/master/Pictures/reset from parallel port.jpg" }})  

For power you want to connect the 3.5V of the PlayStation to the 5V of the Arduino Nano.  


This is where I placed my Arduino Nano:  

![PlayStation with Mod]({{ "https://github.com/pyroesp/PlayStation-1-Reset-Mod/raw/master/Pictures/playstation with mod.jpg" }})  

![PlayStation with Mod 2]({{ "https://github.com/pyroesp/PlayStation-1-Reset-Mod/raw/master/Pictures/playstation with mod 2.jpg" }})  

*******************
<br/>
### Reading controller data

I previously tried to decode PlayStation controller data so I already know what it looks like.  
![GUNCON Logic Analyzer - Shooting Outside of screen]({{ "/assets/2018-06-14-shooting-bad-guys-2/LA-GUNCON-1.png" }})  

NOCASH from [problemkaputt](https://problemkaputt.de/psx-spx.htm) has a ton of information about the PlayStation and its controllers.  

In short, the PlayStation uses a serial IO which resembles SPI. 
From the PlayStation schematics you can see that there's a TX, RX, CLK and CS. Both memory cards and control port share the same bus.  
In addition to the bus and to, I assume, save on IO pins, controller port 1 and memory card 1 share the same CS. Controller port 2 and memory card 2 share a different CS.  

To differentiate a controller from a memory card the first byte transmitted from the PlayStation is the address of the device it wants to access.
A byte of value 0x81 will access the memory card. A byte of 0x01 will access the controller.  
Add to that the CS and it can read the 4 devices plugged in.  
The PlayStation will then send byte 0x42 for a read command.

For a controller, the next 2 bytes sent are its ID.  
For example, the value 0x5A41 is the ID to a digital controller.  
Then comes the switch data.  


The Arduino Nano is connected to CS, CLK, TX and RX.  
It will only read TX and RX when the CS of port 1 goes low and on the rising edge of CLK.  

Once the data transfer is done it's going to decode the data to check whether or not this data is a read command of controller 1.  

Reading TX and RX is done through polling, because I had a few issues trying to read data using interrupts.  

*******************
<br/>
### Parsing data

The data is checked against a few defines in the code.  
First it's going to check if the controller is the selected device and if the read command has been sent.  
~~~
  // Check first command for device selected
  if (cmd.device_select == CMD_SEL_CTRL_1 && cmd.command == CMD_READ_SW){
~~~  

Then it's checking the ID of the controller.  
~~~
    // Check ID
    switch(data.id){
      case ID_GUNCON_CTRL:
        // ...
        break;
      case ID_DIG_CTRL:
      case ID_ANP_CTRL:
      case ID_ANS_CTRL:
        // ...
        break;
~~~  

I only have a digital controller, analog controller and GUNCON (light gun) so I have only programmed those IDs in the code.  
~~~
#define ID_DIG_CTRL 0x5A41 // digital
#define ID_ANP_CTRL 0x5A73 // analog/pad
#define ID_ANS_CTRL 0x5A53 // analog/stick
#define ID_GUNCON_CTRL 0x5A63 // light gun
~~~  

After that it's going to check whether or not the reset button combination has been pressed against a predefined value.  
~~~
/* Key Combo */
#define KEY_COMBO_CTRL 0xFCF6 // select-start-L2-R2 1111 1100 1111 0110
#define KEY_COMBO_GUNCON 0x9FF7 // A-trigger-B 1001 1111 1111 0111
~~~  

Changing those defines will change the reset button combination.  

*******************
<br/>
### Resetting the PlayStation

Once a reset command from the user has been detected, the Arduino Nano is going to pull the reset line of the PlayStation low for a few ms.  
~~~
    // Check switch combo
    if (key_combo != 0 && 0 == (data.switches ^ key_combo)){
      // change RESET from input to output, logic low (PORT is already 0)
      PS1_DIR_IO |= _BV(RESET);
      delay(100); // hold reset for 100ms
      // change RESET back to input
      PS1_DIR_IO &= ~_BV(RESET);
      delay(REBOOT_DELAY); // wait 30 sec before next reset, this is to prevent multiple reset one after the other
    }
~~~  

A reboot delay of 30 seconds is added so that the PlayStation can boot properly before getting a new reset command.  

*******************
<br/>
### Video

<p align="center"><iframe width="560" height="315" src="https://www.youtube.com/embed/lb_uCGyv6pY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></p>