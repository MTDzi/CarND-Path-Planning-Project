# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program

# Write up
## Introduction
The purpose of this project was to implement a path planning procedure. The environment was a highway with three lanes in the Udacity simulator. The path planner should propose paths that would be both: safe, and comfortable for the passengers.

## Requirements
#### 1. The car is able to drive at least 4.32 miles without incident.
I've attached a `output.mp4` file with the run. There's one glitch/discontinuity at about 5:25, which was a result of changing workspaces on my system (Ubuntu), but I wasn't cheating or anything.

#### 2. The car drives according to the speed limit.
Yes, it does. I followed the walkthrough on that.

#### 3. Max Acceleration and Jerk are not Exceeded.
Same as above.

#### 4. Car does not have collisions.
That's also fine.

#### 5. The car stays in its lane, except for the time between changing lanes.
Yes, it does.

#### 6. The car is able to change lanes.
It is, however, I've noticed it does it perhaps a bit too rapidly (in certain instances). But it depends on the thresholds I've used to discern whether a car is deemed "close". I've used thresholds that made watching the consecutive runs more interesting, and allowed me to debug the rest of the procedure.

## Reflections / conclusions
Although I enjoyed the lectures on FSMs and the notion of choosing a path based on a cost function seems the right way to go in a more general setting, I didn't define a cost function. My paths were generated by a set of `if`-`else` clauses that "decided" whether or not the car should stay in its lane, or change it.

This approach was, I think, way easier to debug because when working with a cost function, one needs to tune the coefficients/weights of the individual terms that go into the total cost. I've struggled a LOT with choosing those weights in the MPC project, so this time I wanted to keep it simple.

The problem with choosing those weights is that once you modify even one weight to address the ONE error made by the car, you're very likely to create several OTHER new errors that will become apparent in the next run in the simulator. And so on, and so forth, until you finally converge.

And if the simulator would offer a non-graphical API that could run faster, this would be a valid strategy. But watching how the car fails after 5 minutes of runtime in the simulator, then correcting the weights, then again 5 minutes in the simulator, and the again, and again... It simply took too long in the MPC project, so I decided to take a different route this time.

But was it faster? Actually, yes, it was very fast to get to a "runnable" path planner.

But can it handle all quirky situations on the highway? Well, no, I've encountered situations that, if I was lucky, would require adding another batch of `if`-`else` clauses. But if I was unlucky because the error would be caused by something I didn't thought of building my decision-making construct... then the cost-based approach would be potentially better, because all I would need to do is add another term that would account for this rare, not yet handled scenario.

To summarize: I would go with the cost-based approach if you would be able to run the simulations faster, perhaps in a non-GUI-interface.

### Simulator.
You can download the Term3 Simulator which contains the Path Planning Project from the [releases tab (https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2).

### Goals
In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

#### The map of the highway is in data/highway_map.txt
Each waypoint in the list contains  [x,y,s,dx,dy] values. x and y are the waypoint's map coordinate position, the s value is the distance along the road to get to that waypoint in meters, the dx and dy values define the unit normal vector pointing outward of the highway loop.

The highway's waypoints loop around so the frenet s value, distance along the road, goes from 0 to 6945.554.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./path_planning`.
