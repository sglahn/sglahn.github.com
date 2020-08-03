---
layout: post
title: "Marlin, Gen6 And Why It Is A Good Idea To Print From SD Cards"
category: ["3D-Printing"]
date: 2014-02-22 21:00:26 +0200
featured-img: sdcards
---
When I first read about the microSD card feature of the Gen6 board that comes with the Portabee 3d-printer I regarded it as totaly useless. But after a lot of mysterious connection problems between Pronterface and the Portabee during the print apparently triggered by other electrical appliances in the same room I started to investigate the issue.

Sometimes, when I switched on the lights of the room or my soldering iron the Portabee stopped. Pronterface showed an error message which said something about a connection problem. Pronterface was not able to connect to the printer again and I needed to restart both! the printer and the PC. This problem was caused by electromagnetic interferences to the Gen6 board. USB is quite prone to EMI effects via the power line. Large motors, a fan for example, starting and stopping on the same ring main can cause power spikes and knock out the USB connection. I searched the Internet and found a lot of suggestions like buying an uninterruptable power supply or making a shielded case for the board and connect it to ground or buying a higher quality usb cable or just shorten the usb cable. But a different solution seems much easier: Print from an microSD card.

### How it works
Format the microSD card to the FAT32 format. Copy the GCode files to the microSD card. For the GCode files use file names in the [8.3 format](http://en.wikipedia.org/wiki/8.3_filename). That means the file name must have at most eight characters, optionally followed by a period . and a filename extension of at most three characters. Example: <i>FOO.G</i>. Check the microSD card for hidden files and folders and remove them. My Linux distribution automatically created a <i>.Trash</i> folder which caused a lot of problems. Insert the microSD card in the board. If you are using Pronterface select <i>SD</i> and <i>SD Print</i>. Pronterface will show a list where you can select the GCode file and start the print. Pronterface will then show the progress and the temparatures during the print and allow to stop the print process. But be aware to never disconnect the USB cable from the PC during the print because the Portabee 3d-printer will stop.
