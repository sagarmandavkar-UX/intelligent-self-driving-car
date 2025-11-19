# Android Control App

This directory contains the Android mobile application for controlling and monitoring the intelligent self-driving car.

## Overview

The Android app provides a real-time dashboard, manual override controls, and route planning interface. It communicates with the Arduino controller via Bluetooth for seamless command transmission and telemetry display.

## Features

### 1. **Real-Time Dashboard**
- Live sensor data visualization
- GPS location and route tracking
- Speed, battery, and distance metrics
- Camera feed (if enabled)
- System status indicators

### 2. **Manual Control Interface**
- Touch-based joystick for directional control
- Emergency stop button
- Speed adjustment slider
- Mode toggle (Manual / Autonomous)

### 3. **Route Planning**
- Interactive map with waypoint selection
- GPS coordinate input
- Saved routes library
- Distance and ETA calculations

### 4. **Settings & Configuration**
- Bluetooth device pairing
- Sensor calibration
- Parameter tuning (PID gains, thresholds)
- Data logging options

## Screenshots

```
┌─────────────────────────────────────┐
│  Intelligent Self-Driving Car       │
│                                     │
│  ┌─────────────────────────────┐   │
│  │   📍 GPS: 36.14, -86.80      │   │
│  │   ⚡ Battery: 85%             │   │
│  │   🚗 Speed: 2.5 m/s          │   │
│  │   📏 Distance: 125m          │   │
│  └─────────────────────────────┘   │
│                                     │
│  Mode: [Manual] [Autonomous]        │
│                                     │
│  ┌──────────┐  ┌──────────┐        │
│  │    ↑     │  │  Route   │        │
│  │  ← ● →  │  │ Planning  │        │
│  │    ↓     │  └──────────┘        │
│  └──────────┘                       │
│                                     │
│  [🛑 EMERGENCY STOP]                │
└─────────────────────────────────────┘
```

## Architecture

### Tech Stack
- **Language**: Java / Kotlin
- **UI Framework**: Material Design Components
- **Communication**: Bluetooth Classic (SPP)
- **Maps**: Google Maps SDK
- **Charts**: MPAndroidChart for telemetry visualization

### Project Structure

```
android/
├── app/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/selfdriving/
│   │   │   │   ├── MainActivity.java
│   │   │   │   ├── BluetoothService.java
│   │   │   │   ├── DashboardFragment.java
│   │   │   │   ├── ControlFragment.java
│   │   │   │   ├── MapFragment.java
│   │   │   │   └── SettingsActivity.java
│   │   │   ├── res/
│   │   │   │   ├── layout/
│   │   │   │   ├── drawable/
│   │   │   │   └── values/
│   │   │   └── AndroidManifest.xml
│   │   └── test/
│   └── build.gradle
├── gradle/
└── README.md
```

## Installation

### Prerequisites
- Android Studio Arctic Fox or later
- Android SDK 21+ (Lollipop and above)
- Physical Android device (Bluetooth required)

### Setup

1. **Clone and open project:**
   ```bash
   git clone https://github.com/sagarmandavkar-UX/intelligent-self-driving-car.git
   cd intelligent-self-driving-car/android
   ```

2. **Open in Android Studio:**
   - File → Open → Select `android/` folder
   - Wait for Gradle sync

3. **Configure Google Maps API:**
   - Get API key from Google Cloud Console
   - Add to `local.properties`:
     ```
     MAPS_API_KEY=your_api_key_here
     ```

4. **Build and install:**
   ```bash
   ./gradlew assembleDebug
   adb install app/build/outputs/apk/debug/app-debug.apk
   ```

## Bluetooth Communication Protocol

### Command Format

**From App → Arduino:**
```
Command Structure: [OPCODE][PARAM]

Examples:
F:255    # Move forward at max speed
B:128    # Move backward at half speed
L:200    # Turn left at speed 200
R:150    # Turn right at speed 150
S:000    # Stop
A:001    # Enable autonomous mode
M:001    # Enable manual mode
```

**From Arduino → App:**
```
Telemetry Format: JSON

{
  "gps": {"lat": 36.1447, "lon": -86.8027},
  "speed": 2.5,
  "battery": 85,
  "distance_traveled": 125,
  "obstacles": [2.5, 3.1, null, 1.8],
  "mode": "autonomous",
  "status": "ok"
}
```

## Key Components

### BluetoothService.java

Handles Bluetooth connection and data transmission.

```java
public class BluetoothService {
    private BluetoothSocket socket;
    private InputStream inputStream;
    private OutputStream outputStream;
    
    public void connect(BluetoothDevice device) {
        UUID uuid = UUID.fromString("00001101-0000-1000-8000-00805F9B34FB");
        socket = device.createRfcommSocketToServiceRecord(uuid);
        socket.connect();
        inputStream = socket.getInputStream();
        outputStream = socket.getOutputStream();
    }
    
    public void sendCommand(String command) {
        outputStream.write(command.getBytes());
    }
    
    public String readData() {
        byte[] buffer = new byte[1024];
        int bytes = inputStream.read(buffer);
        return new String(buffer, 0, bytes);
    }
}
```

