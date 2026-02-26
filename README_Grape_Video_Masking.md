# Grape Masking using Computer Vision

## Overview

This project performs **grape segmentation from video input** using classical computer vision techniques. The system reads a video file, processes each frame, isolates grape regions based on their color characteristics, and generates a processed output video where only grape areas are highlighted.

Unlike image-based segmentation, this project demonstrates a **frame-by-frame video processing pipeline**, making it suitable for real-time or large-scale agricultural monitoring applications.

This project demonstrates:
- Video processing using OpenCV  
- Color-based segmentation in HSV color space  
- Frame-by-frame masking  
- Morphological noise removal  
- Video writing and output generation  

Applications include vineyard monitoring, yield estimation, automated harvesting support, and precision agriculture systems.

---

## Project Flow

The system follows a frame-based video processing pipeline:

1. Import required libraries  
2. Load input video  
3. Extract frames sequentially  
4. Convert each frame to HSV color space  
5. Apply color thresholding to detect grapes  
6. Clean the mask using morphological operations  
7. Apply the mask to the frame  
8. Write the processed frame to an output video  

```
Video Input → Frame Extraction → HSV Conversion → Color Thresholding → Mask Cleaning → Output Video
```

---

## Installation

Install required dependencies:

```bash
pip install opencv-python numpy
```

---

## Project Structure

```
grape_masking.ipynb
input_video.mp4
output_video.mp4
README.md
```

---

## Import Libraries

```python
import cv2
import numpy as np
```

### Explanation

- **cv2** – Handles video reading, frame processing, and video writing  
- **numpy** – Used for defining threshold ranges and mask operations  

---

## Load Video Input

```python
cap = cv2.VideoCapture("input_video.mp4")
```

### Explanation

The video is opened and processed frame-by-frame instead of loading the entire video into memory. This approach enables efficient processing and supports real-time applications.

---

## Initialize Video Writer

```python
width  = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
fps    = cap.get(cv2.CAP_PROP_FPS)

fourcc = cv2.VideoWriter_fourcc(*'mp4v')
out = cv2.VideoWriter("output_video.mp4", fourcc, fps, (width, height))
```

### Explanation

This step prepares the output file where processed frames will be saved as a new video.

---

## Frame-by-Frame Processing

```python
while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        break
```

Each frame is processed independently to apply grape segmentation.

---

## Convert Frame to HSV

```python
hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
```

### Explanation

HSV separates color information from brightness, making it more robust for color-based object segmentation.

---

## Color Thresholding (Mask Creation)

```python
lower_purple = np.array([120, 50, 50])
upper_purple = np.array([170, 255, 255])
mask = cv2.inRange(hsv, lower_purple, upper_purple)
```

### Explanation

Pixels within the defined grape color range are marked as foreground, creating a binary mask for each frame.

---

## Morphological Cleaning

```python
kernel = np.ones((5,5), np.uint8)
mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel)
mask = cv2.morphologyEx(mask, cv2.MORPH_CLOSE, kernel)
```

### Explanation

- **Opening** removes small noise regions  
- **Closing** fills small gaps inside grape regions  
- Improves the quality of segmentation  

---

## Apply Mask to Frame

```python
result = cv2.bitwise_and(frame, frame, mask=mask)
```

### Explanation

The mask is applied to retain only grape regions while removing the background.

---

## Write Output Frame

```python
out.write(result)
```

After processing all frames:

```python
cap.release()
out.release()
cv2.destroyAllWindows()
```

### Explanation

Ensures proper resource cleanup and saves the final processed video.

---

## Video Processing Pipeline (Detailed)

Each frame follows:

1. Frame Capture  
2. HSV Conversion  
3. Color Thresholding  
4. Morphological Cleaning  
5. Mask Application  
6. Output Writing  

This loop runs continuously for the full duration of the video.

---

## Practical Applications

- Grape yield estimation  
- Vineyard monitoring  
- Automated harvesting systems  
- Fruit detection and analysis  
- Precision agriculture analytics  

---

## Limitations

- Sensitive to lighting conditions  
- Requires threshold tuning for different environments  
- Background objects with similar color may affect performance  

---

## Future Improvements

- Adaptive color thresholding  
- Deep learning segmentation (U-Net / Mask R-CNN)  
- Automatic grape counting  
- Real-time deployment in field environments  

---

## Learning Outcomes

This project demonstrates:

- Frame-by-frame video processing  
- Color-based segmentation in video streams  
- Morphological mask refinement  
- End-to-end video analytics pipeline  

---

## Portfolio Value

This project highlights experience in:

- Video-based computer vision  
- Classical segmentation techniques  
- Agricultural AI applications  

Relevant for roles such as:
- Computer Vision Engineer  
- Machine Learning Engineer  
- Video Analytics Engineer  
- Precision Agriculture AI  

---

## Author

**Manasa Vijayendra Gokak**  
Graduate Student – Data Science