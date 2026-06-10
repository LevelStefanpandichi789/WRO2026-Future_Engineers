# HyperLine Robotics – WRO 2026 Future Engineers

## Team Introduction

HyperLine Robotics is a Romanian team competing in the World Robot Olympiad 2026, Future Engineers category.

The team consists of:

### Stefan Pandichi

Responsible for electronics development, PCB design, embedded programming and hardware integration.

### Horatiu Olteanu

Responsible for software development, testing, system validation and competition strategy.

The objective of our project is to develop a compact autonomous vehicle capable of completing all WRO Future Engineers tasks reliably and consistently. Throughout development, our focus was not only achieving high speed, but also ensuring repeatable performance under different track layouts and obstacle configurations.

---

# Project Overview

The WRO Future Engineers challenge requires teams to design and build a fully autonomous vehicle capable of navigating a randomized track without human intervention.

The competition consists of two main stages.

### Open Challenge

The robot must complete multiple laps while identifying the driving direction and performing accurate turns around the field.

### Obstacle Challenge

The robot must detect colored traffic signs and determine on which side the obstacle should be passed. After completing the required laps, the robot must locate the parking zone and perform an autonomous parallel parking maneuver.

To solve these tasks, our robot combines:

* computer vision;
* inertial navigation;
* distance sensing;
* closed-loop steering control;
* autonomous decision-making.

The entire system was designed around reliability and repeatability rather than maximum speed.

---

# Design Philosophy

During development we established several key objectives:

### Reliability

The robot should complete the course consistently even if lighting conditions, obstacle positions or track layouts vary.

### Simplicity

Every subsystem should be as simple as possible while still achieving the required performance.

### Maintainability

Hardware and software should be easy to modify, debug and repair between test sessions.

### Modularity

Vision, navigation and motion control are separated into independent modules to simplify testing and troubleshooting.

---

# Mechanical Design

## Chassis

The robot is built around a custom PCB chassis.

Instead of using a conventional frame and separate electronics board, the PCB serves both as the mechanical structure and electrical backbone of the robot.

This approach provides several advantages:

* reduced wiring;
* lower weight;
* improved rigidity;
* compact packaging;
* easier assembly.

The chassis also simplifies maintenance because sensors, power electronics and connectors are integrated directly into the structure.

---

## Drivetrain

The robot uses a rear-wheel-drive configuration.

Power is provided by a Pololu 30:1 HPCB gearmotor connected to a miniature differential system.

The differential allows both rear wheels to rotate at different speeds during cornering, reducing tire scrub and improving stability.

After testing several motor options, we selected the 30:1 gearbox because it offered the best compromise between:

* acceleration;
* top speed;
* parking precision;
* obstacle handling.

A faster motor reduced low-speed control, while slower options significantly increased lap times.

---

## Steering System

Steering is performed using an MG90S servo motor connected to a parallel steering linkage.

We deliberately chose a parallel steering configuration instead of Ackermann steering because:

* it is mechanically simpler;
* it requires fewer components;
* it is easier to calibrate;
* it produces predictable steering behavior.

Although Ackermann steering can improve cornering efficiency, our testing showed that the additional complexity was not justified for the competition environment.

---

## Downforce System

To increase traction without increasing mass, the robot uses an active downforce system.

A high-speed BLDC motor drives an impeller located in the center of the chassis. The impeller generates negative pressure underneath the robot, increasing the normal force applied to the driving wheels.

This solution improved:

* cornering stability;
* acceleration;
* braking consistency.

The impeller is activated throughout the run and controlled through an electronic speed controller connected to the main controller.

---

# Electronics Architecture

The robot uses a distributed architecture consisting of two processing systems.

## Main Controller

An Arduino Nano ESP32 handles:

* motion control;
* steering;
* sensor processing;
* state management;
* parking logic.

The ESP32 was selected because it provides sufficient processing power while maintaining low power consumption and a compact form factor.

---

## Vision Processor

Visual processing is performed by an OpenMV RT1062 camera.

The camera independently performs:

* obstacle detection;
* wall detection;
* color recognition;
* course direction recognition.

Instead of sending complete image data, the camera transmits only processed information to the ESP32 through UART communication.

This greatly reduces computational load on the main controller.

---

## Inertial Measurement Unit

A BMI088 IMU provides angular velocity and acceleration measurements.

The gyroscope is used primarily for heading estimation.

Because camera detections can occasionally be delayed or affected by lighting conditions, the IMU acts as the primary navigation reference during driving.

---

## Distance Sensors

Multiple distance sensors are positioned around the robot.

Their functions include:

* wall detection;
* lane positioning;
* parking alignment;
* direction determination at startup.

The combination of vision and distance sensing provides greater reliability than relying on a single sensing method.

---

# Software Architecture

The software is divided into two major subsystems.

## Vision System

The OpenMV camera continuously analyzes incoming frames and searches for:

* colored traffic signs;
* directional markers;
* walls;
* parking references.

The camera processes images using color thresholding and region-of-interest filtering.

Only important information is transmitted to the ESP32, reducing communication bandwidth and processing requirements.

---

# Hardware Components

The robot is built using a simple and compact hardware architecture designed to provide reliable autonomous operation while remaining easy to maintain and debug.

| Component | Function |
|------------|------------|
| Arduino UNO | Main controller responsible for motor control, sensor processing and navigation logic |
| HuskyLens | Computer vision system used for object and color recognition |
| ESP8266 (ESP-01/ESP-12 module) | Wireless communication and debugging interface |
| Servo Motor | Controls the steering angle of the front wheels |
| DC Motor | Provides propulsion for the robot |
| L298N Motor Driver | Controls the drive motor speed and direction |
| 4 Wheels | Ensure stable movement and traction on the competition field |
| Battery Pack | Supplies power to the entire robot |

