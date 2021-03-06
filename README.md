Files to build the PicoMite. MMBasic running on the Raspberry Pi Pico

NB: This should be built against pico-sdk version 1.3. 
Previous versions were built against a modified sdk version 1.2. V1.3 now works out-of-the-box

The file layout should be:

anything/pico-sdk

anything/PicoMite/all source and header files

The code is developed under VSCODE on W11

Compiled version and documentation is available on https://geoffg.net/picomite.html

Change list from V5.07.00
***********************************************************************************************************************
V5.07.03RC10

New command LOAD JPG. 
LOAD JPG fname$ [,xleft%] [,ytop%]. 
This loads a jpg to the display. Like the CMM2 it does not support progressive jpg. Unlike the CMM2 it doesn't care how big the jpg is and whether it exceeeds the screen limits. 
POKE DISPLAY HRES n. 
POKE DISPLAY VRES m. 
Allows you to override the display limits. Like the rest of the POKE DISPLAY commands (and POKE in general) use with care and don't blame me for the consequences. 
Increased the maximum display width in OPTION DISPLAY to 240 characters. 
POKE DISPLAY command [,data1] [,data2] [,datan]. 
This command sends commands and associated data to the display controller for a connected display. 
This allows the programmer to change parameters of how the display is configured. 
e.g. POKE DISPLAY &H28 will turn off an SSD1963 display and POKE DISPLAY &H29 will turn it back on again. 
It is up to the Basic programmer to read and understand the datasheet for the display in use to make use of the command. 
Works for all displays except ST7790 and GDEH029A1. 
Automatically resizes compatible terminal programs to match the OPTION DISPLAY parameters. 
Thanks to Rich Geldreich et al for the jpg decoder https://github.com/richgel999/picojpeg

V5.07.03RC8

Enables F1 as a user programmable function key. 
Fixes bug in cursor positioning in editor. 
Fixed bug error message when an invalid flash page is selected. 
Implements support for SSD1963 parallel displays.

SSD1963_4, SSD1963_5, SSD1963_5A, SSD1963_7, SSD1963_7A, SSD1963_8. 
OPTION LCDPANEL SSD1963_n, orientation. 
It is assumed the SSD1963 is configured for 1963_PWM backlight control in which case the BACKLIGHT command will work as expected. 
Pin connections are as follows:
DB0 pin 1:GP0, 
DB0 pin 2:GP1, 
DB0 pin 4:GP2, 
DB0 pin 5:GP3, 
DB0 pin 6:GP4, 
DB0 pin 7:GP5, 
DB0 pin 9:GP6, 
DB0 pin 10:GP7, 
RS  pin 17:GP13, 
WR  pin 19:GP14, 
RD  pin 20:GP15, 
RESET pin 21:GP16, 
SSD displays support RGB666 and are much faster than SPI displays. The firmware includes full support for console mode. 
The downside is the number of pins used. 
A typical configuration could be: 
GP0-GP7 SSD1983, 
GP8-GP9 PS2 Keyboard, 
GP10-GP12 SPI for touch and SDcard, 
GP13-GP16 SSD1963, 
GP18-GP19 Touch CS and IRQ, 
GP22  SD CS, 
Leaving: 
GP17 I2C_SCL/COM1_RX/PWM0B, 
GP20 I2C_SDA/COM2_TX/PWM2A, 
GP21 I2C_SCL/COM2_RX/PWM2B, 
GP26 ADC0/SPI2_CLK/I2C2_SDA/PWM5A, 
GP27 ADC1/SPI2_OUT/I2C2_SCL/PWM5B, 
GP28 ADC2/SPI2_IN/I2C_SDA/COM1_TX/PWM6A

