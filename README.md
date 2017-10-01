
# Model Perdictice Control(MPC) Project
Udacity Self-Driving Car Engineer Nanodegree Term 2, Project 5

---

## Introduction

In this project, a model predictive controller is implemented to drive a car around track in a simulator.


## Setup

#### 1. Install dependencies

* cmake >= 3.5
* make >= 4.1(mac, linux), 3.81(Windows)
* gcc/g++ >= 5.4
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
* [Ipopt](https://projects.coin-or.org/Ipopt)

#### 2. Build

* clone this repo

```
mkdir build && cd build
cmake .. && make
./mpc
```

## Reflection

#### 1. The Model

At first, the following information are send from simulator.

* `ptsx` (Array<float>) - The global x positions of the waypoints.
* `ptsy` (Array<float>) - The global y positions of the waypoints. This corresponds to the z coordinate in Unity
since y is the up-down direction.
* `psi` (float) - The orientation of the vehicle in **radians** converted from the Unity format to the standard format expected in most mathemetical functions (more details below).
* `psi_unity` (float) - The orientation of the vehicle in **radians**. This is an orientation commonly used in [navigation](https://en.wikipedia.org/wiki/Polar_coordinate_system#Position_and_navigation).
* `x` (float) - The global x position of the vehicle.
* `y` (float) - The global y position of the vehicle.
* `steering_angle` (float) - The current steering angle in **radians**.
* `throttle` (float) - The current throttle value [-1, 1].
* `speed` (float) - The current velocity in **mph**.

The global x, y position then transform into vehicle coordinate and fit with a 3rd order polynomial. In vehicle coordinate, x,y and psi are all zero at inital condition. The cross track error(CTE) can be calculated as the difference at the point x = 0 and psi error(epsi) is equal to minus arc tangent of the first order derivative at x = 0. 

The cost function is taken into consideration. Minimize the cte and the epsi values is the most important task of the controller, so I use 1000 as the weight for both of them and others are 1.

The state are then submitted into the Ipopt optimizer. The optimizer returns the actuate value delta and a.

#### 2. Latency

There is 100ms latency between actuation and the simulator. To deal with it, I use the kinetic model to predict where the vehicle is after 100ms and then use this value as the vehicle's current position. It's important that the delta is positive when we rotate counter-clockwise, or turn left. In the simulator however, a positive value implies a right turn and a negative value implies a left turn. So we have to use -delta instead of delta in the kinetic model. 

#### 3. Timestep Length and Frequency

The timestep length `N` and frequency `dt` are using to decide how many state the we are looking into the future. If the values are too large, it seems that the model react slower. If the value are too small, the model becomes unstable. I finally use N = 10 and dt = 0.1 .  





```python

```