---

## Controller

The Arduino UNO acts as the central processing unit of the robot. It receives information from the sensors and executes the navigation algorithms required for the challenge.

## Vision System

The HuskyLens camera is responsible for detecting colored objects and relevant field elements. Using onboard image processing, it provides simplified information to the Arduino, reducing computational requirements.

## Steering System

The steering mechanism is controlled by a servo motor connected to the front wheels. This allows the robot to perform accurate turns and maneuver through the course.

## Wireless Communication

An ESP8266 module is used for wireless communication and debugging during development. This allows quick monitoring of robot behavior and simplifies software testing.

## Drive System

The robot uses a DC motor connected to the rear axle through a motor driver. The four-wheel configuration provides stability and predictable handling during autonomous operation.

## Motion Control

The ESP32 executes all vehicle control functions.

Its responsibilities include:

* steering control;
* speed control;
* obstacle handling;
* parking execution;
* state transitions.

Motion stability is achieved through a gyroscope-based PD controller.

The controller continuously compares the desired heading with the measured heading and generates steering corrections.

This approach proved significantly more reliable than encoder-only navigation.

---

# State Machine

The robot operates as a finite state machine.

Each state is responsible for a specific task.

| State   | Description                  |
| ------- | ---------------------------- |
| WAIT    | Waiting for start signal     |
| DRIVE   | Normal autonomous navigation |
| FOLLOW  | Tracking a detected obstacle |
| AVOID   | Executing avoidance maneuver |
| RECOVER | Returning to driving path    |
| PARK    | Parallel parking procedure   |
| STOP    | End of run                   |

Using a state machine prevents different behaviors from interfering with each other and simplifies debugging.

---

# Navigation Strategy

## Open Challenge

During the Open Challenge, the robot follows a predefined driving strategy based on heading control.

The camera determines the course direction by detecting colored markers placed on the field.

Once the direction is known, the robot maintains its heading using the gyroscope and performs 90-degree turns when required.

The primary objective is consistent lap completion with minimal heading drift.

---

## Obstacle Challenge

The obstacle challenge extends the open challenge algorithm by adding traffic sign detection.

The camera identifies red and green signs and determines the relative position of each obstacle.

The robot then performs the following sequence:

1. Detect obstacle.
2. Track obstacle position.
3. Determine avoidance side.
4. Execute avoidance maneuver.
5. Return to nominal trajectory.

This approach allows the robot to react dynamically to randomized obstacle placement.

---

# Parking Strategy

After completing the required laps, the robot enters parking mode.

Parking is performed using:

* gyroscope heading;
* front distance sensor;
* rear distance sensor.

The procedure consists of:

1. Aligning with the parking lane.
2. Moving to a predefined staging position.
3. Executing a controlled parking maneuver.
4. Correcting final position.
5. Stopping completely.

Several parking strategies were tested during development before selecting the final implementation.

---

# Testing and Development

Testing played a critical role throughout the project.

Every major mechanical and software modification was validated through repeated track testing.

## Key Problems Encountered

| Problem                         | Cause                     | Solution                   |
| ------------------------------- | ------------------------- | -------------------------- |
| Steering oscillation            | Excessive controller gain | Controller retuning        |
| Inconsistent obstacle detection | Variable lighting         | Improved camera thresholds |
| Wide corner exits               | Excessive speed           | Corner speed reduction     |
| Parking inaccuracies            | Sensor noise              | Filtering and calibration  |
| Direction detection failures    | Marker visibility         | Updated camera ROI         |

---

## Iterative Development

The robot evolved through multiple revisions.

### Version 1

Focused on basic driving functionality and mechanical validation.

### Version 2

Improved sensor placement and parking consistency.

### Version 3

Introduced obstacle handling improvements and refined software architecture.

Each revision improved reliability and reduced failure rates during complete runs.

---

# Engineering Decisions

Several important design decisions shaped the final robot.

| Decision                | Reason                            |
| ----------------------- | --------------------------------- |
| PCB chassis             | Compact and lightweight structure |
| OpenMV vision           | Fast image processing             |
| ESP32 controller        | High performance and flexibility  |
| Differential drivetrain | Improved cornering behavior       |
| IMU-based navigation    | Stable heading estimation         |
| Parallel steering       | Simpler implementation            |
| Active downforce        | Increased traction                |

These choices were based on testing rather than theoretical performance alone.

---

# Repository Structure

```text
WRO-robot
│
├── 3D-models
├── github-commits
├── media
│   ├── robot-photos
│   └── team-photos
│
└── README.md
```

The repository documents the complete engineering process, including mechanical design, software development and testing.

---

# Future Improvements

Although the robot already performs all required competition tasks, several future improvements are possible.

Potential upgrades include:

* more advanced path planning;
* adaptive speed control;
* sensor fusion algorithms;
* automatic parameter tuning;
* enhanced parking accuracy.

These improvements could further increase reliability and overall performance.

---

# Conclusion

The HyperLine Robotics project combines mechanical engineering, electronics and embedded software into a compact autonomous vehicle designed for the WRO 2026 Future Engineers competition.

Throughout development, our focus remained on creating a reliable and repeatable system capable of operating under varying competition conditions.

The final robot successfully integrates computer vision, inertial navigation, distance sensing and autonomous decision-making into a single platform capable of completing both competition challenges.
