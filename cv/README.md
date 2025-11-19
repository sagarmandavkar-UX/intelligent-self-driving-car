# Computer Vision Pipeline

This directory contains computer vision modules for lane detection, obstacle recognition, and real-time image processing.

## Overview

The CV pipeline enables the autonomous vehicle to perceive its environment through camera-based visual analysis, complementing sensor data for robust navigation.

## Modules

### 1. **lane_detection.py**
Detects lane markings and calculates vehicle position relative to road center.

**Features:**
- Canny edge detection
- Hough line transform for lane identification
- Region of interest masking
- Lane curvature calculation

**Performance:**
- Inference time: <50ms per frame
- Accuracy: 85% in varied lighting conditions
- Frame rate: 20 FPS

**Usage:**
```python
from lane_detection import LaneDetector

detector = LaneDetector()
lanes = detector.detect(frame)
```

### 2. **object_detection.py**
Identifies obstacles, vehicles, and pedestrians using deep learning models.

**Models:**
- **YOLO v5**: Real-time object detection
- **MobileNet SSD**: Lightweight detection for edge devices

**Classes Detected:**
- Vehicles (cars, trucks, motorcycles)
- Pedestrians
- Traffic signs
- Static obstacles

**Performance:**
- Detection range: 2-15 meters
- Confidence threshold: 0.75
- Inference time: <80ms

**Usage:**
```python
from object_detection import ObjectDetector

detector = ObjectDetector(model='yolov5')
objects = detector.detect(frame)
```

### 3. **image_processing.py**
Preprocessing utilities for image enhancement and normalization.

**Functions:**
- Color space conversion (BGR → HSV, Grayscale)
- Gaussian blur for noise reduction
- Histogram equalization
- Perspective transformation
- Brightness/contrast adjustment

**Usage:**
```python
from image_processing import preprocess

processed = preprocess(frame, blur=True, equalize=True)
```

## Pipeline Architecture

```
Camera Feed
    ↓
Image Preprocessing
    ↓
    ├→ Lane Detection → Lane Position
    ↓
    └→ Object Detection → Obstacle Map
    ↓
Sensor Fusion (with Ultrasonic/GPS)
    ↓
Navigation Decision
```

## Dependencies

```bash
pip install opencv-python torch torchvision numpy
```

## Model Training

### Lane Detection
Trained on custom dataset of 5,000 road images:
- Daytime: 60%
- Night: 20%
- Rain/fog: 20%

### Object Detection
Fine-tuned YOLOv5 on COCO + custom dataset:
- 80 object classes
- 10,000 training images
- Data augmentation (rotation, flip, brightness)

## Performance Optimization

### Techniques Used:
1. **Model Quantization**: INT8 inference for 2x speedup
2. **Frame Skipping**: Process every 2nd frame for 30 FPS video
3. **ROI Processing**: Only analyze relevant image regions
4. **GPU Acceleration**: CUDA support for PyTorch models

### Benchmarks:

| Operation | CPU (ms) | GPU (ms) |
|-----------|----------|----------|
| Lane Detection | 45 | 15 |
| Object Detection | 150 | 35 |
| Preprocessing | 10 | 5 |
| **Total Pipeline** | **205** | **55** |

## Configuration

**config.yaml:**
```yaml
camera:
  resolution: [640, 480]
  fps: 30
  
lane_detection:
  canny_low: 50
  canny_high: 150
  roi_vertices: [[0, 480], [320, 280], [640, 480]]
  
object_detection:
  model: yolov5s
  confidence: 0.75
  nms_threshold: 0.4
```

## Testing

```bash
# Unit tests
python -m pytest tests/

# Visual debugging
python lane_detection.py --debug --video test_footage.mp4

# Benchmark
python benchmark.py --iterations 100
```

## Integration with Main System

The CV pipeline sends processed data to the Arduino controller:

```python
# cv_integration.py
from cv import LaneDetector, ObjectDetector
from communication import BluetoothModule

bt = BluetoothModule()
lane_det = LaneDetector()
obj_det = ObjectDetector()

while True:
    frame = camera.read()
    lanes = lane_det.detect(frame)
    objects = obj_det.detect(frame)
    
    # Send to Arduino
    bt.send({
        'lane_center': lanes.center_offset,
        'obstacles': objects.distances
    })
```

## Future Enhancements

- [ ] Semantic segmentation for road surface analysis
- [ ] Traffic light detection and classification
- [ ] Depth estimation from monocular camera
- [ ] Night vision enhancement (low-light optimization)
- [ ] Multi-camera fusion (360° perception)
- [ ] Sign recognition (speed limits, stop signs)

## Troubleshooting

**Issue: Low FPS**
- Solution: Reduce resolution, enable GPU, use model quantization

**Issue: Poor lane detection**
- Solution: Adjust Canny thresholds, recalibrate ROI, check lighting

**Issue: False positive detections**
- Solution: Increase confidence threshold, improve training data

---

*Computer vision is the "eyes" of the autonomous vehicle, enabling intelligent perception beyond basic sensors.*
