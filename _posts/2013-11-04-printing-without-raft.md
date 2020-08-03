---
layout: post
title: "Printing Without Raft"
category: ["3D-Printing"]
date: 2013-11-04 21:00:26 +0200
featured-img: raft
---
I spend some more time experimenting with my Portabee 3D printer. In the example Cura configuration printing with raft is recommmended. Raft means a few layers of lines below the bottom of the object. It's purpose is to prevent warping but it gives you a hard time removing it after the print.
<center><figure><img src="/images/2013/raft1.jpg"><figurecaption><a href="http://www.thingiverse.com/thing:34816">Tape Dispenser</a></figurecaption></figure></center>

I wanted to print without raft but I had problems getting the first layer to stick to the kapton tape. The problem was exact z height adjustment. What helped here was the so called "paper check" to see if a sheet of paper just passes between the noozle and the headbed. I tested this in all 4 corners of the bed and adjusted the screws. What also helped was to reduce the sprint speed of the bottom layer in Cura to 15 mm/s. I also set the start distance of the the first layer to the skirt to 0 mm and the line count of the skirt to 2 for extruder priming. It is also a good idea to clean the kapton tape with a towel before the print because sometimes even a little finger print grease can unstuck the first layer of the object. On the pictures below you can see the results.
<center><figure><img src="/images/2013/raft2.jpg"><img src="/images/2013/raft3.jpg"><figurecaption></figurecaption></figure></center>
