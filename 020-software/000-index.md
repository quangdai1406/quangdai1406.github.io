---
layout: page
title: Software
permalink: /software/
---

The software is divided into two pieces: The robot software, and the web interface. The robot software performs the majority of the tasks, including hardware communication and localization. The web interface displays the robot’s status, selected mode of operation, the map of the environment, sensor data, and allows the user to control the robot.

The robot software is written in Javascript and runs on [Node.js](http://nodejs.org/), which was chosen because it is the same language the web interface is required to use, it has a large number of existing modules, and its asynchronous event-based design is ideal for robotics applications. It is composed of several modules, which are shown in the figure below along with with how they work together. The web interface software is organized in a similar fashion. These modules communicate with each other using one-way events, similar to hardware interrupts, heavily inspired by the [Flux architecture](https://facebook.github.io/flux/). The current software contains all modules shown in the figure with the exception of the Pose module, which is still being built. Modules labeled “Hardware” communicate directly with external hardware but perform no calculations on the data received; modules labeled “Fixtures” contain pre-known data; and modules labeled “Stores” compute and store data from other modules.

The robot software communicates with the web interfacing using [socket.io](http://socket.io/), which uses WebSockets to provide a real-time, low-latency data link between the robot and the browser. Data is transmitted between the two in the form of events, which have a human-readable name and a JSON data object. Any module on the robot can listen to any event from the web interface, and vice versa. The documentation on each module lists which events are sent and what data those contain.

![Software block diagram]({{site.url}}/images/diagramSoftware.png){: class="fullwidth"}



