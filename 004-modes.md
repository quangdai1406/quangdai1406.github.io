---
layout: page
title: Modes of Operation
permalink: /modes/
---

In the ideal product the robot will operate in four modes: Explore, Map (Autonomous), Map
(Manual), Monitor, and Home. The current works-like prototype supports only the Map
(Manual) mode.

## Explore

In the Explore mode the robot will explore a new and unknown environment. It will leave its
charging dock to begin to map and record a 2-dimensional view of obstacles (walls, machinery,
cabinets, etc) in the entirety of a cleanroom. As it roams the environment it will identify walls
and corners and save their locations, to be used as landmarks for localization. It will also perform
localization as it explores to keep track of where it is and where identified landmarks are. A
human operator will be required in this mode to ensure the map is being built correctly, and to
help the robot identify locations it has already visited. The map of the environment will be
displayed on the web interface as it is being generated. After the entire cleanroom is mapped the
operator can specify which locations are more or less important for air testing, which locations it
should not enter, and in which locations WiFi should be disabled. It will then calculate the best
path to scan the entire facility in the least amount of time and with minimal disturbance to
cleanroom personnel.

## Map (Autonomous)

In this mode, the robot will use the map created in Explore mode to conduct air quality
monitoring of the cleanroom. It will traverse the calculated best path, monitoring air particulate
count using the air quality sensing module. The data collected from the module will be wirelessly
transmitted to a web interface and displayed onto the 2-D map of the environment in the
associated locations. It will also intelligently determine when an area should be re-tested, such as
if the measured particle count is over a certain threshold defined by the user. If the contamination
levels are indeed unacceptably high, the robot can send an alert to on-hand staff to investigate.

## Map (Manual)

In this mode, the robot is directly controlled by a human operator, and performs air quality
monitoring as in the Map (Autonomous) mode. Air data may be gathered in real time or
collected at the end of a run.

## Monitor

In this mode the robot remains stationary and plots air quality in a single location as it changes
over time. This is useful if the particle count in a location measured in Map mode is over a
certain threshold and additional monitoring is desired to rule out false positives or normal
variations in air quality levels.

## Home

In the Home mode the robot will return to its charging dock by the shortest path that will
minimally disturb cleanroom personnel. While in Map (Autonomous) or Explore mode it will
enter this mode automatically once the battery charge is low. Once it is fully charged it will
automatically resume its previous mode. An operator may also command it to enter this mode, in
which case it will not automatically resume its previous mode.
