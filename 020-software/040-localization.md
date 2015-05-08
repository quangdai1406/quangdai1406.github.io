---
layout: page
title: Localization Algorithm
permalink: /software/localization/
---

The robot performs localization by identifying two corners in the environment and triangulating its position to them. To do this, it calculates an estimate of its position using the desired speed and direction set by the web interface, integrating to obtain position. The estimate is used to pick a "landmark range", these ranges are pre-programmed in and identify which two landmarks should be visible at any given position on the map.

Some minor changes were made to this algorithm while implementing it in Javascript.

- Get an estimate of your position first
- Identify two landmarks in your known environment, they should be easy to find in the LIDAR readings. They should be at least 15 cm away, but not too far away as closer is more accurate. They should also be either exactly horizontal or exactly vertical. There should not be another wall between us or almost between us and the landmark as it’s possible error in our estimates position makes it actually between us and the landmark. Record approximately how far away they are, the location of the first point, and distance thresholds, which is determined as follows: 
    - Case 1) If the wall is vertical and to the left, P1 is on top and P2 is on bottom
    - Case 2) If the wall is vertical and to the right, P1 is on bottom and P2 is on top
    - Case 3) If the wall is horizontal and to the bottom, P1 is on the left and P2 is on the right
    - Case 4) If the wall is horizontal and to the top, P1 is on the right and P2 is on the left
    - Landmarks could be inside or outside corners, in this case we need the approximate distance to the corner.
    - One or both landmarks could also be wall edges, in this case we need the approximate distance to the edge and the approximate distance to the wall behind the edge. The wall on the other side should not be extremely close.
- Look for the two landmarks:
    - If the landmark is an inner wall find a value that is approximately the value from above and larger than nearby values. Filter out landmarks that are more than 10cm off from the expected distance away. For the initial position we can also just filter out anything further away than 50cm, that’s simpler.
    - If the landmark is a wall edge we instead find a value which is approximately the value from above, larger than nearby values on one side, and values on the other side are approximately the distance found earlier
    - If the landmark is an outer wall, find a value that is approximately the value from above and less than nearby values. Basically the opposite of an inner wall.
- Values are named as follows from now on:
    - theta2, d2 is the one with the smaller theta
    - theta1, d1 is the one with the larger theta
- Calculate the angle between the two readings: theta = theta1 - theta2
- If theta1 - theta2 > 180 then we are facing between the two readings, do the following;
    - d = d2
    - theta = theta2
    - phi = asin (d1 / z * sin(theta2 - theta1 - 180) )
- Otherwise we are not facing between the two readings, do the following:
    - d = d1
    - theta = theta1
    - phi = asin (d2 / z * sin(theta1 - theta2) )
- (d is now the distance to P1, phi is now the angle to form a right triangle with the robot and P1 at the two other non-90 corners)
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
- (x and y are now your actual position)
- Determine the third angle of that triangle rho = 90 - phi and depending on which case it is do the following:
    - case 1) baseAngle = -90
    - case 2) baseAngle = 90
    - case 3) baseAngle = 180
    - case 4) baseAngle = 0
- heading = baseAngle + rho - theta

