# RGBWW32C3
## a new, smaller RGBWW controller built around the ESP32C3M0 module

This is based on the original design by Patrick Jahns but modified to fit into a much smaller footprint and use the much more capable ESP32 Platform - in this case the C3 Wroom 02 module.

The ESP32C3 is a risc-v based implementation of the ESP32 µC family, providing much the same peripherals as the xtensa based chips, but at a much lower price point.

The design is straight forward:

![ESP32C3 module schemantic](https://github.com/pljakobs/RGBWW32C3/blob/Ureg/MCU.PNG)

According to the [ESP32C3Wroom02 Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-c3-wroom-02_datasheet_en.pdf) the strapping pins are
|Pin|Default|SPI Boot (normal)|Download Boot (prg)|
|IO2|N/A|HIGH|HIGH|
|IO8|N/A|ignore|HIGH|
|IO9|weak pullup|HIGH|LOW|

Therefore, IO2 and IO8 need to be pulled high, IO9 should be pulled high for normal operation and pulled to GND for programming / Booting
That results in the following strapping / switching schematic:

![ESP32C3 pin strapping](https://raw.githubusercontent.com/pljakobs/RGBWW32C3/Ureg/StrappingAndPullup.PNG)

The output stage has, from a schematic point of view, mostly remained unchanged (as if there was a lot to change on driving N-Channel MOSFETS). The actual MOSFETs however have been replaced with much smaller packaged types. Currently, I'm looking at Alpha&Omega devices AO4808 or better (there are some in the family that go up to 11.5A Drain current per channel and 30V VDS which would make the mini a fully featured replacement for the original controller, although the TO92 IRFZ44N theoretically could drive 49A of drain source current, but the design was never built to provide that).

![Lightinator Mini output stage](https://raw.githubusercontent.com/pljakobs/RGBWW32C3/Ureg/Outputs.PNG)

One of the biggest changes from the original design by Patrick and the last Version (green boards) was that I ditched the 3rd party DC/DC modules in favour of a power converter on board, designed with [TI Power Designer Webbench](https://webench.ti.com/power-designer/switching-regulator?powerSupply=0), a tool that I have come to value a lot. 
This time, the requirement was to make the power converter smaller but no less powerful (still capable of pushing up to 600mA at 3.3V but optimized for 200mA)

![the switching power supply on board](https://raw.githubusercontent.com/pljakobs/RGBWW32C3/Ureg/PowerSupply.PNG)

The last bit of the schematic is to programming port. This device will not be easily programmed "at home" as it should, normally, come without a header installed (which just now makes me think why I have a PRG button on it after all, I should remove that). The programming port is six pads on the bottom of the board, fit to mount a 1/20" SMD pin header that matches the six pin cable of the [esp_prog](https://docs.espressif.com/projects/espressif-esp-iot-solution/en/latest/hw-reference/ESP-Prog_guide.html). The intention is to have a printed jig where the boards can be initially programmed, everything after that should be done OTA

![programming port](https://raw.githubusercontent.com/pljakobs/RGBWW32C3/Ureg/ProgrammingHeader.PNG)

The routed board in Eagle 7.7 (yes, I'm still using it when things need to go fast) now looks like this:

![routed board](https://raw.githubusercontent.com/pljakobs/RGBWW32C3/Ureg/board.PNG)


