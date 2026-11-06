# PANASONIC_TOUGHPAD_FZM1-UserConfig
The following is my own Windows11/FydeOS/Debian13.7/Bunsenlabs Carbon/... UserConfig on a PANASONIC TOUGHPAD FZ-M1
> [!CAUTION]
> The following shall be followed carefully as it is nothing but an end-user notepad listing things that worked (or didn't) at some point of time

## Why going rugged?
You already know why.

## Hardware
### Disassembly guides
#### Video by protoncomputers
[![Watch the video](https://img.youtube.com/vi/g2-gWtlfr4U/hqdefault.jpg)](https://youtu.be/g2-gWtlfr4U)

#### Full disassembly guide (pdf) by MrJoshua
[Read it here](https://tigerstyle.co.uk/docs/MrJoshuaFZM1.pdf) | [Mirror as of 2024-08-21](MrJoshuaFZM1.pdf)

## Panasonic Recovery Disc Creation Utility
[Recovery Disc Creation Utility (Recovery Image Downloading Edition) for Windows 10 Recovery Image Download Service (v5.00L10) - Windows 10 preinstalled model ](https://global-pc-support.connect.panasonic.com/dldocs/079074)

## Gnome-based DE
### How to use the FZ-M1 without any external KeyBoard
#### Access the keyboard from inside the Gnome based DE
Under accessibility setings, look for "Keyboard and text input" menu. There select "Virtual Keyboard"

#### Access the keyboard from the logon screen
Under accessibility settings, select "Display accessibility options from Quick Settings"

## Additional Panasonic Hardware support
### Touch mode and passive stylus
See the excellent work done by MichaelH1981: [FZ-M1-Touch-Mode](https://github.com/MichaelH1981/fz-m1-touch-mode/blob/main/README.en.md). In a few words, it allows for a full support of the capacities/modes of the screen digitizer, including: 
- touch
- pen
- pen-touch
- glove
- water

Making it possible to use stock Panasonic passive stylus which was not possible untill September 2026.

### Programmable buttons
In order to make programmable buttons work, two opposite approches have been used : 
- Specific [kernel module](https://github.com/matmutant/tbtn-driver) adapted for the FZ-M1 based on [tbtn driver](https://github.com/matmutant/tbtn-driver) for FZ-G1.
- Dedicated [service](https://github.com/MichaelH1981/fz-m1-buttons) which allows for it to work without worrying about direct kernel compatibility. 

### Rear camera support [WIP]
> [!NOTE]
> Rear camera is still a black hole in the config. I couldn't make it work and apparently no one else did either.

dmesg outputs :
```
> $ sudo dmesg | grep -i int3472
[    5.901006] int3472-tps68470 i2c-INT3472:05: TPS68470 REVID: 0x21
[    5.901415] int3472-tps68470 i2c-INT3472:05: error -ENODEV: No board-data found for this model
```
&
```
> $ sudo dmesg | grep -i ipu3
[    6.557199] ipu3_imgu: module is from the staging directory, the quality is unknown, you have been warned.
[    6.562099] ipu3-imgu 0000:00:05.0: enabling device (0000 -> 0002)
[    6.562395] ipu3-imgu 0000:00:05.0: device 0x1919 (rev: 0x1)
[    6.562442] ipu3-imgu 0000:00:05.0: physical base address 0x00000000f6000000, 4194304 bytes
[    6.692956] ipu3-cio2 0000:00:14.3: enabling device (0000 -> 0002)
[    6.693315] ipu3-cio2 0000:00:14.3: device 0x9d32 (rev: 0x1)
[    6.755653] ipu3-imgu 0000:00:05.0: loaded firmware version irci_irci_ecr-master_20161208_0213_20170112_1500, 17 binaries, 1212984 bytes
```



checking ACPI tables : 
```sudo cat /sys/firmware/acpi/tables/DSDT > dsdt.bin``` then ```iasl -d dsdt.bin```  
Then ```grep -iE "INT3472|TPS68470|CAM|OV|IMX" dsdt.dsl``` contains the following output : 
```
        Device (CAM0)
            Name (_DDN, "IMX135-CRDG2")  // _DDN: DOS Device Name
                Return (SBUF) /* \_SB_.PCI0.I2C2.CAM0._CRS.SBUF */
                Return (PAR) /* \_SB_.PCI0.I2C2.CAM0.SSDB.PAR_ */
        Device (CAM1)
            Name (_DDN, "OV2740-CRDG2")  // _DDN: DOS Device Name
                Return (SBUF) /* \_SB_.PCI0.I2C4.CAM1._CRS.SBUF */
                Return (PAR) /* \_SB_.PCI0.I2C4.CAM1.SSDB.PAR_ */
            Name (_HID, "INT3472")  // _HID: Hardware ID
            Name (_CID, "INT3472")  // _CID: Compatible ID
```
NB : IMX135-CRDG2 is the 13Mpx CMOS Sony IMX135 rear camera (that is currently not working) & OV2740-CRDG2 is the Omnivision 2Mpx front camera

## Bunsenlabs
Specific config can be seen [here](https://github.com/matmutant/Bunsenlabs-UserConfig)

