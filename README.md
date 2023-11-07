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

