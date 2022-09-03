---
title: G2Core Motion Controller
author: andrelimzs
date: 2020-06-19T16:21:33+00:00
url: /g2core-motion-controller/
neve_meta_enable_content_width:
  - off
categories:
  - CNC Machine
showToc: true
math: true
draft: false
---
[G2Core][1] is a motion control system, similar to [grbl][2], [mach3/4][3], [linuxCNC][4], [etc][5]. It runs on a 32-bit ARM Cortex M3 Arduino Due, which means plenty of power for additional features.

{{< figure src="https://i0.wp.com/www.robgray.com/temp/Due-pinout-WEB.png?w=1200" width=640 align="center" caption="[Due Pinout](https://forum.arduino.cc/index.php?topic=132130.0)" >}}

## Configuration in G2core<figure class="wp-block-table">

| Term          | Meaning                                             |
| ------------- | --------------------------------------------------- |
| machine       | BOARD + SETTINGS_FILE                               |
| CONFIG        | Specifies default BOARD/SETTINGS_FILE for a machine |
| BOARD         | Specify board type and revision                     |
| BASE_BOARD    | Specify underlying hardware platform                |
| SETTINGS_FILE | Specify default settings                            |</figure> 

<hr class="wp-block-separator" />

## Enable Motors and Axes

1. Create a new SETTINGS_FILE, `settings_defaultdue.h`

2. Enable motors `M1_POWER_MODE: MOTOR_ALWAYS_POWERED`

3. Set motor power level `M1_POWER_LEVEL: 1.0`

4. Enable axes `X_AXIS_MODE: AXIS_STANDARD`

## Build and Flash

1. Add a new config in `boards.mk` which will use the new `settings_defaultdue.h`

```
ifeq ("&#36;(CONFIG)","DefualtDue")
    ifeq ("&#36;(BOARD)","NONE")
        BOARD=g2v9k
    endif
    SETTINGS_FILE="settings_defaultdue.h"
endif
````
2. `Clean` then `Build Solution`

3. [Flash][6] the board using `arduino-flash-tools`, which in my case is

```
mode COM3 BAUD=1200
bossac.exe --port=COM3 -U false -e -w -v -b C:\Github\g2\g2core\bin\DefualtDue-gShield\g2core.bin -R
```

## Connect

1. Switch the USB cable to the **other port** on the board, and connect to CNCjs/UGS/Chilipeppr


[1]: https://github.com/synthetos/g2
[2]: https://github.com/grbl/grbl
[3]: https://www.machsupport.com/
[4]: https://linuxcnc.org/
[5]: https://all3dp.com/2/cnc-router-software-find-the-tool-for-you/
[6]: https://github.com/synthetos/g2/wiki/Flashing-g2core-with-Windows