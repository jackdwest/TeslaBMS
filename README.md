Forked from BobbyBleacher.  In order to compile you will need the following libraries:

 - due_can (https://github.com/collin80/due_can)
 - due_wire (https://github.com/collin80/due_wire)
 - Wire_EEPROM (https://github.com/collin80/Wire_EEPROM)
 - MemoryFree (https://github.com/McNeight/MemoryFree)
 
Addition of local LCD display using I2C to display pack voltage and low/high or max-min cell voltages:

The LCD will be connected by I2C to the Due.  Power the LCD shield with 5v from the Due and connect the shield to Due SDA1 and SCL1.  This is working on my setup but I have ordered a level shifter to protect the Due.

Addition of an Adafruit RGBLCD will require the following libraries:
 - Adafruit_RGB_LCD_Shield_Library (https://github.com/adafruit/Adafruit-RGB-LCD-Shield-Library.git)
   on line 28 of .cpp change #include <Wire.h> to #include <due_wire.h>
 - Adafruit MCP23017 library (https://github.com/adafruit/Adafruit-MCP23017-Arduino-Library.git)
   again, change the #include <Wire.h> to #include <due_wire.h> this time in the .h file

Planned changes include serial communication with an Electric Imp for internet connectivity and browser display of data.  

My take on Collin's BMS. Changes listed below:

- Now balances to the lowest cell between two packs connected together in series.
- CSV output for those that might use it.
- Cells balance for 30s and then update again, turning the balancers off momentarily to get accurate readings.
- Baud set 612500 (I have older Tesla packs)
 
Known Bugs:

- System hangs once a day and must be restarted. Reason unknown.  SOLVED
- Minor bug with display of data to Serial Console. SOLVED


Arduino compatible project to interface with the BMS slave 
board on Tesla Model S modules.

The modules are daisy-chained together with a TTL interface.
The interface uses a Molex 15-97-5101 connector and runs at
612500 baud. This can be a difficult baud rate to match with
arduino compatible processors. The Arduino Due and Teensy
3.5/3.6 boards are confirmed to be able to generate a suitably
close baud rate. The factory wiring to each module is comprised
of two sets of 5 differently colored wires:

* Red = 5V input to the module
* Green = Gnd for power and signal
* Gray = Fault output
* Yellow = UART Wire
* Blue = UART Wire

The fault output is active low. Use your own pull up to the fault line and if the line is pulled low then a fault has occurred.

Here is a PDF that explains how the wiring between modules and the master board is supposed to be:
https://cdn.hackaday.io/files/10098432032832/wiring.pdf
