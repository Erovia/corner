---
title: "Planck Through Hole Kit Build log"
menuTitle: "Planck THK"
weight: 10
---

### The Planck Through Hole Kit

The Planck Through Hole Kit (THK) was announced by Jack Humbert on the [/r/olkb subreddit](https://www.reddit.com/r/olkb/comments/9kgdds/planck_through_hole_kit_all_throughhole/) on 2018.10.01.  
[The hardware](https://github.com/olkb/planck_thk) was released under GPLv3 with the intention to be accessible to everyone.  
The [original QMK pull request](https://github.com/qmk/qmk_firmware/pull/4034) never got merged and documentation/information about the board is scarce.  
As QMK is a pretty fast-moving project, the original code for the keyboard did not build with new QMK.  

I first heard about this keyboard when a community member tried to flash his board and asked for help.  
Sadly, I was not able to work out all the things without having access to the hardware and my abition to make it work again had to be put on hold.  
Until Jack was kind enough to send my two unassembled kits.  
Now I have the hardware fully working with mainline QMK so I thought I'll document a process of building and compiling and flashing this board.  

### Hardware

The gerber files for the PCB and the bottom case can be downloaded from [the OLKB GitHub](https://github.com/olkb/planck_thk/releases).  
That repo also has a nice link to the [bill of materials (BOM)](https://octopart.com/bom-tool/NWghestL) for all the components needed for the assembly.  
Of course, most can be substituted with similar parts from the local store, dad's shed or Aliexpress.  

As the BOM has the "Schematic Reference" field filled in for all components, building the board is quite easy.  

{{% notice note %}}
When soldering the LEDs, the cathode (shorter leg) should face the USB port.
{{% /notice %}}

The BOM for future reference:  
| Qty | Line Item | Manufacturer | MPN | Octopart URL | Schematic Reference |
|-|-|-|-|-|-|
| 1 | UB-M5BR-DM14-4D | JST | UB-M5BR-DM14-4D | https://octopart.com/ub-m5br-dm14-4d-jst-54155138 | USB PORT |
| 1 | TP124005-2 | DB Unlimited | TP124005-2 | https://octopart.com/tp124005-2-db+unlimited-59659708 | SPEAKER |
| 48 | 1N4151TR | Vishay | 1N4151TR | https://octopart.com/1n4151tr-vishay-9342963 | D* |
| 1 | 1301.9303 | Schurter | 1301.9303 | https://octopart.com/1301.9303-schurter-5416046 | RESET |
| 1 | ECS-160-20-4VX | ECS International | ECS-160-20-4VX | https://octopart.com/ecs-160-20-4vx-ecs+international-19118787 | Y1 |
| 1 | S5B-PH-K-S(LF)(SN) | JST | S5B-PH-K-S(LF)(SN) | https://octopart.com/s5b-ph-k-s%28lf%29%28sn%29-jst-248886 | ALT USB PORT |
| 1 | ATMEGA32A-PU | Microchip | ATMEGA32A-PU | https://octopart.com/atmega32a-pu-microchip-77760288 | MICROCONTROLLER |
| 1 | 1-2199299-5 | TE Connectivity | 1-2199299-5 | https://octopart.com/1-2199299-5-te+connectivity-34963610 | MICROCONTROLLER SOCKET |
| 1 | LD1117AV33 | STMicroelectronics | LD1117AV33 | https://octopart.com/ld1117av33-stmicroelectronics-526794 | POWER REGULATOR |
| 3 | s04b-sz | JST | S04B-SZ | https://octopart.com/s04b-sz-jst-87088753 | I2C |
| 1 | 210-4ES | CTS Components | 210-4ES | https://octopart.com/210-4es-cts+components-19788403 | DIP SWITCH |
| 1 | 67997-410HLF | Amphenol ICC / FCI | 67997-410HLF | https://octopart.com/67997-410hlf-amphenol+icc+%2F+fci-90333672 | JTAG HEADER |
| 1 | 68602-406HLF | Amphenol ICC / FCI | 68602-406HLF | https://octopart.com/68602-406hlf-amphenol+icc+%2F+fci-90340423 | ISP HEADER |
| 4 | CFR-25JT-52-68R | Yageo | CFR-25JT-52-68R | https://octopart.com/cfr-25jt-52-68r-yageo-16252182 | R1, R2, R4, R5 |
| 1 | LTL-4222 | Lite-On | LTL-4222 | https://octopart.com/ltl-4222-lite-on-869424 | PWR |
| 1 | LTL-4232 | Lite-On | LTL-4232 | https://octopart.com/ltl-4232-lite-on-868849 | ACT |
| 1 | SR595C104KARTR1 | AVX | SR595C104KARTR1 | https://octopart.com/sr595c104kartr1-avx-2348402 | C3 |
| 2 | SR151A200JAR | AVX | SR151A200JAR | https://octopart.com/sr151a200jar-avx-20091099 | C1, C2 |
| 1 | FK18X5R1A475K | TDK | FK18X5R1A475K | https://octopart.com/fk18x5r1a475k-tdk-21525622 | C4 |
| 2 | PEC12R-4217F-N0024 | Bourns | PEC12R-4217F-N0024 | https://octopart.com/pec12r-4217f-n0024-bourns-26648364 | ENCODERS |
| 1 | CFR-25JT-52-1K5 | Yageo | CFR-25JT-52-1K5 | https://octopart.com/cfr-25jt-52-1k5-yageo-16252146 | R3 |

### ISP Flashing

The first time, you will need to ISP flash the microcontroller to set the fuses and flash the bootloader.  
The [QMK ISP flashing quide](https://docs.qmk.fm/#/isp_flashing_guide) shows how you can use a spare ProMicro or Teensy as flasher, but I like my cheap USBasp I got off of Aliexpress.  

{{% notice warning %}}
The keyboard has a 6-pin ISP header which makes you think that you can just connect your 6-pin USBasp, but sadly the pins are mirrored on the board.  
{{% /notice %}}

My recommendation is to use female-to-male dupont cables for the connection.  

![Planck THK ISP pins](/images/thk_isp_pins.jpg)

I set my USBasp to 3.3V, but it should work with 5V as well.  

If everything is connected well, the power (red) LED should be on, your computer should recognize the USBasp and the flashing command below should work without any errors.  
If you experience any issues, the first thing you check should be the correct pin connections.  

### Bootloader

Originally, the bootloadHID bootloader was used for the THK as can be seen in [the old branch](https://github.com/qmk/qmk_firmware/tree/planck_thk) and if someone would want that, the following [gist](https://gist.github.com/jackhumbert/50cbfead6ab27b6b3ee42f8c59d72776) can be of some help.  
Personally, I think USBasp is a much nicer one as it can be used with `avrdude` and other, newer through hole boards use it as well.

As USBasp usually requires two buttons to reset the board, but the THK has only one, I chose a [fork](https://github.com/gblargg/usbasploader) that has some nice features, like the ability to boot into bootloder with a reset button.  
I have a [fork, pre-configured for the THK](https://github.com/Erovia/planck_thk_usbasploader).  

Setting the fuses, compiling and flashing the bootloader is this simple:  

```shell
[erovia@ws]$ git clone https://github.com/Erovia/planck_thk_usbasploader.git
[erovia@ws]$ cd planck_thk_usbasploader
[erovia@ws]$ make fuse
[erovia@ws]$ make flash
```

Once done, disconnect the ISP flasher from the keyboard, connect the board to your computer and press the **Reset** button.  
If everything went well, it should be recognized as an USBasp flasher.  

### Firmware

The firmware currently lives in [my QMK fork](https://github.com/Erovia/qmk_firmware/tree/planck_thk_revived), but I'll get it merged upstream, to not repeat history.  
The board should be compatible with most Planck and ortho4x12/planck_mit keymaps, but I created a `thk` one, to showcase it's unique features.  

```shell
[erovia@ws]$ qmk flash -kb planck/thk -km thk
```

### Case

*Soon...*
