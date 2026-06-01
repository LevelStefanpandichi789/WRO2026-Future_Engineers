# Summary
This is a fully autonomous WRO 2026 Future Engineers robot designed from scratch by Precision Parking from Constana, Romania. It features a parallelogram steering system, runs on a Pololu 30:1 HPCB micro gearmotor through a sealed RC differential, and is controlled by an Arduino Nano ESP32. The robot navigates a randomized track, detects and avoids colored traffic sign cubes in real time using an OpenMV H7 camera, maintains heading using a BMI088 IMU with gyro PID, and executes a precise 3-maneuver parallel parking sequence after completing three full laps.

Robotics & Automation > Autonomous Vehicles

Tags: wro2026 esp32 autonomous parallel-parking pcb-chassis computer-vision rc-differential servo-steering openmv imu downforce pid encoder-odometry jlcpcb


# 
Precision Parking
WRO 2026 Future Engineers Robot
updated [DATE] | published [DATE]


---


# The Team

# Stefan Pandichi
Age: 17
High School: Colegiul National Pedagogic "Constantin Bratescu"

Hi! I'm Stefan Pandichi from Romania, and this is my first WRO season. This is my first season in Future Engineers. I am passionate about robotics, especially electronics and the latest algorithms and tech. Over the years I have worked on multiple robotics projects including line followers, sumo bots, and air quality modules. Apart from robotics I also enjoy cybersecurity, programming, and cycling.

# Olteanu-Mihail Horatiu-Anton
Age: 15
High School: Colegiul National "Mihai Eminescu"

Hi! I'm Olteanu-Mihail Horatiu-Anton from Romania, and this is my 1st WRO season competing alongside Stefan Pandichi. I have participated in RoboMission multiple times, gaining valuable experience in solving various problems that may arise while building a robot. I have a strong interest in technology and robotics and am always eager to learn and experiment with new ideas.


---


# Challenge Overview

The WRO 2026 Future Engineers challenge pushes teams to develop a fully autonomous vehicle capable of navigating a dynamic and randomized racetrack using sensors, computer vision, and advanced control algorithms. The goal is to complete multiple laps while adapting to randomized obstacles, following strict driving rules, and successfully executing a parallel parking maneuver at the end of the course.

Open Challenge: The vehicle must complete three laps on a track with randomly placed inside walls, navigating purely by sensors and gyro heading.

Obstacle Challenge: The vehicle must complete three laps while detecting and responding to randomly placed red and green traffic sign cubes. Red markers mean the vehicle must stay on the right side of the lane. Green markers mean the vehicle must stay on the left side of the lane. After completing the three laps, the vehicle must locate the designated parking zone and perform a precise parallel parking maneuver within a limited space.

Scoring is based on accuracy, technical documentation, and speed, rewarding teams that balance efficiency, adaptability, and innovation.


---


# The Robot

The robot is built around a custom PCB chassis manufactured by JLCPCB, which serves simultaneously as the structural frame and the electronics mainboard. This design eliminates the need for a separate electronics board, reduces total weight, and ensures clean and reliable wiring throughout. The drivetrain uses a sealed RC differential at the rear driven by a Pololu 30:1 HPCB micro gearmotor through a 3D-printed pinion gear. The motor is held in a 3D-printed support with the Li-Po battery mounted directly above it, keeping the center of mass low and centered. At the front, fully 3D-printed wheel hubs each run on two bearings (inner and outer) for a rigid wobble-free axle. A downforce impeller (1020 coreless motor) pulls air from underneath the robot, increasing grip on all four wheels without adding mass.

Parts List:

