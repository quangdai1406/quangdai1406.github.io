---
layout: page
title: Hardware
permalink: /software/hardware/
---

This page is currently under construction.

Hardware modules are very similar to regular stores, except they communicate directly with hardware. All hardware modules are located in the `lib/hardware` folder, PRU code is located in `lib/pru0`.

These are different from hardware drivers. Drivers provide the low-level interfaces used to communicate with hardware, such as I2C. These modules use those drivers to provide a high-level interface to specific hardware components. The only exception is the PRU code, which communicates with a custom Assembly program running on a PRU.

## LIDAR (`lidar.js`)

## Motors (`motors.js`)

## PRU and Air Quality (`pru.js`)

## PRU Assembly Code (`lib/pru0/aq.p`)

## Load (`load.js`)

## WiFi (`wifi.js`)

## LCD (`lcd.js`)

## IMU (`imu.js`)

## Optical Encoders (`oe.js`)
