# Box Size Estimation with RANSAC

A computer vision project that estimates the physical dimensions (height, length, width) of a box using depth sensor data and RANSAC plane fitting.

## Overview

This project processes Kinect depth camera data to:
1. Extract and visualize amplitude and distance images
2. Apply morphological filtering to reduce noise
3. Use RANSAC to detect floor and box top planes
4. Calculate box dimensions from 3D point cloud data
5. Visualize results with annotated overlays

## Features

- **Data Processing**: Load and explore Kinect .mat files containing amplitude, distance, and point cloud data
- **Noise Reduction**: Apply median and mean filters to improve signal quality
- **Plane Detection**: Robust RANSAC implementation for floor and box-top plane extraction
- **Dimension Measurement**: Calculate height (plane-to-plane distance), length, and width (corner-to-corner distances)
- **Visualization**: Multiple visualization modes including:
  - Amplitude and distance images
  - 3D point cloud plots
  - Segmented masks with color overlays
  - Corner detection and annotation

## Requirements

```txt
numpy
scipy
matplotlib
pathlib
```

## Usage

### Running the Notebook

1. Open `Box_Size_Estimation.ipynb` in Jupyter or VS Code
2. Run cells sequentially from top to bottom
3. Select which example to process by uncommenting the appropriate lines in the data loading cell

### Workflow Steps

#### 1. Data Exploration
- Load Kinect sensor data from .mat files
- Visualize amplitude and distance images
- Analyze depth value distributions

#### 2. Preprocessing
- Apply median filter (3×3) to distance data to reduce speckle noise
- Apply mean/median filters to amplitude data
- Remove invalid pixels (NaN, zero, negative values)
- Filter by amplitude threshold to remove weak signals
- Remove extreme distance values using percentile-based thresholding

#### 3. Floor Plane Detection (RANSAC #1)
- Extract valid 3D points from point cloud
- Run RANSAC to fit a plane to the floor
- Parameters: `max_iter=5000`, `distance_thresh=0.05`
- Apply morphological opening/closing to clean floor mask

#### 4. Box Top Detection (RANSAC #2)
- Remove floor inliers from candidate points
- Run second RANSAC to detect box top plane
- Parameters: `max_iter=2000`, `distance_thresh=0.007`
- Extract largest connected component as box top

#### 5. Dimension Calculation
- **Height**: Calculate perpendicular distance between floor and box-top planes
- **Length & Width**: 
  - Detect 4 corners from box mask using extreme pixel coordinates
  - Map corners to 3D coordinates
  - Compute pairwise distances
  - Extract edge lengths (ignoring diagonals)

#### 6. Visualization
- Generate overlay with color-coded regions:
  - Background: Light blue
  - Floor: Navy (dark blue)
  - Box top: Crimson (red)
  - Corners: Orange markers


## Project Structure

```
.
├── Box_Size_Estimation.ipynb  # Main notebook
├── data/                      # Kinect sensor data files
│   ├── example1kinect.mat
│   ├── example2kinect.mat
│   ├── example3kinect.mat
│   └── example4kinect.mat
└── README.md                 # This file
```

