---
layout: post
title: "Prototyping Stand With Integrated Power Supply"
date: 2020-06-09 06:00:00 +0000
categories: [3D-Printing, Microcontroller]
featured-img: prototyping-stand
---
![](/images/2020/breadboard-done.jpg)

# Motivation
I wanted to tidy up my workbench, so I decided to build a prototyping stand for my electronic projects. 
A big advantage of this is that you can carry the projects around without breaking them.
I also want to use it to teach my kids electronics.
That is why it must be easy to use and look nice.

The prototyping stand has an integrated power supply which supplys 3.3V and 5V.
It has enough space to work comfortable because it features two breadboards.
And it features banana sockets because they look cool.

# Parts needed
![](/images/2020/breadboard-parts-needed.jpg)
The power supply is an cheap hw-131 board (2,50€ on eBay) which has short circuit protection.
Apart from that
- some M3 screws and nuts
- four banana sockets
- two breadboards
- an on/off switch
- a green LED
- a 9V battery and a battery cable 
are needed.

# 3D Printed Parts
![](/images/2020/breadboard-printed-parts.jpg)
The stand consists of four 3d printed parts which I designed with Fusion 360.
I printed the parts with 50% infill to achive the appropriate strenght.
I also used two different colors.
The parts can be downloaded here:

The <a href="/assets/stl/breadboardstand-front.stl">front</a>:
<div class="stl-model" data-file="/assets/stl/breadboardstand-front.stl"></div>
The <a href="/assets/stl/breadboardstand-left.stl">left side</a> with the battery holder:
<div class="stl-model" data-file="/assets/stl/breadboardstand-left.stl"></div>
The <a href="/assets/stl/breadboardstand-right.stl">right side</a>:
<div class="stl-model" data-file="/assets/stl/breadboardstand-right.stl"></div>
The <a href="/assets/stl/breadboardstand-top.stl">top</a> with the holes for the banana sockets, power switch and more:
<div class="stl-model" data-file="/assets/stl/breadboardstand-top.stl"></div>

# Assembly
The first step of the assembly is to unsolder the build in green power LED from the hw-131 board. 
The LED is replaced by a bigger one on top of the prototyping stand.
![](/images/2020/breadboard-unsolder.jpg)

Next, the hw-131 board is sticked in the holes (marked red) at the bottom of the 3d printed "top" part, no gluing necessary.
The holes are so small that the hw-131 board sits very tight.

![](/images/2020/breadboard-power1.jpg)
![](/images/2020/breadboard-power2.jpg)

Then, the power switch can be soldered to the battery and the hw-131 power supply with some cables.
![](/images/2020/breadboard-power-supply.jpg)

The next step is to solder the new power LED to the hw-131 board.
The new LED uses the same connections as the old, unsolderd LED.
The new LED is glued to the printed "top" part with plastic glue.
![](/images/2020/breadboard-led.jpg)

A test: When the on/off switch is toggled, the LED lights up!
![](/images/2020/breadboard-led2.jpg)

Add banana sockets to the "top" part.
![](/images/2020/breadboard-banana.jpg)

The last step: Solder the banana sockets to the hw-131 power supply.
![](/images/2020/breadboard-banana2.jpg)

Happy prototyping!
![](/images/2020/breadboard.jpg)