1x Arduino Nano ESP32
1x OpenMV H7 Camera
1x BMI088 IMU Sensor
1x Pololu 30:1 HPCB Micro Metal Gearmotor 6V with encoder
1x MG90S Micro Servo Motor
1x 1020 Coreless DC Motor (downforce impeller)
1x IFX9201SG Motor Driver
1x RFR3411 MOSFET (impeller PWM low-side switch)
1x D24V50F5 Voltage Regulator (5V, 5A)
1x 2S 300mAh Li-Po Battery (7.4V, 60C)
1x Sealed RC Differential (rear drivetrain)
4x Pololu PWM Digital Distance Sensors (front, back, left, right)
4x Cast silicone tires on 3D-printed dual-bearing hubs
3D-printed: servo base, 90 degree camera mount bracket, motor support, 4x bearing rings, front wheel hubs
Custom PCB chassis ordered from JLCPCB
2x M2 screws for steering hub pivots
Various M3 screws, nylon locking nuts, and washers


---


# Mobility Management

Drivetrain

The drivetrain uses a sealed RC differential at the rear, driven by a Pololu 30:1 HPCB micro gearmotor through a 3D printed pinion to differential input gear. The motor is held in a 3D-printed support with the battery mounted above, keeping the center of mass centered and low. Rear outputs rotate in bearings seated inside 4 printed rings that are super-glued to the PCB chassis, minimizing friction and parts count. At the front, the wheels are fully 3D-printed and each wheel runs on two bearings (inner and outer) for a rigid wobble-free hub that steers precisely.

To maximize grip on the track without adding mass, we use a downforce impeller (1020 coreless motor) that pulls air from under the robot, increasing the normal force. The impeller is PWM-controlled via an RFR3411 MOSFET (low-side switch). The drive motor is controlled by an IFX9201SG driver (PWM + DIR) with an active brake pulse for precise stopping, while the encoder on the gearmotor provides odometry for short accurate moves such as avoidance hops and parking nudges.

Motor: Pololu 30:1 HPCB Micro Metal Gearmotor 6V
Model: 30:1 HPCB | Voltage: 6V | No-load Speed: 1000 RPM | No-load Current: 120mA | Stall Torque: ~0.4 kg/cm | Stall Current: 1.6A | Encoder: Yes (12 CPR) | Function: Drives the robot

Following past testing we selected the high-power 30:1 Micro Metal Gearmotor (6V) for the drive system. This motor provides an optimal balance of speed and torque allowing the robot to maintain stability while navigating turns. The built-in encoder (12 counts per revolution) combined with the gear ratio and wheel diameter gives a conversion factor of 1.8326 mm per tick, enabling centimeter-level odometry accuracy essential for precise parking nudges and avoidance hops.

Wheels and Tires (Silicone)
Our robot uses cast silicone tires on 3D-printed hubs. Silicone provides high repeatable static friction on painted boards and vinyl, which pairs perfectly with the rear differential and downforce impeller. Rims are 3D-printed hubs with dual bearings (inner and outer) for a rigid wobble-free wheel. The tire is a cast silicone ring fitted onto a mechanical bead on the rim with no harsh solvents needed. Rear wheels mount directly to the diff outputs and fronts ride on steering hubs for low friction.

Motor Driver: IFX9201SG
Operating Voltage: 5.5V to 45V | Logic Voltage: 3.3V / 5V compatible | PWM Frequency: up to 20 kHz | Max Continuous Current: 5A | Max Peak Current: 8A per channel | Protections: Overtemperature, Overcurrent, Undervoltage, Short-to-GND/Battery | Function: Controls the drive motor

The IFX9201SG motor driver is integrated directly into the PCB chassis, eliminating external wiring to the driver and ensuring compact reliable communication with the Arduino Nano ESP32.

Impeller: 1020 Coreless DC Motor
Type: Coreless DC | Voltage: 3.7V to 7.4V | Shaft Diameter: 1.0mm | No-Load Speed: ~53,000 RPM @ 3.7V | Weight: ~4.5g | Average Current: ~1A @ 3.7V | Peak Current: ~2.5A | Function: Drives the downforce impeller


---


Steering

The steering system is based on a parallelogram mechanism where both front wheels turn at the same angle through a single servo-controlled linkage. This provides predictable stable steering without the complexity of Ackermann geometry, making it well suited for an autonomous vehicle with PID control. The maximum turning angle is 80 degrees in both directions, enabling the robot to navigate sharp corners efficiently. The steering arm is directly connected to the MG90S servo output shaft. Both front wheels move simultaneously ensuring immediate and proportional response. The wheels pivot on M2 screw axles allowing smooth rotation with minimal friction.

