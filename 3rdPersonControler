'''
THIRD PERSON CAR CONTROL PROGRAM
AUTHOR: COLIN GRAVES
'''
from math import sqrt, atan2, pi
from picarx import Picarx
import time, threading
import csv
import pygame
import board
import adafruit_bno055

#PYGAME INNIT
pygame.init()
joystick = pygame.joystick.Joystick(0)
joystick.init()
#PICARX INNIT
px = Picarx()
#IMU INNIT
i2c = board.I2C()  
last_val = 0xFFFF
sampling_rate = 100
sensor = adafruit_bno055.BNO055_I2C(i2c)

value1 = 0
def anglecontrol(distance, direction, gear):
    # set steering angle
    steering = distance*.5
    if steering > 35:
        steering = 35
    # flip steering if going counter clockwise
    if direction == "ccw":
        steering = -steering
    # flip steering if going in reverse
    if gear == 1:
        steering = -steering
    px.set_dir_servo_angle(steering)

# returns the smallest angle between desired vector and actual vector. 
# specifies which way to turn to make actual angle closer to the desired.
def anglecalc(angle):
    global value1
    delta = (value1 - angle) % 360
    distance = min(delta, 360 - delta)
    direction = 'cw' if delta > 180 else 'ccw'
    return (distance, direction)

#Get the orientation value of the car 0-360
def read_sensor_values():
    breaker = 0
    while breaker != 1:
        if (sensor.euler[0] is not None):
            euler_angle = sensor.euler[0]
            if (euler_angle is not None):
                value1 = euler_angle
                breaker = 1
    return value1

#Threadded Timer Interrupt That Gathers IMU Data
def getData():
    global value1
    value1 = read_sensor_values()
    
    # Get current timestamp
    timestamp = time.strftime("%Y-%m-%d %H:%M:%S")
    
    # Write sensor data to CSV file
    with open('sensor_data.csv', 'a') as csvfile:
        writer = csv.writer(csvfile)
        writer.writerow([timestamp, value1])
    
    threading.Timer(.2, getData).start()
    
getData()

def manual_control():
    global value1
    movement = 0
    gear = 0
    while True:
        pygame.event.get()
        # Get the value of the left joystick's x-axis
        x_axis0 = joystick.get_axis(0) #steering axis
        #y_axis = joystick.get_axis(1)
        
        lt_throttle = joystick.get_axis(2) #left throttle  axis
        rt_throttle = joystick.get_axis(5) #right throttle acis
        x_axis = joystick.get_axis(4)
        y_axis = -joystick.get_axis(3)

        rt_button = joystick.get_button(8)
        lt_button = joystick.get_button(7)

        # If the rt button is pressed, increase the motor's speed
        if lt_throttle > -.99:
            movement -= ((lt_throttle+1))
            gear = 1
        elif rt_throttle > -.99:
            movement += ((rt_throttle+1))
            gear = 0
        else:
            movement = 0
            gear = 2

        if movement >= 65:
            movement = 65
        elif movement <= -65:
            movement = -65
    
        px.forward(movement)
        
        # Transform controllers stick cartesian position to polar coordinates
        r = sqrt(x_axis**2 + y_axis**2)
        theta = atan2(y_axis, x_axis)

        # convert angle to degrees
        theta_degrees = theta * 180 / pi

        # ensure angle is in the range of 0 to 360 degrees
        if theta_degrees < 0:
            theta_degrees += 360
        distance, direction = anglecalc(theta_degrees)
        anglecontrol(distance, direction, gear)

            

manual_control()


