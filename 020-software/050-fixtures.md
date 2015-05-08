---
layout: page
title: Data Fixtures
permalink: /software/fixtures/
---

Fixtures contain fixed data that rarely or never changes. All fixtures are located in the `lib/fixture` folder.

## Known Map (`map.js`)

This fixture contains all wall locations in the test environment as an array of walls. It calculates an occupancy grid from the wall locations and exports both the wall locations and occupancy grid. It also contains code to convert wall locations from inches to centimeters, which is not currently in use.

## LCD Font (`sourceCodePro.js`)

This fixture contains the font used by the LCD. This data was created from [Adobe's Source Code Pro font](https://github.com/adobe-fonts/source-code-pro) using the same method outlined on the [Microview website](http://learn.microview.io/font/creating-fonts-for-microview.html). The character size was set to 10x16, double the size used on their website, to make the text easier to read. Other parameters were adjusted as necessary to make the text look visually pleasing.

## Landmark Ranges (`landmarks.js`)

This fixture contains an array of landmark range objects, which have the following properties:

- Array `x` - Minimum and maximum x values for the range
- Array `y` - Minimum and maximum y values for the range
- Landmark `landmark1` - The first landmark, where "first" matches that specified in the localization algorithm
- Landmark `landmark2` - The second landmark

The following properties are calculated from the above data and added to the objects:

- Number `i` - Range index
- Number `distance` - Distance between the two landmarks
- String `orientation` - One of "horizontal" or "vertical"

Each Landmark object contains the following properties:

- Number `x`
- Number `y`
- String `type` One of "innercorner", "outercorner", "lowedge", or "highedge"

This fixture exports an object containing a single method `getLandmarkRange`, which takes a single parameter `location`, an object containing the x and y positions of the robot.
