# Precision Parking – WRO 2026 Future Engineers

## Team Introduction

Precision Parking is a Romanian team participating in the World Robot Olympiad 2026 in the Future Engineers category.

The team consists of two members:

### Stefan Pandichi

Responsible for hardware integration, electronics assembly and robot construction.

### Horatiu Olteanu

Responsible for software development, testing and system optimization.

The purpose of this project is to design and build an autonomous vehicle capable of completing the WRO Future Engineers challenges while maintaining reliability and consistent performance.

---

# Project Overview

The WRO Future Engineers challenge requires teams to create a fully autonomous vehicle capable of navigating a competition field without human intervention.

The vehicle must:

* drive autonomously around the track;
* identify visual elements on the field;
* react to traffic signs and obstacles;
* complete multiple laps;
* perform a parking maneuver at the end of the run.

Our robot was designed with a strong focus on simplicity, reliability and ease of maintenance.

Instead of using a large number of sensors and complex hardware, we aimed to achieve the required functionality using a compact architecture built around computer vision and basic motion control.

---

# Design Philosophy

During development we focused on several important objectives.

## Reliability

The robot should complete its tasks consistently during repeated runs.

## Simplicity

The mechanical and electronic systems should remain simple enough to be easy to debug and repair.

## Modularity

Hardware and software components should be separated into independent subsystems.

## Repeatability

The robot should behave predictably and produce similar results under similar conditions.

---

# Hardware Components

The robot uses a compact hardware architecture built around commonly available educational robotics components.

| Component    | Function           |
| ------------ | ------------------ |
| Arduino UNO  | Main controller    |
| HuskyLens    | Vision processing  |
| L298N        | Motor driver       |
| Servo Motor  | Steering control   |
| DC Motor     | Vehicle propulsion |
| 4 Wheels     | Mobility system    |
| Battery Pack | Power supply       |

---

# Mechanical Design

## Chassis

The robot is built on a lightweight chassis designed to support all electronic and mechanical components while maintaining stability during movement.

Special attention was given to component placement in order to achieve balanced weight distribution.

The battery was positioned near the center of the robot to reduce instability during turns.

---

## Drive System

Movement is provided by a DC motor connected to the drive wheels through a simple transmission system.

The drivetrain was designed to provide enough torque for reliable movement while maintaining a reasonable driving speed.

The motor is controlled through an L298N motor driver connected directly to the Arduino UNO.

---

## Steering System

Steering is performed using a servo motor mounted at the front of the vehicle.

The servo allows precise wheel positioning and enables the robot to navigate corners and avoid obstacles.

Multiple steering angles were tested during development to find a balance between maneuverability and stability.

---

# Electronics Architecture

The robot uses a centralized control architecture.

The Arduino UNO acts as the main controller and coordinates all subsystems.

```text
HuskyLens
    │
    ▼
Arduino UNO
    │
 ┌──┴──┐
 ▼     ▼
Servo L298N
          │
          ▼
      DC Motor
```

The Arduino receives information from the HuskyLens camera and uses this information to generate movement commands.

---

# Vision System

The HuskyLens is responsible for visual recognition tasks.

The camera can detect and track objects, colors and visual markers placed on the field.

Using its onboard image processing capabilities, the HuskyLens reduces the amount of processing required from the Arduino UNO.

Instead of analyzing images directly, the Arduino only receives processed information from the camera.

This approach simplifies software development and improves system responsiveness.

---

# Software Architecture

The software running on the Arduino UNO is organized into multiple logical modules.

These modules are responsible for:

* receiving data from the HuskyLens;
* controlling motor speed;
* controlling steering angle;
* obstacle handling;
* parking execution.

Separating functionality into independent modules simplified testing and debugging.

---

# State Machine

The robot operates using a finite state machine.

Each state represents a specific behavior.

| State  | Function                |
| ------ | ----------------------- |
| WAIT   | Waiting for start       |
| DRIVE  | Normal navigation       |
| DETECT | Visual object detection |
| AVOID  | Obstacle avoidance      |
| PARK   | Parking procedure       |
| STOP   | End of run              |

Using a state machine prevents conflicts between different behaviors and improves software reliability.

---

# Navigation Strategy

## Normal Driving

During normal operation, the robot continuously moves forward while monitoring information received from the HuskyLens.

Based on the detected visual elements, the robot adjusts its steering angle and maintains its trajectory.

---

## Obstacle Handling

When the HuskyLens detects a relevant obstacle or traffic sign, the robot performs a predefined avoidance maneuver.

The maneuver consists of:

1. Detecting the obstacle.
2. Determining the required direction.
3. Adjusting steering.
4. Passing the obstacle.
5. Returning to the original path.

This process allows the robot to react autonomously during the challenge.

---

# Parking Strategy

At the end of the run, the robot enters parking mode.

The parking procedure consists of:

1. Detecting the parking area.
2. Aligning the vehicle.
3. Adjusting steering angle.
4. Moving into the parking space.
5. Stopping completely.

Several parking approaches were tested before selecting the final solution.

The final implementation prioritizes reliability over speed.

---

# Testing and Development

Testing was an essential part of the development process.

Each modification was validated through multiple trial runs.

The main testing objectives were:

* steering accuracy;
* obstacle detection;
* driving consistency;
* parking precision;
* overall reliability.

---

## Main Problems Encountered

| Problem                       | Solution                       |
| ----------------------------- | ------------------------------ |
| Unstable steering             | Servo recalibration            |
| Inconsistent object detection | HuskyLens parameter adjustment |
| Wide turns                    | Reduced vehicle speed          |
| Parking errors                | Improved parking sequence      |
| Mechanical vibration          | Chassis reinforcement          |

---

# Development Process

The robot evolved through several development stages.

### Version 1

Basic mechanical platform with manual control testing.

### Version 2

Integration of HuskyLens and autonomous navigation.

### Version 3

Improved obstacle handling and parking performance.

Each version introduced improvements based on practical testing results.

---

# Engineering Decisions

| Decision          | Reason                                         |
| ----------------- | ---------------------------------------------- |
| Arduino UNO       | Simple and reliable controller                 |
| HuskyLens         | Easy integration and onboard vision processing |
| L298N Driver      | Reliable motor control                         |
| Servo Steering    | Accurate directional control                   |
| Four-Wheel Layout | Stable movement platform                       |

These decisions helped keep the robot simple while meeting competition requirements.

---

# Repository Structure

```text
WRO-robot
│
├── 3D-models
├── media
│   ├── robot-photos
│   └── team-photos
│
└── README.md
```

---

# Future Improvements

Potential future improvements include:

* faster obstacle recognition;
* smoother steering control;
* adaptive speed management;
* improved parking precision;
* more advanced navigation algorithms.

---

# Conclusion

The Precision Parking project demonstrates how a compact autonomous vehicle can be developed using accessible robotics components and computer vision technologies.

By combining the Arduino UNO, HuskyLens vision system and a simple mechanical platform, we created a robot capable of autonomously navigating the competition environment and completing the required tasks.

The project provided valuable experience in robotics, programming, electronics and engineering design while preparing for participation in the WRO 2026 Future Engineers competition.