Steering Servo: MG90S
Model: MG90S | Voltage: 5V | Torque: 2.2 kg/cm | Signal Type: PWM | Average Current: 120mA | Peak Current: 500mA | Weight: ~13.4g | Gears: Plastic | Function: Controls steering

Servo positions are calibrated experimentally. STEERING_LEFT is set to 140, STEERING_CENTER to 85, and STEERING_RIGHT to 40. The steer() function accepts a normalized input in the range -1 to +1 and maps it to these calibrated values, allowing higher-level PID controllers to output normalized steering commands without worrying about servo limits directly.


---


Chassis and Component Mounting

At the national stage we used a fully 3D-printed chassis which allowed us to quickly prototype a compact and optimized structure. For the current version we upgraded to a PCB-based chassis manufactured by JLCPCB. This PCB acts as both the structural frame and the electronics mainboard simultaneously, integrating motor driver, voltage regulator, sensor headers, servo connector, and ESP32 socket directly into the board. This not only reduces weight further but also integrates electronics directly into the structure making the robot lighter, cleaner, and more reliable.

Key features: the PCB chassis ensures a strong lightweight structure optimizing performance. The battery is centrally placed on top of the motor ensuring even weight distribution and stability. The high power impeller sucks air under the robot and creates an artificial downforce effect maintaining grip. Pre-designed slots for motor, steering servo, and camera make assembly quick and efficient. Integrated PCB routing eliminates unnecessary wiring ensuring a cleaner and more reliable setup. Super glue secures servo and other small parts keeping each part in place even after longer runs.

3D-printed components include the servo base (super-glued to PCB), 90 degree camera mount bracket (2 screws to chassis, 2 screws to camera), motor support (fixed with 2x M3 screws), 4x bearing rings (glued to PCB for rear differential rotation), and front wheel hubs (each with 2 bearings, inner and outer).


---


Power and Sensing

Li-Po Battery: 2S 300mAh
Model: 2S Li-Po | Capacity: 300mAh | Voltage: 7.4V | Discharge Rate: 60C | Weight: 12g | Connector: JST | Function: Powers the entire robot

Arduino Nano ESP32: Main Controller
Microcontroller: ESP32 | Flash Memory: 4MB | SRAM: 520KB | Frequency: 240MHz | Input Voltage: 5V | Average Current: 200mA | Peak Current: 500mA | Function: Controls all robot components

IMU Sensor: BMI088
Gyroscope Range: +/-2000 deg/s | Accelerometer Range: +/-24g | Interface: I2C / SPI | Supply Voltage: 3.0V to 3.6V | Current Draw: ~3.2mA | Function: Tracks orientation and yaw angle

The gyroscope is configured at 400 Hz with 47 Hz bandwidth. A drift calibration routine runs at startup where the sensor averages its output while stationary for a set period, computing a drifts_z bias that is subtracted from all subsequent readings. The integrated yaw angle gz is used directly as input to the PID steering controller.

OpenMV H7 Camera: Vision Processing
Microcontroller: STM32H7 | Flash Memory: 32MB | RAM: 512KB | Frequency: 480MHz | Resolution: 640x480 | Frame Rate: 60fps | Average Current: 300mA | Function: Detects traffic sign cubes and track lines

The camera runs a Python script processing frames in RGB565 format at QVGA (320x240) resolution with fixed exposure, gain, and white balance for consistent LAB color thresholds. It communicates with the ESP32 over UART at 19200 baud sending compact single-line messages. Messages sent by the camera: S+-0.XXX for normalized horizontal error for cube following, RED or GREEN when a cube is close to trigger avoidance, BLUE or ORANGE when a track line is detected to set lap direction, BLACK when a wall is detected to trigger a 90 degree turn, and 1 or 2 for current confirmed lap direction sync.

