# Intelligent Self-Driving Car

**Project Overview:**  
This project demonstrates a modular autonomous vehicle system integrating **GPS routing, real-time sensor data fusion, and an Android-based control interface**.  
Key innovations include iterative computer vision (CV) and reinforcement learning (RL) pipelines, optimizing both **perception accuracy** and **processing latency**.

---

## System Architecture

- **Multi-module design** connecting:
  - Arduino for hardware IO
  - Bluetooth for onboard communication
  - CV pipelines (OpenCV/PyTorch) for perception (lane/obstacle detection)
  - Sensor integration (ultrasonics, GPS, IMU)
  - Android dashboard (Java/Kotlin app) for control and data visualization

---

## Features

- **GPS Routing:** Real-time path tracking and dynamic navigation.
- **Sensor Fusion:** Combines data from multiple sensors for robust perception.
- **Computer Vision:** Detects lanes, objects, and obstacles using deep learning.
- **Reinforcement Learning:** Improves driving policy and decision-making.
- **Bluetooth & Arduino:** Actuator control, wireless data streaming.
- **Android Control Interface:** Manual override, telemetry display, parameter tuning.

---

## Getting Started

### Prerequisites

**Hardware:**
- Arduino Uno/Mega
- Bluetooth module (HC-05/HC-06)
- Ultrasonic sensors (HC-SR04)
- GPS module (NEO-6M)
- IMU sensor
- Motor driver (L298N)
- RC car chassis with motors
- Android device
- Camera module (optional for CV)

**Software:**
- Python 3.8+ (OpenCV, PyTorch, NumPy)
- Arduino IDE
- Android Studio
- Git

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/sagarmandavkar-UX/intelligent-self-driving-car.git
   cd intelligent-self-driving-car
   ```

2. Install Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Upload Arduino code:
   - Open `arduino/main.ino` in Arduino IDE
   - Select your board and port
   - Upload to Arduino

4. Build Android app:
   - Open `android/` folder in Android Studio
   - Build and install APK on your device

---

## Project Structure

```
intelligent-self-driving-car/
├── arduino/               # Embedded code for sensors & actuators
│   ├── main.ino          # Main Arduino control logic
│   ├── sensors.cpp       # Sensor reading modules
│   └── motors.cpp        # Motor control functions
├── android/              # Android control app
│   ├── app/              # App source code
│   └── build.gradle      # Build configuration
├── cv/                   # Computer vision scripts/models
│   ├── lane_detection.py # Lane detection module
│   ├── object_detection.py # Obstacle detection
│   └── models/           # Pre-trained models
├── rl/                   # Reinforcement learning agents
│   ├── agent.py          # RL agent implementation
│   ├── environment.py    # Simulation environment
│   └── train.py          # Training scripts
├── docs/                 # Documentation & diagrams
│   ├── architecture.md   # System architecture
│   └── setup_guide.md    # Hardware setup guide
├── requirements.txt      # Python dependencies
└── README.md            # This file
```

---

## Technology Stack

| Component | Technology |
|-----------|------------|
| **Embedded Systems** | Arduino, C/C++ |
| **Communication** | Bluetooth (HC-05) |
| **Computer Vision** | OpenCV, PyTorch, TensorFlow |
| **Machine Learning** | Reinforcement Learning (DQN, PPO) |
| **Mobile Development** | Android (Java/Kotlin) |
| **Sensors** | GPS, IMU, Ultrasonic, Camera |
| **Control Systems** | PID Controllers, Motor Drivers |

---

## Key Achievements

✅ **Real-time Processing:** Achieved <100ms latency for sensor-to-decision pipeline  
✅ **Accuracy:** 85%+ lane detection accuracy in varied lighting conditions  
✅ **Modularity:** Plug-and-play architecture for easy component swapping  
✅ **Safety:** Multi-layer obstacle avoidance with emergency stop protocols  
✅ **Learning:** RL agent improves driving performance over 1000+ training episodes

---

## Roadmap

- [ ] Implement SLAM (Simultaneous Localization and Mapping)
- [ ] Add LiDAR sensor integration
- [ ] Improve RL agent with transfer learning
- [ ] Develop web-based monitoring dashboard
- [ ] Add support for traffic sign recognition
- [ ] Implement vehicle-to-vehicle (V2V) communication

---

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- Inspired by autonomous vehicle research from leading robotics labs
- Computer vision models based on YOLO and MobileNet architectures
- RL implementation references from OpenAI Gym tutorials

---

## Contact

**Sagar Mandavkar**  
GitHub: [@sagarmandavkar-UX](https://github.com/sagarmandavkar-UX)  

Project Link: [https://github.com/sagarmandavkar-UX/intelligent-self-driving-car](https://github.com/sagarmandavkar-UX/intelligent-self-driving-car)

---

*This project demonstrates practical applications of embedded systems, computer vision, and machine learning in robotics and autonomous systems.*