New command FLUSH [#]filenumber. 
This causes any outstanding writes to a file to be written to disk or waits for serial output to be complete. 
Fixed error handling for too many labels and/or functions and subroutines (max 224). 
Improved USB console receive. 
Fixes bug in GUI BITMAP when using monochrome displays. 
Fixes bug in handling of mouse escape sequences in the editor introduced in 5.07.03b1.

V5.07.03RC5

Performance tuning. 
Fixed bug in FLASH SAVE introduced in 5.07.03b1. 
Added FORMAT$() function as per CMM2
Minor change to DIR$() function to allow use with a single parameter in which case the type defaults to FILE. 
Changes to FILE command to deal with relative paths from a sub-directory. 
Fixes bug in loading F6-F9 user function definitions.

V5.07.03RC1

Various tidying up in preparation for release of V5.07.03

V5.07.03b3

Merge of VGA and standard codesets

V5.07.03b2

YOU MUST LOAD clear_flash BEFORE LOADING THIS VERSION. 
Program now runs from flash memory freeing up much more memory, following tuning there is no significant impact on performance. 
Maximum program size now 124Kbytes, Flash slots reduced from 10 to 7. 
FLASH RUN and FLASH CHAIN now execute direct from the specified flash slot without changing the main program. 
OPTION MEMORY removed as no longer relevant. 
OPTION BAUDRATE n now sets the baudrate for when a serial console is used. 
Fixes bug in using SYSTEM I2C or I2C2 for general I2C use if I2C READ does not use a string as the variable.  
Fixes bug in COM1 where buffer overrun is losing characters. 
Support for a PS2 Keyboard added. 
OPTION KEYBOARD NO_KEYBOARD/US/FR/GR/IT/BE/UK/ES. 
use level conversion between the Pico pins and the PS2 socket or run the keyboard at 3.3V. 
Connect GP8 to PS2 socket CLOCK pin via level converter.  
Connect GP9 to PS2 socket DATA pin via level converter. 
Connect VBUS to PS2 socket +5V. 
Connect GND to PS2 socket GND. 
Increases drive level for SDcard output pins (CD, MOSI and CLOCK) which MAY improve SDcard reliability when the SPI bus is loaded with additional devices (LCD and/or touch). 
Reduces maximum file name length to 63 characters but increase maximum files that can be listed with FILES command to 1000. 
Fixed memory leak caused by ctrl-C out of FILES command. 

V5.07.02b2

Fixes bug in day$(now) function. 
Fixes bug where writing text to an SPI LCD that overlapped the bottom of the screen would fail to de-assert LCD_CS. 
Fixes bug that added an extra space after a REM command each time the program was edited. 
NEW subcommands for SETTICK. 
SETTICK PAUSE, myint [,tickno] ' pauses the tick so that the interrupt is delayed but the current count is maintained. 
SETTICK RESUME, myint [,tickno] ' resumes the tick after a pause. 
NEW subcommands for FLASH. 
FLASH LIST no [,all] ' lists in full the program held in the flash slot. 

V5.07.02b1

Fixes bug in ON KEY keyno,int
Increases drive level for SDcard output pins (CD, MOSI and CLOCK)
Reduces maximum file name length to 63 characters but increase maximum files that can be listed with FILES command to 1000

V5.07.02b0
Support for 240x240 round GC9A01 display


V5.07.01b1: 
Fixed bug in epoch function. 
Increased number of WS2812 LEDs supported to 256. 
MM.INFO(pinno GPnn) implemented to give physical pin number for a given GP number. 

V5.07.01b2: 
Improvement to terminal serial output used by command stacking. 
Implements a logarithmic scale for the volume control so that PLAY VOLUME 50,50 should sound half as loud as 100,100. 
Also applies to PLAY SOUND n, ch, type, freq [,vol]

V5.07.01b3: 
Fixes bug in SETPIN pinno,IR. 
Fixes bug in parameters following subcommands/sub-functions that are enclosed in brackets e.g. POKE WORD (anything),anything or ? PEEK(WORD (anything)). 
Allows variables or string literals in the SOUND command for both the channel and sound type. The original syntax is still also allowed.

V5.07.01b4: 
YOU MUST EXECUTE OPTION RESET BEFORE LOADING THIS VERSION
Implements the option of using a standard uart as the console. 
OPTION SERIAL CONSOLE uartapin, uartbpin. 
uartapin and uartbpin can be any valid pair of rx and tx pins for either com1 (uart0) or com2( uart1). The order you specify them is not important. 
Use:
OPTION SERIAL CONSOLE DISABLE
to revert to normal the USB console

V5.07.01b5: 
Re-compile under sdk V1.3

V5.07.01b6: 
Fixes bug in GPS receipt where the first read of GPS(DATE) may give an incorrect answer. 
Fixes bug in reporting the line of an error when goto/gosub to a line number is used. 
Fixes bug where OPTION SERIAL CONSOLE DISABLE doesn't work after reboot. 
Fixes filenames so that linux which doesn't understand that "The" and "the" are the same word will compile. 

V5.07.01b7
Implements LIST ALL fname$. 
Fixes bug in GUI SWITCH. 
Restores original program if AUTOSAVE is terminated with Ctrl-C or XMODEM R terminates with an error.

V5.07.01b8
Clears variable memory after Ctrl-C out of Autosave. 
Stops creation of spurious "Reset" USB device.  

V5.07.01b9
Further rework of GUI SWITCH. 
Rename FileIO.c for Linux. 
AUTOSAVE "file" now reports a "Syntax Error" rather than "Unknown command". 
EDIT "file" reporting an error rather than just ignoring the argument.

V5.07.01 release. 
Incorporate changes above + force reset after OPTION AUDIO

7-dec-2021: Fixed filename cases for Linux. No functional code changes 

***********************************************************************************************************************

PicoMite MMBasic


<COPYRIGHT HOLDERS>  Geoff Graham, Peter Mather
Copyright (c) 2021, <COPYRIGHT HOLDERS> All rights reserved.
    
Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met: 
1.	Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
2.	Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer
    in the documentation and/or other materials provided with the distribution.
3.	The name MMBasic be used when referring to the interpreter in any documentation and promotional material and the original copyright message be displayed 
    on the console at startup (additional copyright messages may be added).
4.	All advertising materials mentioning features or use of this software must display the following acknowledgement: This product includes software developed 
    by Geoff Graham, Peter Mather.
5.	Neither the name of the <copyright holder> nor the names of its contributors may be used to endorse or promote products derived from this software 
    without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY Geoff Graham, Peter Mather AS IS AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES
OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL <COPYRIGHT HOLDERS> BE LIABLE FOR ANY DIRECT, 
INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; 
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, 
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE. 

************************************************************************************************************************