Pololu PWM Distance Sensors (x4)
Detection Range: 50cm | Output: Digital pulse width where HIGH-time encodes distance | Supply Voltage: 3.0V to 5.5V | Current Draw: ~30mA enabled / ~0.4mA disabled | Resolution: 3mm | Update Rate: ~50 to 110 Hz | Function: Wall sensing, parking alignment, lap direction detection at start

Each sensor is placed on one side of the chassis (front, back, left, right). Distance is read using pulseIn() and converted to millimeters. A timeout of 30ms returns -1 for invalid readings.

D24V50F5 Voltage Regulator
Input Voltage: 6V to 38V | Output Voltage: 5V | Output Current: 5A | Protections: Short-circuit and thermal shutdown | Function: Converts 7.4V Li-Po to stable 5V for all logic components


---


Components Coding

Drive Motor

The motor driver is based on the Infineon IFX9201SG which allows us to directly manage the motor with only two control signals: a PWM pin that regulates speed and a direction pin that selects forward or reverse rotation. No external library is required for motor control. The move(int speed) function takes an input in the range -255 to +255. The absolute value is written to the PWM channel while the sign determines rotation direction. A dedicated stop_motor() function applies a short reverse torque pulse before setting the PWM duty cycle to zero, ensuring the robot stops quickly without uncontrolled coasting. Since the robot is powered by an ESP32, PWM signals are generated using the LEDC hardware utility.

void motor_driver_setup() {
  pinMode(PWMA, OUTPUT);
  pinMode(DIRA, OUTPUT);
  ledcSetup(PWM_MOTOR_CHANNEL, PWM_MOTOR_FREQ, PWM_MOTOR_RESOLUTION);
  ledcAttachPin(PWMA, PWM_MOTOR_CHANNEL);
  move(0);
}

void move(int speed) {
  ledcWrite(PWM_MOTOR_CHANNEL, abs(speed));
  digitalWrite(DIRA, speed > 0 ? LOW : HIGH);
}

void stop_motor() {
  move(-3);
  delay(100);
  move(0);
}

For odometry the motor includes an encoder. An interrupt-based approach counts ticks in real time, avoiding the need for a dedicated library. The encoder provides 12 counts per revolution and by applying the gear ratio, wheel diameter, and pi, we obtain a conversion factor of MM_PER_TICK = 1.8326. This enables the robot to measure traveled distance with centimeter-level accuracy.

void encoder_setup() {
  pinMode(ENCODER_A, INPUT_PULLUP);
  pinMode(ENCODER_B, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(ENCODER_A), encoderISR, RISING);
  encoder_ticks = 0;
}

float read_cm() {
  noInterrupts();
  int32_t t = encoder_ticks;
  interrupts();
  float dist_mm = t * MM_PER_TICK;
  float dist_cm = dist_mm / 10.0f;
  return dist_cm;
}

Impeller

The impeller generates downforce to increase stability and traction at high speed. We use an RFR3411 MOSFET wired as a low-side switch. Only a single PWM pin from the ESP32 is needed to drive the MOSFET gate. The PWM duty cycle determines how much power is applied to the impeller: 0 is off and 255 is full speed.

void impeller_setup() {
  pinMode(PWM_IMPELLER, OUTPUT);
  ledcSetup(PWM_IMPELLER_CHANNEL, PWM_IMPELLER_FREQ, PWM_IMPELLER_RESOLUTION);
  ledcAttachPin(PWM_IMPELLER, PWM_IMPELLER_CHANNEL);
}

void setImpeller(int _pwm) {
  ledcWrite(PWM_IMPELLER_CHANNEL, constrain(_pwm, 0, 255));
}

Servo Motor

The steering system is controlled by a MG90S servo which adjusts the angle of the front wheels. Three reference positions are defined: STEERING_LEFT = 140, STEERING_CENTER = 85, STEERING_RIGHT = 40. These values were experimentally calibrated to match the geometry of the steering mechanism. The steer(double steering_angle) function accepts an input in the range -1 to +1 representing normalized steering where -1 is maximum right, 0 is centered, and +1 is maximum left.

