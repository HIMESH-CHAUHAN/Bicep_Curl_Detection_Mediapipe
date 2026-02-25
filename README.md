# Bicep_Curl_Detection_Mediapipe

A small computer vision project that detects **bicep curls** and counts **reps** in real time using the **MediaPipe Pose** model and a webcam feed via **OpenCV**.

MediaPipe detects **33 body landmarks**. This project uses the left arm landmarks to compute the **elbow angle** and determine whether the arm is in the **up** or **down** stage.

---

## Concept

MediaPipe Pose estimates keypoints (landmarks) on the body. For bicep curls we track:

- **Landmark 11**: Left Shoulder  
- **Landmark 13**: Left Elbow  
- **Landmark 15**: Left Wrist  

### Rep Counting Logic
1. Read webcam frames
2. Detect pose landmarks
3. Extract shoulder, elbow, wrist coordinates
4. Calculate the angle at the elbow
5. Determine stage:
   - Angle is large (e.g., **~160°**) → arm **down**
   - Angle is small (e.g., **~30°**) → arm **up**
6. Count **1 rep** when movement goes **Down → Up → Down** (one complete curl cycle)

---

## Tech Stack
- **Python**
- **OpenCV (`opencv-python`)**: webcam capture + on-screen drawing
- **MediaPipe (`mediapipe`)**: pose detection
- **NumPy (`numpy`)**: math and angle calculation

---

## Setup / Installation

Install dependencies:

```bash
pip install mediapipe opencv-python numpy
