# Kalman Filter

## Definition

A Kalman Filter is a filtering algorithm that tries to estimate values based on measurements and pre-determined models. This can be applied in VEX Robotics to reduce noise in different sensor measurements, such as a noisy [ultrasonic rangefinder](../electronics/vex/vex-sensors/untitled-1.md) measurement or a noisy trend in an average value obtained from multiple sensors.

## How it works

There's a lot of math involved in putting together a Kalman Filter. Thankfully it is a commonly used filter and there are some great explanations of how it works already. The links below are a recommended place to look:

* [Explanation with Python Code](http://robotics.itee.uq.edu.au/~elec3004/2015/tutes/Kalman_and_Bayesian_Filters_in_Python.pdf)
* [A Practical Guide to State Space Control, Chapter 9](https://file.tavsys.net/control/controls-engineering-in-frc.pdf)

## Defining a Model

The math behind a Kalman Filter is complex but the same regardless of the system. The system model, on the other hand, can range from a simple first order system to a gigantic transfer function. It all depends on the dynamics of the system that you are measuring and the physics that define its movement.

### The Math Behind Motors

The equations defining the physics of a motor's movement can be found in Chapter 13.1.2 of [A Practical Guide to State Space Control, Chapter 9](https://file.tavsys.net/control/controls-engineering-in-frc.pdf) as linked above. 

There are a handful of variables that are identified in the linked equation. Those can be found by running a few tests with a motor running by itself (not connected to a drivetrain or flywheel or anything).

- K_t = measured torque / measured current at stall

- R = set voltage / measured current at stall

- K_v = measured angular velocity / (set voltage - measured current * R) measured with no load

The torque, current, voltage, and velocity can all be accessed for a V5 motor through your programming language of choice.

Plug the values you find for those variables into the equations from 13.1.2, and then you have your transfer function for the Kalman Filter's state matrix.

### First Order Transfer Functions

First order transfer functions contain just two variables as opposed to three for a motor's second order function. The two variables are:

- K_ss: The steady state gain, or the amount of change in the state for 1 unit of input
- tau: The time constant, or the time from the start of a step in the input to it reaching 63% of steady state

These can be found by:

- K_ss: Increase the input by one unit, like 1 volt for a motor. Measure the change in the state variable when the system settles, like an increase in velocity.
- tau: Measure the time it takes for the output to get to 63% of the steady state value measured previously.

These constants then become part of the following transfer function:

    K_ss
------------
 tau * s + 1

## Recommendations

Defining a good model for the filter is _really hard_. A lot more factors play into the behavior of a mechanical system than simple equations can easily define. This means that the model can only help identify sensor values that are extreme outliers. The Kalman filter won't do much to improve the sensor values in this case and will just act as a complicated low-pass filter. A Kalman Filter isn't worth the hassle if you don't have multiple sensors to fuse data from or a simple system with a _great_ model.

If you do have multiple sensors that cover the same physical state then a Kalman Filter is a great choice. A good example of this would be a robot with encoders on each side of the tank drive and a gyroscope. In that example, both the encoder wheels and the gyroscope measure the robot's heading so their values can be fused to result in one, more accurate reading.

### Contributing Teams to this Article:

* [BLRS](https://purduesigbots.com/) \(Purdue SIGBots\)

