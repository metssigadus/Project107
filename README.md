# Project107
My second shield for Arduino

Project107 shall measure temperatures around the house (some 15+ of DS18b20 sensors) and report the results to some central system. I want to push the limits, play with tens of sensors and cables of various quality.

Some design ideas:

a) it is not easy for an Arduino Uno to run a meaningful TCP stack together with the main functionality. The alternatives are: Arduino Mega or, this time, dedicating another host (Arduino or even Raspberry Pi) for the network connectivity. The current idea is to bind two controllers via I2C. One shall run the main task, another does the network connectivity. I will probably not start to develop the I2C interhost code until the main functionality gets polished.

b) It is not yet clear for me, whether a RTC is needed on the edge or not.
This is why it is there. I am using a "DS3231 for Pi" as in [previous experiments](https://github.com/metssigadus/ArduinoShieldMonster)
b/c this is what I have in posession. I desoldered the original header and replaced it with another one
so that the contacts of the backup battery are visible and the pin designations too.

c) This time I use an OLED vs the 14 pin 2x16 char LCD. The OLED model is a 0.96" standard product with 7 contacts (GND, VCC, D0, D1, RES, DC, CS). Both SPI and I2C are supported (resistors on the back side of the PCB need resoldering for I2C). The idea is that a small and almost integrated screen makes debugging easier. We'll see.

d) Three headers for the DS18b20 sensors are provided to test several sensors concurrently (and some extra via the intermediate PCBs). The final count of the sensors is yet unknown, as well as the lenght of the cables, thus the idea is to experiment with simultaneous instances of the Wire library.

e) The idea is to raise the usability and avoid the separate programs for the discovery and measurement. An automatic discovery is preferred.

Here is the pinout plan for the project:

.

This is the planned functionality:

1. The host obtains the network time from the paired gost and sets data/time on the RTC.
2. The host enumerates all temperature sensors discovered (could also be a boot time option)
3. In a never-ending cycle, the host polls all sensors and sends the results to the paired host (the one with the Ethernet connectivity).
4. Current measurements are shown on the OLED. If the name/location of the sensor is known (probably located in EEPROM), it is shown instead of the sensor UID.

Some obsessive limitations apply like in the previous project:
 - no WiFi! Only wired connectivity (I2C, Ethernet)
 - hosts are managed automatically, IP addresses do not reside in code.
