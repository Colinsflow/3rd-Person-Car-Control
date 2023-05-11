# 3rd-Person-Car-Control
This program mimics car control similar to what is seen in 3rd person perspective video games.

Designed By: Colin Graves

Platform Used: SunFounder PiCar-X

Modifications: 3D Printed Upper Chassis

Sensors: Adafruit 9-Axis Inertial Measurement Unit (IMU)

Controller: Sony Dualshock 4 Remote [Bluetooth]

Benefits:
The user can intuitively maneuver a robot.

Result:
Using I2C to interface with an inertial measurement unit, self orientation of the robot was achieved. By using a PS4 Controller and moving the joystick in any direction, the robot will turn facing the direction that the stick is pointing in. (Assuming the controller is angled perpendicular to earth's magnetic poles & parallel with the ground) 

IO:
-Right Trigger: Accelerator

-Left Trigger: Break

-Right Stick: Intended Direction

Project Expansion:
An IMU can be placed on the PS4 controller to eliminate the need to keep the controller fixed relative to the orientation of the earth.


