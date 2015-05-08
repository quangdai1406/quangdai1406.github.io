---
layout: page
title: Web Interface
permalink: /software/webinterface/
---

The web interface is built using [React](http://facebook.github.io/react/) and [socket.io](http://socket.io/). React is a web application view library centered around the concepts of components, uni-directional data flows. It allows us to build the interface out of small components that we can completely re-render anytime their data changes. Socket.io provides a real-time data link between the robot and the web interface. [Bootstrap](http://getbootstrap.com/) is used to provide basic styling for the components.

In addition to React components, the web interface uses data stores. Components display data to the user; stores synchronize data with the robot.

## General files

All files are located in the `lib` folder to separate 3rd party dependencies, source code, and built files.

### `index.html`

This is the only HTML file for the web interface. It contains no content, and does nothing but run the Javascript code.

### `index.js`

This is the main entry point for the web interface is which sets up the component hierarchy and passes control to React.

### `socket.js`

This file establishes the socket.io connection and exports it to all other files that need it, or provides a [mock](http://en.wikipedia.org/wiki/Mock_object) if it cannot be found (useful for testing).

### `httpServer.js`

This file is not part of the web interface; it starts a web server which serves the files to the browser. This file is run by npm after building the code.

### `less/main.less`

This file contains the custom [LESS](http://lesscss.org/) styles for the web interface, overriding the Bootstrap defaults.

### `dispatcher.js`

This file contains a [Flux](https://facebook.github.io/flux/) dispatcher for emitting events. This is not currently in use as all events (except for keyboard or gamepad events) are emitted by socket.io from the robot.

## Components

Components are located in the `lib/components` folder. Component names start with capital letters as they are similar to class constructors in class-based languages.

### `Header.js`

This component displays the header bar at the top of the interface showing the project's name and the robot's CPU and memory usage. It obtains data from the Load store.

### `Footer.js`

This component displays the project information at the bottom of the interface.

### `SpeedVisualization.js`

This component displays a 2D visualization of the robot's current desired speed using arrows. It obtains data from the Motor store. This file also contains an Arrow component, which is used for the visualization.

### `EnableFlags.js`

This component displays checkboxes to enable or disable various pieces of hardware. Currently, the only one present is the motor driver; the LIDAR would be beneficial to add. It obtains data from the Motor store.

### `AirQuality.js`

This component displays both a graph of air quality measurements versus time, displayed using the [chart.js](http://www.chartjs.org/) library, and a list of readings color-coded by air quality index. The graph does not update properly if it is initialized with zero or one data points present; to fix this just refresh the page to re-initialize. Performance of this component is not ideal with large data sets (over 30 minutes, or 60 readings) but was sufficient for the demonstration. This component obtains data from the Aq store.

### `Map/Map.js`

This component displays the environment map using several SVG components located in the `Map` subdirectory.

### `Map/MapCursorPosition.js`

This component displays the current mouse cursor position in centimeters.

Required properties:

- Number `height` The height of the SVG viewbox in centimeters
- Number `width` The width of the SVG viewbox in centimeters
- Object `cursor` An object containing the current cursor position in properties `x` and `y`.

### `Map/KnownMap.js`

This component displays the walls in the known map. Optionally it can also display the occupancy grid built from the known wall locations by uncommenting the relevant line in the `render()` function. It obtains data from the KnownMap store.

Required properties:

- Number `height` The height of the SVG viewbox in centimeters
- Number `width` The width of the SVG viewbox in centimeters

### `Map/EstimatedPositionTrail.js`

This component displays all past robot locations as a thin trail on the map. It obtains data from the EstimatedPosition store.

Required properties:

- Number `height` The height of the SVG viewbox in centimeters
- Number `width` The width of the SVG viewbox in centimeters

### `Map/Lidar.js`

This component displays the following sets of data:

- Latest LIDAR readings using the LidarReading component
- The current landmark range (or zone) being used for localization
- The robot's current estimated and calculated positions as triangles
- All identified landmarks as small red dots
- The landmarks the robot is actually attempting to use for localization as blue dots, these are larger if there are exactly two as expected
- The known positions of the two expected landmarks in the current landmark range

This component obtains data from the EstimatedPosition, ActualPosition, and Lidar stores.

This component should be broken into several smaller ones as it is currently too monolithic.

Required properties:

- Number `height` The height of the SVG viewbox in centimeters
- Number `width` The width of the SVG viewbox in centimeters

### `Map/LidarReading.js`

This component displays a set of LIDAR readings.

Required properties:

- Array `readings` A set of readings, each reading should be an object with the properties `distance` and `angle`

### `Map/Landmarks.js`

This component was used for testing the localization data with generated data and is no longer in use.

## Stores

Stores are located in the `lib/stores` folder. Stores are static objects which inherit from the Node.js built-in EventEmitter object. All stores emit a single event named `change`, which occures whenever their data has changed. Stores either contain a `data` property which contains the latest data, or a set of getter methods.

The following stores simply listen to socket.io events from the robot, update their data to match that sent from the robot, emit a `change` event, and store their data in a `data` property which is identical to the data sent from the robot:

- `ActualPosition` - Listens to updates from the ActualPosition store on the robot.
- `Aq` - Listens to updates from the Aq store on the robot.
- `Imu` - Listens to updates from the Imu store on the robot. This store is not currently in use.
- `Landmarks` - Listens to updates from the Landmarks store on the robot. This store is not currently in use.
- `Lidar` - Listens to updates from the Lidar store on the robot.
- `Load` - Listens to updates from the Load store on the robot.
- `Speed` - Listens to updates from the Speed store on the robot. This store is not currently in use.

The following stores do not follow the exact pattern above:

### `EstimatedPosition`

Listens to updates from the EstimatedPosition store on the robot. This store follows the above pattern, but also stores every data object received in an array which may be retreived using the `getAll()` method.

### `KnownMap`

This store emits no events and does not contain a data property. It stores the walls and occupancy grid of the known map, which may be retreived using the `getWalls()` and `getGrid()` methods, respectively. The data from these two methods is obtained from the Map fixture on the robot by including it directly, not by asking the robot for it.

### `Motor`

Listens to updates from the Motors store on the robot. This store follows the above pattern but adds the following additional functionality:

- Listens to the keydown and keyup events on the W, A, S, and D keys. When W or S is pressed it sets the robot speed; when A or D is pressed it sets the robot direction.
- Polls any connected PS3 gamepad every 100ms, if new data is obtained it sets the desired speed and direction to match the position of the 1 and 0 axes. This only works on Google Chrome.
- Sends a socket.io event to the robot whenever one of the above results in a new speed and/or direction
- Calculates the length and angle of a vector representing the robot's desired speed and direction
- Sets or clears the motor enable flag

This store's data property contains the following:

- Motor `left`
- Motor `right`
- Number `desiredSpeed`
- Number `desiredDirection`
- Number `actualSpeed` No longer used
- Number `actualDirection` No longer used
- Number `desiredSpeedLength`
- Number `desiredAngle`
- Number `actualSpeedLength` No longer used
- Number `actualAngle` No longer used

Each Motor object contains the following properties:

- Number `desiredSpeed`
- Number `desiredDirection`
- Number `actualRpm` No longer used
