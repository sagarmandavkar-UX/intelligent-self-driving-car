# Arduino Code

This directory contains the embedded code for the intelligent self-driving car's Arduino controller.

## Components

- **main.ino**: Main Arduino control logic integrating all sensors and actuators
- **sensors.cpp/h**: Sensor reading modules (ultrasonic, GPS, IMU)
- **motors.cpp/h**: Motor control and PWM functions
- **bluetooth.cpp/h**: Bluetooth communication with Android app

## Hardware Connections

### Motor Driver (L298N)
- IN1, IN2 → Digital pins 5, 6 (Left motor)
- IN3, IN4 → Digital pins 9, 10 (Right motor)
- ENA, ENB → Digital pins 3, 11 (PWM for speed control)

### Sensors
- **Ultrasonic (HC-SR04)**:
  - Trig → Pin 7
  - Echo → Pin 8
- **GPS (NEO-6M)**:
  - TX → Pin 4 (RX)
  - RX → Pin 3 (TX)
- **IMU**: I2C (SDA → A4, SCL → A5)

### Bluetooth (HC-05)
- TX → Pin 2
- RX → Pin 1

## Setup Instructions

1. Install Arduino IDE
2. Install required libraries:
   - TinyGPS++
   - Wire (for I2C)
   - SoftwareSerial
3. Connect hardware according to the pin diagram
4. Upload `main.ino` to Arduino board

## Communication Protocol

Commands received from Android app via Bluetooth:
- `F`: Move forward
- `B`: Move backward
- `L`: Turn left
- `R`: Turn right
- `S`: Stop
- `A`: Enable autonomous mode
- `M`: Manual control mode

## Notes

- Ensure proper power supply (7-12V for motors, 5V for Arduino)
- Calibrate sensors before first use
- Check serial monitor at 9600 baud for debugging
