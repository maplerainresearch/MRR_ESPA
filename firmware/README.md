This directory contains ZIP files of firmware which works with the MRR ESP 3DP board.

MRR ESP 3DP board works with the [Marlin fork](https://github.com/luc-github/Marlin) currently under development by [Luc](https://github.com/luc-github). This fork provides WiFi support as well as a web UI based on Luc's [ESP3D](https://github.com/luc-github/ESP3D). Snapshots of the repo will be posted here. You can download the ZIP file, unpack it, and edit the Configuration.h and Configuration_adv.h to suit the size of your printer, drivers, etc. If you are using TMC2130 drivers in SPI mode, you will also need to edit pins_ESP32.h to specify the CS pins for the drivers. The fork has been tested to compile under Arduino IDE 1.8.5 using Arduino-ESP32 core v1.0.0 (stable release).

If you are using an external host such as the MKS TFT32 or Octoprint, you can also use vanilla Marlin 2.0, which has ESP32 support. However, this does not come with WiFi nor a web UI.

When flashing firmware, use a baud rate of 115200.

Default admin name and password:<br>
User: admin<br>
Password: admin<br>
<br>
Default general user name and password:<br>
User: user<br>
Password: user<br>

Default mDNS name: **marlinesp**, which can be accessed on web browsers by going to `http://marlinesp.local/`

For more about the web UI, see Luc's [ESP3D-WEBUI](https://github.com/luc-github/ESP3D-WEBUI) repository.

# Instructions for flashing firmware

The firmware is built on the Arduino framework with the ESP32 core from Espressif. If you have experience building and flashing firmware using the Arduino-ESP32 core, you probably do not need this guide and can easily flash the firmware using your usual method. Otherwise, you can follow the following to flash the firmware.

1. First, you need Arduino IDE installed on your computer (Windows, Mac, or Linux). Go to [Arduino IDE's download page](https://www.arduino.cc/en/main/software) to download the version of the IDE for your computer. Arduino IDE's installation guide can be found [here](https://www.arduino.cc/en/Guide/HomePage).

2. Once Arduino IDE has been installed, follow the instructions [here](https://github.com/espressif/arduino-esp32/blob/master/docs/arduino-ide/boards_manager.md) to install the ESP32 core for Arduino IDE. It is recommended to install the stable release.

3. Download or clone the [Marlin fork](https://github.com/luc-github/Marlin) currently under development by [Luc](https://github.com/luc-github).

4. Connect the board to your computer via USB cable.

5. Launch Arduino IDE, and open the sketch for the firmware. This file is named `Marlin.ino` and can be found in the downloaded firmware's `Marlin` subdirectory.

6. In the IDE's menu, go to `Tools`, then `Board`, and choose `ESP32 Dev Module` which is the generic ESP32 module. Then, under `Tools`, `Port`, choose the COM port that your board is connected to. For upload speed, go to `Tools`, `Upload speed`, and set to `115200`.

7. Edit the `Configuration.h` and `Configuration_adv.h` files to define the size of your printer, endstops, and other settings. See the section below for some basic settings which need to be changed.

8. Click on the "Upload" icon of the IDE to compile the firmware and flash it onto the board. This will take quite a while.

9. Once the firmware has been flashed onto the board, you can open the IDE's serial monitor (by pressing Ctrl+Shift+M, or from the menu via `Tools`, then `Serial monitor`) to make sure the firmware has been flashed properly. Enter `M503` into the serial monitor and you should get a response from the board giving the current settings such as steps/mm and PID values.

10. Via the serial monitor, you can configure your WiFi settings. See the section below on how to do so. You can also set up WiFi using a web browser (see relevant section below).

# Instructions for setting up WiFi via terminal

Once the firmware has been flashed for the first time, there is a need to do an initial setup for the WiFi settings.<br>
In Arduino IDE, open up the serial monitor to connect to the board. You can also use any other terminal program to open the COM port that the board is connected to. Then, type in the following commands 1 to 3 to set up the board in station mode (connect to an existing WiFi router). To set up in AP mode, use commands 1, 4, and 5.

1. `M586 P0 S1 R80`<br>
Enable HTTP on port 80.<br>
Replace `R80` with `Rxx` (xx is the port number) to run the HTTP server on another port.

2. `M587 S"abcd" P"12345" I192.168.0.25`<br>
WiFi settings; the above will connect to a SSID of `abcd` with password `12345` and attempt to use a static address of `192.168.0.25` (note: the quotation marks are necessary for SSID and password). Change `abcd`, `12345`, and `192.168.0.25` to your own values. `I192.168.0.25` can be left out if a static address is not needed.

3. `M588 S1 P1`<br>
This will start up WiFi in station mode, using the settings set by `M587`. The IP address of the webserver will be displayed in the terminal. Use this IP address to connect to the webserver. For example, going to `http://192.168.0.25:80/` on your web browser following the above example.

4. `M589 S"ESP32" P"12345" I192.168.10.1`<br>
WiFi settings; the above will create an access point with SSID of `ESP32` and password `12345`, and the webserver will be on IP address `192.168.10.1` (note: the quotation marks are necessary for SSID and password). WPA2 security will be used by default.

5. `M588 S2 P1`<br>
This will start up WiFi in AP mode, using the settings set by `M589`.

# Instructions for setting up WiFi via web browser

To be completed.

# Uploading webUI

When you first go to the webUI address (if you have mDNS, this should be http://marlinesp.local/ by default), you will be asked to upload the webUI files. Click on "Choose files", select `index.html.gz`, then upload. Then, refresh your browser page; the webUI should load. A copy of `index.html.gz` can be found in the firmware directory, under `<firmware directory>/Marlin/data/`.<br>

If you have the [Arduino ESP32 filesystem uploader](https://github.com/me-no-dev/arduino-esp32fs-plugin), you can also choose to upload the required files from Arduino IDE. After flashing the firmware, go to `Tools`, then choose `ESP32 Sketch Data Upload`.

# Common settings which need to be changed in Marlin

The following are some common settings which need to be changed in Marlin to configure the firmware for your specific printer.<br>

In `Configuration.h`:
To be completed.<br>

In `Configuration_adv.h`:
To be completed.<br>

In `Marlin\src\pins\pins_ESP32.h`:
The following lines need to be added in order for microSD card to work.<br>
```
#define MOSI_PIN           23
#define MISO_PIN           19
#define SCK_PIN            18
#define SS_PIN              5
#define SDSS                5
```

# Macros

The webUI (based on ESP3D) allows user-defined macros to be created. This allows "buttons" for custom commands (GCODE), such as buttons for babystepping.<br>

For example, to add a command to move to babystep Z by 0.01 downwards:
1. Create a text file with the gcode. In this case the gcode would be "M290 Z-0.01". Save it with a ".g" extension. In this case, it can be named `babyzdn.g`.
2. Upload that file to the ESP3D File System (Not to the SD card); this is the SPIFFS file system, where `index.html.gz` is stored.
3. Click on the macro editor button in the controls panel. click the plus icon to add a macro. Give it a name, select a color, set the target as ESP and enter the path to the gcode file you uploaded.
4. Click Save
(Example based on [GRBL_ESP32 wiki](https://github.com/bdring/Grbl_Esp32/wiki/ESP3D-Web-UI-for-Grbl_ESP32); GRBL_ESP32 is a fork of GRBL with luc-github's ESP3D webUI.)<br>
