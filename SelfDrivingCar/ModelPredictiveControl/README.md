# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---

## WriteUP: Discussing Rubric Points

**Introduction**
This project aims to write a C++ code that can drive the car on the simulated Unity environment. the simulator communicates the telemetry and waypoint data using websocket. We can control the car by sending it steering and throttle(acceleration) commands. The rubric requires that the solution is robust to 100 ms latency.


### Model Details:

The kinematic model takes into account the car's x and y cordinates, its orientation angle ($\psi$), the velocity (v), cross trak error (cte), and orientation error ($e\psi$). The actuator outputs are acceleration (a) and delta (sterring angle). Model calculates the state and actuators values from the current state using these equations:

$x_{t+1} = x_t + v_t cos(\psi_t) dt$

$y_{t+1} = y_t + v_t sin(\psi_t) dt$

$\psi_{t+1} = \psi_t + \frac {v_t} {L_f} \delta_t dt $

$v_{t+1} = v_t + a_t dt $

$cte_{t+1} = f(x_t) - y_t + v_t sin(e\psi_t) dt$

$ e\psi_{t+1} = \psi_t - \psi des_t + \frac {v_t} {L_f} \delta_t dt $



### Choice of N and dt

In the project I chose the values of N (timestep length) as 10 and dt (timestep duration) as 0.1.  The length shoulb be chosen so that it is small enough to allow for efficient computing and large enough to make an accurate plan ahead. When I tried value greater than 10, the Udacity Simulator was not able to handle it. For values less than 10 the car was unstable because very little planning ahead!

The timestep duration too needs to have an optimum value, a large value allows us to plan further, and small value let us have accurate computation.

### Polynomial Fitting and MPC Preprocessing

We convert the waypoints obtained to vehicle cordinates as it reduces the further computation. We use a third degree polynomial to fit CTE. 


### Model Predictive Control with Latency
 to deal with latency I added a step to predict where the vehicle would be after 0.1 seconds (100 ms latency). Using the equations mentioned above I predicted a state after latency (Main.cpp Lines: 129-148). The state is then fed to `mpc.Solve()` function defined in MPC.cpp. 
 
 The MPC.cpp further puts constraints on the state values. The weights assigned to each costs also helps in providing a better control. 
 
 ### The vehicle must successfully drive a lap around the track.
 
 The car successfully drives a lap around the track. It could have been better and more smooth, but then the cost of computation would increase.
 
 










---
# Original Udacity Readme

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.

* **Ipopt and CppAD:** Please refer to [this document](https://github.com/udacity/CarND-MPC-Project/blob/master/install_Ipopt_CppAD.md) for installation instructions.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page). This is already part of the repo so you shouldn't have to worry about it.
* Simulator. You can download these from the [releases tab](https://github.com/udacity/self-driving-car-sim/releases).
* Not a dependency but read the [DATA.md](./DATA.md) for a description of the data sent back from the simulator.


## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`.

## Tips

1. It's recommended to test the MPC on basic examples to see if your implementation behaves as desired. One possible example
is the vehicle starting offset of a straight line (reference). If the MPC implementation is correct, after some number of timesteps
(not too many) it should find and track the reference line.
2. The `lake_track_waypoints.csv` file has the waypoints of the lake track. You could use this to fit polynomials and points and see of how well your model tracks curve. NOTE: This file might be not completely in sync with the simulator so your solution should NOT depend on it.
3. For visualization this C++ [matplotlib wrapper](https://github.com/lava/matplotlib-cpp) could be helpful.)
4.  Tips for setting up your environment are available [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)
5. **VM Latency:** Some students have reported differences in behavior using VM's ostensibly a result of latency.  Please let us know if issues arise as a result of a VM environment.

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/b1ff3be0-c904-438e-aad3-2b5379f0e0c3/concepts/1a2255a0-e23c-44cf-8d41-39b8a3c8264a)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).
