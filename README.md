# sick_scan2
## Table of contents

- [Supported Hardware](#supported-hardware)
- [Start node](#start-node)
- [Bugs and feature requests](#bugs-and-feature-requests)
- [Creators](#creators)

This stack provides a ROS2 driver for the SICK TiM series of laser scanners 
mentioned in the following list.


## Supported Hardware

This driver should work with all of the following products.

ROS Device Driver for Sick Laserscanner - supported scanner types: 


| **device name**    |  **part no.**                                                                                                                | **description**                                | **tested?**     |
|--------------------|------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------|:---------------:|
| TiM551             | [1060445](https://www.sick.com/media/docs/9/29/229/Operating_instructions_TiM55x_TiM56x_TiM57x_de_IM0051229.PDF)                 | 1 layer max. range: 10m, ang. resol. 1.00[deg] | ✔ [stable]|
|                    |                                                                                                                                  | Scan-Rate: 15 Hz   |                 |
| TiM561             | [1071419](https://www.sick.com/media/docs/9/29/229/Operating_instructions_TiM55x_TiM56x_TiM57x_de_IM0051229.PDF)                 | 1 layer max. range: 10m, ang. resol. 0.33 [deg]| ✔ [stable]|
|                    |                                                                                                                                  | Scan-Rate: 15 Hz   |                 |
| TiM571             | [1079742](https://www.sick.com/media/docs/9/29/229/Operating_instructions_TiM55x_TiM56x_TiM57x_de_IM0051229.PDF)                 | 1 layer max. range: 25m, ang. resol. 0.33 [deg]| ✔ [stable]|
|                    |                                                                                                                                  | Scan-Rate: 15 Hz   |                 |
|                    |                                                                                                   | Opening angle: +/- 50 [deg]   |                 |

##  Start Node

Use the following command to start ROS node:

For TiM551, TiM561, TiM571:
ros2 launch sick_scan2 sick_tim_5xx.launch.py

## Sopas mode
This driver supports both COLA-B (binary) and COLA-A (ASCII) communication with the laser scanner. Binary mode is activated by default. Since this mode generates less network traffic.
If the communication mode set in the scanner memory is different from that used by the driver, the scanner's communication mode is changed. This requires a restart of the TCP-IP connection, which can extend the start time by up to 30 seconds. 
There are two ways to prevent this:
1. [Recommended] Set the communication mode with the SOPAS ET software to binary and save this setting in the scanner's EEPROM.
2. Use the parameter "use_binary_protocol" to overwrite the default settings of the driver.
3. Setting "use_binary_protocol" to "False" activates COLA-A and disables COLA-B (default)


## Bugs and feature requests

- Stability issues: Driver is experimental and brand new 
- Sopas protocol mapping:
-- All scanners: COLA-B (Binary)
- Software should be further tested, documented and beautified

## Troubleshooting 

1. Check Scanner IP in the launch file. 
2. Check Ethernet connection to scanner with netcat e.g. ```nc -z -v -w5 $SCANNERIPADDRESS 2112```.
   For further details about setting up the correct ip settings see [IP configuration](doc/ipconfig/ipconfig.md) 
3. View node startup output wether the IP connection could be established 
4. Check the scanner status using the LEDs on the device. The LED codes are described in the above mentioned operation manuals.
5. Further testing and troubleshooting informations can found in the file test/readme_testplan.txt
6. If you stop the scanner in your debugging IDE or by other hard interruption (like Ctrl-C), you must wait until 60 sec. before
   the scanner is up and running again. During this time the MRS6124 reconnects twice. 
   If you do not wait this waiting time you could see one of the following messages:
   * TCP connection error
   * Error-Message 0x0d
7. Amplitude values in rviz: If you see only one color in rviz try the following:
   Set the min/max-Range of intensity display in the range [0...200] and switch on the intensity flag in the lauch file  
8. In case of network problems check your own ip address and the ip address of your laser scanner (by using SOPAS ET).
   * List of own IP-addresses: ifconfig|grep "inet addr"
   * Try to ping scanner ip address (used in launch file) 
9. If the driver stops during init phase please stop the driver with ctrl-c and restart (could be caused due to protocol ASCII/Binary cola-dialect).
   
## SUPPORT
 
* In case of technical support please open a new issue. For optimal support, add the following information to your request:
 1. Scanner model name,
 2. Ros node startup log,
 3. Sopas file of your scanner configuration.
  The instructions at http://sickusablog.com/create-and-download-a-sopas-file/ show how to create the Sopas file.
* In case of application support please use [https://supportportal.sick.com ](https://supportportal.sick.com).


## Installation

In the following instructions, replace `<rosdistro>` with the name of your ROS distro (e.g., `bouncy`).

### From source

```bash
source /opt/ros/$ROS_DISTRO/setup.bash
mkdir -p ~/sick_scan_ws/src/
cd ~/sick_scan_ws/src/
git clone --single-branch git://github.com/SICKAG/sick_scan2.git
cd ..
colcon build --symlink-install
source ~/sick_scan_ws/install/setup.bash
```

## Quick Start

```bash
cd ~/sick_scan_ws
ros2 launch sick_scan2 sick_tim_5xx.launch.py
rviz2 ./install/sick_scan2/share/sick_scan2/launch/rviz/tim_5xx.rviz

```

## Keywords

TiM5xx 
TiM551 
TiM561 
TiM571 


## Creators

**Michael Lehning**

- <http://www.lehning.de>

on behalf of SICK AG 

- <http://www.sick.com>

------------------------------------------------------------------------

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f1/Logo_SICK_AG_2009.svg/1200px-Logo_SICK_AG_2009.svg.png" width="420">

![Lehning Logo](http://www.lehning.de/style/banner.jpg "LEHNING Logo")

