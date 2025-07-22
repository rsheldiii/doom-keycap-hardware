# Doom Keycap Hardware Repository

![](https://i.imgur.com/QxaZdQg.jpeg)


Here are all the files associated with the hardware for the Doom Keycap. If you're looking for the code, it's [here](https://github.com/rsheldiii/rp2040-doom-LCD/).

I had every intention of fabricating and selling these myself, but I got caught up on how to manufacture the keycap itself without A. breaking the bank, or B. doing a group buy before realizing my design wasn't feasible. With that, coupled with (happy) life events, I decided to open-source the hardware now.

## Doom Business Card

I handed out quite a few of these during OpenSauce 2025! If you're one of those people and you're seeing this message, try back in a couple of days. The BOM in the kicad files for the keycap is _almost_, but not quite, the same as the business card; I also need to upload a stencil. Once I'm back home I'll do both!

## Doom Keycap Kicad

This directory houses the Kicad files for the final version of the Doom Keycap PCB. It was essentially a mashup of the RP2040 minimal pcb and Adafruit's schematic for the [MAX98357AETE breakout board](https://learn.adafruit.com/adafruit-max98357-i2s-class-d-mono-amp/downloads), with a semi-standard FPC connector for SPI displays and smaller parts. Quick facts:

* The footprint library did not make the jump to a new operating system, but the footprints are embedded in the PCB file; you can right-click "edit footprint", save-as, and then reassociate them in the schematic. Or you know, you could not.
* I used screens from [this](https://www.aliexpress.us/item/3256803794221438.html?spm=a2g0o.order_list.order_list_main.4.651c1802EaKSD2&gatewayAdapt=glo2usa) aliexpress listing, which have FPC connectors that look like [this](https://imgur.com/btXJBdo). No other FPC connector will work on this iteration of the board (obviously)
* Finding a small but powerful enough speaker was a challenge. The small screen versions of the keycap did not have a speaker installed due to size constraints.
* I calculated the value of the crystal oscillator's capacitors wrong; it starts up, but it requires an extra flag during program compilation to give the crystal more time to stabilize. It should work as-is, but optimizing those values would be ideal
* The hardware design / components selected might not be well-optimized, I'm a computer scientist by trade. I did my best though.
* The flash memory could definitely be smaller, but beware: not all memory chips are drop-in replacements. Similarly, the voltage regulator could have a much smaller footprint, I just couldn't find one at the time. If you do swap the regulator and you want to play doom on the keycap with a keyboard, remember the regulator will be supplying electricity to the keyboard, not the other way around. 


## Doom Keycap Case

Here are the final couple STLs I generated for the model, plus the fusion 360 file I used to generate them. 

* the f3d file should have comments in the parameters section about the size of the larger screen, if you wish to make the large version of the keycap.
* If you want to install a speaker on the small version, you'll need to increase the space available to the PCB
* These parts were printed on a resin printer in transparent resin, processed, and then hit with clear gloss lacquer. My plan was to switch to an acrylic insert for mass production.


## Fabrication tips

* Unless you are a god-tier solderer, you're not going to get away with hand-soldering these boards; you will need a stencil and a reflow oven or similar. OSHPark has a sister-service for cheap stencils, which is what I used.
* You _can_ get away with a single stencil for the top of the board and hand-soldering the bottom though. the only tricky part is the FPC connector, but with extra flux and dragging the tip, the solder should wick where it needs to go. 
* The LED on the board is keyed to blink at certain critical moments in the program; if you aren't getting _any_ blinking at all, it means the device is malfunctioning. If you get _some_ blinking that stops, you probably forgot to burn the wad file to memory - I can't tell you how many times I did this. A screen does not have to be installed for the program to run successfully.
* if you upload a program that runs fine, but is then forgotten, you need to set PICO_XOSC_STARTUP_DELAY_MULTIPLIER. The crystal isn't stabilizing fast enough, which is causing the RP2040 to give up on reading the flash chip
* Fitment on the case is very tight; try not to over-bend the FPC on the screen
* Power can be delivered through the USB connector, or through the pads on the bottom of the PCB (refer to schematic for polarity). The voltage regulator in the BOM has more than enough power to run a simple keyboard, though I would be careful with LED keyboards. 1S lipos should work as well. If you want to use another power source, there are very many voltage regulators in the SOT223 package
