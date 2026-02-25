# Bicep Curl Detection (MediaPipe + OpenCV)

A real-time **bicep curl rep counter** using a webcam feed. This project uses **MediaPipe Pose** to detect body landmarks, computes the **elbow angle** (shoulder–elbow–wrist), and counts repetitions based on **Up/Down** stages.

---

## Features
- Live webcam feed using OpenCV
- Pose landmark detection using MediaPipe Pose (33 landmarks)
- Elbow angle calculation (Left arm: Shoulder–Elbow–Wrist)
- Rep counting with stage detection (`down` → `up`)
- On-screen overlay showing **angle**, **reps**, and **stage**
- Skeleton drawing overlay

---

## Concept (How it works)

MediaPipe Pose estimates landmarks on the body. For bicep curls we track:
- **11**: Left Shoulder  
- **13**: Left Elbow  
- **15**: Left Wrist  

### Rep counting logic
1. Read webcam frames
2. Detect pose landmarks
3. Extract shoulder, elbow, wrist coordinates
4. Calculate the angle at the elbow
5. Determine stage:
   - Angle high (example: **~160°**) → **down**
   - Angle low (example: **~30°**) → **up**
6. Count **1 rep** when the arm transitions **down → up** (and must return to down before the next rep)

---

## Requirements
- Python 3.8+ recommended
- A working webcam

Dependencies:
- `mediapipe`
- `opencv-python`
- `numpy`

---

## Setup / Installation

Install dependencies:

```bash
pip install mediapipe opencv-python numpy
```

---

## Run the project

```bash
python bicep_curl.py
```

### Controls
- Press **ESC** to exit

---

## Configuration / Tuning

You may need to tune thresholds depending on camera angle and range of motion.

### Rep thresholds (inside the script)
Typical logic:
- `angle > 160` → stage becomes **down**
- `angle < 30` and stage was **down** → count **1 rep** and set stage to **up**

If reps are not counting correctly, print the angle values and adjust the thresholds.

### Webcam index
If you have multiple cameras:

```python
cap = cv2.VideoCapture(0)  # try 1, 2, ...
```

### Resolution

```python
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 1280)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 720)
```

### MediaPipe confidence

```python
mp_pose.Pose(min_detection_confidence=0.5, min_tracking_confidence=0.5)
```

---

## Tips for best results
- Keep your full upper body visible in the frame (especially shoulder, elbow, wrist)
- Use good lighting
- Avoid occluding the arm (hand behind body, going out of frame)
- A slight side-facing camera angle often improves elbow-angle stability

---

## Known limitations
- Tracks **left arm only** by default
- 2D angle calculation can be sensitive to camera viewpoint and depth changes
- Occlusions and low visibility may reduce accuracy

---

## Reference
- MediaPipe Pose documentation: https://developers.google.com/mediapipe
````
