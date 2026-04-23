# Projet-de-robotique
## Introduction

This repository presents a robotics project developed as part of my Master’s degree program.
<p align="center">
<img width="1247" height="1663" alt="image" src="https://github.com/user-attachments/assets/881fe19a-0c9d-41f4-9322-f8f25aa00bb9" />
</p>

The project consists of an autonomous robot designed to follow a white line on a black surface. It is equipped with an object detection system that allows it to stop whenever an obstacle is detected in its path.

At the end of the track, the robot uses a robotic arm ,designed in Fusion 360, to push down a figure placed on a platform, and then it turns to return to its starting position.

The robot was programmed using the Arduino IDE, and features a heartbeat LED that reflects its activity through speed-based blinking. An integrated MP3 module also provides audio feedback, announcing the different states and actions of the robot throughout its operation.

## Different Parts

Several components are required to make this project possible. Below are the main elements used in the system.
A schematic diagram is used to illustrate the connections between the Arduino and the different modules.
<p align="center">
<img width="1064" height="811" alt="BLOCDIAGRAMME drawio" src="https://github.com/user-attachments/assets/25357d75-80b7-4048-9a5f-d3d61b55d577" />
</p>

### Arduino Mega

A microcontroller is necessary to act as the brain of the project. In this case, an Arduino Mega was chosen because it provides a larger number of input/output pins compared to other boards. It can also deliver sufficient current to power multiple connected components through the ATmega2560.
<p align="center">
<img width="583" height="508" alt="image" src="https://github.com/user-attachments/assets/42e5784f-ec7a-4ff8-809b-6bafbd7ca48f" />
</p>

https://docs.arduino.cc/hardware/mega-2560/

### Robot chassis

The chassis used for this robot includes integrated motors, but it is unfortunately no longer available on DFROBOT.
<p align="center">
<img width="600" height="600" alt="image" src="https://github.com/user-attachments/assets/b30af661-fdf1-4886-959f-84d274728716" />
</p>

### MG946R
To make the robot arm move, I used two MG946R servos. These servos are high-torque actuators (up to 13 kg·cm at 6 V) designed for precise positioning and are controlled using PWM signals from the Arduino. One servo is placed at the base of the robot arm to move it forward and backward, while the second servo controls the upper part of the arm, allowing it to move up and down.
<p align="center">
<img width="500" height="501" alt="image" src="https://github.com/user-attachments/assets/d05a1615-dbcc-4045-9a71-247ba71999f4" />
</p>

https://towerpro.com.tw/product/mg946r/
### drv8871

The DRV8871 is a motor driver used to control brushed DC motors by acting as an interface between a microcontroller and the motor, allowing safe handling of higher voltage (6.5 V to 45 V) and current (up to 3.6 A peak). It contains an internal H-bridge made of four N-channel MOSFETs, which enables the motor to rotate in both directions depending on the logic signals applied to the two control pins, IN1 and IN2. The driver supports speed control using PWM (Pulse Width Modulation), where the duty cycle determines the motor speed. It also includes built-in current regulation via the ILIM pin, as well as protection features such as undervoltage lockout, overcurrent protection, and thermal shutdown, ensuring safe and reliable operation. In the robot project, two DRV8871 drivers are used—one for the left motors and one for the right—allowing independent control of each side for movement and steering.

#### IN1 / IN2 Logic Control

- **IN1 = 1, IN2 = 0 → Forward**
  - OUT1 = High, OUT2 = Low  
  - Current flows from OUT1 → OUT2  
  - Motor spins forward  

- **IN1 = 0, IN2 = 1 → Reverse**
  - OUT1 = Low, OUT2 = High  
  - Current flows from OUT2 → OUT1  
  - Motor spins backward  

- **IN1 = 1, IN2 = 1 → Brake (slow decay)**
  - Both outputs are driven LOW  
  - Motor stops quickly (braking effect)  

- **IN1 = 0, IN2 = 0 → Coast / Sleep**
  - Outputs are High-Z (disconnected)  
  - Motor spins freely and gradually stops  
  - Enters low-power sleep mode after ~1 ms  

