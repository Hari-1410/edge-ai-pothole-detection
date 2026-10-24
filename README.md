# THEDAN — Edge-AI Urban Infrastructure Monitoring

**THEDAN** is an edge-AI prototype for continuous urban infrastructure monitoring. This repository contains the road-anomaly detection and Raspberry Pi deployment component: an INT8-quantized YOLOv5n model running on Raspberry Pi 4 using CPU-only inference.

The broader THEDAN concept combines camera-based road inspection with location-aware monitoring and reporting. This repository documents the computer-vision/edge-inference component; GPS integration, fleet-level monitoring, cloud workflows, and a complete vehicle-mounted deployment should only be described as implemented when their code and test evidence are added here.

## Project Scope

Urban roads develop potholes and other visible defects that can create safety risks and maintenance costs. THEDAN explores using an embedded edge device to detect road anomalies locally and support infrastructure-monitoring workflows without relying on a GPU or continuous cloud inference.

### Current repository implementation

- YOLOv5n object detection configured for pothole detection.
- Exported TensorFlow Lite INT8 model for Raspberry Pi 4 (ARM Cortex-A72).
- Camera/video inference scripts and post-processing pipeline.
- Confidence filtering, non-maximum suppression, and bounding-box output.
- Runtime logging and sample detection/system telemetry.
- Training/export commands and recorded evaluation artifacts.

### Broader THEDAN direction

- Vehicle-mounted camera and GPS for location-aware road observations.
- Continuous road-condition monitoring in real operating environments.
- Aggregation of detected issues into infrastructure-monitoring workflows.

These broader capabilities are project goals and should not be assumed to be implemented in this repository unless the corresponding source code, hardware integration, and test evidence are present.

## System Overview

**Current edge-inference path**

Camera or video input → frame preprocessing → INT8 TensorFlow Lite inference → confidence filtering → non-maximum suppression → bounding-box output → telemetry logging.

## Model and Evaluation

The repository and recorded artifacts report the following figures. Reproduce the evaluation and benchmark on the target device before treating them as independently verified.

| Metric | Reported value |
|---|---:|
| Model | YOLOv5n |
| Input resolution | 320 × 320 |
| Parameters | approximately 1.76 million |
| Dataset images | 2,602 |
| Annotations | 9,652 |
| Classes | 1 (pothole) |
| mAP@0.5 | 76.3% |
| Precision | 80.5% |
| Recall | 71.2% |
| INT8 model size | approximately 1.93 MB |
| Raspberry Pi 4 throughput | approximately 13–16.6 FPS in recorded benchmarks |
| Approximate latency at 15 FPS | 66 ms/frame |

Results are sensitive to input resolution, model/runtime versions, camera format, thread count, and logging overhead. These values describe the road-anomaly detector, not the end-to-end performance of a complete city-scale infrastructure-monitoring system.

## Hardware and Software

**Target hardware**
- Raspberry Pi 4
- ARM Cortex-A72 CPU
- Camera or video input source

**Software stack**
- Python
- TensorFlow Lite runtime
- OpenCV
- NumPy
- PyTorch/YOLOv5 for training and export

## Repository Structure

```text
data/                 Dataset configuration
diagrams/             System, inference, and model-workflow diagrams
edge/                 Real-time camera and video inference scripts
models/               Model files (FP32 and INT8 TFLite)
results/              Sample telemetry and evaluation plots
training/             Training and export command references
README.md             Project documentation
```

## Setup and Run

Use a Raspberry Pi OS environment compatible with the dependencies and TensorFlow Lite runtime.

```bash
sudo apt update
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
python edge/realtime_inference.py
```

For video-file inference, inspect the arguments and input handling in `edge/video_inference.py` before running it. Camera device names and runtime dependencies may need adjustment for your hardware.

## Training and Export

Install the training dependencies in a separate environment:

```bash
pip install -r requirements_training.txt
```

The recorded training configuration is available in `training/train_command.txt`; the export command is in `training/export_command.txt`. Review `data/data.yaml` and ensure the referenced dataset is available locally before training. Dataset imagery is not necessarily included in this repository.

## Evidence and Validation

- Sample detections and system metrics are stored under `results/`.
- Evaluation curves are stored under `results/`.
- Deployment benchmarks should specify Raspberry Pi model, OS, runtime, model input size, thread count, and whether logging is enabled.
- The repository currently focuses on pothole detection. Adding GPS tagging, uploading/aggregating detections, or broader anomaly classes requires implementation and validation of those components.

## Roadmap

- Integrate GPS coordinates and timestamps with each detected road anomaly.
- Define an event record format for detections, confidence, location, and device status.
- Add reliable local buffering and optional synchronization for intermittent connectivity.
- Evaluate detection quality across diverse roads, lighting, weather, and camera mounting conditions.
- Build a map/dashboard for reviewing and prioritizing road-maintenance observations.
- Benchmark sustained performance, power, storage, and thermal behavior in realistic deployment conditions.

## License and Attribution

This repository includes work derived from the upstream project [Visionx_road_anomaly_edge-detection-arm](https://github.com/yashi-here/Visionx_road_anomaly_edge-detection-arm). Preserve applicable upstream attribution, notices, and license terms when modifying or redistributing the code. The existing MIT license should be read together with the upstream project's licensing and the licenses of included dependencies and datasets.

---

**Status:** Edge pothole-detection component of THEDAN. The complete vehicle-mounted, GPS-integrated, continuous urban-infrastructure platform should be described as fully implemented only after its integration is present in this repository and tested.
