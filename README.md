# BITLINK WIFI MODEM

![BITLINK WIFI](/pics/BITLINKWIFICASE3.jpg)

## Introduction
The BITLINK WIFI MODEM is a ESP8266 based RS-232 modem that uses Hayes AT style commands for connecting computers to a telnet BBS.
Designed to sit on a desk seperate from a computer, I wanted this to look reminiscent of an old US Robotics Courier modem with red flashing lights
and a sleek black case. Also since its supported by the firmware I've added full flow control(RTS,CTS,DSR,DTR,CD,RI) so the modem can be used for
hosting BBS servers.

The firmware used is a fork of Zimodem by [Bo Zimmerman](https://github.com/bozimmerman/Zimodem).

## Hardware Features

### PCB
The modem PCB is layed out with a DE-9F connector and wired as DCE. The modem uses full flow control and by default can auto hangup on DTR drop
or wake on Ring. Status LED's are bufferd using PNP transistors from the 3.3V signals of the MAX chip to get the most brightness.

To power the modem, you can use any center positive 2.1mm X 5.5mm DC power supply ranging from 5 to 12 Volts with at least 500mA current.
5V is preferred to keep the 3.3V regulator as cool as possible. A ON-OFF slide switch is added for convenience.

### Enclosure
To have this modem look like a USR Courier I needed a case with an angular look, so I chose a Serpac A20 enclosure with a clear end panel.
I designed cut templates that you just tape and drill to the case for the IO ports and switch. 

## Firmware Flashing
The firmware provided in this repo has been pre-compiled and saved as a .bin binary. For flashing you will need a modern computer
with either a real COM port or a USB to RS-232 adapter, a DE-9 straight thru cable, a PCB jumper shunt or jumper wire, and
firmware flashing software. I used nodemcu-flasher for windows due to ease of use.

### nodemcu-flasher instructions
With the Modem OFF and connected to a modern computer, Jumper pin J3 labelled FLASH, then turn ON the modem. With the nodemcu software 
running, select the .bin file you would like to use in the config tab. 

In the advance settings tab, select 
* Baudrate: 57600
* Flash size: 4MByte
* Flash speed: 40MHz
* SPI Mode: DIO

Then go to the operation tab and select the correct COM port then click Flash. The nodemcu flasher should show the flashing status
and you can see the indicator LED's on the modem flicker. When it finishes, remove the J3 jumper and press the reset button or power cycle the modem.
It may take a minute for the Zimodem firmware to configure the SPIFFS.

To test, close the flasher sofware and open a terminal emulator like PuTTY at the default baudrate. If you see the Zimodem 
initialization your all set!

## First time setup
Default serial baudrate is 1200bps, 8N1.
Upon first initialization type AT+CONFIG to connect to a wireless router, set the baud rate and flow control.

More detailed instructions are at the [Zimodem repo](/firmware/ZIMODEM_README.txt).

## Parts List
Most parts are SMD and can be ordered from Mouser or Digikey.

| Reference                  | Description                                              |
|----------------------------|----------------------------------------------------------|
| R1, R2, R3, R4, R5         | 10K Ohm, 0805 SMD, Resistor                              |
| R6, R7, R8, R9, R10, R11   | 47 Ohm, 0805 SMD, Resistor                               |
| C1                         | 100uF, 16V, 6.3 dia SMD, Elec Capacitor                  |
| C2                         | 22uF, 10V, Kemet Case B (1311) SMD, Tant Capacitor       |
| C3, C4, C5, C6, C7, C8, C9 | 0.1uF, 50V, 0805 SMD, MLCC Capacitor                     |
| Q1, Q2, Q3, Q4, Q5, Q6     | MMBT3906, SOT-23-3 SMD, PNP Bipolar Transistor           |
| D1                         | SS12, SMA (DO-214AC) SMD, 1A Schottky Barrior Diode      |
| D2, D3, D4, D5, D6, D7     | 5mm Red Diffused LED, Right Angle THT                    |
| U1                         | NCV1117DT33T, TO-252-3 (DPAK) SMD, 3.3V 1A LDO REGULATOR |
| U2                         | MAX3237EAI+T, SSOP-28 SMD, RS-232 Interface IC           |
| U3                         | ESP-12F(ESP8266), SMD-22, Integrated WiFi Module         |
| SW1A                       | Slide Switch, EG1224 Right Angle THT                     |
| SW2A                       | Tactile Switch, 6x5mm THT                                |
| J1                         | DC Barrel Jack, 2.1x5.5mm Right Angle THT                |
| J3                         | 1x2 Pin Header, 2.54mm Pitch THT                         |
| J5                         | 9 Pin D-Sub Connector, DE-9F THT                         |
| MISC                       | Printed Circuit Board, 2 Layer, 1oz Copper               |
| MISC                       | SERPAC Case, A20 Black                                   |
| MISC                       | SERPAC Clear End Panel, 2003-CL                          |

NOTE: R12, R13, R14, J2 and J4 are not used and should not be populated.

## Known Issues
* Zimodem firmware sometimes crashes when there is a lot of serial data
	* Workaround: Use RTS/CTS flow control as default
* Don't update firmware over the air as it will use Bo Zimmerman's unmodified code and brick the modem
 
## Changes
* Revision 1.0
	* Initial version
* Revision 1.1
	* Fixed error on RS232 pinout
	* Shifted some components around
	* Removed unnecessary resistors for LED's

## References and Acknowledgements
* [bozimmerman's Zimodem firmware](https://github.com/bozimmerman/Zimodem)
* [nodemcu firmware flasher for Windows](https://github.com/nodemcu/nodemcu-flasher)
* [Sub-Etha Software for the inspiration and help](https://subethasoftware.com/category/rs232-to-internet/)
