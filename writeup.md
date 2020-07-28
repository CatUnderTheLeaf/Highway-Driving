# **Highway Driving** 

## Writeup

---



The goals / steps of this project are the following:
* To design a path planner that is able to create smooth, safe paths for the car to follow along a 3 lane highway with traffic.
* A successful path planner will be able to keep inside its lane, avoid hitting other cars, and pass slower moving traffic all by using localization, sensor fusion, and map data.
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/histogram.jpg "Histogram"
[image2]: ./examples/samples.jpg "Samples for each label"
[image3]: ./examples/grayscale.jpg "Grayscale"
[image4]: ./examples/test_images.jpg "Test images"
[image5]: ./examples/predictions.jpg "Predictions for test images"
[image6]: ./examples/top5_0.jpg "Top 5"
[image7]: ./examples/top5_1.jpg "Top 5"
[image8]: ./examples/top5_2.jpg "Top 5"
[image9]: ./examples/top5_3.jpg "Top 5"
[image10]: ./examples/top5_4.jpg "Top 5"
[image11]: ./examples/top5_5.jpg "Top 5"
[image12]: ./examples/top5_6.jpg "Top 5"
[image13]: ./examples/top5_7.jpg "Top 5"
[image14]: ./examples/top5_8.jpg "Top 5"


## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/1971/view) individually and describe how I addressed each point in my implementation.  

---
### Compilation

#### 1. The code compiles correctly. Code must compile without errors with cmake and make.

Yes, the code compiles without errors.

### Valid Trajectories

#### 1. The car is able to drive at least 4.32 miles without incident. The top right screen of the simulator shows the current/best miles driven without incident. Incidents include exceeding acceleration/jerk/speed, collision, and driving outside of the lanes. Each incident case is also listed below in more detail.

Yes, the car is able to drive at least 4.32 miles without incident.


#### 2. The car drives according to the speed limit. The car doesn't drive faster than the speed limit. Also the car isn't driving much slower than speed limit unless obstructed by traffic.

Yes, the car drives according to the speed limit. Almost always the car drives with maximum allowed speed, it slowes down to avoid collisions. 


#### 3. Max Acceleration and Jerk are not Exceeded. The car does not exceed a total acceleration of 10 m/s^2 and a jerk of 10 m/s^3.

Yes, max Acceleration and Jerk are not Exceeded. A car starts with null velocity and than slowly accelerates. During changing lines Jerk is not exceeded.


#### 4. Car does not have collisions. The car must not come into contact with any of the other cars on the road.


#### 5. The car stays in its lane, except for the time between changing lanes. The car doesn't spend more than a 3 second length out side the lane lanes during changing lanes, and every other time the car stays inside one of the 3 lanes on the right hand side of the road.

Yes, the car stays in its lane, except for the time between changing lanes.


#### 6. The car is able to change lanes. The car is able to smoothly change lanes when it makes sense to do so, such as when behind a slower moving car and an adjacent lane is clear of other traffic.

Yes, when the car is behind a slower moving car and an adjacent lane is clear of other traffic the car is able to smoothly change lanes


### Reflection

#### 1. There is a reflection on how to generate paths. The code model for generating paths is described in detail. 

Path is generated in `void PathPlanner::PlanPath()` function in 'pathplanner.cpp'. My pipeline looks:
1. Initialize Pathplanner with map.
2. Every timestamp update it with data from simulator.
3. Clear previous path.
4. Analize data from Sensor Fusion and make decisions `bool PathPlanner::SenseCars(int path_size)`:
  * a car should not collide with other cars;
  * if a car ahead is slower we have ;
  * decision is made using cost functions. There are two cost functions: `safety_cost` which penalizes for unsafe gaps and `speed_cost` which penalizes for lower speed;
  * as a result we got a lane id our car should move or stay in.
5. Fill new path with points from the previous path, if there are any. `void PathPlanner::FillFromPreviousPath(int path_size)`
5. Initialize spline object in function `vector<double> PathPlanner::setSpline(int path_size)` with 2 points from the last path and 3 waypoints in front of the car in target lane.
6. The path is generated using spline object and this code cares that a car moves with reference speed and without exceeding jerk and total acceleration.
