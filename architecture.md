# Rescue Vision Architecture & Engineering Specification

## 1. System Mission & Context
**Rescue Vision** is an AI-assisted computer vision decision-support system designed to assist search & rescue (SAR) operators and emergency first responders in locating human presence candidates across catastrophic environments (collapsed rubble, building ruins, flood zones, landslide debris, and dense forest/smoke conditions).

---

## 2. Component Architecture

```
┌────────────────────────────────────────────────────────┐
│                   Rescue Vision UI                     │
│    React 18 + Vite + TypeScript + Tailwind CSS         │
│    Recharts Telemetry + Leaflet OpenStreetMap Tactical │
└───────────────────────────┬────────────────────────────┘
                            │ REST APIs / Multipart Form
┌───────────────────────────▼────────────────────────────┐
│                    FastAPI Gateway                     │
│     CORS Middleware, File Validator, Static Mounts     │
└───────────────┬───────────────────────────┬────────────┘
                │                           │
┌───────────────▼──────────┐    ┌───────────▼────────────┐
│   Detection & Inference  │    │ Custom Training Center │
│  Ultralytics YOLO26      │    │ Background Workers     │
│  OpenCV Tactical Overlay │    │ Dataset Validator      │
│  ByteTrack Tracker       │    │ Checkpoint Hot-Swap    │
└───────────────┬──────────┘    └───────────┬────────────┘
                │                           │
┌───────────────▼───────────────────────────▼────────────┐
│               Data Persistence & Storage               │
│      SQLite / SQLAlchemy + Local Managed Storage       │
│      /storage/uploads  /storage/results  /runs         │
└────────────────────────────────────────────────────────┘
```

---

## 3. Computer Vision Pipeline (Ultralytics YOLO26)
- **Model Loader**: Singleton `DetectionService` initializes pretrained weights (`yolo26n.pt`) once into memory.
- **Hardware Acceleration**: Automatic fallback to `CPU` with native detection of `CUDA` when compatible GPUs are present.
- **Detection & Class Filtering**:
  - Focuses on the `person` class (COCO class ID 0).
  - Annotates high-contrast tactical rescue bounding boxes (Neon Orange with corner callouts and confidence tags).
  - Other environmental objects are bounded in Tactical Cyan.
- **Video Tracking**:
  - Employs frame-by-frame inference with persistence tracking.
  - Distinguishes total detections from unique tracked human IDs.
  - Generates web-streamable MP4 videos with embedded HUD statistics.

---

## 4. Custom Model Training & Fine-Tuning Pipeline
- **Dataset Structure Verification**:
  - Enforces standard YOLO format (`data.yaml`, `images/train`, `images/val`, `labels/train`, `labels/val`).
  - Verifies normalized coordinate boundaries `[0.0, 1.0]`.
- **Background Execution**:
  - Training runs in a detached daemon thread.
  - Prevents blocking HTTP requests.
  - Streams epoch progress, box loss, and mAP metrics into database records.
- **Model Registry & Hot-Swapping**:
  - Validates checkpoint weights on disk before activation.
  - Updates active model without server restarts.

---

## 5. Security & Reliability
- Strict file extension whitelisting (`.jpg`, `.jpeg`, `.png`, `.webp`, `.mp4`, `.avi`, `.mov`).
- Path traversal mitigation via `Path.resolve()` boundary checks.
- Bounded video frame limits (capped at 900 frames) to prevent OOM in local runtime environments.
- Safe archive extraction preventing ZipSlip vulnerabilities.
