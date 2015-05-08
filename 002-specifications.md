---
layout: page
title: Specifications
permalink: /specifications/
---

## Robot Features

The current works-like prototype has the following features:

- Battery-operated
- Particle count sensor
- LCD
- 802.11n WiFi for wireless communication
- Web interface
- USB interface for setup and debugging

## Robot Specifications

|                                           | Minimum | Nominal | Maximum | Notes
|-------------------------------------------|:-------:|:-------:|:-------:|-------
| Battery life (hours)                      | 1.4     | 2.8     | 6.0     | 6
| Detectable particle size (um)             | 1.0     |         |         | 3
| Detectable ISO classes                    | 3       |         | 9       | 3
| Detectable particle count (particles/m^3) | 0       |         | 98M     | 3
| Sensor measurement period (seconds)       |         | 30      |         | 3
| Particle count % noise                    |         | 25      |         | 3
| Environment size                          |         |         | 120 x 120 cm | 1
| Speed                                     |  0      | TBD     | TBD     | 2
| Distance from two nearest landmarks (cm)  | 15      |         | 600     | 5
| LIDAR range (cm)                          | 15      |         | 600     | 5
| Operating temperature (degC)              | 0       |         | 45      | 4

Notes:

1. The environment size limitation was arbitrarily chosen to be the size of the current test
environment, and is not due to any software or hardware limitations.
2. The localization algorithm requires that the robot is within sight of two landmarks.
These limits are determined by the range of the LIDAR.
3. Particle count specifications are determined by the air quality sensor used, and is not a
limitation of any other hardware or software. Determining the ISO class of a
cleanroom is not possible with a single sensor; this specification is the range of classes
that allow for particles of 1 um or larger which is the range detectable by the
current sensor. Noise was determined experimentally.
4. The operating temperature is determined by the limitations of all internal components.
The most restrictive of these is the air quality sensor.
5. LIDAR range is determined by the LIDAR used.
6. Battery life calculates are given in the Mechanics & Electronics section.

## Web Interface Requirements

The web interface has the following requirements for proper functionality:

- Windows, Linux, or Mac OS X
- Google Chrome 35+
- Node.js 0.12
- WiFi (802.11n with WPA2 security recommended)
- PlayStation 3 controller recommended for manual control
- 1280x720 or greater resolution

The USB debugging interface requires Ubuntu 14.04 or newer. It is not compatible with Mac OS X and has limited functionality in Windows.

