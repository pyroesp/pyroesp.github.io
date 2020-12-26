---
layout: post
title:  "PlayStation 1 reset mod, part 3"
date:   2020-12-26 01:40:00 +0200
categories: [electronics, reverse engineering, playstation, modification]
---

### Recap

Some time passed since I've made the reset mod for the PlayStation.  
Since then, the xStation came out which replaces the whole CD drive. Pretty cool device made on an ESP32!  

The xStation has a special menu you can access by pressing the reset button for 2s.  
Someone on my github requested I added this feature so the reset mod would work with the xStation.  

And so I did.  

************************
<br/>

### New firmware

Because the xStation expects a reset of 2s I decided to add a new button combination just for that.  
The new button combination is : **SELECT + CROSS + L2 + R2**  

With this button combination you'll reset the PlayStation for 2s.  
The other combinations still work like before.  

************************
<br/>

### Updating

The reset mod has pads on the back of the PCB were you can connect a programmer for ICSP.  
Some programmers like the PICKit3 I have can output 5V to power the target which probably isn't good to put on the 3.5V bus of the PlayStation.  
I recommend you disconnect the 3.5V from the PlayStation as there is nothing to block the programmer voltage.  

Once connected either compile the program or use the hex file in the release to flash the reset mod.  

If you don't have a programmer then don't panic, your reset mod can support the xStation too with an extra bit of circuit.  

************************
<br/>

### No programmer?

I get that not everyone has a PIC programmer to upgrade the firmware from v1.1 to v1.2.  
That is not an issue if you're ok with adding an extra little mod.  

![pulse extender schematic]({{ "https://raw.githubusercontent.com/pyroesp/PlayStation-1-Reset-Mod/master/pictures/pulse%20extender/pulse%20extender.PNG" }})  

The schematic above is a pulse extender for the v1.1 reset mod.  
Instead of connecting the reset to the PlayStation motherboard, you connect it to the 555 set as a pulse extender.  
The output of the 555 goes to a NPN trasistor which will pull the reset of the PlayStation low for 2s.


Below is my test PCB which I've succesfully tested on my PlayStation.  

![pulse extender pcb]({{ "https://raw.githubusercontent.com/pyroesp/PlayStation-1-Reset-Mod/master/pictures/pulse%20extender/pulse%20extender%20pcb.jpg" }})  

Obviously you can make it as small and compact as you want, this was just for testing the circuit.  
The below oscilloscope image shows the small pulse from the reset mod, in blue, and the long pulse from the 555, in yellow.

![pulse extender scope]({{ "https://raw.githubusercontent.com/pyroesp/PlayStation-1-Reset-Mod/master/pictures/pulse%20extender/pulse%20extender%20scope.jpg" }})  

Note1: The 555 here is one that works on the 3.5V. In my test I used a TLC555C.  
Note2: Any other pulse extender would work here. This is just a quick no microcontroller fix.  

************************
<br/>

### Thanks to

* [ArcadeTV](https://twitter.com/arcadetv/status/1340730293402169347) for the request.  
* [_ramapcsx2](https://twitter.com/_ramapcsx2) for the xStation and taking the time to respond to a few questions I had.