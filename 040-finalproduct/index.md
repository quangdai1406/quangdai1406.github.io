---
layout: page
title: Ideal Final Product
permalink: /finalproduct/
---

This page is currently under construction.

This page describes the additional work that must be done in order to have a completely functional final product, capable of being used in a cleanroom.

## Chassis
The 3D printed robot is a non-functional model of 76.2mm. This gives users a surface conceptual understanding of the ideal robot. However, the dimensions of an ideal prototype chassis are 2ft. by 1.5ft. by 2.5ft. The size of the body was determined to ensure visibility, based on the safety concerns of cleanroom experts. There will be a 2ft. deep rectangular cavity that begins at the top of the chassis. It will house the extendable arm. It is also reasonably sized for simple storage purposes.

## IR Sensors
The infrared sensors will be integrated into the system to support the mapping and localization capabilities of the robot. They will provide the robot with short proximity readings of walls and obstacles of the given environment.

## AQ Sensor
The robot must be capable of using different sensors, selected based on price and cleanroom rating. Ideally the robot would detect which sensor is connected and configure itself properly. Our online research suggests that for a class 100 cleanroom, that will be between $300-500 for a 0.3um sensor; existing solutions for cleanrooms cost over $1k. For a class 1 cleanroom, that will be between $5k-10k for a 0.1um sensor; existing solutions for cleanrooms cost approximately $20k. These prices are lower than existing solutions as they are for just the sensor unit, not a complete product with casing, power supply, display, etc.

## Webcam
The webcam stream is an optional feature. It will provide users with a live video stream of the robot’s whereabouts. It is supplemental to the LIDAR and IR sensors map data. It was deemed optional due to the neutral response of interviewed cleanroom personnel.

## Alarms / Lights
In efforts to further address safety concerns of experts the ideal prototype will wear yellow cased flashing alert LEDs on its exterior for a pronounced presence. There will be two on each side (left and right). They are 8in. in length and 1in. in thickness. The color yellow was selected due to the the lighting in photolithography rooms of cleanrooms. It is a dull yellow instead of the normal white everywhere else. This is due to the light-sensitive nature of the wafers during this stage of production - white light would corrupt the product. Any other color would prohibit the robot from entering such areas.

## Extendable Arm
When discussing the project with cleanroom experts, those interviewed explained air 2.5ft-6ft above the ground would require the most testing. The purpose of the extendable arm is allow the robot to provide air quality monitoring and testing at any desirable height. The extension feature will be enabled through the use of servo motors. It is composed of three parts:

**Arm base** - 7.25in. by 7.25in. by 2in. It is a rectangular base with a 6.25x6.25 square hole where the air quality sensors sits in the retracted state and from where the extendible components emerge. It sits on top of the robot.

**Arm inner** - 4.75in. by 4.75in. by 28in. It is a rectangular solid. It fits into the outer arm’s hollow interior. The air sensor will be placed on top of this.

**Arm outer** - 6in. by 6in. by 28in. It is a rectangular pole with a hollowed interior measuring 5x5in to hold the arm inner piece. It protrudes out from the arm base unit that sits on the chassis.


## Charging Base
This aspect of the project would serve as the home and charging location for the robot. The dimensions math the chassis width and height. It would be stationed in an area of the cleanroom that permits seamless integration.

## Seals and Shielding
The entire robot, specially the tires and extendable arm piece, require seals to ensure they do not produce additional contaminants while operating in the cleanroom. Also, While operating, some of the components generate EMI which will require shielding to prevent robot interference with surrounding machines and devices.

## Data storage
Data must be stored long-term. The BeagleBone Black has internal flash memory, an SD card slow, and a USB port. Thus, an SD card or USB flash drive may be used for WiFi-less data retrieval, or data may be stored on the internal memory and retreived with the web interface over WiFi.

## Battery
A longer-lasting battery should be incorporated into the design so the robot can run longer between charges.

## Explore mode
The "Explore" mode must be implemented with the functionality described in the Modes of Operation page. This will require completing the SLAM portion of the project, including the following:

- Localization with only a partially-built map
- Detecting of ideal landmarks for localization
- Construction of map from LIDAR readings

## Autonomous Navigation
The robot must be completely autonomous after initial setup. This is necessary for hands-off operation, and includes charging. The only time human intervention may be required is if the robot localization algorithm fails (pick up the robot and move it back to the charging station, ideally this will never happen), or when the air quality is detected to be beyond the set threshold. In addition it must support all functionality described in the "Map (Autonomous)" mode in the Modes of Operation page.

## Monitor and Home modes
The "Monitor" mode is partially implemented, but must be completed as a separate mode.

The "Home" mode must be implemented with the functionality described in the Modes of Operation page.