void steering_servo_setup() {
  steeringServo.attach(STEERING_SERVO);
  delay(100);
  steeringServo.write(STEERING_LEFT);
  delay(300);
  steeringServo.write(STEERING_RIGHT);
  delay(300);
  steeringServo.write(STEERING_CENTER);
  delay(300);
}

void steer(double steering_angle) {
  if (steering_angle > 1)  steering_angle = 1;
  if (steering_angle < -1) steering_angle = -1;
  steering_angle = map_double(steering_angle, -1, 1, STEERING_LEFT, STEERING_RIGHT);
  steeringServo.write(steering_angle);
}

IMU

To keep the robot on a straight path and execute precise turns we rely on the Bosch BMI088 IMU. We primarily use the Z-axis gyroscope for yaw angle estimation. During initialization gyro_setup() configures the BMI088 via I2C. A key challenge with gyroscopes is drift, the tendency of small measurement errors to accumulate over time. To correct this the setup function performs a calibration routine: for a predefined duration the sensor output is averaged while the robot is stationary, producing a drifts_z bias subtracted from all subsequent readings.

void gyro_setup() {
  Wire.begin();
  imu.initialize();
  imu.setAccScaleRange(RANGE_6G);
  imu.setAccOutputDataRate(ODR_200);
  imu.setAccPoweMode(ACC_ACTIVE);
  imu.setGyroScaleRange(RANGE_2000);
  imu.setGyroOutputDataRate(ODR_400_BW_47);
  imu.setGyroPoweMode(GYRO_NORMAL);

  double start = millis();
  gyro_last_read_time = start;
  gz = 0;

  while (millis() - start < DRIFT_TEST_TIME * 1000) {
    double now = millis();
    double dt = (now - gyro_last_read_time) * 0.001;
    float rate = imu.getGyroscopeZ();
    gz += rate * dt;
    gyro_last_read_time = now;
  }

  drifts_z = gz / DRIFT_TEST_TIME;
  gz = 0;
  gyro_last_read_time = millis();
}

void read_gyro_data() {
  double now = millis();
  double dt  = (now - gyro_last_read_time) * 0.001;
  float rate = imu.getGyroscopeZ();
  double corrected = rate - drifts_z;
  gz += corrected * dt;
  gyro_last_read_time = now;
}

Distance Sensors

The robot integrates four Pololu PWM distance sensors placed on all four sides. Each sensor outputs a pulse width signal where the duration is proportional to the measured distance. The pulseToMM() function converts pulse width to millimeters using PW_OFFSET_US = 1000.0 microseconds offset at ~0 mm and PW_US_PER_MM = 1.0 microsecond per millimeter. If no valid pulse is received the function returns -1 to indicate a timeout.

static const float PW_OFFSET_US   = 1000.0f;
static const float PW_US_PER_MM   = 1.0f;
static const unsigned long PULSE_TIMEOUT_US = 30000UL;

static inline int distanceSensorPin(DistanceDir d) {
  switch (d) {
    case FRONT_DIR: return PWM_DIST_FRONT;
    case LEFT_DIR:  return PWM_DIST_LEFT;
    case RIGHT_DIR: return PWM_DIST_RIGHT;
    case BACK_DIR:  return PWM_DIST_BACK;
  }
  return PWM_DIST_FRONT;
}

static inline float pulseToMM(unsigned long pw_us) {
  if (pw_us == 0) return -1.0f;
  float mm = (float(pw_us) - PW_OFFSET_US) / PW_US_PER_MM;
  if (mm < 0) mm = 0;
  return mm;
}


---


Obstacle Management

Open Round

During the Open Round the robot follows a straight trajectory using a PID controller based on gyro yaw ensuring stable movement. To determine turns the camera detects Orange and Blue lines on the track. An Orange line means a right turn and a Blue line means a left turn. The turn is executed when the robot reaches an approximate distance from the front black wall detected by the camera.

Obstacle Round: Four-State Machine

