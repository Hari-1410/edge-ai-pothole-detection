# 🚀 Edge AI Pothole Detection on Raspberry Pi (ARM CPU Optimized)

Real-time pothole detection system deployed on Raspberry Pi 4 (ARM Cortex-A72) using an INT8-quantized YOLOv5n model with CPU-only inference.

---

## 🧠 My Contribution
- Designed and optimized a full-edge AI inference pipeline
- Achieved ~15 FPS real-time inference (CPU-only)
- Implemented multi-threading + resolution optimization
- Performed system profiling (FPS, latency, temperature)

---

## ⚡ Key Results
- mAP@0.5: 76.3%
- FPS: 13–16.6
- Latency: ~66 ms
- Model Size: 3.66MB → 1.93MB (~47% reduction)
- Performance Gain: ~5×

---

## 🧠 Problem Statement
Road surface damage causes safety risks and maintenance issues.

This project builds a low-power, real-time edge AI system capable of detecting potholes without GPU/NPU acceleration.

---

## 🏗 System Pipeline

Camera → Preprocessing → TFLite Inference → Filtering → NMS → Output → Logging

---

## 📊 Model Summary

| Metric | Value |
|-------|------|
| Model | YOLOv5n |
| Params | 1.76M |
| GFLOPs | 4.1 |
| Input | 320×320 |
| mAP@0.5 | 76.3% |

---

## 📦 Dataset
- Images: 2602  
- Annotations: 9652  
- Classes: 1 (pothole)  
- Avg objects/image: ~3.7  
- Source: Roboflow  

---

## ⚙️ Optimization Strategy
- Reduced input resolution (640 → 320)
- Matched camera input to model size
- Used 4-thread inference
- Applied INT8 quantization
- Reduced preprocessing overhead

---

## 🖥 Edge Deployment

Hardware:
- Raspberry Pi 4
- ARM Cortex-A72
- CPU-only

Software:
- TensorFlow Lite
- OpenCV
- NumPy

---

## 🚀 Performance

| Input Size | FPS |
|-----------|-----|
| 416×416 | ~3 |
| 320×320 | 13–16.6 |

Latency: ~66 ms/frame

---

## 🌡 Stability
- Temperature: 46°C – 66°C
- No thermal throttling
- Stable long runs

---

## 📊 Logs
Tracked:
- FPS
- CPU
- RAM
- Temperature

Files:
- results/sample_detection.csv
- results/sample_system_metrics.csv

---

## 🛠 Installation

Run:

    sudo apt update
    pip install -r requirements.txt
    python edge/realtime_inference.py

---

## 🧪 Training

Run:

    pip install -r requirements_training.txt

    python train.py --img 320 --batch 16 --epochs 100 --data data.yaml --weights yolov5n.pt

---

## 📂 Structure

    models/
    edge/
    training/
    results/
    diagrams/
    data/

---

## 🔮 Future Work
- NPU acceleration
- Distributed edge systems
- Cloud analytics

---

## 🏆 Conclusion
Efficient models + quantization + system optimization enable real-time vision on ARM CPUs without GPUs.

---

## 📜 License
MIT License
