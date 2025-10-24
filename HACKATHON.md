# Hand-Eye Robot Calibration Hackathon

## 🎯 Overview

Welcome to the Hand-Eye Robot Calibration Challenge! This 4-hour hackathon focuses on solving the **eye-in-hand** calibration problem - one of the fundamental challenges in robotic vision systems.

**Duration:** 4 hours  
**Difficulty:** Intermediate  
**Prerequisites:** Python, Linear Algebra, Computer Vision basics

---

## 📋 Problem Statement

### What is Hand-Eye Calibration?

In robotics, **hand-eye calibration** determines the transformation relationship between a camera mounted on a robot's end-effector (the "hand") and the robot's coordinate system. This calibration is crucial for:

- Visual servoing
- Object manipulation
- Precise pick-and-place operations
- 3D reconstruction from robot-mounted cameras

### Eye-in-Hand Configuration

In this challenge, you'll work with an **eye-in-hand** setup where:
- The camera is mounted on the robot's end-effector (moving with the robot)
- The calibration target (chessboard) is stationary in the workspace
- As the robot moves, the camera captures different views of the same chessboard

### Mathematical Formulation

The hand-eye calibration problem solves for the unknown transformation **X** (camera to end-effector):

```
A * X = X * B
```

Where:
- **A**: Transformation between two robot end-effector poses
- **B**: Transformation between corresponding camera observations
- **X**: Camera-to-end-effector transformation (what we want to find)

This is known as the **AX=XB problem** in robotics.

---

## 📦 Dataset Description

### Structure

```
dataset/
├── images/                          # Chessboard images from 5 robot poses
│   ├── image_00.png
│   ├── image_01.png
│   ├── image_02.png
│   ├── image_03.png
│   └── image_04.png
├── poses/                           # Robot end-effector poses
│   ├── pose_00.yaml
│   ├── pose_01.yaml
│   ├── pose_02.yaml
│   ├── pose_03.yaml
│   └── pose_04.yaml
```

### Dataset Details

- **Number of poses:** 5 calibration poses
- **Chessboard pattern:** 14×8 internal corners
- **Square size:** 7 mm
- **Image resolution:** Varies (see camera_intrinsics.yaml)
- **Coordinate system:** Right-handed, units in millimeters

### File Formats

#### Pose Files (YAML)
Each pose file contains:
```yaml
pose_matrix:
  data: 4x4 homogeneous transformation matrix
position:
  x, y, z: Translation in mm
orientation:
  matrix: 3x3 rotation matrix
  roll, pitch, yaw: Euler angles in degrees
  quaternion: [w, x, y, z]
```

## 🧠 Your Tasks

You must build a complete calibration pipeline that performs the following steps:

### Step 1: Detect Chessboard Corners

- Use `cv2.findChessboardCorners()` to detect corners in each image
- Refine corner positions using `cv2.cornerSubPix()`
- Visualize and save corner detections for verification

### Step 2: Estimate Camera Poses (B)

- For each image, use `cv2.solvePnP()` with known 3D chessboard points and detected corners
- Compute the camera pose relative to the chessboard
- Express each pose as a 4×4 homogeneous matrix

### Step 3: Parse Robot Poses (A)

- Load robot pose matrices from YAML files
- Compute relative transformations between consecutive robot poses

### Step 4: Compute Camera Motion (B)

- Similarly, compute relative transformations between consecutive camera poses from Step 2

### Step 5: Solve Hand-Eye Calibration

- Estimate X using one or more of the available methods

### Step 6: Validate the Calibration

- Project known 3D chessboard points using the estimated X and robot poses
- Compute reprojection error between observed and projected points
- Visualize results (draw reprojected corners on images)

---


## 📝 Submission Guidelines

**Submit the following:**

1. **Code Repository:**
   - All source code
   - Requirements.txt
   - README with instructions

2. **Results:**
   - Calibration matrix (X)
   - Validation metrics
   - Plots and visualizations

3. **Report as markdown file:**
   - Methodology description
   - Results and analysis
   - Conclusions and discussion

