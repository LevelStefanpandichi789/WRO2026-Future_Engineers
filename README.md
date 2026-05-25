# Summary

This is a fully autonomous WRO 2025 Future Engineers robot designed from scratch by the Nerdvana Taurus Team from Romania. It features a parallelogram steering system, a custom PCB chassis with integrated electronics, a downforce impeller, and performs a precise parallel parking maneuver at the end of each run.

**Category:** Robotics & Automation > Autonomous Vehicles

**Tags:** `wro2025` `esp32` `autonomous` `parallel-parking` `pcb-chassis` `computer-vision` `rc-differential` `servo-steering` `openmv` `imu`

---

This is our custom-built autonomous car for the WRO 2025 Future Engineers competition. The chassis is a custom PCB manufactured by JLCPCB, acting as both the structural frame and the electronics mainboard — reducing weight and integrating all wiring directly into the structure. The robot navigates a randomized track, detects and avoids colored traffic sign cubes using an OpenMV H7 camera, and executes a 3-maneuver parallel parking sequence after completing 12 turns.

**Parts List:**

- 1x Arduino Nano ESP32
- 1x OpenMV H7 Camera
- 1x BMI088 IMU Sensor
- 1x Pololu 30:1 HPCB Micro Metal Gearmotor 6V (with encoder)
- 1x MG90S Micro Servo Motor
- 1x 1020 Coreless DC Motor (downforce impeller)
- 1x IFX9201SG Motor Driver
- 1x RFR3411 MOSFET (impeller PWM control)
- 1x D24V50F5 Voltage Regulator (5V, 5A)
- 1x 2S 300mAh Li-Po Battery (7.4V, 60C)
- 1x Sealed RC Differential (rear drivetrain)
- 4x Pololu PWM Distance Sensors (front, back, left, right)
- 4x Cast silicone tires on 3D-printed dual-bearing hubs
- 3D-printed: servo base, camera mount, motor support, bearing rings, front wheels
- Custom PCB chassis (JLCPCB)
- M2 and M3 screws, nuts, washers

---