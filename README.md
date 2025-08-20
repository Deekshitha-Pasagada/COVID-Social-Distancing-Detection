# COVID-Social-Distancing-Detection
Real-time social-distancing detection with TensorFlow + OpenCV; projects detections to a bird’s-eye view and flags distance violations.

# Social Distancing Detection (TensorFlow + OpenCV)
Real-time social distancing analyzer using TensorFlow Object Detection and OpenCV. It detects people in video frames, projects detections onto a **bird’s-eye view** using a perspective transform, and flags pairs that violate a minimum distance threshold.

# What’s inside
- **Object detection** wrapper for frozen TF models (`src/tf_model_object_detection.py`)
- **Bird’s-eye view transform** to measure pairwise distances (`src/bird_view_transfo_functions.py`)
- **Main pipeline** to process a video and render outputs (`src/social_distanciation_video_detection.py`)
- **Calibration tools** to pick perspective points (`src/calibrate_with_mouse.py`) and build videos from frames (`src/create_video.py`)
- **Config** for homography points & image params (`conf/config.yml`, `conf/config_birdview.yml`)
- **Sample assets:** `video/PETS2009.avi`, images & a demo `img/result.gif`

# Setup
1) Create & activate a virtual environment (recommended):
  ```bash
  python -m venv .venv # Windows: .venv\Scripts\activate
  source .venv/bin/activate # macOS/Linux
  ```
2) Install Dependencies:
```bash
pip install -r requirements.txt
```
The original project pinned older versions (TF 2.1.0, numpy 1.18.3). If you hit version conflicts, try those exact pins or use a recent CPU-only TF and adjust code accordingly.

# Download a detection model
Place a **TensorFlow Object Detection (frozen inference graph)** under `models/`, e.g.:
```
models/
└─ faster_rcnn_inception_v2_coco_2018_01_28/
   └─ frozen_inference_graph.pb
```
At runtime, the app will list subfolders in `models/` and prompt you to choose one.

# Calibrate perspective (optional but recommended)
Update `conf/config_birdview.yml` (or `conf/config.yml`) with four corner points of the ground plane in your scene.  
Use `src/calibrate_with_mouse.py` to click points on a reference frame and copy the coordinates into the YAML.

# Run
```bash
# from project root
python src/social_distanciation_video_detection.py
# - it will prompt you to select a model folder under 'models/'
# - it will process the video (default: video/PETS2009.avi)
# - it writes two outputs to 'output/': original with overlays + bird’s-eye view
```
Press `q` to stop the video loop.

# Notes
- Outputs are saved to `output/video.avi` and `output/bird_view.avi` (MJPG codec).
- If using your own video, place it under `video/` and adjust paths if needed.
- Distance thresholds, colors, and drawing options are set in the main script.
