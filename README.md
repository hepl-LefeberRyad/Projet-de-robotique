# Projet-de-robotique
## Introduction

This repository presents a robotics project developed as part of my Master’s degree program.

The project consists of an autonomous robot designed to follow a white line on a black surface. It is equipped with an object detection system that allows it to stop whenever an obstacle is detected in its path.

At the end of the track, the robot uses a robotic arm—designed using Fusion 360—to place a figure, then performs a turn to return to its starting position.

The robot was programmed using the Arduino IDE, and features a heartbeat LED that reflects its activity through speed-based blinking. An integrated MP3 module also provides audio feedback, announcing the different states and actions of the robot throughout its operation.

## Different Parts

Several components are required to make this project possible. Below are the main elements used in the system.
A schematic diagram is used to illustrate the connections between the Arduino and the different modules.
<p align="center">
<img width="1064" height="811" alt="BLOCDIAGRAMMEFINAL drawio" src="https://github.com/user-attachments/assets/606585dd-171c-48b5-8995-b1ba2b9cb0c9" />
</p>

### Arduino Mega

A microcontroller is necessary to act as the brain of the project. In this case, an Arduino Mega was chosen because it provides a larger number of input/output pins compared to other boards. It can also deliver sufficient current to power multiple connected components through the ATmega2560.
<p align="center">
<img width="583" height="508" alt="image" src="https://github.com/user-attachments/assets/42e5784f-ec7a-4ff8-809b-6bafbd7ca48f" />
</p>

### Robot chassis

The chassis used for this robot includes integrated motors, but it is unfortunately no longer available on DFROBOT.
<p align="center">
<img width="600" height="600" alt="image" src="https://github.com/user-attachments/assets/b30af661-fdf1-4886-959f-84d274728716" />
</p>

### drv8871

To make the robot drive the project has to include a driver that makes the motors move forward an backwards and use differnt speeds. The motor driver i used is the DRV8871 motordriver and i used 2 drivers, one for the motors on the left side the other for the motors of the right side. 
The drv8871 is controlled by a 2 pins IN1 and IN2
<p align="center">
<img width="800" height="615" alt="image" src="https://github.com/user-attachments/assets/368e62a3-b080-439e-af11-4cc8910ef4d9" />
</p>

### Qtr MD-01-A
I used 3 IR sensors to follow the line, the 3 sensors are positionned over a line, one over the white line in the center, and the other 2 on the right and left over the black surface.

### HC-SR04
### jRQ6500
### Lipo battery


