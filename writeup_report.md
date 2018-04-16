# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---

[//]: # (Image References)

[image1]: ./equations.png 


## Introduction
In this project, I implemented the Model Predictive Control Method to drive the car around the track. 

## Vehicle Model
The vehicle model has six state variables: x position(x), y position(y), heading(psi), velocity(v), crosstrack error(cte) and heading error(epsi). There are two actuators: steering angle and throttle value. 

The vehicle model is based on the following equations: 
![alt text][image1] 

## Preprocessing
At each time point, we can read the vehicle's position, heading and velocity from the simulator. 
According to the vehicle's current position and heading, the waypoints on the reference trajectory are converted from the global coordinates to the vehicle's coordinates. Then, the crosstrack error and heading error are calculated from the coefficients in the polynomial fitting of the reference trajectory. 

The state variables and the coefficients of the reference trajectory are imported into the MPC solver. The MPC solver calculates the optimal solution for steering angle and throttle value. 

## MPC Solver
The prediction horizon is the duration over which future predictions are made. I chose N(number of timestep) to be 15 and dt(timestep duration) to be 0.1 sec. These values mean that the prediction horizon is 1.5 sec. I chose dt to be 0.1 sec because the system has a 100 msec delay. It is easier to describe this delay in the model when the delay is the same as the timestep duration. I also tried other N values, such as N=10 or N=20, and other dt values, such as dt=0.05. Those values cause a larger oscillation of the vehicle motion. 

I considered the 100 msec latency of the system. The acceleration and steering angle impact the vehicle kinematics after a duration of 100 msec. Thus, I chose dt to be 0.1 sec. The vehicle model uses the acceleration and steering angle which are in the previous timestep to determine its motion. 

The cost function includes the crosstrack error, heading error, velocity change, actuation changes and the value gap between actuations. I set the weight for each factor to be 1 at the beginning. Then, I started to tune those weights. I set the weight of steering angle changes to be a large number to reduce the oscillation of the vehicle motion. I also set the weight of velocity change to be a large number to keep the vehicle speed constant.