- **PWM**
  - PWM is applied to one input while the other is kept constant  
  - Example: IN1 = 1 and IN2 = PWM controls forward speed  
  - Higher duty cycle equals to a faster motor speed  
  - Lower duty cycle equals to a slower motor speed
  
<p align="center">
<img width="800" height="615" alt="image" src="https://github.com/user-attachments/assets/368e62a3-b080-439e-af11-4cc8910ef4d9" />
</p>

https://www.ti.com/product/DRV8871

### Qtr MD-01-A

The QTR sensor is an infrared reflectance sensor used to detect differences in surface color or distance. It operates with a voltage between 2.9 V and 5.5 V and outputs an analog voltage between 0 V and VCC depending on how much light is reflected back to the sensor. The sensor works by emitting infrared light and measuring how much of that light is reflected by the surface in front of it. Bright surfaces reflect more light, resulting in a lower output voltage, while dark surfaces reflect less light, resulting in a higher output voltage.

The optimal sensing distance is around 5 mm and each sensor consumes up to 32 mA.

In this project, three QTR sensors are used to detect and follow a line. The sensors are positioned next to each other under the robot:
- The middle sensor is placed above the line (white surface)
- The left and right sensors are placed over the darker background (black surface)

By comparing the values from the three sensors, the robot can determine its position relative to the line:
- If the center sensor detects the line → the robot is correctly aligned
- If the left sensor detects the line → the robot has moved left and needs to correct to the right
- If the right sensor detects the line → the robot has moved right and needs to correct to the left

The analog output of each sensor is read using the Atmega ADC. These values are then used to adjust the motor speeds through the motor drivers, enabling the robot to follow the line smoothly.

The Pololu Arduino library was used to simplify and streamline the coding process.

<p align="center">
<img width="887" height="751" alt="image" src="https://github.com/user-attachments/assets/51d2b7d3-236b-4288-9aa6-c87225c886a8" />
</p>
https://www.pololu.com/product/4241

### HC-SR04
The HC-SR04 ultrasonic sensor is used in this project to detect objects in front of the robot and prevent collisions. It works by sending out an ultrasonic sound pulse from the TRIG pin, which travels through the air until it hits an object and is reflected back to the sensor. The ECHO pin measures the time it takes for the sound wave to return. By calculating this time delay, the distance to the object can be determined. In the code, a distance threshold of 8 cm is used; when an object is detected within this range, the robot stops to avoid a collision.
<p align="center">
<img width="1024" height="505" alt="image" src="https://github.com/user-attachments/assets/fe119dc0-60ea-491f-9c3f-ab9b278964a7" />
</p>

### jRQ6500
The JQ6500 is a programmable MP3 module that uses a UART connection with the Arduino Mega to play audio files. In this project, prerecorded sounds are used to indicate the robot’s state; for example, when the robot starts, a voice recording announces “start.”

The library used for controlling the module was downloaded from GitHub. Credit goes to sleemanj and all contributors for making this easy-to-use library available to the public:
https://github.com/sleemanj/JQ6500_Serial
<p align="center">
<img width="750" height="500" alt="image" src="https://github.com/user-attachments/assets/986caf04-c5ea-4c49-9456-9ee66d3336fb" />
</p>

https://components101.com/modules/jq6500-mp3-player-module-pinout-features-datasheet-working-application-alternative

### Lipo battery
To power the four DC motors, I used an 11.1 V 3-cell LiPo battery with a capacity of 2200 mAh, which provides sufficient voltage and current to drive the motors through the motor drivers.
<p align="center">
<img width="1000" height="923" alt="image" src="https://github.com/user-attachments/assets/ac7fd41d-81df-4bd6-ae51-262cbb69ca14" />
</p>

### 5V powerbank
The other elements of the project are powered by a 5V power bank, which supplies the Arduino Mega, the sensors, the LED, and the MP3 module.
<p align="center">
<img width="366" height="741" alt="image" src="https://github.com/user-attachments/assets/120537e2-45cd-4cff-80ab-f1e84a4bad70" />
</p>
