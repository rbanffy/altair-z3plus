# altair-z3plus

Docker image for running an Altair Z-80 based computer with Z3PLUS on CP/M Plus

To start the service locally, run:

```shell
docker run -p 8823:8823 rbanffy/altair-z3plus
```

When the service comes up, it listens to port 8823. The emulated machine boots under CP/M Plus. In order to start MP/M, the first user to connect (running `telnet localhost 8823`) needs to run the MPM.COM program, which loads and starts the MP/M II environment:

```plain

LDRBIOS3 v1.07 27-Jun-2010

CP/M V3.0 Loader V1.00 18-Jan-2002
Copyright (C) 1982, Digital Research

 BNKBIOS3 SPR  ED00  0A00
 BNKBIOS3 SPR  AD00  1300
 RESBDOS3 SPR  E700  0600
 BNKBDOS3 SPR  7F00  2E00
 
 57K TPA

BIOS3 Banked for SIMH Altair V-1.27, 4 HD, 02-May-2009

A>z3plus

                             ---  Z3PLUS  ---
                    The Z-System for CP/M PLUS (CP/M 3)
                  Vers. 1.02    (c) 1988 Bridger Mitchell
                     [ TPA: 0100 - CA05        50.50k ]



TCSELECT, Version 1.2

Selected Terminal is: VT100 DEC               

ZCPR3 TERMINAL DESCRIPTOR LOADED

A0:COMMANDS>

```

At this point, you are in the MP/M prompt. It shows the current user number (from 0 to 15) and the currently active drive. Up to four users can connect at the same time, and will automatically be directed to different user areas.

## Contents of the Docker image

| Drive | Contents                      |
| ----- | ----------------------------- |
| A:    | System disk + Z3PLUS          |
| B:    | Second Z3PLUS disk            |
| C:    | Sorcim's SuperCalc 2          |
| D:    | Ashton and Tate's dBase II    |
| E:    | MicroPro's WordStar Release 4 |
| F:    | Microsoft's Multiplan 4       |
| G:    | Miscellaneous games           |
| H:    | Miscellaneous applications    |
| I:    | First hard drive, empty       |
| J:    | Second hard drive, empty      |

All files in the disk images are in user area 0.

## Creating empty disk images

In order to create an empty 8GB (the largest size supported under CP/M) hard disk image, use the `dd` utility:

```shell
dd if=/dev/zero bs=1024 count=8192 | tr '\000' '\345' > empty_hard_disk.dsk
```

To create an empty 8" floppy image, use:

```shell
dd if=/dev/zero bs=137 count=8128 | tr '\000' '\345' > empty_floppy.dsk
```

## Further Reading

The Z3PLUS user's guide is available from <http://gaby.de/ftp/pub/cpm/z3plus.pdf>.

Information specific to the Altair Z80 emulator can be found at <http://bitsavers.trailing-edge.com/simh.trailing-edge.com_201206/pdf/altairz80_doc.pdf>.
