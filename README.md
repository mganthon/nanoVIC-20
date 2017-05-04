# nanoVIC-20
Emulating a CBM VIC-20 with NTSC video on a standalone Arduino Nano

Project originaly published here: https://www.hackster.io/janost/the-nano-vic-20-e37b39
I am not the creator.  I only want to document my experiments with this with the goal of expanding the project to be a self-contained VIC-20 clone using a PS/2 keyboard, SD floppy emulator, and LED indicator. The goal is to enclose all of this in the keyboard. - mganthon

## STORY

The VIC-20 emulator on an AVR
Can a Commodore VIC-20 be emulated on a single AVR chip?

Yes, I did and it has composite NTSC video output aswell.

Intended to run on an ATmega328, I pulled my Nano3 board out of the drawer.
Wired up 2 resistors for sync and video and an RCA plug for the 5" TFT monitor.

The schematics is simple and can be seen here to the right.
It's just a 470ohm and 1K resistor connected to TX and D2 on the Nano.
It mixes the pixelstream and sync pulses to a composite NTSC videosignal.

## The challange
How do you squeeze the 5K RAM/20K ROM VIC-20 on to the ATmega328?
And give it a video output?

Well it's a tight fit but it works.

### Memorymap:
$0000-0340 VIC-RAM, maps to AVR SRAM for zeropage and stack.
$0400-07FF VIC RAM, maps to AVR EEPROM for Basic code.
$0800-09FF VIC-RAM, maps to AVR SRAM for Basic variables.
$1E00-1FFF VIC-RAM, maps to AVR SRAM for videomemory buffer.
$C000-DFFF VIC Basic ROM, maps to AVR flash for a Basic ROM image.
$E000-FFFF VIC Kernal ROM, maps to AVR flash for a Kernal ROM image

All portpins are still free on the Nano for attaching a scanned keyboard matrix or joysticks.

It runs a MOS6502 CPU emulator and generates NTSC video on the fly from the videobuffer.

The video is only B/W but has the original resolution of 22x23 chars or 176x184 pixels.
The video is shifted out using the USART on the ATmega.

There isn't enough memory on the Nano to do a real bitmap so it creates the pixeldata on the fly from the font ROM. 
It renders the character from the original CBM ROM font.

Once booted it has 1535 bytes free for basic code.

As there is no keyboard scanner implemented yet characters from keyboard must to be poked into the keyboard buffer.

## Sketch
To compile the code you need both the C20megaEMU_STA.ino file and the cpu.c file.

The project uses about 25K flash and 1900bytes of SRAM.
It also uses the entire 1K EEPROM on the AVR for Basic memory.

## The Commodore VIC-20

## The schematic