The ESP32 runs a state machine with four states.

PID is the default state for gyro PD straight-line driving. It uses a PD loop on the integrated gyro yaw gz versus the desired heading current_angle_gyro, calling steer(pid_error) and move(robot_speed) to drive straight.

FOLLOW_CUBE activates when the camera sends a steering follow message. The servo is set directly to the camera's steering hint to chase the nearest visible cube. If no follow message arrives within 250-500ms the robot returns to PID since the cube is assumed lost or passed.

AVOID_CUBE triggers when a RED or GREEN proximity message arrives (cube is very close). A hardcoded avoidance maneuver executes: turn ~37 degrees away from the cube, drive ~8cm forward while holding that heading, then transition to AFTER_CUBE.

AFTER_CUBE re-aligns the robot to the track center using gyro PID while ignoring all camera commands for a set period, flushes the UART buffer, then returns to PID.

switch (currentState) {
  case PID:
    double err = current_angle_gyro - gz;
    pid_error = (err) * kp + (pid_error - pid_last_error) * kd;
    pid_last_error = pid_error;
    steer(pid_error);
    move(robot_speed);
    break;

  case FOLLOW_CUBE:
    if (millis() - last_follow_cube > FOLLOW_CUBE_LOST_TIME) {
      currentState = PID;
    }
    steer(follow_cube_angle);
    move(robot_speed);
    break;

  case AVOID_CUBE:
    pass_cube(cube_avoid_direction);
    break;

  case AFTER_CUBE:
    while (millis() - last_cube_time > 1500) {
      read_gyro_data();
      double error = current_angle_gyro - gz;
      pid_error = (error)*kp + (pid_error - pid_last_error) * kd;
      pid_last_error = pid_error;
      steer(pid_error);
      move(robot_speed);
    }
    currentState = PID;
    cameraSerial.flush();
    break;
}

Cube Avoidance Subroutine

When in AVOID_CUBE the robot executes a hardcoded movement including a fixed turn plus move forward maneuver to avoid the cube, then transitions to AFTER_CUBE.

void pass_cube(char cube_direction) {
  read_gyro_data();
  int sign = (cube_direction == 'R') ? 1 : -1;
  double start_angle  = gz;
  double target_angle = start_angle - sign * AVOIDANCE_ANGLE;
  move_until_angle(robot_speed, target_angle);
  move_cm_gyro(8, robot_speed, target_angle);
  last_cube_time = millis();
  currentState   = AFTER_CUBE;
  flush_messages();
}

Starting from Parking

At the start the robot reads the left and right distance sensors. The side with the greater reading indicates the direction to exit. The robot steers ~75 degrees toward the track, reverses slightly to align, and enters its standard operating loop.

if (RUN_MODE == 1) {
  if (readDistanceMM(LEFT_DIR, 3) < readDistanceMM(RIGHT_DIR, 3)) {
    move_until_angle_max(exit_speed, 75);
    move(65);
    delay(100);
    turn_direction = 1;
    move_until_angle_max(exit_speed, 0);
    move_straight_on_gyro(-robot_speed, 900);
  } else {
    move_until_angle_max(exit_speed, -70);
    move(65);
    delay(100);
    turn_direction = -1;
    move_until_angle_max(exit_speed, 0);
    move_straight_on_gyro(-robot_speed, 1800);
  }
}


---


Parallel Parking Sequence

After completing 12 turns (3 full laps) the robot executes a fully deterministic parallel parking routine using front and back distance sensors together with the gyro angle. The sequence has five phases.

Phase 1: Front-Wall Alignment. The robot drives forward until it reaches a fixed offset from the front wall: 470mm for the left course and 460mm for the right course. This establishes a consistent longitudinal reference point for the entire parking sequence.

Phase 2: Lane Reorientation. A 90 degree snap turn into the parking lane updates current_angle_gyro by adding turn_direction multiplied by 90. On left courses an additional -3 degree trim compensates for accumulated gyro drift.

