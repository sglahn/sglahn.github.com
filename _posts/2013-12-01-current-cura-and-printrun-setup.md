---
layout: post
title: "Software Settings For 3D Printing With The Portabee"
category: ["3D-Printing"]
date: 2013-12-01 21:00:26 +0200
featured-img: software-settings
---
As I mentioned in a previous posting I am using Cura for g-code generation and Printrun/pronterface as an interface for the printer. I have been asked for my software settings so I uploaded my Cura profile file to [Github](https://github.com/sglahn/3dprinting/tree/master/Cura-Config). 

For me the Cura profile is subject of constant changes because I have not found, and propably never will, a configuration that works well for all models. But nevertheless here are some important configuration parameters I am using and which I regard as static(I am using Linux btw):

### Cura preferences
- Steps per E = 514 
- Machine width = 120 
- Machine depth = 120 
- Machine height = 120 
- Extruder count = 1 
- Heated bed = Yes 
- Serial port = /dev/ttyUSB0 
- Baudrate = 250000 

### Pronterface settings
- Port = /dev/ttyUSB0 
- Baudrate = 250000 
- Bedtemp_pla = 60 
- Build_dimensions = 120x120x120+0+0+0 
- E_feedrate = 50 
- Preview_extrusion_width = 0.5 
- Temperature_pla = 185 
- Xy_feedrate = 3000 
- Z_feedrate = 201 
