# BT Interface for TSDZ2 Open Source Firmware

⚠⚠⚠ The current revision of this board is untested and requires a firmware update for full functionality - not fit for use *yet*! ⚠⚠⚠

This repository is an updated version of MSpider65's [TSDZ2-ESP32-PCB](https://github.com/TSDZ2-ESP32/TSDZ2-ESP32-PCB) project. If you are not familiar with the original project, it is a PCB and some custom firmware (plus Android app) that make the TSDZ2 eBike mid-drive kit smarter (fully customisable settings, log power, speed, etc.) and higher performance. It's a really fantastic project and definitely worth checking out if you're considering the TSDZ2 kit.

For more information on the project, see Mspider65's wiki page: [wiki.](https://github.com/TSDZ-ESP32/TSDZ-ESP32-wiki/wiki)

This update aims to make it more affordable (~$12 vs ~$23 for components). It also aims to use components that are generally slightly easier to solder by hand. It has been done in [KiCAD](https://www.kicad.org), ensuring full control of the source design files.

<img src="Images\PCB-Top.png" alt="PCB-Top" style="zoom: 50%;" />

## Changes

- All connector are now 0.1" pin headers. The intention for these (except the programming connector) is to directly solder the TSDZ2's wires to these as pads instead of actually using pin headers. Being 0.1", some connectors (JST XH?) will also fit, and you are welcome to use as desired.

- 5V and 3.3V regulator changes - cost savings.

- Programming header now uses RST and GPIO0 pins directly (no need for jumpers), with the intention of using RTS and CTS lines of USB-to-serial adaptor to automate putting ESP32 in bootloader mode. More information can be found [here](https://github.com/espressif/esptool/wiki/ESP32-Boot-Mode-Selection#automatic-bootloader).
  TLDR: connect RST and GPIO0 pins to USB-to-serial adaptor like so, and programming should "just work". 
  ⚠ **Make sure you only power the board via 3.3V! **⚠
  
  | Programming Header Pin | USB-to-serial Pin |
  | ---------------------- | ----------------- |
  | RST                    | RTS               |
  | IO0                    | DTR               |
  
- Temperature sensor changed to much cheaper and also lower spec device. (Cause for firmware update.) Still is plenty accurate enough for monitoring PCB temperature and is much cheaper.

- Added solder bridge pads - removes need for diode. Make sure you bridge these with a soldering iron after programming the ESP32.

- Programming header is on breakoff part of board.

- Redrawn from scratch in KiCAD.

## Pinout

<img src="Images\Pinout.png" alt="PCB-Top" style="zoom: 67%;" />

## FAQs

#### Why not use MSpider65's original PCB design?

I have use the original design and it works very well. After needing another PCB recently I saw that many of the components have become very expensive (general silicon shortage at this time...). This updated board aims to be much cheaper while having the same functionality and remaining the same size. This will require an update to the ESP32's firmware, but this should be a small change.

#### What's with the bit of the PCB sticking out?

This is for programming the ESP32 with the USB-to-serial adaptor. Once the ESP32 has been programmed, but the tracks on the top and the bottom with a knife and then snap it off. MSpider65's project allows OTA (over-the-air) firmware updates so you won't be needing it again.

#### I successfully programmed the ESP32 but the board won't turn on when connected to battery power?

After programming, you need to short JP1 & JP2 on the PCB with a soldering iron. This is instead of using a diode in the design. (Slight cost saving and more efficient.) A trade-off I personally like in this case. Let me know if you prefer a diode...

#### Why is there a USB-to-Serial adaptor in the BOM?

As part of the aim to keep cost to a minimum, all items can be sourced from [LCSC](https://lcsc.com). Both the USB-to-serial adaptors and 2.4GHz antenna are quite inexpensive from LCSC and so have been included in the BOM for convenience. Of course if you are making more than one board you only require one USB-to-serial adaptor, but multiple antennas.

#### How can I order the PCB?

You can order the bare PCB from [JLCPCB](https://jlcpcb.com). You will need to select 4-layers, 0.8mm thickness and "JLC7629" controlled impedance. I also recommend getting a stencil and using solder paste. Example of suitable PCB options:

<img src="Images\Example-PCB-Options.png" alt="PCB-Top" style="zoom: 67%;" />



#### How can I order the components?

Upload "TSDZ2-ESP32-Alt_BOM.csv" to the "BOM tool" on LCSC [here](https://lcsc.com/bom.html#/upload). Only select quantity and part number columns. Be sure to adjust how many USB-to-serial adaptors you require before checking out. Also note that currently if you order the PCB before the components you can get a discount on postage with LCSC at checkout.

#### How did you curve the track/add teardrops?

By using the [KiCAD round tracks](https://github.com/mitxela/kicad-round-tracks) and [teardrops](https://github.com/NilujePerchut/kicad_scripts/tree/master/teardrops) plugins.

