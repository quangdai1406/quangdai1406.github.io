---
layout: page
title: Start-up Process
permalink: /software/startup/
---

This page describes start-up procedures taken by the robot software for tasks such as calibrating sensors.

## General tasks

Upon startup, the robot software stops some processes started by the OS automatically to free network ports and reduce CPU usage (jekyll and apache2). It also exports the custom device tree overlay to the BeagleBone Black's cape manager to enable the PRU.

After the above completes the software starts socket.io and loads all other software modules, which initialize as necessary.

## CalibratedImu Store

Upon startup this module takes the first 200 readings from the IMU to calculate the offset in accelerometer readings, which is subtracted from all future readings. This must occur while the robot is stationary and flat on the ground.

## Imu Hardware Store

This module has no software start-up procedure, however for the heading to be accurate a manual set-up is necessary. After the CalibratedImu store is finished, the robot must be slowly rotated 360 degrees in either direction (changing direction part-way through rotation is fine) for several seconds. The software will continuously calibrate, this calibration is not accurate until magnetic fields have been measured in all directions.

## WiFi Hardware Store

Upon startup this module determines whether the robot is currently connected to a WiFi network, and if so, what IP address it has. If it is not connected to one it runs the necessary terminal commands to connect. After connecting, or if it is connected but has no IP address, it runs the necessary terminal commands to obtain a DHCP lease. Finally it reports the connected network name and IP address on the LCD.

## Pru Hardware Store

Upon startup this module loads the compiled Assembly code onto the PRU and executes it.
