# MRR ESPA
## A basic 3D printer control board based on ESP32

This is a bare-minimum 3D printer control board based on the ESP32 microcontroller, which comes with built-in WiFi and BlueTooth.

Features:
- Able to use up to 4 stepper drivers: X, Y, Z, and E0
- 12V to 24V input power supply
- Separate power supply for the heat bed
- X, Y, and Z min endstops
- Allows the use of a Z-axis probe, such as an inductive sensor, running on the input supply voltage (12V to 24V)
- AUX1 connector for use with an external host, such as the closed-source MKS TFT32 (**Untested!!**)
- Physical size of 99.5mm by 90.5mm. Mounting holes are 3.5mm in diameter, with centers 4mm from the edges.
- "RESET" button can be used to reset the board.

# Firmware

Marlin 2.0 has added support for ESP32. This board was based on the initial pins definition used. <br>
In addition, [Luc](https://github.com/luc-github) has been working on a Marlin fork which incorporates part of his ESP3D webUI into Marlin itself.

## Flashing firmware

- You should be able to flash firmware through the USB port. Depending on your operating system, you may need to install drivers for the CH340. Please use Google or another search engine to find the right CH340 driver for your operating system.
- To flash firmware by directly connecting to TX/RX pins, you will need to connect IO0 to GND, then reboot (press "BOOT" button) to enter flash mode. After flashing the firmware, disconnect IO0 from GND, and press "BOOT" button again to reboot into normal mode.
- Use a baud rate of 115200.
- Once the firmware has been flashed for the first time, further updates can be flashed via OTA.

# Jumpers

- VUSB_EN: Leave open for normal operation; only short (close) this jumper if you need to power the board via USB. Note: Powering the board with both PSU and USB may cause unstable operation during heating.
- PWR_SEL: Connect the middle pin PWR_SEL to pin labeled "12V" if using 12V; connect PWR_SEL to pin labeled "24V" if using 24V.
- Stepper driver jumpers:
  - MS1: VIO to set high; SPI if using TMC SPI mode
  - MS2: VIO to set high; SPI if using TMC SPI mode
  - MS3: VIO to set high; SPI if using TMC SPI mode
  - RST: SLP if using A4988/DRV8825; SPI if using TMC SPI mode
  - Note: TMC SPI mode for the ESP32 is still under development. MRR ESPA is hardware-ready in anticipation of proper software support.

# Pins

See separate PIN_DEF.txt file.

# License
Released under CERN Open Hardware Licence v1.2. See LICENSE.txt for details.

## Warning

- This product uses high currents which may cause fires. Use with caution.
- Do not use input voltages higher than 24V. Input current must not exceed 15A.
- Do not attempt to use more than 15A for the heated bed. While the MOSFET is capable of such currents, the connectors are not. If you need more than 15A, use an external MOSFET. In any case, it is highly recommended to use an external MOSFET for the heated bed to avoid drawing high currents through this product.
- If using a current of more than 10A for the heated bed, it is highly recommended to provide active cooling for the board as the MOSFET and adjacent areas can get very hot due to the high current.
- The underside of the board has PCB traces which are exposed for better cooling. These traces may be carrying high currents and can get very hot. DO NOT touch them, and DO NOT allow these traces to come into contact with metal or other conductive materials.
- Do not attempt to use more than 5A for the hot end. While the MOSFET is capable of such currents, the connectors are not. If you need more than 5A, use an external MOSFET.
- Do not reverse polarity.
- The breakout and endstop pins are rated for 3.3V, which is the voltage the ESP32 operates at. Attempting to feed inputs above 3.3V to these pins may damage the board.
- It is recommended to power the board only via USB (i.e. turn off PSU power supply) when flashing the board.

## Disclaimer

This product is provided as-is. While care has been taken to design a safe product, it is the user's responsibility to make sure precautions are taken to prevent damage or injury when using this product. This includes adhering to all warnings provided. By using this product, the user accepts responsibility to bear all liabilities from any damage or injury arising from the use of this product.
