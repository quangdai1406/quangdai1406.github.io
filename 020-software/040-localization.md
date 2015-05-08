---
layout: page
title: Localization Algorithm
permalink: /software/localization/
---

The robot performs localization by identifying two corners in the environment and triangulating its position to them. To do this, it calculates an estimate of its position using the desired speed and direction set by the web interface, integrating to obtain position. The estimate is used to pick a "landmark range", these ranges are pre-programmed in and identify which two landmarks should be visible at any given position on the map.

This algorithm requires landmark positions to be known beforehand in the form of "landmark ranges". Each range should include the following:

- Which area of the map it applies to (x and y ranges)
- Two landmarks with their x and y positions, and which type of landmark it is. This landmark type should be one of the following:
    - innercorner: two walls intersect to form a corner with angle is 90 degrees
    - outercorner: two walls intersect to form a corner with angle 270 degrees
    - lowedge: a single wall ends, the wall extends in the counter-clockwise direction from the end-point
    - highedge: opposite of lowedge. A single wall ends, the wall extends in the clockwise direction from the end-point.

Landmarks must satisfy the following restraints to be able consistently detect them with the LIDAR:

- They should be at least 10cm away from the robot's position, this is a limitation of the LIDAR
- They should be either exactly horizontal or exactly vertical, this is a limitation of the software to make the algorithm simpler
- The landmarks should be no farther away than necessary (or, prefer closer ones to far away ones), as closer LIDAR readings are more accurate and closer together
- The landmarks should be at least 5cm apart so they can be distinguished from each other

Landmarks should be in the following order, depending on the position of the wall in question:

- Case 1) If the wall is vertical and to the left, P1 is on top and P2 is on bottom
- Case 2) If the wall is vertical and to the right, P1 is on bottom and P2 is on top
- Case 3) If the wall is horizontal and to the bottom, P1 is on the left and P2 is on the right
- Case 4) If the wall is horizontal and to the top, P1 is on the right and P2 is on the left

The following information is necessary for each landmark range, and may be calculated from the above easily:

- Distance between the landmarks
- Whether the landmarks are vertical or horizontal

The algorithm below is used to determine the actual position of the robot. Angles are all clockwise, as that is the direction of rotation of the LIDAR.

First we attempt to find the two landmarks specified in the landmark range in the LIDAR data.

- First, get an estimate of your position
- Determine which landmark range the robot is currently located in
- Search the LIDAR data for landmarks one reading at a time. Edges also look like corners according to this algorithm so search for those first.
    - lowedge: Readings before the current reading will be closer, but the next reading will be much farther away
    - highedge: Readings after the current reading will be closer, but the previous reading will be much farther away
    - innercorner: Readings both before and after the current reading will be closer
    - outercorner: Readings both before and after the current reading will be farther away
- We should now have landmarks for all corners visible to us. Filter the landmarks by throwing out any that do not meet the following criteria, searching each set of two landmarks:
    - Distance between them should be close to the expected distance given in the landmark range
    - Angle the landmarks are found at should be somewhat close to the expected angle, the estimate is not that accurate so just check that if it's expected to be in front of us, we should not find it behind us
    - Angle between them should be close to the expected angle in the landmark range
    - The landmarks should be the expected type
- If after filtering there are not exactly two landmarks, stop running this algorithm and wait for new LIDAR data. We cannot calculate our position.
- Sort the two landmarks, from now on we will use the following variable names:
    - d2 and theta2 are the distance and angle obtained from the LIDAR for the point with the the smaller theta.
    - d1 and theta1 are the distance and angle obtained from the LIDAR for the point with the the larger theta.
    - theta = theta1 - theta2

Now that we have two landmarks we can calculate the robot's actual position.

- If we are facing between the two landmarks then point 1 and point 2 will be backwards, detect this as follows. Also calculate the angle phi which is the angle to form a right triangle with the robot and P1 at the two other non-90 corners
    - If (thea1 – thea2) > 180 then we are facing between the two readings:
        - d = d2
        - theta = theta2
        - phi = asin (d1 /  z * sin(theta2 – theta1 – 180))
    - Otherwise:
        - d = d1
        - Theta = theta1
        - phi = asin(d2 / z * sin(theta1 – theta2)
- Calculate the distance offsets Δx and Δy from point 1, then use those to calculate our exact position relative to the origin of the map.
- If the wall is vertical (case 1 or 2)
    - Δx = d * sin(phi)
    - Δy = d * cos(phi)
    - If the wall is to the left (case 1)
        - x = P1x + Δx
        - y = P1y - Δy
    - Otherwise the wall is to the right (case 2)
        - x = P1x - Δx
        - y = P1y + Δy
- If the wall is horizontal (case 3 or 4)
    - Δx = d * cos(phi)
    - Δy = d * sin(phi)
    - If the wall is to the bottom (case 3)
        - x = P1x + Δx
        - y = P1y + Δy
    - Otherwise the wall is to the top (case 4)
        - x = P1x - Δx
        - y = P1y - Δy
- x and y are now the robot's actual position.

Finally, calculate the robot's actual heading as follows:

- Determine the third angle of that triangle rho = 90 - phi and depending on which case it is do the following:
    - case 1) baseAngle = -90
    - case 2) baseAngle = 90
    - case 3) baseAngle = 180
    - case 4) baseAngle = 0
- heading = baseAngle + rho - theta
