# 🚁 SkyGuardian: Advanced Drone, Airplane, and Helicopter Detection

## 📖 The Problem: Securing Our Skies
In modern airspace management, the rapid increase in unmanned aerial vehicles (UAVs) or drones has posed significant security and privacy challenges. Traditional detection systems, originally designed for large commercial aircraft and helicopters, often struggle to clearly identify small, fast-moving drones. Our goal with this project was to build a robust object detection model capable of distinctly identifying **Drones**, **Airplanes**, and **Helicopters** in various complex environmental conditions.

## 🛠️ Our Journey & Methodology

### 1. The Initial Solution & Dataset
We began our journey with an original curated dataset encompassing three classes:
- `0`: Airplane
- `1`: Drone
- `2`: Helicopter

Our initial Exploratory Data Analysis (EDA) (detailed in `Drone_Detection.ipynb`) showed a relatively balanced dataset (Drone: 6104, Airplane: 2274, Helicopter: 2374 instances). However, during our first iterations of model training, we noticed that while the model performed well on airplanes and helicopters due to their distinct and rigid structures, detecting drones proved challenging. Drones come in various sizes and shapes, and they often blend into complex backgrounds like cityscapes or cluttered skies.

### 2. Iteration: Enhancing the Model with More Data
To solve this lack of generalization, we realized the model needed to *see* more examples of drones in diverse environments. We decided to reiterate by augmenting our training data. We pulled in an additional dataset specifically focused on diverse drone imagery from Roboflow (`drones-mev5s`). 

**The Labeling Bottleneck:**
Merging datasets is rarely straightforward. We hit a major roadblock: inconsistent class labeling. In our original dataset, the `Drone` class was mapped to ID `1`, whereas in the newly acquired dataset, the `Drone` class was mapped to ID `0`. If left unaddressed, this would heavily confuse the model during training, causing catastrophic failure in performance.

**The Fix:** 
We engineered a custom Python remapping script (documented in `Drone_Detection.ipynb`) to iterate through the new dataset's label files (`.txt`). We meticulously checked and remapped the class IDs so that `0` correctly pointed to Airplane and `1` correctly pointed to Drone across the entire aggregated dataset. This ensured a unified, clean data pipeline.

### 3. Model Training & Validation
With a robust, combined, and properly labeled dataset, we leveraged the power of the **YOLO** (You Only Look Once) architecture for fast and precise object detection. We chose YOLO for its state-of-the-art balance between real-time inference speed and high mean Average Precision (mAP).

## 📊 Results & Model Performance

After rigorous training on the enhanced dataset, the model's ability to generalize and correctly draw bounding boxes around distant and fast-moving drones improved dramatically. The updated dataset led to a significant reduction in false positives (e.g., mistaking birds for drones) and a dense confusion matrix diagonal, proving our remapping and data augmentation strategies were successful.

**Here is a snapshot of our model's performance and validation:**

### Training & Evaluation Metrics
*(Below are evaluation captures illustrating the performance charts and validaton curves achieved by our trained model.)*

<img src="Drone_detection/output/results/Screenshot 2025-09-26 031402.png" alt="Model Performance Metric 1" />
<br>
<img src="Drone_detection/output/results/Screenshot 2025-09-26 031435.png" alt="Model Performance Metric 2" />

### Detection Insights
*(Below are evaluation captures illustrating precise bounding boxes and class confidence scores.)*

<img src="Drone_detection/output/results/Screenshot 2025-09-26 031502.png" alt="Detection Output 1" />
<br>
<img src="Drone_detection/output/results/Screenshot 2025-09-26 031551.png" alt="Detection Output 2" />

**Real-Time Video Predictions:** 
The model is highly effective on live/recorded footage. You can view a specific real-time object detection inference illustration by clicking on or downloading the following video clip:

🎥 **<a href="Drone_detection/output/results/Recording 2025-09-17 172326 (2).avi">Watch Real-Time Drone Detection Output</a>**

*(Note: Depending on your browser, `.avi` files may need to be downloaded to play locally. You can also view the `video_predictions.mp4` file alongside other recordings in the `output/results/` directory)*

---

## 🚀 Quick Start Guide

### Requirements
- Python 3.8+
- PyTorch
- OpenCV
- Ultralytics YOLOv8 (or YOLOv5 depending on your specific environment setup)

### Setup & Inference
1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/drone-detection.git
   cd drone-detection
   ```

2. **Run Inference on an Image:**
   You can easily test the best trained weights on the provided sample images.
   ```bash
   # Adjust 'yolo' command based on the specific version used in your environment
   yolo task=detect mode=predict model="output/my_output (1)/Dorne_detection_res/run_1/weights/best.pt" source="drones/drone.png"
   ```

## 📂 Project Structure
- `Drone_Detection.ipynb`: The main notebook containing our EDA, data processing (label remapping), and training pipeline. It walks through the exact narrative of our problem-solving process.
- `data/`: Contains the original and augmented datasets (ensure you have the `updated dataset` configuration).
- `output/`: Contains the run results, trained weights (`best.pt`, `last.pt`), and evaluation metrics (`Dorne_detection_res`).
- `output/results/`: Contains evaluation screenshots, graphs, and video predictions demonstrating model performance.
- `drones/`: Test images for quick inference and sanity checks.
- `Experiments_Notebooks/`: Additional exploratory environments and iterative trials.

## 🔮 Future Work
- **Real-time Tracking:** Implementing DeepSORT algorithms for continuous, persistent tracking of drones across consecutive video frames.
- **Edge Deployment:** Quantizing the model for deployment on edge devices like the Raspberry Pi or NVIDIA Jetson Nano to enable real-time, on-site security monitoring without heavy compute requirements.

---
**Author:** Mohamed Abdelghafar
**License:** MIT
