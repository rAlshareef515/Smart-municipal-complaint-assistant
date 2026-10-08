# Smart Municipal Complaint Assistant

A computer vision system that uses **YOLO11n** to detect common municipal infrastructure issues from street images and generate an initial municipal complaint report.

## Project Idea

The system allows the user to upload or capture a street image, then automatically detects visible municipal issues such as:

- Potholes
- Sidewalk damage
- Road barriers

After detection, the system generates an initial Arabic municipal report based on the detected issue.

## Objective

The goal of this project is to support municipal complaint reporting using computer vision by:

- Detecting visible road and infrastructure damage
- Identifying the type of issue
- Drawing bounding boxes around detected objects
- Providing confidence scores
- Generating an initial municipal complaint description
- Providing a simple Gradio interface for end users

## Dataset

The dataset contains three object detection classes:

- `barriers`
- `sidewalks`
- `pothole`

Original dataset size:

- Total images: 31,795

The dataset was re-split into:

- Training: 70%
- Validation: 10%
- Test: 20%

Final split:

- Training: 22,256 original images
- Validation: 3,179 images
- Test: 6,360 images

After augmentation, the final training set contained:

- 33,384 training images

## Data Augmentation

Augmentation was applied only to the training data to improve model robustness and avoid data leakage.

The applied augmentation strategies included:

1. Horizontal Flip
2. Brightness Adjustment
3. Contrast Adjustment
4. Rotation
5. Scaling
6. Gaussian Blur

Bounding boxes were transformed together with the images to preserve correct object locations.

## Model

The project uses:

**YOLO11n**

The model was initialized using pretrained weights and fine-tuned on the municipal dataset using transfer learning.

### Training Configuration

- Epochs: 20
- Image size: 640 × 640
- Batch size: 16
- Hardware: Tesla T4 GPU
- Task: Object Detection

## Evaluation Results

The final model was evaluated on the held-out test set containing 6,360 images.

### Overall Performance

| Metric | Result |
|---|---:|
| Precision | 62.2% |
| Recall | 62.0% |
| mAP@50 | 65.1% |
| mAP@50-95 | 42.2% |

### Performance by Class

| Class | Precision | Recall | mAP@50 | mAP@50-95 |
|---|---:|---:|---:|---:|
| Barriers | 57.1% | 58.6% | 59.3% | 41.6% |
| Sidewalks | 61.0% | 57.5% | 60.7% | 35.4% |
| Pothole | 68.6% | 70.0% | 75.3% | 49.6% |

The strongest class was **Pothole**, with an mAP@50 of **75.3%**.

## Confusion Matrix

The normalized confusion matrix showed approximately:

- Barriers: 83% correct classification
- Sidewalks: 73% correct classification
- Potholes: 85% correct classification

The main confusion occurred between sidewalk damage and potholes.

## User Interface

A **Gradio** interface was created to make the model easier to use.

The user can:

- Upload an image
- Capture an image using a webcam
- Run the detection model
- View detected objects and bounding boxes
- Receive an Arabic municipal complaint report

The workflow is:

`Image → YOLO Detection → Issue Classification → Municipal Report`

## Example Report Logic

The model detects the issue class, while a rule-based layer converts the detected class into an initial municipal complaint description.

For example:

- `pothole` → Road surface damage / pothole report
- `barrier` → Road obstruction / barrier report
- `sidewalk` → Sidewalk damage report

YOLO performs the visual detection, while the reporting text is generated using rule-based logic.

## Files

- `Smart_Municipal_Complaint_Assistant.ipynb`  
  Main project notebook

- `best.pt`  
  Best trained YOLO model weights

## Technologies Used

- Python
- YOLO / Ultralytics
- OpenCV
- Albumentations
- Gradio
- Google Colab
- Matplotlib

## Limitations

The model currently detects only the three classes it was trained on.

Performance may be affected by:

- Poor lighting
- Image blur
- Unusual camera angles
- Small or partially visible objects
- Different road environments
- Class imbalance

A prediction of "no detected issue" means that the model did not detect any of the trained classes, not that the image is guaranteed to contain no municipal issue.

## Future Improvements

Possible future improvements include:

- Adding more municipal issue classes
- Expanding the dataset
- Improving class balance
- Adding more diverse environmental conditions
- Increasing training duration
- Improving the reporting logic
- Connecting the system to a real municipal complaint platform
- Deploying the model as a web or mobile application

## Project Pipeline

1. Data collection
2. Dataset inspection
3. Train / Validation / Test split
4. Data leakage check
5. Data augmentation
6. YOLO11n fine-tuning
7. Validation
8. Test evaluation
9. Confusion matrix analysis
10. Prediction visualization
11. Gradio deployment interface

---

Computer Vision Project  
Saudi Digital Academy / Computer Vision Training
