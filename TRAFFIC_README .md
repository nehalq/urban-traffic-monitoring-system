# Urban Traffic Monitoring System

> An AI-powered multi-module framework for real-time urban traffic monitoring using computer vision (Haar Cascade & YOLOv8), sound classification (YAMNet), and graph-based route optimization (BFS & DFS) — built with Python, OpenCV, and TensorFlow.

---

## Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Modules](#modules)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Results](#results)
- [Future Work](#future-work)
- [Team](#team)

---

## Overview

Urban traffic management is a critical challenge for modern cities. This project presents an **Intelligent Urban Traffic Monitoring System** that autonomously monitors and analyzes urban traffic by integrating three AI-driven modules:

- **Visual detection** of vehicles using Haar Cascade and YOLOv8
- **Audio classification** of urban sounds (sirens, honks, crashes) using YAMNet
- **Route optimization** across a city road network using BFS and DFS graph traversal

Together, these modules form a comprehensive system that processes data from cameras and audio sensors deployed across intersections to improve traffic flow and incident response.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Python 3 |
| Computer Vision | OpenCV, YOLOv8 (Ultralytics) |
| Object Detection | Haar Cascade, YOLOv8x |
| Sound Classification | TensorFlow, TensorFlow Hub, YAMNet, Librosa |
| Graph Algorithms | NetworkX, BFS, DFS |
| Visualization | Matplotlib |
| Deep Learning | TensorFlow, CNN (via YOLO) |

**Keywords:** Computer Vision, Object Detection, YOLO, Haar Cascade, Sound Classification, YAMNet, Graph Traversal, BFS, DFS, Real-Time Traffic Monitoring, OpenCV, TensorFlow, Python, Artificial Intelligence, Expert Systems

---

## Project Structure

```
urban-traffic-monitoring-system/
│
├── inputs/
│   ├── road.mp4                  # Input traffic video
│   └── police-siren.mp3          # Input audio clip for sound analysis
│
├── outputs/
│   ├── haar_video.mp4            # Output video with Haar Cascade detections
│   └── yolo_video.mp4            # Output video with YOLO detections
│
├── models/
│   ├── cars.xml                  # Pre-trained Haar Cascade XML classifier
│   └── yolov8x.pt                # YOLOv8x pre-trained weights
│
├── module1_haar_detection.py     # Haar Cascade vehicle detection
├── module2_yolo_detection.py     # YOLOv8 vehicle detection
├── module3_sound_analysis.py     # YAMNet urban sound classification
├── module4_graph_traversal.py    # BFS & DFS route optimization
└── README.md
```

---

## Modules

### Module 1 — Image & Video Detection (Haar Cascade)

Detects cars in real-time video using a pre-trained Haar Cascade classifier (`cars.xml`).

**How it works:**
- Converts each frame to grayscale (reduces computational load)
- Applies `detectMultiScale` with `scaleFactor=1.2`, `minNeighbors=4`, `minSize=(40,40)`
- Draws blue bounding boxes labeled "Car" around each detected vehicle
- Saves the annotated video to `outputs/haar_video.mp4`

**Best for:** Lightweight, resource-friendly detection on edge devices.

---

### Module 2 — Image & Video Detection (YOLOv8)

Detects multiple vehicle types (cars, trucks, motorcycles) with confidence scores using YOLOv8x.

**How it works:**
- Loads `yolov8x.pt` pre-trained weights via the Ultralytics library
- Runs the model frame-by-frame on the input video
- Extracts bounding box coordinates, class ID, and confidence score per detection
- Draws red bounding boxes with labels (e.g., `car 0.91`, `truck 0.93`)
- Saves the result to `outputs/yolo_video.mp4`

**Best for:** High-accuracy, multi-class detection in real-time scenarios.

---

### Module 3 — Sound Analysis (YAMNet)

Classifies urban audio events — sirens, honking, and crashes — using Google's YAMNet deep neural network trained on AudioSet (500+ sound classes).

**How it works:**
- Loads audio at 16 kHz using Librosa and converts to TensorFlow tensor
- Runs audio through YAMNet to get per-frame probability scores
- Compares scores against custom detection thresholds:

| Sound Category | Keywords | Threshold |
|---|---|---|
| Blockage honk | Toot | 0.12 |
| Emergency siren | Siren, Police car (siren), Ambulance (siren) | 0.15 |
| Crash | Breaking | 0.10 |

- Outputs detected sound events with confidence scores and frame indices

---

### Module 4 — Graph-Based Route Optimization (BFS & DFS)

Models Karachi's road network as a directed graph and finds the optimal route between two intersections using BFS and DFS.

**Graph setup:**
- 15 intersections as nodes (e.g., CliftonBeach, Saddar, Gulshan, TariqRoad)
- Edges carry attributes: `distance` (km), `duration` (minutes), `traffic` (level 1–10)

**Algorithms:**
- **BFS** — Explores level by level, optimal for shortest path in unweighted graphs
- **DFS** — Explores depth-first, useful for discovering all possible routes

**Sample Result:**
```
BFS Optimal Path: ['CliftonBeach', 'Saddar', 'Gulshan', 'TariqRoad', 'LuckyOneMall'] Duration: 71 min
DFS Optimal Path: ['CliftonBeach', 'Saddar', 'Gulshan', 'TariqRoad', 'LuckyOneMall'] Duration: 71 min
```

---

## Getting Started

### Prerequisites

```bash
pip install opencv-python ultralytics tensorflow tensorflow-hub librosa networkx matplotlib
```

### Clone the Repository

```bash
git clone https://github.com/your-username/urban-traffic-monitoring-system.git
cd urban-traffic-monitoring-system
```

### Download Models

- **Haar Cascade XML** — Download `cars.xml` from [OpenCV GitHub](https://github.com/opencv/opencv/tree/master/data/haarcascades) and place it in `models/`
- **YOLOv8 weights** — Auto-downloaded on first run via Ultralytics, or place `yolov8x.pt` in `models/`

### Add Input Files

The input video and audio files, along with sample outputs, are available on Google Drive:

📁 **[Download Inputs & Outputs from Google Drive](https://drive.google.com/drive/folders/1n6rWmls6mf2yW8DI4XIBRGQVf9BmalTY?usp=sharing)**

> Download the `inputs/` folder and place files at `inputs/road.mp4` and `inputs/police-siren.mp3`. Sample output videos are also included in the `outputs/` folder.

---

## Usage

### Run Haar Cascade Detection

```bash
python module1_haar_detection.py
# Output saved to: outputs/haar_video.mp4
```

### Run YOLO Detection

```bash
python module2_yolo_detection.py
# Output saved to: outputs/yolo_video.mp4
```

### Run Sound Analysis

```bash
python module3_sound_analysis.py
# Prints detected sound events with confidence scores
```

### Run Graph Traversal

```bash
python module4_graph_traversal.py
# Prints BFS and DFS optimal paths with duration
# Displays city road network graph with highlighted route
```

---

## Results

### Haar Cascade — Vehicle Detection
- Detected cars with blue bounding boxes labeled "Car"
- Effective for real-time detection on standard hardware

### YOLOv8 — Vehicle Detection
- Detected cars and trucks with red bounding boxes and confidence scores (e.g., `car 0.91`, `truck 0.93`)
- Significantly higher accuracy and multi-class support compared to Haar Cascade

### Sound Analysis — YAMNet
```
*** ALERT: CITY SOUND EVENTS DETECTED ***
- Sound Category: emergency_siren
  - Detected Sound: Siren, Confidence: 0.56, Frame: 0
  - Detected Sound: Police car (siren), Confidence: 0.56, Frame: 0
  - Detected Sound: Siren, Confidence: 0.59, Frame: 2
```

### Graph Traversal — Route Optimization
```
BFS Optimal Path: CliftonBeach → Saddar → Gulshan → TariqRoad → LuckyOneMall
Duration: 71 minutes
```

---

## Future Work

- **Real-time integration** — Connect live CCTV camera feeds instead of pre-recorded video
- **Traffic signal control** — Use detection results to dynamically adjust traffic light timings
- **Weighted shortest path** — Replace BFS/DFS with Dijkstra's or A* for more accurate route optimization using real-time traffic weights
- **Dashboard** — Build a live monitoring dashboard with Streamlit or Flask
- **Cloud deployment** — Deploy the system on AWS or GCP for city-scale monitoring

---

## Team

**Course:** CT-361 Artificial Intelligence and Expert System  
**Department:** Computer Science and IT — SSCS (Specialization in Data Science)  
**University:** NED University of Engineering and Technology

| Name | Student ID |
|---|---|
| Neha Laeeq | DT-22002 |
| Hassan Hayat | DT-22010 |
| Shayan | DT-22037 |
| Ahmed Hassan | DT-22041 |

---

## License

This project was developed for academic purposes as part of CT-361 at NED University of Engineering and Technology.
