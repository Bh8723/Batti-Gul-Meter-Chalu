# Batti-Gul-Meter-Chalu

## Hackathon: Campus Power Quest

### Team Information
- **Team Name:** Batti Gul Meter Chalu
- **Team Members:**
  - Bhavya Singhania (Automation)
  - Arjav Jayesh Patel (Mechanical Design)
  - Harsh Gupta (Automation)
  - Abhisek Kumar (Electronics)
  - Aditya Khemkha (Electronics)
  - Hriday Shrimal (Mechanical Design)

### Project Overview
This project is an advanced electrical surveillance robot capable of navigating the BITS Goa campus, analyzing power machinery, and identifying regions of electrical failure. The robot integrates mechanical, electronics, and automation systems to achieve autonomous navigation and task execution.

### Repository Structure
- **Mechanical:** Contains CAD files and necessary documentation.
- **Electronics:** Includes PCB design files.
- **Automation:** Holds the ROS2 package for path planning, control, and cone detection.
- **Documentation:** Comprehensive report and additional references.

### Task Breakdown

# Mechanical Design
The link to the Fusion file: [Fusion File](https://a360.co/3M39VT6)

- **Suspension:** Shock absorbers for each wheel.
- **Rear Wheels:** Airless wheels for internal damping and shock absorption, larger in size for better stair climbing.
- **Front Wheels:** Dolly system incorporated for stair climbing with smaller wheels for decreased weight.
- **Body:** Hollow body made of lightweight materials like carbon fiber to protect electronics and reduce weight.
- **Robotic Arm:** A standard 3 DOF robotic arm mounted on a 360° base for increased mobility.

**Areas of Further Improvement:**
- Incorporate a toolbox for basic repair work.
- Add a scissor lift for vertical reach.
- Add a rubber track for better grip while climbing stairs.

# Electronics

In our surveillance robot project, we completed several key tasks related to electronics and power management. Here’s a summary:

##### 1. Tinkercad Circuit Design
Link to the project file: [Tinkercad Project]([https://www.tinkercad.com/things/bRyijdVhIUR-erc-2?sharecode=3oIzXF7Mm18kAz-Btp90mGPKjFg-ehq5pCYR2AVXfwg](https://www.tinkercad.com/things/eqRMwIjnBey-copy-of-erc-2?sharecode=OZxS86ysJPRXR7avZWXkSrCzZDRnlIUP_vu86FL3ntc))

Components:
- **Arduino UNO Shield:** Main controller for the robot.
- **Two Motor Drivers (L293D):** Control four DC motors for wheel movement.
- **Ultrasonic Sensor:** For obstacle detection. Note: A LIDAR sensor would be used in real-world applications.
- **Servo Motor:** Simulated a camera that rotates towards detected obstacles or incidents.

The robot uses a differential drive mechanism for effective movement.

##### 2. PCB Design
(File included)

Our PCB design includes:
- **Arduino UNO Shield:** Central control unit.
- **DPDT Relay:** Controls the robotic arm for picking and dropping objects.
- **L7805 5V Voltage Regulator:** Provides stable 5V power to various components.
- **NRF24L01P:** Communication module for long-distance data transmission.
- **ACS-712-05B:** Current sensor for monitoring power usage.
- **Screw Terminals:** For connecting components like the ultrasonic sensor, robotic arm, and servo motor.
- **Mounting Holes for Cytron Motor Driver:** Securely attach the motor driver to the PCB.

A 5V voltage regulator ensures components receive the correct voltage. With an external PA (Power Amplifier) and LNA (Low Noise Amplifier), the communication range can be extended to approximately 1000 meters (1 km) in open space.

#### Power Calculations

For the project, the following power calculations have been made to ensure that the 3S8P LiPo battery (17,600mAh) will be sufficient for at least 1 hour of operation.

**Components and Power Consumption:**

- **Arduino UNO Shield:** `P = 11.1V × 0.2A = 2.22W`
- **DPDT Relay:** `P = 5V × 0.1A = 0.5W`
- **L7805 Voltage Regulator:** (Depends on load, assume it's driving the 5V components)
- **NRF24L01P:** `P = 3.3V × 0.012A = 0.0396W`
- **ACS-712-05B:** `P = 5V × 0.01A = 0.05W`
- **Ultrasonic Sensor:** `P = 5V × 0.015A = 0.075W`
- **Robotic Arm + Servo Motor:** `P = 5V × 2A = 10W`
- **Cytron Motor Driver (with four motors):** `P = 11.1V × 16A = 177.6W`

**Total Power Consumption:**

Summing up the power consumption of all components:
   Total Power = 2.22W + 0.5W + 0.0396W + 0.05W + 0.075W + 10W + 177.6W

    Total Power ≈ 190.4856W

## Trace Width

For external layers in air: 26.479 mil

# Automation
Link to package - https://drive.google.com/file/d/1vJMVdHjOD6n0xipUPtd-66NJv4V5c6Jd/view?usp=sharing

## Path Planning

### Development Process
The first step to allow smooth navigation was to accurately locate the obstacles around the turtlebot and giving it an idea of the world. To achieve this we performed SLAM (Simultaneous Localization and Mapping). SLAM (Simultaneous Localization and Mapping) in ROS2 enables robots to build maps of their environment while tracking their own position. Via Rviz, We used the LiDaR and IMU sensor we were able to map the world but due to drift in odometry values making a perfect map was a rather tedious task, as distant areas of maps were inconsistent with each other due to the drift, although after several attempts a good enough map was created.

We created another launch file nav2.launch.py to launch Rviz and created a new node “init_amcl_pose_publisher” to publish the initial position of the bot to the navigation stack.

In the path_planner file, we used nav2 to set the goal positions ie the cones and the nav2 library using the weight maps created via SLAM automatically draws a path to go to the goal pose.

But this implementation had an unforeseen issue which we were not abe to resolve: Odometry drift, after travelling large enough distance there is a noticeable difference in the position in gazebo world and the rviz navigation stack, due to this the tracked path fails and the bot returns to original pose.

**How to Use**

To execute the implementation, run the following commands in parallel terminals:
1. `ros2 launch hackathon_automation robo.launch.py`  
   Launches the Gazebo world with the turtlebot setup.
2. `ros2 launch hackathon_automation nav2.launch.py`  
   Launches the navigation stack and Rviz with the integrated map and initial robot position.
3. `ros2 run hackathon_automation path_planner.py`  
   Starts navigation, sets the goal pose, and initiates robot movement.

   
