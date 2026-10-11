# Rescue Vision — AI-Based Human Body Detection for Disaster Response

[![Python](https://img.shields.io/badge/Python-3.11%20%7C%203.14-blue.svg)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.143.0-green.svg)](https://fastapi.tiangolo.com)
[![Ultralytics YOLO26](https://img.shields.io/badge/YOLO-Ultralytics%20YOLO26-orange.svg)](https://docs.ultralytics.com)
[![React](https://img.shields.io/badge/React-18-cyan.svg)](https://react.dev)
[![Vite](https://img.shields.io/badge/Vite-5.4-purple.svg)](https://vitejs.dev)
[![TailwindCSS](https://img.shields.io/badge/Tailwind-3.4-38bdf8.svg)](https://tailwindcss.com)

**Rescue Vision** is a complete, full-stack, AI-assisted computer vision web application designed to help search and rescue teams identify possible human presence in challenging disaster environments such as collapsed buildings, earthquake rubble, flood zones, landslide debris, and dense forest/smoke scenes.

---

## 🌟 Core Features

- **Tactical Detection Workspace (`/detect`)**:
  - Image detection (JPG, PNG, WEBP) with side-by-side original and annotated output previews.
  - Video analysis (MP4, AVI, MOV) with unique person ID tracking and frame-level statistics.
  - Configurable confidence threshold and image resolution settings.
  - JSON and CSV metadata export for incident reports.
- **Mission Dashboard (`/dashboard`)**:
  - Real-time telemetry: media analyzed, human candidates found, open incidents, and mean latency.
  - Historical detections timeline area chart using Recharts.
  - Live system status pills (device CPU/CUDA and loaded checkpoint).
- **Incident Geo-Management (`/incidents`)**:
  - Full CRUD for rescue incidents with priority, severity, and status tagging.
  - OpenStreetMap & Leaflet interactive map with custom tactical pins.
  - CSV export for on-site rescue team briefings.
- **Custom Model Training Center (`/training`)**:
  - YOLO dataset structure validator and sample dataset provisioner.
  - Non-blocking background training worker using Ultralytics PyTorch API.
  - Real-time epoch progress bar, loss/mAP convergence curves, and terminal stdout logs.
- **Model Registry & Hot-Swapping (`/models`)**:
  - Register, evaluate, and inspect model checkpoints with validation mAP scores.
  - Safe one-click activation with runtime weight verification.
- **Operational Analytics (`/analytics`)**:
  - Stored database ground truth analytics and model benchmark comparisons.
- **Application Settings (`/settings`)**:
  - Inference heuristics, device diagnostics, and interactive Swagger OpenAPI link (`/docs`).

---

## 🏗️ Architecture & Technology Stack

- **Frontend**: React 18, Vite, TypeScript, Tailwind CSS, Recharts, Lucide React, React Router 6, Axios, Leaflet.
- **Backend**: FastAPI, Uvicorn ASGI, Ultralytics YOLO26 (`yolo26n.pt`), OpenCV, Pillow, PyTorch.
- **Database**: SQLite with SQLAlchemy ORM (architected for seamless PostgreSQL migration).

---

## 🚀 Getting Started

### Prerequisites
- **Python**: 3.11 or compatible (Tested with Python 3.14 / 3.11)
- **Node.js**: v18+ (Tested with v24.18.0)
- **Git** (optional)

---

### 1. Backend Setup

Open a terminal and navigate to the backend directory:

```bash
cd rescue-vision/backend
```

#### Windows (PowerShell):
```powershell
# Create and activate virtual environment (optional if global python has packages)
python -m venv .venv
.\.venv\Scripts\Activate.ps1

# Install dependencies
python -m pip install -r requirements.txt

# Start backend ASGI server
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

#### macOS / Linux:
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

Backend will be available at:
- **API Base**: `http://localhost:8000`
- **Interactive Swagger Docs**: `http://localhost:8000/docs`

---

### 2. Frontend Setup

In a separate terminal, navigate to the frontend directory:

```bash
cd rescue-vision/frontend
```

```bash
# Install dependencies
npm install

# Start development server
npm run dev
```

Frontend will be available at:
- **Application URL**: `http://localhost:5173`

---

## 🧪 Running Automated Tests

Run backend unit and integration tests (validating health, database persistence, dataset validator, and real YOLO26 inference):

```bash
cd rescue-vision/backend
python -m pytest tests/test_api.py -v
```

---

## ⚠️ Operational Disclaimer

Rescue Vision provides **candidate decision-support leads** to human search and rescue operators. Conventional RGB optical computer vision cannot penetrate opaque solid rubble, detect vital signs, or assess biological status. All detections must be physically verified by qualified rescue personnel.