### DashboardFragment.java

Displays real-time telemetry and system status.

```java
public class DashboardFragment extends Fragment {
    private TextView speedText, batteryText, distanceText;
    private LineChart sensorChart;
    
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_dashboard, container, false);
        
        // Initialize UI components
        speedText = view.findViewById(R.id.speed_text);
        batteryText = view.findViewById(R.id.battery_text);
        sensorChart = view.findViewById(R.id.sensor_chart);
        
        // Start telemetry updates
        startTelemetryUpdates();
        
        return view;
    }
    
    private void updateDashboard(TelemetryData data) {
        speedText.setText(String.format("%.1f m/s", data.speed));
        batteryText.setText(String.format("%d%%", data.battery));
        updateSensorChart(data.obstacles);
    }
}
```

### ControlFragment.java

Manual control interface with joystick and buttons.

```java
public class ControlFragment extends Fragment {
    private JoystickView joystick;
    private Button emergencyStopBtn;
    
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_control, container, false);
        
        joystick = view.findViewById(R.id.joystick);
        joystick.setOnMoveListener((angle, strength) -> {
            sendControlCommand(angle, strength);
        });
        
        emergencyStopBtn = view.findViewById(R.id.emergency_stop);
        emergencyStopBtn.setOnClickListener(v -> {
            bluetoothService.sendCommand("S:000");
        });
        
        return view;
    }
    
    private void sendControlCommand(int angle, int strength) {
        String command = convertToMotorCommand(angle, strength);
        bluetoothService.sendCommand(command);
    }
}
```

## UI Components

### Custom JoystickView

```java
public class JoystickView extends View {
    private Paint paint;
    private float centerX, centerY, radius;
    private float touchX, touchY;
    
    @Override
    protected void onDraw(Canvas canvas) {
        // Draw outer circle
        canvas.drawCircle(centerX, centerY, radius, paint);
        
        // Draw joystick position
        canvas.drawCircle(touchX, touchY, radius/3, paint);
    }
    
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        touchX = event.getX();
        touchY = event.getY();
        
        // Calculate angle and strength
        float dx = touchX - centerX;
        float dy = touchY - centerY;
        float angle = (float) Math.atan2(dy, dx);
        float strength = (float) Math.sqrt(dx*dx + dy*dy) / radius;
        
        // Notify listener
        if (listener != null) {
            listener.onMove((int)Math.toDegrees(angle), (int)(strength*100));
        }
        
        invalidate();
        return true;
    }
}
```

## Permissions

**AndroidManifest.xml:**
```xml
<uses-permission android:name="android.permission.BLUETOOTH" />
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
<uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.INTERNET" />
```

## Data Logging

The app can log telemetry data to CSV for analysis.

```java
public class DataLogger {
    private FileWriter writer;
    
    public void startLogging(String filename) {
        File file = new File(getExternalFilesDir(null), filename);
        writer = new FileWriter(file);
        writer.write("timestamp,speed,battery,distance,obstacles\n");
    }
    
    public void logData(TelemetryData data) {
        String line = String.format("%d,%.2f,%d,%d,%s\n",
            System.currentTimeMillis(),
            data.speed,
            data.battery,
            data.distance,
            Arrays.toString(data.obstacles)
        );
        writer.write(line);
    }
}
```

## Testing

### Unit Tests
```bash
./gradlew test
```

### Instrumented Tests
```bash
./gradlew connectedAndroidTest
```

### Bluetooth Mock Testing
```java
@Test
public void testBluetoothCommands() {
    BluetoothService service = new BluetoothService();
    service.sendCommand("F:255");
    assertEquals("F:255", service.getLastCommand());
}
```

## Future Enhancements

- [ ] Voice control integration
- [ ] AR overlay for obstacle visualization
- [ ] Multi-vehicle fleet management
- [ ] Cloud sync for route history
- [ ] Offline maps support
- [ ] Gesture-based controls
- [ ] Dark mode theme
- [ ] Tablet-optimized layout

## Troubleshooting

**Issue: Bluetooth won't connect**
- Ensure HC-05 module is powered
- Check pairing PIN (default: 1234)
- Verify permissions are granted
- Try unpairing and re-pairing

**Issue: Delayed telemetry**
- Reduce telemetry update frequency
- Check Bluetooth signal strength
- Close background apps

**Issue: App crashes on startup**
- Check Android version compatibility
- Verify all permissions granted
- Clear app cache and data

---

*The Android app serves as the user-friendly interface for interacting with the autonomous vehicle, providing control, monitoring, and configuration capabilities.*
