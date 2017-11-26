# Highway-Path-Planning_SelfDrivingCar_Term3_P1

###Introduction

This project is the Udacity "Self Driving Car" nanodegree program's term 3, project 1. 
As part of this project, main requirement is to autonomously drive the car around the track of 4.32 miles without any incident.


Here incident refers to following categories:

- Keeping the given speed limit of 50mph
- No collision with other cars
- Keeping the lane
- When there is a vehicle in front which is driving below speed limit, then try to possible change lane 
- At any time, total acceleration of 10 m/s^2 and a jerk of 10 m/s^3 should not be exceeded, which makes it uncomfortable for passenger


###Rubric Points

Please find the video from above link, the view the output of path planner. 

The path planner satisfies all the requirements, and car drives indefinitely on track in any traffic situations.

#### Model Documentation
This project can have n number of different way to deal with the required problems. I have followed the approach to create a small Finite State Machine model, which takes care of taking different decisions based on the current state and also takes care of avoiding all incidents.


#####States Description

-  Keep Lane

This is the default state of the system, this state represents, that the car need to be in the same lane, and not need to change the lane, when possible, try to accelerate and reach the max allowed speed limit of 50mph.

- Prepare Lane Change

This state represents the status when car need to prepare for lane change since the other car in front is driving slower than the speed limit. In this state, car also tries to find out the best lane to change, and best time to change lane safely and efficiently.

- Change Lane : Left

During this state, car changes lane, and then speeds up to reach max allowed speed limit. In this state car also takes the stablization period of 10 simulation cycle (~1sec), before making next lane change, this allows car to be stable.

- Change Lane : Right

During this state, car changes lane, and then speeds up to reach max allowed speed limit. In this state car also takes the stablization period of 10 simulation cycle (~1sec), before making next lane change, this allows car to be stable.


This state is different than left lane change, since it gets priority when cost of changing lane becomes same for left and right. The logic is to avoid reaching to left lane which is also considered as the fast lane for highways.

##### Information In-outs

We are provided with the different way points around the track to follow. 

- Simulator expects:

-- The sequence of x,y coordinates to follow. The simulator covers each point in 0.02 sec, so the spacing between points, decides the speed of the car.


- Simulator provides following information:

-- Car's position in global x,y coordinates

-- Car's position in Frenet coordinate system

-- Car's velocity and yaw 

-- It also provides the list of remaining points, which are not yet visited by simulator and provided as last path points.


##### Path generation steps

- Our main goal is to create a path which is free from jerks and acceleration limits. For that the most important point to remember is, the path should be continuous, means we should try to utilize the previous path points which were provided to simulator.

- To ease the calculations we convert the global coordinates to car coordinates, and at the end revert from car to global coordinates.

- We chose the different way points, which are atleast 30m apart and then pass those points to spline to fit into a smooth curve.

- By using the spline we try to fit approx 50 path points for simulator to follow by providing the spaced points x points and getting corresponding y points.

- Once we have this vector of points available then we convert them to global coordinates and pass to simulator.