Phase 3: Lane Squaring and Staging. The robot drives forward 200mm while holding heading, then reverses until the back sensor reads 300mm from the rear wall. It then advances to the staging distance: 900mm from the front wall on the left course and 1590mm on the right course.

Phase 4: Three-Maneuver Parallel Parking. For the left course: reverse arc to -75 degrees, straighten backwards to -5 degrees, forward tuck to +5 degrees. For the right course: reverse arc to +75 degrees, straighten backwards to +5 degrees, forward tuck to -2 degrees.

Phase 5: Final Stop. Servo centers, motor cuts to zero, robot enters a 20-second idle state ending the run.

if (turn_count == 12 && RUN_MODE == 1) {
  move_straight_on_gyro(robot_speed, 500);
  if (turn_direction == -1) {
    move_to_distance(FRONT_DIR, 470.0f, 10.0f, 55, current_angle_gyro);
    current_angle_gyro += turn_direction * 90;
    current_angle_gyro -= 3;
    move_until_angle_max(60, current_angle_gyro);
    move_straight_on_gyro(robot_speed, 200);
    move_to_distance(BACK_DIR, 300.0f, 10.0f, -55, current_angle_gyro);
    move_to_distance(FRONT_DIR, 900.0f, 10.0f, 55, current_angle_gyro);
    move_until_angle_max(-60, (current_angle_gyro) - 75);
    move_until_angle_max(-60, (current_angle_gyro) - 5);
    move_until_angle_max(60, (current_angle_gyro) + 5);
  } else {
    move_to_distance(FRONT_DIR, 460.0f, 10.0f, 55, current_angle_gyro);
    current_angle_gyro += turn_direction * 90;
    move_until_angle_max(60, current_angle_gyro);
    move_straight_on_gyro(robot_speed, 200);
    move_to_distance(BACK_DIR, 300.0f, 10.0f, -55, current_angle_gyro);
    move_to_distance(FRONT_DIR, 1590.0f, 10.0f, 55, current_angle_gyro);
    move_until_angle_max(-63, (current_angle_gyro)+75);
    move_until_angle_max(-63, (current_angle_gyro)+5);
    move_until_angle_max(60, (current_angle_gyro)-2);
  }
  steeringServo.write(STEERING_CENTER);
  delay(300);
  move(0);
  delay(20000);
}


---


Cost Analysis

Components:
Arduino Nano ESP32 - $21.42
Drive Motor 6V 30:1 HPCB - $22.45
IFX9201SG Motor Driver - $19.92
Steering Servo MG90S - $4.05
OpenMV H7 Camera - $80.00
BMI088 Gyroscope - $8.50
Pololu Distance Sensor x4 - $71.80
LiPo Battery 2S 300mAh - $5.60
D24V50F5 Voltage Regulator - $29.95
Custom Silicone Wheels x4 - $36.96
RC Differential - $19.04
Experimental Parts - $35.00
Total Component Cost: $354.39

PCB Manufacturing via JLCPCB:
PCB Manufacturing (5 boards) - $39.10
PCB Assembly (5 boards) - $72.75
Total PCB Cost: $111.85

3D Printing:
1000g filament PLA and PLA-CF - $20.00

Other Materials (screws, wiring, connectors) - $9.00

Total Project Cost: $495.24


---


Repository Structure

The repository is organized as follows. The 3D-models folder contains all STL and STEP files for printed parts including old design iterations. The electrical-schematics folder contains PCB design files and circuit diagrams. The github-commits folder contains commit logs and change tracking. The media folder contains robot photos, team photos, and recorded testing videos. The src folder contains the full source code including C++ firmware for the ESP32 and Python vision script for the OpenMV. The technical-draws folder contains mechanical blueprints. The video folder contains competition and demo footage.

Let me know what you think! The 3D model files are in the /3D-models folder, electrical schematics in /electrical-schematics, and all source code in /src. Feel free to fork and modify for your own autonomous vehicle build. The Fusion360-equivalent design files are included so feel free to modify and make it best for your use case.

Github: https://github.com/andreipopescufilimon/WRO2026_Future_Engineers